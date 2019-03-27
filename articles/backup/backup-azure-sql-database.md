---
title: Copia de seguridad de bases de datos de SQL Server en Azure | Microsoft Docs
description: Este tutorial explica cómo realizar una copia de seguridad de SQL Server en Azure. En este tutorial también se explica cómo se realiza la recuperación de SQL Server.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 03/19/2019
ms.author: raynew
ms.openlocfilehash: 4eaaff859811e4d97cbd4f73231d702285792064
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58285452"
---
# <a name="about-sql-server-backup-in-azure-vms"></a>Acerca de la copia de seguridad de SQL Server en máquinas virtuales de Azure

Las bases de datos SQL Server son cargas de trabajo críticas que requieren un bajo objetivo de punto de recuperación (RPO) y retención a largo plazo. Puede hacer una copia de seguridad de las bases de datos de SQL Server que se ejecutan en máquinas virtuales de Azure mediante [Azure Backup](backup-overview.md).

## <a name="backup-process"></a>Proceso de copia de seguridad

Esta solución aprovecha las API nativas de SQL para realizar copias de seguridad de las bases de datos SQL.

* Una vez que especifique la VM con SQL Server que desea proteger y que consulte las bases de datos que hay en ella, el servicio Azure Backup instalará una extensión de copia de seguridad de cargas de trabajo en la máquina virtual con el nombre `AzureBackupWindowsWorkload` .
* Esta extensión consta de un coordinador y un complemento SQL. Mientras que el coordinador es el responsable de desencadenar los flujos de trabajo de varias operaciones como la configuración de las copias de seguridad o la copia de seguridad y restauración, el complemento es responsable de flujo de datos real.
* Para poder detectar bases de datos en esta máquina virtual, Azure Backup crea la cuenta `NT SERVICE\AzureWLBackupPluginSvc`. Dicha cuenta se usa para realizar operaciones de copia de seguridad y restauración, y requiere permisos de administrador del sistema de SQL. Azure Backup aprovecha la cuenta  `NT AUTHORITY\SYSTEM`  para la detección y consulta de bases de datos, por lo que debe tener un inicio de sesión público en SQL. Si la VM con SQL Server no se ha creado en Azure Marketplace, es posible que reciba el error  **UserErrorSQLNoSysadminMembership**. Si esto sucede,  [siga estas instrucciones](backup-azure-sql-database.md).
* Una vez que desencadene la protección de la configuración en las bases de datos seleccionados, el servicio de copia de seguridad configura el coordinador con las programaciones de las copias de seguridad y otros detalles de la directiva, que la extensión almacena en caché localmente en la máquina virtual 
* A la hora programada, el coordinador se comunica con el complemento y empieza a transmitir los datos de la copia de seguridad en secuencias desde el servidor con SQL Server mediante VDI.  
* El complemento envía los datos directamente en el almacén de Recovery Services, lo que elimina la necesidad de una ubicación de almacenamiento provisional. El servicio Azure Backup cifra los datos y los almacena en cuentas de almacenamiento.
* Cuando se completa la transferencia de datos, el coordinador realiza la confirmación con el servicio de copia de seguridad.

  ![Arquitectura de copia de seguridad de SQL](./media/backup-azure-sql-database/backup-sql-overview.png)

## <a name="before-you-start"></a>Antes de comenzar

Antes de empezar, compruebe lo siguiente:

