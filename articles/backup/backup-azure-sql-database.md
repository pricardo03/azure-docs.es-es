---
title: Copia de seguridad de bases de datos de SQL Server en Azure | Microsoft Docs
description: Este tutorial explica cómo realizar una copia de seguridad de SQL Server en Azure. En este tutorial también se explica cómo se realiza la recuperación de SQL Server.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: 17ec7723044cec391ebe390bbcfba3aa6f2f29ca
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56446858"
---
# <a name="back-up-sql-server-databases-on-azure-vms"></a>Copia de seguridad de bases de datos de SQL Server en máquinas virtuales de Azure 

Las bases de datos SQL Server son cargas de trabajo críticas que requieren un bajo objetivo de punto de recuperación (RPO) y retención a largo plazo. Puede hacer una copia de seguridad de las bases de datos de SQL Server que se ejecutan en máquinas virtuales de Azure mediante [Azure Backup](backup-overview.md). 

En este artículo se muestra cómo hacer una copia de seguridad de una base de datos de SQL Server que se ejecuta en una máquina virtual de Azure en un almacén de Azure Backup Recovery Services. En este artículo, aprenderá a:

> [!div class="checklist"]
> * Comprobar los requisitos previos para la copia de seguridad de una instancia de SQL Server.
> * Crear y configurar un almacén.
> * Detectar bases de datos y configurar copias de seguridad.
> * Configurar la protección automática de las bases de datos.

> [!NOTE]
> Esta característica actualmente está en su versión preliminar pública.

## <a name="before-you-start"></a>Antes de comenzar

Antes de empezar, compruebe lo siguiente:

1. Asegúrese de tener una instancia de SQL Server que se ejecuta en Azure. Puede [crear rápidamente una instancia de SQL Server](../sql-database/sql-database-get-started-portal.md) en Marketplace.
2. Revise las siguientes limitaciones de la versión preliminar pública.
3. Revise la compatibilidad con los escenarios.
4. [Revise las preguntas más frecuentes](faq-backup-sql-server.md) sobre este escenario.


## <a name="preview-limitations"></a>Limitaciones de vista previa

Esta versión preliminar pública presenta una serie de limitaciones.

- La máquina virtual que ejecuta SQL Server requiere conectividad a Internet para acceder a las direcciones IP públicas de Azure. 
- Puede hacer una copia de seguridad de hasta 2000 bases de datos de SQL Server en un almacén. Si tiene más, cree otro almacén. 
- Las copias de seguridad de [grupos de disponibilidad distribuidos](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017) no funcionan completamente.
- No se admiten las instancias de clústeres de conmutación por error (FCI) de los grupos de disponibilidad Always On de SQL Server para las copias de seguridad.
- La copia de seguridad de SQL Server debe configurarse en el portal. Actualmente, no se puede configurar la copia de seguridad con Azure PowerShell, la CLI o las API REST.
- No se admiten operaciones de copia de seguridad y restauración de bases de datos reflejadas, instantáneas de bases de datos y bases de datos de FCI.
- Las bases de datos con un gran número de archivos no se pueden proteger. El número máximo de archivos admitidos no es determinista. No solo depende del número de archivos, sino también de la longitud de la ruta de acceso de los archivos. 

