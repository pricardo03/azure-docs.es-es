---
title: Copia de seguridad de bases de datos de SQL Server en Azure
description: Este tutorial explica cómo realizar una copia de seguridad de SQL Server en Azure. En este tutorial también se explica cómo se realiza la recuperación de SQL Server.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 06/18/2019
ms.author: dacurwin
ms.openlocfilehash: e5d24c35fd2fafc27f2339af5b1c92875b0138d9
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73162206"
---
# <a name="about-sql-server-backup-in-azure-vms"></a>Acerca de la copia de seguridad de SQL Server en máquinas virtuales de Azure

Las bases de datos SQL Server son cargas de trabajo críticas que requieren un bajo objetivo de punto de recuperación (RPO) y retención a largo plazo. Puede hacer una copia de seguridad de las bases de datos de SQL Server que se ejecutan en máquinas virtuales de Azure mediante [Azure Backup](backup-overview.md).

## <a name="backup-process"></a>Proceso de copia de seguridad

Esta solución aprovecha las API nativas de SQL para realizar copias de seguridad de las bases de datos SQL.

* Una vez que especifique la máquina virtual con SQL Server que desea proteger y que consulte las bases de datos que hay en ella, el servicio Azure Backup instalará una extensión de copia de seguridad de cargas de trabajo en la máquina virtual con la extensión `AzureBackupWindowsWorkload` del nombre.
* Esta extensión consta de un coordinador y un complemento SQL. Mientras que el coordinador es el responsable de desencadenar los flujos de trabajo de varias operaciones como la configuración de las copias de seguridad o la copia de seguridad y restauración, el complemento es responsable de flujo de datos real.
* Para poder detectar bases de datos en esta máquina virtual, Azure Backup crea la cuenta `NT SERVICE\AzureWLBackupPluginSvc`. Dicha cuenta se usa para realizar operaciones de copia de seguridad y restauración, y requiere permisos de administrador del sistema de SQL. La cuenta `NT SERVICE\AzureWLBackupPluginSvc` es una [cuenta de servicio virtual](https://docs.microsoft.com/windows/security/identity-protection/access-control/service-accounts#virtual-accounts) y, por lo tanto, no requiere administración de contraseñas. Azure Backup aprovecha la cuenta `NT AUTHORITY\SYSTEM` para la detección y la consulta de bases de datos, por lo que debe tener un inicio de sesión público en SQL. Si no se creó la VM con SQL Server en Azure Marketplace, podría recibir el error **UserErrorSQLNoSysadminMembership**. Si esto sucede, [siga estas instrucciones](#set-vm-permissions).
* Una vez que desencadene la protección de la configuración en las bases de datos seleccionadas, el servicio de copia de seguridad configura el coordinador con las programaciones de las copias de seguridad y otros detalles de la directiva, que la extensión almacena en caché localmente en la máquina virtual.
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
**Zonas geográficas admitidas** | Sudeste de Australia (ASE), Este de Australia (AE), Centro de Australia (AC) y Centro de Australia 2 (AC) <br> Sur de Brasil (BRS)<br> Centro de Canadá (CNC) y Este de Canadá (CE)<br> Asia Suroriental (SEA) y Asia Oriental (EA) <br> Este de EE. UU. (EUS), Este de EE. UU. 2 (EUS2), Centro-oeste de EE. UU. (WCUS), Oeste de EE. UU. (WUS); Oeste de EE. UU. 2 (WUS 2) Centro-norte de EE. UU. (NCUS) Centro de EE. UU. (CUS) Centro-sur de EE. UU. (SCUS) <br> Centro de la India (INC), India del Sur (INS), Oeste de la India <br> Este de Japón (JPE) y Oeste de Japón (JPW) <br> Centro de Corea del Sur (KRC), Sur de Corea del Sur (KRS) <br> Norte de Europa (NE) y Oeste de Europa <br> Sur de Reino Unido (UKS) y Oeste de Reino Unido (UKW) <br> US Gov Arizona, US Gov Virginia, US Gov Texas, US DoD (centro), US DoD (este) <br> Norte de Alemania, Centro-oeste de Alemania <br> Norte de Suiza, Oeste de Suiza <br> Centro de Francia <br> Este de China, Norte de China, Norte de China 2 y Este de China 2
**Sistemas operativos compatibles** | Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2 SP1 <br/><br/> Linux no se admite actualmente.
**Versiones admitidas de SQL Server** | SQL Server 2019, SQL Server 2017 tal como se detalla en la [página de búsqueda del ciclo de vida del producto](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202017), SQL Server 2016 y los SP tal como se detalla en la [página de búsqueda del ciclo de vida del producto](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202016%20service%20pack), SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008 <br/><br/> Enterprise, Standard, Web, Developer, Express.
**Versiones de .NET compatibles** | .NET Framework 4.5.2 o posterior instalado en la máquina virtual

## <a name="feature-consideration-and-limitations"></a>Consideraciones y limitaciones de las características

* La copia de seguridad de SQL Server se puede configurar en Azure Portal o **PowerShell**. No se admite la CLI.
* La solución es compatible con ambos tipos de [implementaciones](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model): las máquinas virtuales de Azure Resource Manager y las máquinas virtuales clásicas.
* La máquina virtual que ejecuta SQL Server requiere conectividad a Internet para acceder a las direcciones IP públicas de Azure.
* La **instancia del clúster de conmutación por error (FCI)** de SQL Server y la de SQL Server AlwaysOn no son compatibles.
* No se admiten operaciones de copia de seguridad y restauración de bases de datos reflejadas ni de instantáneas de bases de datos.
* Si se usa más de una solución para realizar copias de seguridad de una instancia de SQL Server independiente o de un grupo de disponibilidad Always On de SQL, se pueden producir errores en la copia de seguridad, por lo que es aconsejable evitarlo.
* La realización de una copia de seguridad de dos nodos de un grupo de disponibilidad individualmente con las mismas soluciones o soluciones diferentes, también puede dar lugar a errores en la copia de seguridad.
* Azure Backup admite solo los tipos de copia de seguridad Completa y Solo copia completa en las bases de datos **de solo lectura**
* Las bases de datos con un gran número de archivos no se pueden proteger. El número máximo de archivos admitidos es **aproximadamente 1000**.  
* Puede hacer una copia de seguridad de hasta **aproximadamente 2000** bases de datos de SQL Server en un almacén. Si tiene un número mayor de bases de datos, puede crear varios almacenes.
* Puede configurar la copia de seguridad de hasta **50** bases de datos a la vez; esta restricción ayuda a optimizar la carga de copias de seguridad.
* Se admiten bases de datos de hasta **2 TB** de tamaño; si su tamaño es mayor, pueden surgir problemas de rendimiento.
* Para saber el número aproximado de bases de datos que se pueden proteger por servidor, es preciso tener en cuenta factores tales como el ancho de banda, el tamaño de la máquina virtual, la frecuencia de copia de seguridad, el tamaño de la base de datos, etc. [Descargue](https://download.microsoft.com/download/A/B/5/AB5D86F0-DCB7-4DC3-9872-6155C96DE500/SQL%20Server%20in%20Azure%20VM%20Backup%20Scale%20Calculator.xlsx) la herramienta de planeamiento de recursos que proporciona el número aproximado de bases de datos que puede tener por servidor según los recursos de la máquina virtual y la directiva de copia de seguridad.
* En el caso de los grupos de disponibilidad, las copias de seguridad se realizan de los distintos nodos en función de una serie de factores. A continuación, se resume el comportamiento del proceso de copia de seguridad en un grupo de disponibilidad.

### <a name="back-up-behavior-in-case-of-always-on-availability-groups"></a>Comportamiento del proceso de copia de seguridad en el caso de los grupos de disponibilidad Always On

Se recomienda que la copia de seguridad se configure en un único nodo de un grupo de disponibilidad. La copia de seguridad siempre debe configurarse en la misma región que el nodo principal. En otras palabras, siempre es necesario que el nodo principal esté presente en la región en la que va a configurar la copia de seguridad. Si todos los nodos del grupo de disponibilidad están en la misma región en la que se configura la copia de seguridad, no hay ningún problema.

#### <a name="for-cross-region-ag"></a>Para los grupos de disponibilidad con varias regiones

* Independientemente de la preferencia de copia de seguridad, las copias de seguridad no se harán en los nodos que no estén en la misma región donde se configure la copia de seguridad. Esto se debe a que no se admiten las copias de seguridad entre regiones. Si tiene solo dos nodos y el secundario está en la otra región; en este caso, las copias de seguridad seguirán produciéndose desde el nodo principal (a menos que su preferencia de copia de seguridad sea "solo secundaria").
* Si se produce una conmutación por error a una región diferente de aquella en la que se configura la copia de seguridad, las copias de seguridad producirían un error en los nodos de la región conmutada por error.

En función de las preferencias relativas a la copia de seguridad y de los tipos de copia de seguridad (completa, diferencial, de registros, y solo copia completa), se toman las copias de seguridad de un nodo concreto (principal o secundario).

* **Preferencias de copia de seguridad: principal**

**Tipo de copia de seguridad** | **Node**
    --- | ---
    Completo | Principal
    Diferencial | Principal
    Log |  Principal
    Solo copia completa |  Principal

* **Preferencias de copia de seguridad: solo secundaria**

**Tipo de copia de seguridad** | **Node**
--- | ---
Completo | Principal
Diferencial | Principal
Log |  Secundario
Solo copia completa |  Secundario

* **Preferencias de copia de seguridad: secundaria**

**Tipo de copia de seguridad** | **Node**
--- | ---
Completo | Principal
Diferencial | Principal
Log |  Secundario
Solo copia completa |  Secundario

* **Sin preferencias para copia de seguridad**

**Tipo de copia de seguridad** | **Node**
--- | ---
Completo | Principal
Diferencial | Principal
Log |  Secundario
Solo copia completa |  Secundario

## <a name="set-vm-permissions"></a>Establecer permisos de máquina virtual

  Al ejecutar la detección en un servidor SQL Server, Azure Backup hace lo siguiente:

* Agrega la extensión AzureBackupWindowsWorkload.
* Crea una cuenta NT SERVICE\AzureWLBackupPluginSvc para detectar las bases de datos en la máquina virtual. Dicha cuenta se usa para realizar una operación de copia de seguridad y restauración, y requiere permisos de administrador del sistema de SQL.
* Detecta las bases de datos que se ejecutan en una CM, Azure Backup usa la cuenta NT AUTHORITY\SYSTEM. Esta cuenta ofrecer inicio de sesión público en SQL.

Si no creó la VM con SQL Server en Azure Marketplace o si usa SQL 2008 y 2008 R2, es posible que reciba un error **UserErrorSQLNoSysadminMembership**.

Para conceder permisos en el caso de **SQL 2008** y **2008 R2** que se ejecutan en Windows 2008 R2, consulte [aquí](#give-sql-sysadmin-permissions-for-sql-2008-and-sql-2008-r2).

Para las demás versiones, realice los pasos siguientes para corregir los permisos:

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

> [!NOTE]
> Si su servidor SQL Server tiene varias instancias de SQL Server instaladas, debe agregar permisos de administrador del sistema a la cuenta **NT Service\AzureWLBackupPluginSvc** en todas las instancias de SQL.

### <a name="give-sql-sysadmin-permissions-for-sql-2008-and-sql-2008-r2"></a>Conceder permisos de administrador del sistema para SQL 2008 y SQL 2008 R2

Agregue los inicios de sesión **NT AUTHORITY\SYSTEM** y **Service\AzureWLBackupPluginSvc NT** en la instancia de SQL Server:

1. Vaya a la instancia de SQL Server en el Explorador de objetos.
2. Vaya a Seguridad -> Inicios de sesión.
3. Haga clic con el botón derecho en Inicios de sesión y seleccione *Nuevo inicio de sesión...*

    ![Nuevo inicio de sesión mediante SSMS](media/backup-azure-sql-database/sql-2k8-new-login-ssms.png)

4. Vaya a la pestaña General y escriba **NT AUTHORITY\SYSTEM** como nombre de inicio de sesión.

    ![nombre de inicio de sesión para SSMS](media/backup-azure-sql-database/sql-2k8-nt-authority-ssms.png)

5. Vaya a *Roles del servidor* y elija los roles *public* y *sysadmin*.

    ![elegir de roles en SSMS](media/backup-azure-sql-database/sql-2k8-server-roles-ssms.png)

6. Vaya a *Estado*. En Permiso de conexión al motor de base de datos, seleccione *Conceder* y en Inicio de sesión, seleccione *Habilitado*.

    ![Conceder permisos en SSMS](media/backup-azure-sql-database/sql-2k8-grant-permission-ssms.png)

7. Haga clic en Aceptar.
8. Repita la misma secuencia de pasos (de 1 a 7 arriba) para agregar el inicio de sesión NT Service\AzureWLBackupPluginSvc a la instancia de SQL Server. Si el inicio de sesión ya existe, asegúrese de que tenga el rol de servidor sysadmin y, en Estado, en Permiso de conexión al motor de base de dato, se haya seleccionado Conceder y, en Inicio de sesión, se haya seleccionado Habilitado.
9. Después de conceder el permiso, seleccione **Re-discover DBs** (Volver a detectar bases de datos) en el portal: Almacén **->** Infraestructura de Backup **->** Workload in Azure VM (Carga de trabajo en la VM de Azure):

    ![Volver a detectar bases de datos en Azure Portal](media/backup-azure-sql-database/sql-rediscover-dbs.png)

Como alternativa, puede automatizar la concesión de los permisos al ejecutar los siguientes comandos de PowerShell en modo de administrador. El nombre de instancia se establece en MSSQLSERVER de manera predeterminada. Si fuera necesario, cambie el argumento de nombre de instancia en el script:

```powershell
param(
    [Parameter(Mandatory=$false)]
    [string] $InstanceName = "MSSQLSERVER"
)
if ($InstanceName -eq "MSSQLSERVER")
{
    $fullInstance = $env:COMPUTERNAME   # In case it is the default SQL Server Instance
}
else
{
    $fullInstance = $env:COMPUTERNAME + "\" + $InstanceName   # In case of named instance
}
try
{
    sqlcmd.exe -S $fullInstance -Q "sp_addsrvrolemember 'NT Service\AzureWLBackupPluginSvc', 'sysadmin'" # Adds login with sysadmin permission if already not available
}
catch
{
    Write-Host "An error occurred:"
    Write-Host $_.Exception|format-list -force
}
try
{
    sqlcmd.exe -S $fullInstance -Q "sp_addsrvrolemember 'NT AUTHORITY\SYSTEM', 'sysadmin'" # Adds login with sysadmin permission if already not available
}
catch
{
    Write-Host "An error occurred:"
    Write-Host $_.Exception|format-list -force
}
```

## <a name="next-steps"></a>Pasos siguientes

* [Más información](backup-sql-server-database-azure-vms.md) acerca de la copia de seguridad de bases de datos de SQL Server.
* [Más información](restore-sql-database-azure-vm.md) sobre la restauración de copias de seguridad de bases de datos de SQL Server.
* [Más información](manage-monitor-sql-database-backup.md) sobre la administración de copias de seguridad de bases de datos de SQL Server.