1. Asegúrese de tener una instancia de SQL Server que se ejecuta en Azure. Puede [crear rápidamente una instancia de SQL Server](../virtual-machines/windows/sql/quickstart-sql-vm-create-portal.md) en Marketplace.
2. Revise los apartados en que se realizan [consideraciones acerca de las características](#feature-consideration-and-limitations) y se indica la [compatibilidad con los escenarios](#scenario-support).
3. [Revise las preguntas más frecuentes](faq-backup-sql-server.md) sobre este escenario.

## <a name="scenario-support"></a>Compatibilidad con los escenarios

**Soporte técnico** | **Detalles**
--- | ---
**Implementaciones admitidas** | Se admiten máquinas virtuales de Azure de SQL Marketplace y que no son de Marketplace (SQL Server instalado manualmente).
**Zonas geográficas admitidas** | Sudeste de Australia (ASE) y Australia Oriental (AE) <br> Sur de Brasil (BRS)<br> Centro de Canadá (CNC) y Este de Canadá (CE)<br> Asia Suroriental (SEA) y Asia Oriental (EA) <br> Este de EE. UU. (EUS), Este de EE. UU. 2 (EUS2), Centro-oeste de EE. UU. (WCUS), Oeste de EE. UU. (WUS); Oeste de EE. UU. 2 (WUS 2) Centro-norte de EE. UU. (NCUS) Centro de EE. UU. (CUS) Centro-sur de EE. UU. (SCUS) <br> Centro de la India (INC), India del Sur (INS) <br> Este de Japón (JPE) y Oeste de Japón (JPW) <br> Centro de Corea del Sur (KRC), Sur de Corea del Sur (KRS) <br> Norte de Europa (NE) y Oeste de Europa <br> Sur de Reino Unido (UKS) y Oeste de Reino Unido (UKW)
**Sistemas operativos compatibles** | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012<br/><br/> Linux no se admite actualmente.
**Versiones admitidas de SQL Server** | SQL Server 2017; SQL Server 2016, SQL Server 2014, SQL Server 2012.<br/><br/> Enterprise, Standard, Web, Developer, Express.
**Versiones de .NET compatibles** | .NET Framework 4.5.2, y versiones posteriores, instaladas en la máquina virtual

## <a name="feature-consideration-and-limitations"></a>Consideraciones y limitaciones de las características

- La copia de seguridad de SQL Server se puede configurar en Azure Portal o **PowerShell**. No se admite la CLI.
- La máquina virtual que ejecuta SQL Server requiere conectividad a Internet para acceder a las direcciones IP públicas de Azure.
- No se admiten las **instancias de clústeres de conmutación por error (FCI)** de los grupos de disponibilidad Always On de SQL Server.
- No se admiten operaciones de copia de seguridad y restauración de bases de datos reflejadas ni de instantáneas de bases de datos.
- Si se usa más de una solución para realizar copias de seguridad de una instancia de SQL Server independiente o de un grupo de disponibilidad Always On de SQL, se pueden producir errores en la copia de seguridad, por lo que es aconsejable evitar hacerlo.
- La realización de una copia de seguridad de dos nodos de un grupo de disponibilidad individualmente con las mismas soluciones o soluciones diferentes, también puede dar lugar a errores en la copia de seguridad. Azure Backup puede detectar todos los nodos que se encuentran en la misma región que el almacén y protegerlos. Si su grupo de disponibilidad Always On de SQL Server abarca varias regiones de Azure, configure la copia de seguridad de la región que tenga el nodo principal. Azure Backup puede detectar todas las bases de datos del grupo de disponibilidad en función de sus preferencias con respecto a la copia de seguridad y protegerlas.  
- Azure Backup admite solo los tipos de copia de seguridad Completa y Solo copia completa en las bases de datos **de solo lectura**
- Las bases de datos con un gran número de archivos no se pueden proteger. El número máximo de archivos admitidos es **aproximadamente 1000**.  
- Puede hacer una copia de seguridad de hasta **aproximadamente 2000** bases de datos de SQL Server en un almacén. Si tiene un número mayor de bases de datos, puede crear varios almacenes.
- Puede configurar la copia de seguridad de hasta **50** bases de datos a la vez; esta restricción ayuda a optimizar la carga de copias de seguridad.
- Se admiten bases de datos de hasta **2 TB** de tamaño; si su tamaño es mayor pueden surgir problemas de rendimiento.
- Para saber el número aproximado de bases de datos que se pueden proteger por servidor, es preciso tener en cuenta factores tales como el ancho de banda, el tamaño de la máquina virtual, la frecuencia de copia de seguridad, el tamaño de la base de datos, etc. Estamos trabajando en una herramienta de planeación que le ayudaría a calcular estas cifras por su cuenta. La publicaremos en breve.
- En el caso de los grupos de disponibilidad, las copias de seguridad se realizan de los distintos nodos en función de una serie de factores. A continuación, se resume el comportamiento del proceso de copia de seguridad en un grupo de disponibilidad.

### <a name="backup-behavior-in-case-of-always-on-availability-groups"></a>Comportamiento del proceso de copia de seguridad en el caso de los grupos de disponibilidad Always On

En función de las preferencias relativas a la copia de seguridad y de los tipos de copia de seguridad (completa, diferencial, de registros, y solo copia completa), se toman las copias de seguridad de un nodo concreto (principal o secundario).

- **Preferencias de copia de seguridad: principal**

**Tipo de copia de seguridad** | **Node**
    --- | ---
    Completo | Principal
    Diferencial | Principal
    Registro |  Principal
    Solo copia completa |  Principal

- **Preferencias de copia de seguridad: solo secundaria**

**Tipo de copia de seguridad** | **Node**
--- | ---
Completo | Principal
Diferencial | Principal
Registro |  Secundario
Solo copia completa |  Secundario

- **Preferencias de copia de seguridad: secundaria**

**Tipo de copia de seguridad** | **Node**
--- | ---
Completo | Principal
Diferencial | Principal
Registro |  Secundario
Solo copia completa |  Secundario

- **Sin preferencias para copia de seguridad**

**Tipo de copia de seguridad** | **Node**
--- | ---
Completo | Principal
Diferencial | Principal
Registro |  Secundario
Solo copia completa |  Secundario

## <a name="fix-sql-sysadmin-permissions"></a>Corrección de permisos de administrador del sistema de SQL

  Si necesita corregir los permisos debido un error **UserErrorSQLNoSysadminMembership**, haga lo siguiente:

  1. Use una cuenta con permisos sysadmin de SQL Server para iniciar sesión en SQL Server Management Studio (SSMS). A menos que necesite permisos especiales, debería funcionar la autenticación de Windows.
  2. En el servidor SQL Server, abra la carpeta **Seguridad/Inicios de sesión**.

      ![Abrir carpeta Security/Logins para ver las cuentas](./media/backup-azure-sql-database/security-login-list.png)

  3. Haga clic con el botón derecho en la carpeta **Logins** y seleccione **Nuevo inicio de sesión**. En **Inicio de sesión: Nuevo**, seleccione **Buscar**.

      ![En el cuadro de diálogo Inicio de sesión - Nuevo, seleccionar Buscar](./media/backup-azure-sql-database/new-login-search.png)

  4. La cuenta de servicio virtual de Windows **NT SERVICE\AzureWLBackupPluginSvc** se creó durante las fases de registro de la máquina virtual y de detección de SQL. Escriba el nombre de la cuenta como se muestra en **Escriba el nombre del objeto que desea seleccionar**. Seleccione **Comprobar nombres** para resolver el nombre. Haga clic en **OK**.

      ![Seleccionar Comprobar nombres para resolver el nombre de servicio desconocido](./media/backup-azure-sql-database/check-name.png)

  5. En **Roles de servidor**, asegúrese de que está seleccionado el rol **sysadmin**. Haga clic en **OK**. Ahora deben existir los permisos necesarios.

      ![Asegúrese de que el rol del servidor administrador del sistema está seleccionado](./media/backup-azure-sql-database/sysadmin-server-role.png)

  6. Ahora, asocie la base de datos con el almacén de Recovery Services. En Azure Portal, en la lista **Servidores protegidos**, haga clic con el botón derecho en el servidor que tiene un estado de error > **Rediscover DBs** (Volver a detectar bases de datos).

      ![Comprobar que el servidor tiene los permisos adecuados](./media/backup-azure-sql-database/check-erroneous-server.png)

  7. Compruebe el progreso en el área **Notificaciones**. Cuando se detectan las bases de datos seleccionadas, aparece un mensaje de operación correcta.

      ![Mensaje de implementación correcta](./media/backup-azure-sql-database/notifications-db-discovered.png)


## <a name="next-steps"></a>Pasos siguientes

- [Más información](backup-sql-server-database-azure-vms.md) acerca de la copia de seguridad de bases de datos de SQL Server.
- [Más información](restore-sql-database-azure-vm.md) sobre la restauración de copias de seguridad de bases de datos de SQL Server.
- [Más información](manage-monitor-sql-database-backup.md) sobre la administración de copias de seguridad de bases de datos de SQL Server.