Consulte la [sección de preguntas frecuentes](https://docs.microsoft.com/azure/backup/backup-azure-sql-database#faq) para más información sobre los escenarios admitidos y no admitidos.

## <a name="scenario-support"></a>Compatibilidad con los escenarios

**Soporte técnico** | **Detalles**
--- | ---
**Implementaciones admitidas** | Se admiten máquinas virtuales de Azure de SQL Marketplace y que no son de Marketplace (SQL Server instalado manualmente).
**Zonas geográficas admitidas** | Sudeste de Australia (ASE); Sur de Brasil (BRS); Centro de Canadá (CNC); Este de Canadá (CE); Centro de EE. UU. (CUS); Asia Oriental(EA); Este de Australia (AE); Este de EE. UU. (EUS); Este de EE. UU. 2 (EUS2); Centro de la India (INC); Sur de la India (INS); Este de Japón (JPE); Oeste de Japón (JPW); Centro de Corea del Sur (KRC); Sur de Corea del Sur (KRS); Centro-norte de EE. UU. (NCUS); Europa del Norte (NE); Centro y Sur de EE. UU. (SCUS); Sudeste Asiático (SEA); Sur de Reino Unido (UKS); Oeste de Reino Unido (UKW); Centro-oeste de EE. UU. (WCUS); Europa Occidental (WE); Oeste de EE. UU. (WUS); Oeste de EE. UU. 2 (WUS 2)
**Sistemas operativos compatibles** | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012<br/><br/> Linux no se admite actualmente.
**Versiones admitidas de SQL Server** | SQL Server 2017; SQL Server 2016, SQL Server 2014, SQL Server 2012.<br/><br/> Enterprise, Standard, Web, Developer, Express.


## <a name="prerequisites"></a>Requisitos previos

Para poder realizar copias de seguridad de la base de datos de SQL Server, primero debe comprobar si reúne las siguientes condiciones:

1. Identifique o [cree](backup-azure-sql-database.md#create-a-recovery-services-vault) un almacén de Recovery Services en la misma región o configuración regional que la máquina virtual que hospeda la instancia de SQL Server.
2. [Compruebe los permisos de máquina virtual](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms) necesarios para realizar una copia de seguridad de las bases de datos SQL.
3. Compruebe que la máquina virtual tenga [conectividad de red](backup-azure-sql-database.md#establish-network-connectivity).
4. Compruebe que las bases de datos de SQL Server se denominan de acuerdo con las [directrices de nomenclatura](backup-azure-sql-database.md#sql-database-naming-guidelines-for-azure-backup) de Azure Backup.
5. Compruebe que no dispone de otras soluciones de copia de seguridad habilitadas para la base de datos. Deshabilite todas las demás copias de seguridad de SQL Server antes de configurar este escenario. Puede habilitar Azure Backup para una máquina virtual de Azure junto con Azure Backup para una base de datos de SQL Server que se ejecute en la máquina virtual sin que se produzcan conflictos.


### <a name="establish-network-connectivity"></a>Establecimiento de conectividad de red

Todas las operaciones realizadas en la máquina virtual con SQL Server necesitan conectividad a las direcciones IP públicas de Azure. Si falta dicha conectividad, operaciones como detectar bases de datos, configurar y programar copias de seguridad o restaurar puntos de recuperación producirán errores. Establezca la conectividad con una de estas opciones:

- **Allow the Azure datacenter IP ranges** (Permitir los intervalos de direcciones IP del centro de datos de Azure): permite los [intervalos IP](https://www.microsoft.com/download/details.aspx?id=41653) en la descarga. Para el acceso en un grupo de seguridad de red (NSG), use el cmdlet **Set-AzureNetworkSecurityRule**.
- **Deploy an HTTP proxy server to route traffic** (Implementar un servidor proxy HTTP para enrutar el tráfico): cuando hace copia de seguridad de una base de datos de SQL Server en una máquina virtual de Azure, la extensión de copia de seguridad en la máquina virtual usa las API HTTPS para enviar comandos de administración a Azure Backup y datos a Azure Storage. La extensión de copia de seguridad también usa Azure Active Directory (Azure AD) para la autenticación. Enrute el tráfico de extensión de copia de seguridad de estos tres servicios a través del proxy HTTP. El de extensión es el único componente configurado para el acceso a la red pública de Internet.

Cada opción tiene sus ventajas y desventajas.

**Opción** | **Ventajas** | **Desventajas**
--- | --- | ---
Permitir intervalos IP | Sin costos adicionales. | Este escenario es complejo de administrar, ya que los intervalos de direcciones IP cambian con el tiempo. <br/><br/> Proporciona acceso a la totalidad de Azure, no solo a Azure Storage.
Usar un servidor proxy HTTP   | Se permite un control detallado en el proxy sobre las direcciones URL de almacenamiento. <br/><br/> Un único punto de acceso a Internet para las máquinas virtuales. <br/><br/> No están sujetas a cambios de direcciones IP de Azure. | Costos adicionales de ejecutar una máquina virtual con el software de proxy. 

### <a name="set-vm-permissions"></a>Establecer permisos de máquina virtual

Azure Backup lleva a cabo una serie de acciones al configurar la copia de seguridad para una base de datos de SQL Server:

- Agrega la extensión **AzureBackupWindowsWorkload**.
- Para detectar las bases de datos en la máquina virtual, Azure Backup crea la cuenta **NT SERVICE\AzureWLBackupPluginSvc**. Esta cuenta se usa para la copia de seguridad y restauración, y es necesario tener permisos de administrador del sistema.
- Azure Backup aprovecha la cuenta **NT AUTHORITY\SYSTEM** para la detección o consulta de bases de datos, por lo que esta cuenta debe tener un inicio de sesión público en SQL.

Si no se creó la VM con SQL Server en Azure Marketplace, podría recibir el error **UserErrorSQLNoSysadminMembership**. Si esto sucede, [siga estas instrucciones](#fix-sql-sysadmin-permissions).

### <a name="verify-database-naming-guidelines-for-azure-backup"></a>Compruebe las directrices de nomenclatura de las bases de datos de Azure Backup.

Evite lo siguiente en los nombres de bases de datos:

  * Espacios iniciales o finales
  * Signos '!' finales
  * Corchete de cierre ‘]’

Aunque se dispone de alias para los caracteres no admitidos de la tabla Azure, se recomienda evitarlos. [Más información](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?redirectedfrom=MSDN).

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>Detección de bases de datos SQL Server

Detecte las bases de datos que se ejecutan en la máquina virtual. 
1. En [Azure Portal](https://portal.azure.com), abra el almacén de Recovery Services que usa para la copia de seguridad de la base de datos.

2. En el panel del **almacén de Recovery Services**, seleccione **Copia de seguridad**. 

   ![Seleccionar Copia de seguridad para abrir el menú Objetivo de Backup](./media/backup-azure-sql-database/open-backup-menu.png)

3. En el menú **Backup Goal** (Objetivo de Backup), establezca **Where is your workload running** (¿Dónde se ejecuta su carga de trabajo?) en **Azure** (el valor predeterminado).

4. En **What do you want to backup** (¿De qué quiere realizar una copia de seguridad?), seleccione **SQL Server in Azure VM** (SQL Server en la máquina virtual de Azure).

    ![Seleccionar SQL Server en una máquina virtual de Azure para la copia de seguridad](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)


5. En **Backup Goal** > **Discover DBs in VMs** (Objetivo de Backup > Detectar bases de datos en máquinas virtuales), seleccione **Start Discovery** (Iniciar detección) para buscar las máquinas virtuales sin protección en la suscripción. Puede que la operación tarde un rato, según el número de máquinas virtuales sin protección en la suscripción.

    ![La copia de seguridad queda pendiente durante la búsqueda de bases de datos en máquinas virtuales](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. En la lista de máquinas virtuales, seleccione la que ejecuta la base de datos de SQL Server > **Discover DBs** (Detectar bases de datos).

7. Realice el seguimiento de la detección de las bases de datos en el área **Notificaciones**. El trabajo puede tardar un rato en completarse, según el número de bases de datos que haya en la máquina virtual. Cuando se detectan las bases de datos seleccionadas, aparece un mensaje de operación correcta.

    ![Mensaje de implementación correcta](./media/backup-azure-sql-database/notifications-db-discovered.png)

    - Tras la detección, las máquinas virtuales no protegidas deben aparecer en la lista por nombre y grupo de recursos.
    - Si una máquina virtual no aparece en la lista como cabría esperar, compruebe que no se haya copiado ya en un almacén.
    - Varias máquinas virtuales pueden tener el mismo nombre pero pertenecer a distintos grupos de recursos. 

9. Seleccione la máquina virtual que ejecuta la base de datos de SQL Server > **Discover DBs** (Detectar bases de datos). 
10. Azure Backup detecta todas las bases de datos de SQL Server en la máquina virtual. Durante la detección, tiene lugar lo siguiente en segundo plano:

    - Azure Backup registra la máquina virtual en el almacén para la copia de seguridad de la carga de trabajo. Todas las bases de datos de la máquina virtual registrada solo se pueden copiar en este almacén.
    - Azure Backup instala la extensión **AzureBackupWindowsWorkload** en la máquina virtual. No se instala ningún agente en la base de datos SQL.
    - Azure Backup crea la cuenta de servicio **NT Service\AzureWLBackupPluginSvc** en la máquina virtual.
      - Todas las operaciones de copia de seguridad y restauración utilizan la cuenta de servicio.
      - **NT Service\AzureWLBackupPluginSvc** necesita permisos de administrador del sistema de SQL. Todas las máquinas virtuales de SQL Server creadas en Azure Marketplace llevan instalado **SqlIaaSExtension**. La extensión **AzureBackupWindowsWorkload** usa **SQLIaaSExtension** para obtener automáticamente los permisos necesarios.
    - Si no ha creado la máquina virtual desde Marketplace, no tendrá instalado **SqlIaaSExtension** y la operación de detección no se podrá realizar; se muestra el mensaje de error **UserErrorSQLNoSysAdminMembership**. Siga las instrucciones que se indican en [#fix-sql-sysadmin-permissions] para corregir este problema.

        ![Seleccionar la máquina virtual y la base de datos](./media/backup-azure-sql-database/registration-errors.png)



## <a name="configure-a-backup-policy"></a>Configuración de una directiva de copia de seguridad 

Para optimizar las cargas de copia de seguridad, el número máximo de bases de datos en un trabajo de copia de seguridad en Azure Backup se establece en 50.

- Para proteger más de 50 bases de datos, configure varias copias de seguridad.
- Como alternativa, puede habilitar la protección automática. La protección automática protege las bases de datos existentes en una sola operación y protege automáticamente las nuevas bases de datos agregadas a la instancia del grupo de disponibilidad.


Configure la copia de seguridad de la manera siguiente:

1. En el panel del almacén, seleccione **Backup**. 

   ![Seleccionar Copia de seguridad para abrir el menú Objetivo de Backup](./media/backup-azure-sql-database/open-backup-menu.png)

3. En el menú **Backup Goal** (Objetivo de Backup), establezca **Where is your workload running** (¿Dónde se ejecuta su carga de trabajo?) en **Azure**.

4. En **What do you want to backup** (¿De qué quiere realizar una copia de seguridad?), seleccione **SQL Server in Azure VM** (SQL Server en la máquina virtual de Azure).

    ![Seleccionar SQL Server en una máquina virtual de Azure para la copia de seguridad](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    
5. En **Backup Goal** (Objetivo de Backup), seleccione **Configure Backup** (Configurar copia de seguridad).

    ![Seleccionar Configurar copia de seguridad](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    ![Mostrar todas las instancias de SQL Server con bases de datos independientes](./media/backup-azure-sql-database/list-of-sql-databases.png)

6. Seleccione todas las bases de datos que quiera proteger > **OK** (Aceptar).

    ![Protección de la base de datos](./media/backup-azure-sql-database/select-database-to-protect.png)

    - Se muestran todas las instancias de SQL Server (independientes y grupos de disponibilidad).
    - Seleccione la flecha abajo a la izquierda del nombre de instancia o grupo de disponibilidad para filtrar.

7. En el menú **Backup** (Copia de seguridad), seleccione **Backup policy** (Directiva de copia de seguridad). 

    ![Seleccionar directiva de copia de seguridad](./media/backup-azure-sql-database/select-backup-policy.png)

8. En **Choose backup policy** (Elegir directiva de copia de seguridad), seleccione una directiva y haga clic en **OK** (Aceptar).

    - Selección de la directiva predeterminada: **HourlyLogBackup**.
    - Elegir una directiva de copia de seguridad existente creada previamente para SQL.
    - [Definir una nueva directiva](backup-azure-sql-database.md#define-a-backup-policy) basada en el objetivo de punto de recuperación (RPO) y en la duración de retención.
    - Durante la versión preliminar, no se puede editar una directiva de copia de seguridad existente.
    
9. En **Backup menu** (Menú de copia de seguridad), seleccione **Enable backup** (Habilitar copia de seguridad).

    ![Habilitar la directiva de copia de seguridad elegida](./media/backup-azure-sql-database/enable-backup-button.png)

10. Realice el seguimiento del progreso de la configuración en el área de **notificaciones** del portal.

    ![Área de notificaciones](./media/backup-azure-sql-database/notifications-area.png)

### <a name="create-a-backup-policy"></a>Creación de una directiva de copia de seguridad

Una directiva de copia de seguridad define cuándo se realizan las copias de seguridad y cuánto tiempo se conservan. 

- Una directiva se crea en el nivel de almacén.
- Varios almacenes pueden usar la misma directiva de copia de seguridad, pero debe aplicar la directiva de copia de seguridad a cada almacén.
- Al crear una directiva de copia de seguridad, la copia de seguridad completa diaria es la predeterminada.
- Puede agregar una copia de seguridad diferencial, pero solo si configura las copias de seguridad completas para que se realicen semanalmente.
- [Más información](backup-architecture.md#sql-server-backup-types) sobre los diferentes tipos de directivas de copia de seguridad.


Para crear una directiva de copia de seguridad:

1. En el almacén, haga clic en **Backup policies** > **Add** (Directivas de copia de seguridad > Agregar).
2. En el menú **Agregar**, haga clic en **SQL Server in Azure VM** (SQL Server en la máquina virtual de Azure). De esta manera se define el tipo de directiva.

   ![Elección de un tipo de directiva para la nueva directiva de copia de seguridad](./media/backup-azure-sql-database/policy-type-details.png)

3. En **Nombre de la directiva**, escriba un nombre para la nueva directiva. 
4. En el menú **Full Backup policy** (Directiva de copia de seguridad completa), seleccione un valor para **Backup Frequency** (Frecuencia de copia de seguridad) entre **Daily** (Diaria) o **Weekly** (Semanal).

    - En **Diariamente**, seleccione la hora y zona horaria en la que comienza el trabajo de copia de seguridad.
    - Es necesario ejecutar una copia de seguridad completa; no se puede desactivar la opción **Full Backup** (Copia de seguridad completa).
    - Haga clic en **Full Backup** (Copia de seguridad completa) para ver la directiva. 
    - No puede crear copias de seguridad diferenciales para copias de seguridad completas diarias.
    - Para **Semanalmente**, seleccione el día de la semana, la hora y la zona horaria, en la que se inicia el trabajo de copia de seguridad.

    ![Nuevos campos de directiva de copia de seguridad](./media/backup-azure-sql-database/full-backup-policy.png)  

5. De forma predeterminada, todas las opciones de **Retention Range** (Duración de retención) están seleccionadas. Desactive los límites de duración de retención que no quiera usar y establezca los intervalos deseados. 

    - Los puntos de recuperación se etiquetan para la retención en función de su duración de retención. Por ejemplo, si selecciona la frecuencia diaria, solo se desencadena una copia de seguridad completa cada día.
    - La copia de seguridad de un día específico se etiqueta y se retiene según la duración de la retención semanal y la configuración de esta.
    - La duración de retención mensual y anual se comporta de forma similar.

   ![Configuración del intervalo de la duración de retención](./media/backup-azure-sql-database/retention-range-interval.png)

    

6. En el menú de la **directiva de copia de seguridad completa**, seleccione **Aceptar** para aceptar la configuración.
7. Para agregar una directiva de copia de seguridad diferencial, seleccione **Copia de seguridad diferencial**. 

   ![Configuración del intervalo de la duración de retención](./media/backup-azure-sql-database/retention-range-interval.png)
   ![Apertura del menú de directiva de copia de seguridad diferencial](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

8. En **Differential Backup policy** (Directiva de copia de seguridad diferencial), seleccione **Enable** (Habilitar) para abrir los controles de retención y frecuencia. 

    - A lo sumo, puede desencadenar una copia de seguridad diferencial al día.
    - Como máximo, las copias de seguridad diferenciales se pueden retener durante 180 días. Si necesita más tiempo de retención, debe usar copias de seguridad completas.

9. Seleccione **Aceptar** para guardar la directiva y volver al menú principal de la **directiva de copia de seguridad**.

10. Para agregar una directiva de copia de seguridad del registro de transacciones, seleccione **Copia de seguridad de registros**.
11. En el menú **Log Backup** (Copia de seguridad de registros), seleccione **Enable** (Habilitar) y, luego, establezca los controles de retención y frecuencia. Las copia de seguridad de registros pueden realizarse cada 15 minutos y se pueden retener durante un período máximo de 35 días.
12. Seleccione **Aceptar** para guardar la directiva y volver al menú principal de la **directiva de copia de seguridad**.

   ![Editar la directiva de copia de seguridad de registros](./media/backup-azure-sql-database/log-backup-policy-editor.png)

13. En el menú **Directiva de copia de seguridad**, elija si desea habilitar la **compresión de copia de seguridad de SQL**.
    - La compresión está deshabilitada de manera predeterminada.
    - En el back-end, Azure Backup usa la compresión de copia de seguridad nativa de SQL.

14. Después de completar las modificaciones en la directiva de copia de seguridad, seleccione **Aceptar**.



## <a name="enable-auto-protection"></a>Habilitación de la protección automática  

Habilite la protección automática para hacer una copia de seguridad automática de todas las bases de datos existentes, y de las bases de datos que se agreguen en el futuro, en una instancia de SQL Server independiente o en un grupo de disponibilidad de SQL Server AlwaysOn. 

- Al activar la protección automática y seleccionar una directiva, las bases de datos protegidas existentes seguirán usando la directiva anterior.
- No hay límite en el número de bases de datos que se pueden seleccionar para la protección automática en una sola operación.

Para habilitar la protección automática, siga estos pasos:

1. En **Items to backup** (Elementos para copia de seguridad), seleccione la instancia para la que quiere habilitar la protección automática.
2. Seleccione el menú desplegable que aparece debajo de **Autoprotect** (Protección automática) y establézcalo en **On** (Activado). A continuación, haga clic en **Aceptar**.

    ![Habilitar la protección automática en el grupo de disponibilidad Always On](./media/backup-azure-sql-database/enable-auto-protection.png)

3. La copia de seguridad se configura para todas las bases de datos juntas y se puede realizar un seguimiento de ella en **Backup Jobs** (Trabajos de copia de seguridad).


Si tiene que deshabilitar la protección automática, haga clic en el nombre de la instancia en **Configure Backup** (Configurar copia de seguridad) y seleccione **Disable Autoprotect** (Deshabilitar la protección automática). Se seguirá realizando una copia de seguridad de todas las bases de datos. Pero las futuras bases de datos no se protegerán automáticamente.

![Deshabilitar la protección automática en dicha instancia](./media/backup-azure-sql-database/disable-auto-protection.png)


## <a name="fix-sql-sysadmin-permissions"></a>Corrección de permisos de administrador del sistema de SQL

Si necesita corregir los permisos debido un error **UserErrorSQLNoSysadminMembership**, haga lo siguiente:

1. Use una cuenta con permisos sysadmin de SQL Server para iniciar sesión en SQL Server Management Studio (SSMS). A menos que necesite permisos especiales, debería funcionar la autenticación de Windows.
2. En el servidor SQL Server, abra la carpeta **Seguridad/Inicios de sesión**.

    ![Abrir carpeta Security/Logins para ver las cuentas](./media/backup-azure-sql-database/security-login-list.png)

3. Haga clic con el botón derecho en la carpeta **Logins** y seleccione **Nuevo inicio de sesión**. En **Inicio de sesión: Nuevo**, seleccione **Buscar**.

    ![En el cuadro de diálogo Inicio de sesión - Nuevo, seleccionar Buscar](./media/backup-azure-sql-database/new-login-search.png)

3. La cuenta de servicio virtual de Windows **NT SERVICE\AzureWLBackupPluginSvc** se creó durante las fases de registro de la máquina virtual y de detección de SQL. Escriba el nombre de la cuenta como se muestra en **Escriba el nombre del objeto que desea seleccionar**. Seleccione **Comprobar nombres** para resolver el nombre. Haga clic en **OK**.

    ![Seleccionar Comprobar nombres para resolver el nombre de servicio desconocido](./media/backup-azure-sql-database/check-name.png)


4. En **Roles de servidor**, asegúrese de que está seleccionado el rol **sysadmin**. Haga clic en **OK**. Ahora deben existir los permisos necesarios.

    ![Asegúrese de que el rol del servidor administrador del sistema está seleccionado](./media/backup-azure-sql-database/sysadmin-server-role.png)

5. Ahora, asocie la base de datos con el almacén de Recovery Services. En Azure Portal, en la lista **Servidores protegidos**, haga clic con el botón derecho en el servidor que tiene un estado de error > **Rediscover DBs** (Volver a detectar bases de datos).

    ![Comprobar que el servidor tiene los permisos adecuados](./media/backup-azure-sql-database/check-erroneous-server.png)

6. Compruebe el progreso en el área **Notificaciones**. Cuando se detectan las bases de datos seleccionadas, aparece un mensaje de operación correcta.

    ![Mensaje de implementación correcta](./media/backup-azure-sql-database/notifications-db-discovered.png)

Como alternativa, puede habilitar la [protección automática](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) en la instancia completa o el grupo de disponibilidad Always On si selecciona la opción **ON** (Activado) en la lista desplegable correspondiente de la columna **AUTOPROTECT** (Protección automática). La característica de [protección automática](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) no solo habilita la protección en todas las bases de datos existentes en una sola operación, sino que también protege automáticamente las nuevas bases de datos que se agregarán a esa instancia o al grupo de disponibilidad en el futuro.  

   ![Habilitar la protección automática en el grupo de disponibilidad Always On](./media/backup-azure-sql-database/enable-auto-protection.png)

## <a name="next-steps"></a>Pasos siguientes

- [Más información](restore-sql-database-azure-vm.md) sobre la restauración de copias de seguridad de bases de datos de SQL Server.
- [Más información](manage-monitor-sql-database-backup.md) sobre la administración de copias de seguridad de bases de datos de SQL Server.

