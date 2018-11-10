---
title: Copia de seguridad de bases de datos de SQL Server en Azure | Microsoft Docs
description: Este tutorial explica cómo realizar una copia de seguridad de SQL Server en Azure. En este tutorial también se explica cómo se realiza la recuperación de SQL Server.
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
keywords: ''
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2018
ms.author: markgal;anuragm
ms.custom: ''
ms.openlocfilehash: 72d48bd1716e1b62ae92f8317f3f9611ac463453
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50211509"
---
# <a name="back-up-sql-server-databases-to-azure"></a>Copia de seguridad de bases de datos de SQL Server en Azure

Las bases de datos SQL Server son cargas de trabajo críticas que requieren un bajo objetivo de punto de recuperación (RPO) y retención a largo plazo. Azure Backup ofrece una solución de copia de seguridad de SQL Server que requiere una infraestructura cero, lo que significa que no hay ningún servidor de copia de seguridad complejo, agente de administración o almacenamiento de copia de seguridad que haya que administrar. Azure Backup ofrece una administración centralizada para las copias de seguridad de todos los servidores que están ejecutando SQL Server o incluso cargas de trabajo distintas.

En este artículo, aprenderá lo siguiente:

> [!div class="checklist"]
> * Los requisitos previos para la copia de seguridad de una instancia de SQL Server en Azure.
> * Creación y uso de un almacén de Recovery Services.
> * Configuración de copias de seguridad de bases de datos de SQL Server.
> * Establecimiento de una directiva de copia de seguridad (o retención) para los puntos de recuperación.
> * Restauración de la base de datos.

Antes de comenzar los procedimientos de este artículo, debe disponer de una instancia de SQL Server en ejecución en Azure. [Use máquinas virtuales SQL de Marketplace para crear rápidamente una instancia de SQL Server](../sql-database/sql-database-get-started-portal.md).

## <a name="public-preview-limitations"></a>Limitaciones de la versión preliminar pública

Los elementos siguientes son las limitaciones conocidas de la versión preliminar pública:

- La máquina virtual SQL requiere conectividad a Internet para acceder a direcciones IP públicas de Azure. Para más información, consulte [Establecimiento de conectividad de red](backup-azure-sql-database.md#establish-network-connectivity).
- Proteja hasta 2000 bases de datos SQL en un almacén de Recovery Services. Las bases de datos SQL adicionales deben almacenarse en un almacén de Recovery Services independiente.
- [Las copias de seguridad de grupos de disponibilidad distribuidos tienen limitaciones](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017).
- No se admiten las instancias de clústeres de conmutación por error (FCI) de los grupos de disponibilidad Always On de SQL Server.
- Use Azure Portal para configurar Azure Backup para proteger las bases de datos SQL Server. Actualmente no se admite Azure PowerShell, ni la CLI de Azure ni las API REST.

Consulte la [sección de preguntas frecuentes](https://docs.microsoft.com/azure/backup/backup-azure-sql-database#faq) para más información sobre los escenarios admitidos y no admitidos.

## <a name="support-for-azure-geos"></a>Compatibilidad con las geoáreas de Azure

Azure Backup es compatible con las siguientes geoáreas:

- Sudeste de Australia (ASE) 
- Sur de Brasil (BRS)
- Centro de Canadá (CNC)
- Este de Canadá (CE)
- Centro de EE. UU. (CUS)
- Asia Oriental (EA)
- Australia Oriental (AE) 
- Este de EE. UU. (EUS)
- Este de EE. UU. 2 (EUS2)
- India central (INC) 
- India del Sur (INS)
- Este de Japón (JPE)
- Oeste de Japón (JPE)
- Centro de Corea del Sur (KRC)
- Sur de Corea del Sur (KRS)
- Centro-norte de EE. UU. (NCUS) 
- Europa del Norte (NE) 
- Centro-sur de EE. UU. (SCUS) 
- Asia Suroriental (SEA)
- Sur de Reino Unido (UKS) 
- Oeste de Reino Unido (UKW) 
- Centro-oeste de EE. UU. (WCUS)
- Europa Occidental (WE) 
- Oeste de EE. UU. (WUS)
- Oeste de EE. UU. 2 (WUS 2) 

## <a name="support-for-operating-systems-and-sql-server-versions"></a>Compatibilidad con sistemas operativos y versiones de SQL Server

Esta sección describe la compatibilidad de Azure Backup con diversos sistemas operativos y versiones de SQL Server. Se admiten las máquinas virtuales de Azure SQL de Marketplace y otras que no aparecen en Marketplace (en los casos en los que SQL Server se instala manualmente).

### <a name="supported-operating-systems"></a>Sistemas operativos compatibles

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

Linux no se admite actualmente.

### <a name="supported-sql-server-versions-and-editions"></a>Versiones y ediciones admitidas de SQL Server

- SQL Server 2012 Enterprise, Standard, Web, Developer, Express
- SQL Server 2014 Enterprise, Standard, Web, Developer, Express
- SQL Server 2016 Enterprise, Standard, Web, Developer, Express
- SQL Server 2017 Enterprise, Standard, Web, Developer, Express

## <a name="prerequisites"></a>Requisitos previos

Para poder realizar copias de seguridad de la base de datos de SQL Server, primero debe comprobar si reúne las siguientes condiciones:

- Identifique o [cree un almacén de Recovery Services](backup-azure-sql-database.md#create-a-recovery-services-vault) en la misma región o configuración regional que la máquina virtual que hospeda la instancia de SQL Server.
- [Compruebe los permisos en la máquina virtual](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms) necesarios para realizar la copia de seguridad de las bases de datos SQL.
- Compruebe que [la máquina virtual SQL tiene conectividad de red](backup-azure-sql-database.md#establish-network-connectivity).

> [!NOTE]
> Puede tener solo una solución de copia de seguridad a la vez para realizar copias de seguridad de las bases de datos de SQL Server. Deshabilite las demás copias de seguridad de SQL antes de usar esta característica; en caso contrario, las copias de seguridad interferirían y se produciría un error. Puede habilitar Azure Backup para máquinas virtuales IaaS con la copia de seguridad de SQL sin que se produzca ningún conflicto.
>

Si estas condiciones existen en su entorno, continúe en [Configuración de la copia de seguridad de bases de datos de SQL Server](backup-azure-sql-database.md#configure-backup-for-sql-server-databases). Si no cumple alguno de los requisitos previos, continúe leyendo.


## <a name="establish-network-connectivity"></a>Establecimiento de conectividad de red

Para todas las operaciones, la máquina virtual SQL necesita conectividad a las direcciones IP públicas de Azure. Se producirá un error en las operaciones con máquinas virtuales SQL (como detección de base de datos, configuración de copias de seguridad, copias de seguridad programadas, restauración de puntos de recuperación, etc.) si no existe conectividad con las direcciones IP públicas. Use alguna de las siguientes opciones para ofrecer una ruta de acceso clara para el tráfico de copia de seguridad:

- Incluya los intervalos de IP del centro de datos de Azure en una lista de permitidos: para incluir en una lista de permitidos los intervalos de IP del centro de datos de Azure, consulte la [página del centro de descargas para obtener información detallada sobre los intervalos de IP junto con instrucciones](https://www.microsoft.com/download/details.aspx?id=41653). 
- Implemente un servidor proxy HTTP para enrutar el tráfico: cuando haga copias de seguridad de una base de datos SQL en una máquina virtual, la extensión de copia de seguridad de la máquina virtual utiliza las API HTTPS para enviar comandos de administración a Azure Backup y datos a Azure Storage. La extensión de copia de seguridad también usa Azure Active Directory (Azure AD) para la autenticación. Enrute el tráfico de extensión de copia de seguridad de estos tres servicios a través del proxy HTTP. El de extensión es el único componente configurado para el acceso a la red pública de Internet.

Los inconvenientes entre las opciones son: capacidad de administración, control granular y costo.

> [!NOTE]
> Las etiquetas de servicio para Azure Backup deben estar disponibles con la disponibilidad general.
>

| Opción | Ventajas | Desventajas |
| ------ | ---------- | ------------- |
| Creación de una lista blanca con intervalos IP | Sin costos adicionales. <br/> Para abrir el acceso a un grupo de seguridad de red, use el cmdlet **Set-AzureNetworkSecurityRule**. | Es complejo de administrar, ya que los intervalos de IP afectados cambian con el tiempo. <br/>Proporciona acceso a la totalidad de Azure, no solo a Azure Storage.|
| Usar un servidor proxy HTTP   | Se permite un control detallado en el proxy sobre las direcciones URL de almacenamiento. <br/>Un único punto de acceso a Internet para las máquinas virtuales. <br/> No están sujetas a cambios de direcciones IP de Azure. | Costos adicionales de ejecutar una máquina virtual con el software de proxy. |

## <a name="set-permissions-for-non-marketplace-sql-vms"></a>Definición de permisos para máquinas virtuales SQL no incluidas en el catálogo de soluciones

Para hacer una copia de seguridad de una máquina virtual, Azure Backup requiere la instalación de la extensión **AzureBackupWindowsWorkload**. Si usa máquinas virtuales de Azure Marketplace, siga en [Detección de bases de datos SQL Server](backup-azure-sql-database.md#discover-sql-server-databases). Si no se crea la máquina virtual que hospeda las bases de datos SQL desde Azure Marketplace, complete el siguiente procedimiento para instalar la extensión y establecer los permisos apropiados. Además de la extensión **AzureBackupWindowsWorkload**, Azure Backup requiere privilegios de administrador del sistema de SQL para proteger las bases de datos SQL. Para detectar las bases de datos en la máquina virtual, Azure Backup crea la cuenta **NT Service\AzureWLBackupPluginSvc**. Para que Azure Backup detecte las bases de datos SQL, la cuenta **NT Service\AzureWLBackupPluginSvc** debe disponer de permisos de administrador del sistema de SQL y en SQL. En el procedimiento siguiente se explica cómo proporcionar estos permisos.

Para configurar permisos:

1. En [Azure Portal](https://portal.azure.com), abra el almacén de Recovery Services que usa para proteger las bases de datos SQL.

2. En el panel del **almacén de Recovery Services**, seleccione **Copia de seguridad**. Se abre el menú **Objetivo de Backup**.

   ![Seleccionar Copia de seguridad para abrir el menú Objetivo de Backup](./media/backup-azure-sql-database/open-backup-menu.png)

3. En el menú **Objetivo de Backup**, en **¿Dónde se ejecuta su carga de trabajo?**, deje **Azure** como opción predeterminada.

4. En el menú **¿De qué quiere hacer una copia de seguridad?**, expanda el cuadro de lista desplegable y seleccione **SQL Server en una máquina virtual de Azure**.

    ![Seleccionar SQL Server en una máquina virtual de Azure para la copia de seguridad](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    En el menú **Objetivo de Backup** se muestran dos pasos: **Detección de bases de datos en máquinas virtuales** y **Configuración de copia de seguridad**. El paso **Detección de bases de datos en máquinas virtuales** inicia una búsqueda de máquinas virtuales de Azure.

    ![Revisar los dos pasos de Objetivo de Backup](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

5. En **Detección de bases de datos en máquinas virtuales**, seleccione **Iniciar detección** para buscar las máquinas virtuales sin protección en la suscripción. Puede que tarde un tiempo en buscar todas las máquinas virtuales. El tiempo de búsqueda depende del número de máquinas virtuales sin protección en la suscripción.

    ![La copia de seguridad queda pendiente durante la búsqueda de bases de datos en máquinas virtuales](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Una vez que se detecta una máquina virtual sin protección, aparece en la lista. Las máquinas virtuales no protegidas se enumeran por su grupo de recursos y el nombre de máquina virtual. Es posible que varias máquinas virtuales tengan el mismo nombre. Sin embargo, las máquinas virtuales con el mismo nombre pertenecen a distintos grupos de recursos. Si una máquina virtual esperada no aparece en la lista, vea si la máquina virtual ya está protegida en un almacén.

6. En la lista de máquinas virtuales, seleccione la máquina virtual que contiene la base de datos SQL de la que desea realizar la copia de seguridad y, a continuación, seleccione **Detectar bases de datos**. 

    El proceso de detección instala la extensión **AzureBackupWindowsWorkload** en la máquina virtual. La extensión permite al servicio Azure Backup comunicarse con la máquina virtual, para que pueda realizar copias de seguridad de las bases de datos SQL. Una vez instalada la extensión, Azure Backup crea la cuenta de servicio virtual de Windows, **NT Service\AzureWLBackupPluginSvc**, en la máquina virtual. La cuenta de servicio virtual requiere permisos de administrador del sistema de SQL. Durante el proceso de instalación de la cuenta de servicio virtual, si recibe el error `UserErrorSQLNoSysadminMembership`, consulte [Corrección de permisos de administrador del sistema de SQL](backup-azure-sql-database.md#fix-sql-sysadmin-permissions).

    El área de **notificaciones** muestra el progreso de la detección de bases de datos. El trabajo puede tardar unos instantes en completarse. El tiempo de trabajo depende de cuántas bases de datos están en la máquina virtual. Cuando se detectan las bases de datos seleccionadas, aparece un mensaje de operación correcta.

    ![Mensaje de implementación correcta](./media/backup-azure-sql-database/notifications-db-discovered.png)

Una vez asociada la base de datos con el almacén de Recovery Services, el paso siguiente consiste en [configurar el trabajo de copia de seguridad](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

### <a name="fix-sql-sysadmin-permissions"></a>Corrección de permisos de administrador del sistema de SQL

Durante el proceso de instalación, si ve el error `UserErrorSQLNoSysadminMembership`, use una cuenta que tenga permisos de administrador del sistema de SQL Server para iniciar sesión en SQL Server Management Studio (SSMS). A menos que necesite permisos especiales, debería funcionar la autenticación de Windows.

1. En el servidor SQL Server, abra la carpeta Security/Logins.

    ![Abrir carpeta Security/Logins para ver las cuentas](./media/backup-azure-sql-database/security-login-list.png)

2. Haga clic con el botón derecho en la carpeta Logins y seleccione **Nuevo inicio de sesión**. En el cuadro de diálogo **Inicio de sesión - Nuevo**, seleccione **Buscar**.

    ![En el cuadro de diálogo Inicio de sesión - Nuevo, seleccionar Buscar](./media/backup-azure-sql-database/new-login-search.png)

3. La cuenta de servicio virtual de Windows **NT Service\AzureWLBackupPluginSvc** se creó durante las fases de registro de la máquina virtual y de detección de SQL. Escriba el nombre de la cuenta como se muestra en el cuadro **Escriba el nombre del objeto que desea seleccionar**. Seleccione **Comprobar nombres** para resolver el nombre. 

    ![Seleccionar Comprobar nombres para resolver el nombre de servicio desconocido](./media/backup-azure-sql-database/check-name.png)

4. Seleccione **Aceptar** para cerrar el cuadro de diálogo.

5. En el cuadro **Roles de servidor**, asegúrese de que el rol **administrador del sistema** está seleccionado. Seleccione **Aceptar** para cerrar el cuadro de diálogo.

    ![Asegúrese de que el rol del servidor administrador del sistema está seleccionado](./media/backup-azure-sql-database/sysadmin-server-role.png)

    Ahora deben existir los permisos necesarios.

6. Aunque se haya corregido el error de permisos, deberá asociar la base de datos con el almacén de Recovery Services. En la lista **Servidores protegidos** de Azure Portal, haga clic con el botón derecho en el servidor que presenta un estado de error y seleccione **Volver a detectar bases de datos**.

    ![Comprobar que el servidor tiene los permisos adecuados](./media/backup-azure-sql-database/check-erroneous-server.png)

    El área de **notificaciones** muestra el progreso de la detección de bases de datos. El trabajo puede tardar unos instantes en completarse. El tiempo de trabajo depende de cuántas bases de datos están en la máquina virtual. Cuando se detectan las bases de datos seleccionadas, aparece un mensaje de operación correcta.

    ![Mensaje de implementación correcta](./media/backup-azure-sql-database/notifications-db-discovered.png)

Una vez asociada la base de datos con el almacén de Recovery Services, el paso siguiente consiste en [configurar el trabajo de copia de seguridad](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>Detección de bases de datos SQL Server

Azure Backup detecta todas las bases de datos de una instancia de SQL Server. Puede proteger las bases de datos según sus requisitos de copia de seguridad. Utilice el procedimiento siguiente para identificar la máquina virtual que hospeda las bases de datos SQL. Una vez identificada la máquina virtual, Azure Backup instala una extensión ligera para detectar las bases de datos de SQL Server.

1. Inicie sesión en su suscripción en [Azure Portal](https://portal.azure.com/).

2. En el menú izquierdo, seleccione **Todos los servicios**.

    ![Seleccionar Todos los servicios](./media/backup-azure-sql-database/click-all-services.png) <br/>

3. En el cuadro de diálogo **Todos los servicios**, escriba **Recovery Services**. A medida que escribe, la entrada filtra la lista de recursos. Seleccione **Almacenes de Recovery Services** en la lista.

    ![Escribir y elegir Almacenes de Recovery Services](./media/backup-azure-sql-database/all-services.png) <br/>

    Aparece la lista de almacenes de Recovery Services de la suscripción. 

4. En la lista de almacenes de Recovery Services, seleccione el almacén que desea usar para proteger las bases de datos SQL.

5. En el panel del **almacén de Recovery Services**, seleccione **Copia de seguridad**. Se abre el menú **Objetivo de Backup**.

   ![Seleccionar Copia de seguridad para abrir el menú Objetivo de Backup](./media/backup-azure-sql-database/open-backup-menu.png)

6. En el menú **Objetivo de Backup**, en **¿Dónde se ejecuta su carga de trabajo?**, deje **Azure** como opción predeterminada.

7. En el menú **¿De qué quiere hacer una copia de seguridad?**, expanda el cuadro de lista desplegable y seleccione **SQL Server en una máquina virtual de Azure**.

    ![Seleccionar SQL Server en una máquina virtual de Azure para la copia de seguridad](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    En el menú **Objetivo de Backup** se muestran dos pasos: **Detección de bases de datos en máquinas virtuales** y **Configuración de copia de seguridad**.
    
    ![Revisar los dos pasos de Objetivo de Backup](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

8. En **Detección de bases de datos en máquinas virtuales**, seleccione **Iniciar detección** para buscar las máquinas virtuales sin protección en la suscripción. Puede que tarde un tiempo en buscar todas las máquinas virtuales. El tiempo de búsqueda depende del número de máquinas virtuales sin protección en la suscripción.

    ![La copia de seguridad queda pendiente durante la búsqueda de bases de datos en máquinas virtuales](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Una vez que se detecta una máquina virtual sin protección, aparece en la lista. Varias máquinas virtuales pueden tener el mismo nombre. Sin embargo, las máquinas virtuales con el mismo nombre pertenecen a distintos grupos de recursos. Las máquinas virtuales no protegidas se enumeran por su grupo de recursos y el nombre de máquina virtual. Si una máquina virtual esperada no aparece en la lista, vea si la máquina virtual ya está protegida en un almacén.

9. En la lista de máquinas virtuales, seleccione la máquina virtual que contiene la base de datos SQL de la que desea realizar la copia de seguridad y, a continuación, seleccione **Detectar bases de datos**.

    Azure Backup detecta todas las bases de datos SQL de la máquina virtual. Para más información sobre lo que ocurre durante la fase de detección de bases de datos, consulte [Operaciones en segundo plano](backup-azure-sql-database.md#background-operations). Después de detectar las bases de datos SQL, ya está listo para [configurar el trabajo de copia de seguridad](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

### <a name="background-operations"></a>Operaciones en segundo plano

Cuando usa la herramienta **Detectar bases de datos**, Azure Backup ejecuta las siguientes operaciones en segundo plano:

- Registra la máquina virtual en el almacén de Recovery Services para la copia de seguridad de la carga de trabajo. La copia de seguridad de las máquinas virtuales registradas solo puede realizarse en este almacén de Recovery Services. 

- Instala la extensión **AzureBackupWindowsWorkload** en la máquina virtual. La copia de seguridad de una base de datos SQL es una solución sin agente. La extensión está instalada en la máquina virtual y no hay ningún agente instalado en la base de datos SQL.

- Crea la cuenta de servicio, **NT Service\AzureWLBackupPluginSvc**, en la máquina virtual. Todas las operaciones de copia de seguridad y restauración utilizan la cuenta de servicio. **NT Service\AzureWLBackupPluginSvc** necesita permisos de administrador del sistema de SQL. Todas las máquinas virtuales SQL de Marketplace tienen la extensión **SqlIaaSExtension** instalada. La extensión **AzureBackupWindowsWorkload** usa **SQLIaaSExtension** para obtener automáticamente los permisos necesarios. Si la máquina virtual no tiene la extensión **SqlIaaSExtension** instalada, se produce un error en la operación de detección de base de datos y aparece el mensaje de error `UserErrorSQLNoSysAdminMembership`. Para agregar el permiso de administrador del sistema para poder realizar copias de seguridad, siga las instrucciones de [configuración de los permisos de Azure Backup para máquinas virtuales SQL no incluidas en Marketplace](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms).

    ![Seleccionar la máquina virtual y la base de datos](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup-for-sql-server-databases"></a>Configuración de copia de seguridad para bases de datos de SQL Server

Azure Backup ofrece servicios de administración para proteger las bases de datos SQL Server y administrar trabajos de copia de seguridad. Las funciones de administración y supervisión dependen del almacén de Recovery Services. 

> [!NOTE]
> Puede tener solo una solución de copia de seguridad a la vez para realizar copias de seguridad de las bases de datos de SQL Server. Deshabilite las demás copias de seguridad de SQL antes de usar esta característica; en caso contrario, las copias de seguridad interferirían y se produciría un error. Puede habilitar Azure Backup para máquinas virtuales IaaS con la copia de seguridad de SQL sin que se produzca ningún conflicto.
>

Para configurar la protección de una base de datos SQL:

1. Abra el almacén de Recovery Services registrado con la máquina virtual SQL.

2. En el panel del **almacén de Recovery Services**, seleccione **Copia de seguridad**. Se abre el menú **Objetivo de Backup**.

   ![Seleccionar Copia de seguridad para abrir el menú Objetivo de Backup](./media/backup-azure-sql-database/open-backup-menu.png)

3. En el menú **Objetivo de Backup**, en **¿Dónde se ejecuta su carga de trabajo?**, deje **Azure** como opción predeterminada.

4. En el menú **¿De qué quiere hacer una copia de seguridad?**, expanda el cuadro de lista desplegable y seleccione **SQL Server en una máquina virtual de Azure**.

    ![Seleccionar SQL Server en una máquina virtual de Azure para la copia de seguridad](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    En el menú **Objetivo de Backup** se muestran dos pasos: **Detección de bases de datos en máquinas virtuales** y **Configuración de copia de seguridad**.
    
    Si ha completado los pasos de este artículo de forma ordenada, ya habrá detectado las máquinas virtuales no protegidas, y este almacén estará registrado con una máquina virtual. Ahora está listo para configurar la protección de las bases de datos SQL.
    
5. En el menú **Objetivo de Backup**, seleccione **Configurar copia de seguridad**.

    ![Seleccionar Configurar copia de seguridad](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    El servicio Azure Backup muestra todas las instancias de SQL Server con bases de datos independientes, además de los grupos de disponibilidad Always On de SQL Server. Para ver las bases de datos independientes en la instancia de SQL Server, seleccione el botón de contenido adicional que se encuentra a la izquierda del nombre de la instancia. En las imágenes siguientes se muestran ejemplos de una instancia independiente y un grupo de disponibilidad AlwaysOn.

    > [!NOTE]
    > En el caso de un grupo de disponibilidad Always On de SQL Server, se respeta la preferencia de la copia de seguridad de SQL. Sin embargo, debido a la limitación de la plataforma SQL, las copias de seguridad completas y diferenciales se realizan desde el nodo principal. La copia de seguridad de registros se produce según sus preferencias de copia de seguridad. Debido a esta limitación, debe registrarse el nodo principal para los grupos de disponibilidad.
    >

    ![Lista de bases de datos en la instancia de SQL](./media/backup-azure-sql-database/discovered-databases.png)

    Seleccione el botón de contenido adicional situado a la izquierda del grupo de disponibilidad AlwaysOn para ver la lista de bases de datos.

    ![Lista de bases de datos del grupo de disponibilidad AlwaysOn](./media/backup-azure-sql-database/discovered-database-availability-group.png)

6. En la lista de bases de datos, seleccione todas aquellas que desee proteger y, a continuación, seleccione **Aceptar**.

    ![Seleccionar varias bases de datos para protegerlas](./media/backup-azure-sql-database/select-multiple-database-protection.png)

    Puede seleccionar hasta cincuenta bases de datos al mismo tiempo. Si desea proteger más de cincuenta bases de datos, realice varios pases. Después de proteger las cincuenta primeras bases de datos, repita este paso para proteger el siguiente conjunto de bases de datos.

    > [!Note] 
    > Para optimizar las cargas de copia de seguridad, Azure Backup divide grandes trabajos de copia de seguridad en varios lotes. El número máximo de bases de datos de un trabajo de copia de seguridad es cincuenta.
    >
    >

7. Para crear o elegir una directiva de copia de seguridad, en el menú **Copia de seguridad**, seleccione **Directiva de copia de seguridad**. Se abre el menú **Directiva de copia de seguridad**.

    ![Seleccionar directiva de copia de seguridad](./media/backup-azure-sql-database/select-backup-policy.png)

8. En el cuadro de lista desplegable **Elegir directiva de copia de seguridad**, elija una directiva de copia de seguridad y seleccione **Aceptar**. Para más información sobre la creación de una directiva de copia de seguridad, consulte [Definición de una directiva de copia de seguridad](backup-azure-sql-database.md#define-a-backup-policy).

   > [!NOTE]
   > Durante la versión preliminar, no puede editar las directivas de copia de seguridad. Si desea una directiva diferente de la disponible en la lista, debe crearla. Para obtener información sobre cómo crear una nueva directiva de copia de seguridad, consulte la sección [Definición de una directiva de copia de seguridad](backup-azure-sql-database.md#define-a-backup-policy).

    ![Elegir una directiva de copia de seguridad de la lista](./media/backup-azure-sql-database/select-backup-policy-steptwo.png)

    En el menú **Directiva de copia de seguridad** del cuadro de lista desplegable **Elegir directiva de copia de seguridad**, puede: 
    - Seleccionar la directiva predeterminada: **HourlyLogBackup**.
    - Elegir una directiva de copia de seguridad existente creada previamente para SQL.
    - [Definir una nueva directiva](backup-azure-sql-database.md#define-a-backup-policy) basada en el objetivo de punto de recuperación (RPO) y en la duración de retención. 

    > [!Note]
    > Azure Backup admite la retención a largo plazo que se basa en el esquema abuelo-padre-hijo de las copias de seguridad. El esquema optimiza el consumo del almacenamiento de back-end al tiempo que satisface las necesidades de cumplimiento.
    >

9. Después de elegir una directiva de copia de seguridad, en el menú **Copia de seguridad**, seleccione **Habilitar copia de seguridad**.

    ![Habilitar la directiva de copia de seguridad elegida](./media/backup-azure-sql-database/enable-backup-button.png)

    Realice el seguimiento del progreso de la configuración en el área de **notificaciones** del portal.

    ![Área de notificaciones](./media/backup-azure-sql-database/notifications-area.png)


### <a name="define-a-backup-policy"></a>Definición de una directiva de copia de seguridad

Una directiva de copia de seguridad define una matriz del momento en que se realizan las copias de seguridad y durante cuánto tiempo se retienen. Use Azure Backup para programar tres tipos de copia de seguridad de bases de datos SQL:

* Copia de seguridad completa: una copia de seguridad completa de la base de datos realiza una copia de seguridad de toda la base de datos. Una copia de seguridad completa contiene todos los datos de una base de datos específica o de un conjunto de archivos o grupos de archivos, y suficientes registros para recuperar esos datos. A lo sumo, puede desencadenar una copia de seguridad completa al día. Puede elegir realizar una copia de seguridad completa en un intervalo diario o semanal. 
* Copia de seguridad diferencial: la copia de seguridad diferencial se basa en la copia de seguridad de datos completa anterior más reciente. Una copia de seguridad diferencial captura solo los datos que han cambiado desde la copia de seguridad completa. A lo sumo, puede desencadenar una copia de seguridad diferencial al día. No se puede configurar una copia de seguridad completa y una copia de seguridad diferencial en el mismo día.
* Copia de seguridad del registro de transacciones: una copia de seguridad de registros permite realizar la restauración a un momento dado con una precisión de un segundo. A lo sumo, puede configurar las copias de seguridad del registro de transacciones cada 15 minutos.

La directiva se crea al nivel de almacén de Recovery Services. Varios almacenes pueden usar la misma directiva de copia de seguridad, pero debe aplicar la directiva de copia de seguridad a cada almacén. Al crear una directiva de copia de seguridad, la copia de seguridad completa diaria es la predeterminada. Puede agregar una copia de seguridad diferencial, pero solo si configura las copias de seguridad completas para que se realicen semanalmente. En el siguiente procedimiento se explica cómo crear una directiva de copia de seguridad para una instancia de SQL Server en una máquina virtual de Azure. 

> [!NOTE]
> En la versión preliminar, no puede editar una directiva de copia de seguridad. En su lugar, debe crear una directiva con los detalles deseados.  
 
Para crear una directiva de copia de seguridad:

1. En el almacén de Recovery Services que protege la base de datos SQL, haga clic en **Directivas de copia de seguridad** y, a continuación, haga clic en **Agregar**. 

   ![Apertura del cuadro de diálogo para crear una directiva de copia de seguridad](./media/backup-azure-sql-database/new-policy-workflow.png)

   Aparece el menú **Agregar**.

2. En el menú **Agregar**, haga clic en **SQL Server en la máquina virtual de Azure**.

   ![Elección de un tipo de directiva para la nueva directiva de copia de seguridad](./media/backup-azure-sql-database/policy-type-details.png)

   Al seleccionar SQL Server en la máquina virtual de Azure, se define el tipo de directiva y se abre el menú de directiva de copia de seguridad. El menú **Directiva de copia de seguridad** muestra los campos necesarios para cualquier nueva política de copia de seguridad de SQL Server.

3. En **Nombre de la directiva**, escriba un nombre para la nueva directiva.

4. Es obligatoria una copia de seguridad completa; no se puede desactivar la opción **Copia de seguridad completa**. Haga clic en **Copia de seguridad completa** para ver la directiva y editarla. Incluso si no cambia la directiva de copia de seguridad, debería ver los detalles de la directiva.

    ![Nuevos campos de directiva de copia de seguridad](./media/backup-azure-sql-database/full-backup-policy.png)

    En el menú **Directiva de copia de seguridad completa**, en **Frecuencia de copia de seguridad**, elija **Diariamente** o **Semanalmente**. En **Diariamente**, seleccione la hora y zona horaria en la que comienza el trabajo de copia de seguridad. No puede crear copias de seguridad diferenciales para copias de seguridad completas diarias.

   ![Configuración de intervalo diario](./media/backup-azure-sql-database/daily-interval.png)

    Para **Semanalmente**, seleccione el día de la semana, la hora y la zona horaria, en la que se inicia el trabajo de copia de seguridad.

   ![Configuración de intervalo semanal](./media/backup-azure-sql-database/weekly-interval.png)

5. De forma predeterminada, están seleccionadas todas las opciones de **duración de retención**: diaria, semanal, mensual y anual. Anule la selección de los límites de duración de retención no deseados. Establezca los intervalos que desea usar. En el menú de la **directiva de copia de seguridad completa**, seleccione **Aceptar** para aceptar la configuración.

   ![Configuración del intervalo de la duración de retención](./media/backup-azure-sql-database/retention-range-interval.png)

    Los puntos de recuperación se etiquetan para la retención en función de su duración de retención. Por ejemplo, si selecciona la frecuencia diaria, solo se desencadena una copia de seguridad completa cada día. La copia de seguridad de un día específico se etiqueta y se retiene según la duración de la retención semanal y la configuración de esta. La duración de retención mensual y anual se comporta de forma similar.

6. Para agregar una directiva de copia de seguridad diferencial, seleccione **Copia de seguridad diferencial**. Se abre el menú **Directiva de copia de seguridad diferencial**. 

   ![Abrir el menú Directiva de copia de seguridad diferencial](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

    En el menú de **directiva de copia de seguridad diferencial**, seleccione **Habilitar** para abrir los controles de frecuencia y retención. A lo sumo, puede desencadenar una copia de seguridad diferencial al día.
    
    > [!Important] 
    > Como máximo, las copias de seguridad diferenciales se pueden retener durante 180 días. Si necesita más tiempo de retención, debe usar copias de seguridad completas.
    >

   ![Editar la directiva de copia de seguridad diferencial](./media/backup-azure-sql-database/enable-differential-backup-policy.png)

    Seleccione **Aceptar** para guardar la directiva y volver al menú principal de la **directiva de copia de seguridad**.

7. Para agregar una directiva de copia de seguridad del registro de transacciones, seleccione **Copia de seguridad de registros**. Se abre el menú de **Copia de seguridad de registros**.

    En el menú **Copia de seguridad de registros**, seleccione **Habilitar** y establezca los controles de frecuencia y retención. Las copia de seguridad de registros pueden realizarse cada 15 minutos y se pueden retener durante un período máximo de 35 días. Seleccione **Aceptar** para guardar la directiva y volver al menú principal de la **directiva de copia de seguridad**.

   ![Editar la directiva de copia de seguridad de registros](./media/backup-azure-sql-database/log-backup-policy-editor.png)

8. En el menú **Directiva de copia de seguridad**, elija si desea habilitar la **compresión de copia de seguridad de SQL**. La compresión está deshabilitada de manera predeterminada.

    En el back-end, Azure Backup usa la compresión de copia de seguridad nativa de SQL.

9. Después de completar las modificaciones en la directiva de copia de seguridad, seleccione **Aceptar**. 

   ![Aceptar la nueva directiva de copia de seguridad](./media/backup-azure-sql-database/backup-policy-click-ok.png)

## <a name="restore-a-sql-database"></a>Restauración de una base de datos SQL
Azure Backup proporciona funcionalidad para restaurar las bases de datos individuales a una fecha u hora específicas, (con una precisión de un segundo), con las copias de seguridad del registro de transacciones. En función de los tiempos de restauración proporcionados, Azure Backup determina automáticamente si la copia de seguridad será completa o diferencial y también la cadena de copia de seguridad de registros necesaria para restaurar los datos.

También puede seleccionar una copia de seguridad completa o diferencial específica para restaurar a un punto de recuperación específico en lugar de a un momento concreto.

### <a name="pre-requisite-before-triggering-a-restore"></a>Requisito previo antes de desencadenar una restauración

1. Puede restaurar la base de datos en una instancia de SQL Server en la misma región de Azure. El servidor de destino debe estar registrado como origen en el almacén de Recovery Services.  
2. Para restaurar una base de datos cifrada TDE en otra de SQL Server, primero siga los pasos documentados [aquí](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017) para restaurar el certificado en el servidor de destino.
3. Antes de desencadenar la restauración de la base de datos "maestra", inicie la instancia de SQL Server en modo de usuario único con la opción de inicio `-m AzureWorkloadBackup`. El argumento para la opción `-m` es el nombre del cliente. Solo este cliente tiene permiso para abrir la conexión. Para todas las bases de datos del sistema (modelo, master, msdb), detenga el servicio Agente SQL antes de desencadenar la restauración. Cierre las aplicaciones que pueden intentar robar una conexión a cualquiera de estas bases de datos.

### <a name="steps-to-restore-a-database"></a>Pasos para restaurar una base de datos:

1. Abra el almacén de Recovery Services registrado con la máquina virtual SQL.

2. En el panel **almacén de Recovery Services**, en **Uso**, seleccione **Elementos de copia de seguridad** para abrir el menú del mismo nombre.

    ![Abrir el menú Elementos de copia de seguridad](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. En el menú **Elementos de copia de seguridad**, en **Tipo de administración de copia de seguridad**, seleccione **SQL en máquina virtual de Azure**. 

    ![Seleccionar SQL en la máquina virtual de Azure](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    El menú **Elementos de copia de seguridad** muestra la lista de base de datos SQL. 

4. En la lista de bases de datos SQL, seleccione la base de datos que desea restaurar.

    ![Selección de la base de datos que se va a restaurar](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Al seleccionar la base de datos, se abre el menú. Este menú proporciona los detalles de copia de seguridad de la base de datos, incluidos:

    * Los puntos de restauración más antiguos y más recientes.
    * El estado de copia de seguridad de registros de las 24 últimas horas (para bases de datos con el modelo de recuperación optimizado para cargas masivas de registros y el modelo de recuperación optimizado para cargas completas de registros, si está definida la configuración para copia de seguridad del registro de transacciones).

5. En el menú de la base de datos seleccionada, elija **Restaurar BD**. Se abre el menú **Restaurar**.

    ![Seleccionar Restaurar BD](./media/backup-azure-sql-database/restore-db-button.png)

    Cuando se abra el menú **Restaurar**, también se abrirá el menú **Restaurar configuración**. El menú **Restaurar configuración** es el primer paso para configurar la restauración. Use este menú para seleccionar dónde desea restaurar los datos. Las opciones son:
    - **Ubicación alternativa**: restaura la base de datos en una ubicación alternativa y conserva la base de datos de origen original.
    - **Sobrescribir la base de datos**: restaura los datos en la misma instancia de SQL Server que el origen. El efecto de esta opción es que se sobrescribe la base de datos original.

    > [!Important]
    > Si la base de datos seleccionada pertenece a un grupo de disponibilidad AlwaysOn, SQL no permite sobrescribir la base de datos. En este caso, solo está habilitada la opción **Ubicación alternativa**.
    >

    ![Menú Restaurar configuración](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Restauración a una ubicación alternativa

Este procedimiento le guía en la restauración de datos a una ubicación alternativa. Para sobrescribir la base de datos durante la restauración, continúe en [Restauración y sobrescritura de la base de datos](backup-azure-sql-database.md#restore-and-overwrite-the-database). En esta fase, el almacén de Recovery Services está abierto y el menú **Restaurar configuración** está visible. Si no está en esta fase, empiece por [restaurar una base de datos SQL](backup-azure-sql-database.md#restore-a-sql-database).

> [!NOTE]
> Puede restaurar la base de datos en una instancia de SQL Server en la misma región de Azure. El servidor de destino debe estar registrado en el almacén de Recovery Services. 
>

En el menú **Restaurar configuración**, el cuadro de lista desplegable **Servidor** muestra solo las instancias de SQL Server que están registradas en el almacén de Recovery Services. Si el servidor que desea no está en la lista, consulte [Detección de bases de datos SQL Server](backup-azure-sql-database.md#discover-sql-server-databases) para encontrar el servidor. Durante el proceso de detección, los servidores nuevos se registran en el almacén de Recovery Services.

1. En el menú **Restaurar configuración**:

    * En **Where to Restore** (Ubicación de restauración), seleccione **Ubicación alternativa**.
    * Abra el cuadro de lista desplegable **Servidor** y elija la instancia de SQL Server para restaurar la base de datos.
    * Abra el cuadro de lista desplegable **Instancia** y elija una instancia de SQL Server.
    * En el cuadro de diálogo **Nombre de la base de datos restaurada**, escriba el nombre de la base de datos de destino.
    * Si procede, seleccione **Overwrite if the DB with the same name already exists on selected SQL instance** (Sobrescribir si ya existe una base de datos con el mismo nombre en la instancia de SQL seleccionada).
    * Seleccione **Aceptar** para completar la configuración del destino y continúe para elegir un punto de restauración.

    ![Proporcionar valores para el menú Restaurar configuración](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. En el menú **Seleccionar punto de restauración**, puede elegir **Registros (punto en el tiempo)** o **Completo y diferencial** como punto de restauración. Para restaurar a un registro específico en un momento dado, continúe con este paso. Para restaurar un punto de restauración completo y diferencial, vaya al paso 3.

    ![Menú Seleccionar punto de restauración](./media/backup-azure-sql-database/recovery-point-menu.png)

    La restauración a un momento dado solo está disponible para copias de seguridad de registros de bases de datos con un modelo de recuperación optimizado para cargas masivas de registros y un modelo de recuperación optimizado para cargas completas de registros. Para restaurar a un momento dado:

    1. Seleccione **Registros (punto en el tiempo)** como el tipo de restauración.

        ![Elegir el tipo de punto de restauración](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. En **Fecha y hora de restauración**, seleccione el icono de minicalendario para abrir el **calendario**. En **Calendario**, las fechas en negrita contienen puntos de recuperación y la fecha actual está resaltada. Seleccione una fecha con puntos de recuperación. No se pueden seleccionar fechas sin puntos de recuperación.

        ![Abrir el calendario](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Una vez seleccionada una fecha, el gráfico de escala de tiempo muestra los puntos de recuperación disponibles en un rango continuo.

    3. Use el gráfico de escala de tiempo o el cuadro de diálogo **Hora** para especificar una hora específica para el punto de recuperación. Seleccione **Aceptar** para completar el paso del punto de restauración.
    
       ![Abrir el calendario](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        El menú **Seleccionar punto de restauración** se cierra y el menú **Configuración avanzada** se abre.

       ![Menú de configuración avanzada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. En el menú **Configuración avanzada**:

        * Para mantener la base de datos no operativa después de la restauración, establezca **Restaurar con NORECOVERY** en **Habilitado**.
        * Para cambiar la ubicación de restauración en el servidor de destino, escriba una nueva ruta de acceso en la columna **Destino**.
        * Seleccione **Aceptar** para aprobar la configuración. Cierre el menú **Configuración avanzada**.

    5. En el menú **Restaurar**, seleccione **Restaurar** para iniciar el trabajo de restauración. Realice un seguimiento del progreso de la restauración en el área de **notificaciones** o seleccione **Trabajos de restauración** en el menú de la base de datos.

       ![Progreso del trabajo de restauración](./media/backup-azure-sql-database/restore-job-notification.png)

3. En el menú **Seleccionar punto de restauración**, puede elegir **Registros (punto en el tiempo)** o **Completo y diferencial** como punto de restauración. Para restaurar un registro a un momento dado, vuelva al paso 2. Este paso permite restaurar un punto de restauración completo o diferencial específico. Puede ver todos los puntos de recuperación completos y diferenciales de los 30 últimos días. Para ver puntos de recuperación anteriores a 30 días, seleccione **Filtrar** para abrir el menú **Filtrar puntos de restauración**. En el caso de un punto de recuperación diferencial, Azure Backup restaura primero el punto de recuperación completa adecuado y, a continuación, aplica el punto de recuperación diferencial seleccionado.

    ![Menú Seleccionar punto de restauración](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. En el menú **Seleccionar punto de restauración**, seleccione **Completo y diferencial**.

       ![Seleccionar Completo y diferencial](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        El menú muestra la lista de puntos de recuperación disponibles.

    2. Seleccione un punto de recuperación de la lista y seleccione **Aceptar** para completar el procedimiento de punto de restauración. 

        ![Elegir un punto de recuperación completo](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        El menú **Punto de restauración** se cierra y el menú **Configuración avanzada** se abre.

        ![Menú de configuración avanzada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. En el menú **Configuración avanzada**:

        * Para mantener la base de datos no operativa después de la restauración, establezca **Restaurar con NORECOVERY** en **Habilitado**. **Restaurar con NORECOVERY** está deshabilitado de forma predeterminada.
        * Para cambiar la ubicación de restauración en el servidor de destino, escriba una nueva ruta de acceso en la columna **Destino**.
        * Seleccione **Aceptar** para aprobar la configuración. Cierre el menú **Configuración avanzada**.

    4. En el menú **Restaurar**, seleccione **Restaurar** para iniciar el trabajo de restauración. Realice un seguimiento del progreso de la restauración en el área de **notificaciones** o seleccione **Trabajos de restauración** en el menú de la base de datos.

       ![Progreso del trabajo de restauración](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-and-overwrite-the-database"></a>Restauración y sobrescritura de la base de datos

Este procedimiento le guía en la restauración de datos y la sobrescritura de una base de datos. Para restaurar a una ubicación alternativa, vaya a [Restauración a una ubicación alternativa](backup-azure-sql-database.md#restore-to-an-alternate-location). En esta fase, el almacén de Recovery Services está abierto y el menú **Restaurar configuración** está visible (consulte la siguiente imagen). Si no está en esta fase, empiece por [restaurar una base de datos SQL](backup-azure-sql-database.md#restore-a-sql-database).

![Menú Restaurar configuración](./media/backup-azure-sql-database/restore-overwrite-db.png)

En el menú **Restaurar configuración**, el cuadro de lista desplegable **Servidor** muestra solo las instancias de SQL Server que están registradas en el almacén de Recovery Services. Si el servidor que desea no está en la lista, consulte [Detección de bases de datos SQL Server](backup-azure-sql-database.md#discover-sql-server-databases) para encontrar el servidor. Durante el proceso de detección, los servidores nuevos se registran en el almacén de Recovery Services.

1. En el menú **Restaurar configuración**, seleccione **Sobrescribir la base de datos** y, a continuación, seleccione **Aceptar** para completar la configuración del destino. 

   ![Seleccionar Sobrescribir la base de datos](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

    Las configuraciones para **Servidor**, **Instancia** y **Nombre de la base de datos restaurada** no son necesarias.

2. En el menú **Seleccionar punto de restauración**, puede elegir **Registros (punto en el tiempo)** o **Completo y diferencial** como punto de restauración. Para restaurar a un registro específico en un momento dado, continúe con este paso. Para restaurar un punto de restauración completo y diferencial, vaya al paso 3.

    ![Menú Seleccionar punto de restauración](./media/backup-azure-sql-database/recovery-point-menu.png)

    La restauración a un momento dado solo está disponible para copias de seguridad de registros de bases de datos con un modelo de recuperación optimizado para cargas masivas de registros y un modelo de recuperación optimizado para cargas completas de registros. Para restaurar a un segundo específico:

    1. Seleccione **Registro (punto en el tiempo)** como el punto de restauración.

        ![Elegir el tipo de punto de restauración](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. En **Fecha y hora de restauración**, seleccione el icono de minicalendario para abrir el **calendario**. En **Calendario**, las fechas en negrita contienen puntos de recuperación y la fecha actual está resaltada. Seleccione una fecha con puntos de recuperación. No se pueden seleccionar fechas sin puntos de recuperación.

        ![Abrir el calendario](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Una vez seleccionada la fecha, el gráfico de escala de tiempo muestra los puntos de recuperación disponibles.

    3. Use el gráfico de escala de tiempo o el cuadro de diálogo **Hora** para especificar una hora específica para el punto de recuperación. Seleccione **Aceptar** para completar el paso del punto de restauración.
    
       ![Abrir el calendario](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        El menú **Seleccionar punto de restauración** se cierra y el menú **Configuración avanzada** se abre.

       ![Menú de configuración avanzada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. En el menú **Configuración avanzada**:

        * Para mantener la base de datos no operativa después de la restauración, establezca **Restaurar con NORECOVERY** en **Habilitado**.
        * Para cambiar la ubicación de restauración en el servidor de destino, escriba una nueva ruta de acceso en la columna **Destino**.
        * Seleccione **Aceptar** para aprobar la configuración. Cierre el menú **Configuración avanzada**.

    5. En el menú **Restaurar**, seleccione **Restaurar** para iniciar el trabajo de restauración. Realice un seguimiento del progreso de la restauración en el área de **notificaciones** o seleccione **Trabajos de restauración** en el menú de la base de datos.

       ![Progreso del trabajo de restauración](./media/backup-azure-sql-database/restore-job-notification.png)

3. En el menú **Seleccionar punto de restauración**, puede elegir **Registros (punto en el tiempo)** o **Completo y diferencial** como punto de restauración. Para restaurar un registro a un momento dado, vuelva al paso 2. Este paso permite restaurar un punto de restauración completo o diferencial específico. Puede ver todos los puntos de recuperación completos y diferenciales de los 30 últimos días. Para ver puntos de recuperación anteriores a 30 días, seleccione **Filtrar** para abrir el menú **Filtrar puntos de restauración**. En el caso de un punto de recuperación diferencial, Azure Backup restaura primero el punto de recuperación completa adecuado y, a continuación, aplica el punto de recuperación diferencial seleccionado.

    ![Menú Seleccionar punto de restauración](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. En el menú **Seleccionar punto de restauración**, seleccione **Completo y diferencial**.

       ![Seleccionar Completo y diferencial](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        El menú muestra la lista de puntos de recuperación disponibles.

    2. Seleccione un punto de recuperación de la lista y seleccione **Aceptar** para completar el procedimiento de punto de restauración. 

        ![Elegir un punto de recuperación completo](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        El menú **Punto de restauración** se cierra y el menú **Configuración avanzada** se abre.

        ![Menú de configuración avanzada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. En el menú **Configuración avanzada**:

        * Para mantener la base de datos no operativa después de la restauración, establezca **Restaurar con NORECOVERY** en **Habilitado**. **Restaurar con NORECOVERY** está deshabilitado de forma predeterminada.
        * Para cambiar la ubicación de restauración en el servidor de destino, escriba una nueva ruta de acceso en la columna **Destino**.
        * Seleccione **Aceptar** para aprobar la configuración. Cierre el menú **Configuración avanzada**.

    4. En el menú **Restaurar**, seleccione **Restaurar** para iniciar el trabajo de restauración. Realice un seguimiento del progreso de la restauración en el área de **notificaciones** o seleccione **Trabajos de restauración** en el menú de la base de datos.

       ![Progreso del trabajo de restauración](./media/backup-azure-sql-database/restore-job-notification.png)

## <a name="manage-azure-backup-operations-for-sql-on-azure-vms"></a>Administración de operaciones de Azure Backup para SQL en máquinas virtuales de Azure

En esta sección se ofrece información sobre las diversas operaciones de administración de Azure Backup que están disponibles para SQL en máquinas virtuales de Azure. Existen las siguientes operaciones de alto nivel:

* Supervisión de trabajos
* Alertas de copias de seguridad
* Detención de la protección en una base de datos SQL
* Reanudación de la protección en una base de datos SQL
* Desencadenamiento de un trabajo de copia de seguridad ad hoc
* Anulación del registro de un servidor que ejecuta SQL Server

### <a name="monitor-backup-jobs"></a>Supervisión de trabajos de copia de seguridad
Azure Backup es una solución de clase empresarial que proporciona servicios avanzados de alertas de copia de seguridad y notificación de errores. (Consulte [Visualización de alertas de copia de seguridad](backup-azure-sql-database.md#view-backup-alerts)). Para supervisar trabajos específicos, use cualquiera de las siguientes opciones según sus requisitos.

#### <a name="use-the-azure-portal-for-adhoc-operations"></a>Uso de Azure Portal para las operaciones ad hoc
Además, Azure Backup muestra todos los trabajos desencadenados manualmente o ad hoc en el portal **Trabajos de copia de seguridad**. Los trabajos que están disponibles en el portal **Trabajos de copia de seguridad** incluyen:
- Todas las operaciones de configuración de la copia de seguridad.
- Operaciones de copia de seguridad desencadenadas manualmente.
- Operaciones de restauración.
- Registro y detección de operaciones de base de datos.
- Detención de operaciones de copia de seguridad. 

![Portal de trabajos de copia de seguridad](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> No todos los trabajos de copia de seguridad, incluidos los de copia de seguridad completa, diferencial y de copia de seguridad de registros se muestran en el portal **Trabajos de copia de seguridad**. Use SQL Server Management Studio para supervisar los trabajos de copia de seguridad programados, como se describe en la sección siguiente.
>

#### <a name="use-sql-server-management-studio-for-backup-jobs"></a>Uso de SQL Server Management Studio para los trabajos de copia de seguridad
Azure Backup utiliza API nativas de SQL para todas las operaciones de copia de seguridad. Use las API nativas para capturar toda la información sobre trabajos de la [tabla del conjunto de copias de seguridad de SQL](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) en la base de datos msdb.

El ejemplo siguiente es una consulta para capturar todos los trabajos de copia de seguridad de una base de datos denominada **DB1**. Personalice la consulta para realizar una supervisión avanzada.

```
select CAST (
Case type
                when 'D' 
                                 then 'Full'
                when  'I'
                               then 'Differential' 
                ELSE 'Log'
                END         
                AS varchar ) AS 'BackupType',
database_name, 
server_name,
machine_name,
backup_start_date,
backup_finish_date,
DATEDIFF(SECOND, backup_start_date, backup_finish_date) AS TimeTakenByBackupInSeconds,
backup_size AS BackupSizeInBytes
  from msdb.dbo.backupset where user_name = 'NT SERVICE\AzureWLBackupPluginSvc' AND database_name =  <DB1>  
 
```

### <a name="view-backup-alerts"></a>Visualización de alertas de copia de seguridad

Dado que las copias de seguridad de registros se producen con una frecuencia de 15 minutos, en ocasiones, la supervisión de los trabajos de copia de seguridad puede resultar tediosa. Azure Backup proporciona ayuda en esta situación. Se desencadenan alertas por correo electrónico con todos los errores de copia de seguridad. Las alertas se consolidan en el nivel de base de datos por código de error. Se envía una alerta de correo electrónico solo para el primer error de copia de seguridad de una base de datos. Inicie sesión en Azure Portal para supervisar todos los errores de una base de datos. 

Para supervisar las alertas de copia de seguridad:

1. Inicie sesión en la suscripción de Azure en [Azure Portal](https://portal.azure.com).

2. Abra el almacén de Recovery Services registrado con la máquina virtual SQL.

3. En el panel **Almacén de Recovery Services**, seleccione **Alertas y eventos**. 

   ![Seleccionar Alertas y eventos](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. En el menú **Alertas y eventos**, seleccione **Alertas de copias de seguridad** para ver la lista de alertas.

   ![Seleccionar alertas de copia de seguridad](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

### <a name="stop-protection-for-a-sql-server-database"></a>Detención de la protección de una base de datos de SQL Server

Cuando se detiene la protección de una base de datos de SQL Server, Azure Backup pregunta si desea retener los puntos de recuperación. Hay dos formas de dejar de proteger una base de datos SQL:

* Detener todos los trabajos futuros de copia de seguridad y eliminar todos los puntos de recuperación.
* Detener todos los trabajos futuros de copia de seguridad pero dejar los puntos de recuperación.

Si decide detener la copia de seguridad con datos de retención, los puntos de recuperación se limpiarán en función de la directiva de copia de seguridad. Hasta que no se hayan limpiado todos los puntos de recuperación, habrá un cargo del precio de instancia protegida de SQL, más el almacenamiento consumido. Para más información sobre los precios de Azure Backup para SQL, vea la [página de precios de Azure Backup](https://azure.microsoft.com/pricing/details/backup/). 

Para detener la protección de una base de datos:

1. Abra el almacén de Recovery Services registrado con la máquina virtual SQL.

2. En el panel **almacén de Recovery Services**, en **Uso**, seleccione **Elementos de copia de seguridad** para abrir el menú del mismo nombre.

    ![Abrir el menú Elementos de copia de seguridad](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. En el menú **Elementos de copia de seguridad**, en **Tipo de administración de copia de seguridad**, seleccione **SQL en máquina virtual de Azure**. 

    ![Seleccionar SQL en la máquina virtual de Azure](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    El menú **Elementos de copia de seguridad** muestra la lista de base de datos SQL. 

4. En la lista de bases de datos SQL, seleccione la base de datos en la que desea detener la protección.

    ![Seleccionar la base de datos en la que detener la protección](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Al seleccionar la base de datos, se abre el menú.

5. En el menú de la base de datos seleccionada, elija **Detener copia de seguridad**. 

    ![Seleccionar Detener copia de seguridad](./media/backup-azure-sql-database/stop-db-button.png)

    Se abre el menú **Detener copia de seguridad**.

6. En el menú **Detener copia de seguridad**, elija **Retener datos de copia de seguridad** o **Eliminar datos de la copia de seguridad**. Si lo desea, indique un motivo para detener la protección y proporcione un comentario.

    ![Menú Detener copia de seguridad](./media/backup-azure-sql-database/stop-backup-button.png)

7. Seleccione **Detener copia de seguridad** para dejar de proteger la base de datos. 

### <a name="resume-protection-for-a-sql-database"></a>Reanudación de la protección en una base de datos SQL

Si seleccionó la opción **Retener datos de copia de seguridad** al detener la protección de la base de datos SQL, se puede reanudar la protección. Si no se retuvieron los datos de la copia de seguridad, la protección no se puede reanudar. 

1. Para reanudar la protección de la base de datos SQL, abra el elemento de copia de seguridad y seleccione **Reanudar copia de seguridad**.

    ![Seleccionar Reanudar copia de seguridad para reanudar la protección de la base de datos](./media/backup-azure-sql-database/resume-backup-button.png)

   Se abre el menú **Directiva de copia de seguridad**.

2. En el menú **Directiva de copia de seguridad**, seleccione una directiva y, después, seleccione **Guardar**.

### <a name="trigger-an-adhoc-backup"></a>Activación de una copia de seguridad ad hoc

Desencadene copias de seguridad ad hoc según sea necesario. Hay cuatro tipos de copias de seguridad ad hoc:

* Copia de seguridad completa
* Copiar solo copia de seguridad completa
* Copia de seguridad diferencial
* Copia de seguridad de registro

Para obtener información detallada, consulte [Tipos de copias de seguridad de SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/backup-overview-sql-server?view=sql-server-2017#types-of-backups).

### <a name="unregister-a-sql-server-instance"></a>Anulación del registro de una instancia de SQL Server

Para anular el registro de una instancia de SQL Server después de quitar la protección, antes debe eliminar el almacén:

1. Abra el almacén de Recovery Services registrado con la máquina virtual SQL.

2. En el panel **Almacén de Recovery Services**, en **Administrar**, seleccione **Infraestructura de Backup**.  

   ![Seleccionar Infraestructura de Backup](./media/backup-azure-sql-database/backup-infrastructure-button.png)

3. En **Servidores de administración**, seleccione **Servidores protegidos**.

   ![Seleccionar servidores protegidos](./media/backup-azure-sql-database/protected-servers.png)

    Se abre el menú **Servidores protegidos**. 

4. En el menú **Servidores protegidos**, seleccione el servidor del que desea anular el registro. Para eliminar el almacén, debe anular el registro de todos los servidores.

5. En el menú **Servidores protegidos**, haga clic con el botón derecho en el servidor protegido y seleccione **Eliminar**. 

   ![Seleccionar Eliminar](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="faq"></a>Preguntas más frecuentes

En la sección siguiente se proporciona información adicional acerca de la copia de seguridad de bases de datos SQL.

### <a name="can-i-throttle-the-speed-of-the-sql-server-backup-policy"></a>¿Puedo limitar la velocidad de la directiva de copia de seguridad de SQL Server?

Sí. Puede limitar la velocidad a la que se ejecuta la directiva de copia de seguridad para minimizar el impacto en una instancia de SQL Server.

Para cambiar la configuración:

1. En la instancia de SQL Server, en la carpeta C:\Archivos de programa\Azure Workload Backup\bin, abra el archivo **TaskThrottlerSettings.json**.

2. En el archivo TaskThrottlerSettings.json, cambie el valor de **DefaultBackupTasksThreshold** por uno inferior, por ejemplo, 5.

3. Guarde los cambios. Cierre el archivo.

4. En la instancia de SQL Server, abra el **Administrador de tareas**. Reinicie el **servicio de coordinador de carga de trabajo de Azure Backup**.

### <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>¿Puedo ejecutar una copia de seguridad completa desde una réplica secundaria?

No. No se admite esta característica.

### <a name="do-successful-backup-jobs-create-alerts"></a>¿Generan alertas los trabajos de copia de seguridad que se han realizado correctamente?

No. Los trabajos de copia de seguridad correctos no generan alertas. Las alertas se envían únicamente para los trabajos de copia de seguridad con errores.

### <a name="can-i-see-details-for-scheduled-backup-jobs-in-the-jobs-menu"></a>¿Se pueden ver los detalles de los trabajos de copia de seguridad programados en el menú Trabajos?

No. El menú **Trabajos de copia de seguridad** muestra los detalles de los trabajos ad hoc, pero no muestra los trabajos de copia de seguridad programados. Si se producen errores en los trabajos de copia de seguridad programados, puede encontrar los detalles en las alertas de trabajos con errores. Para supervisar todos los trabajos de copia de seguridad ad hoc y los programados, use [SQL Server Management Studio](backup-azure-sql-database.md#use-sql-server-management-studio-for-backup-jobs).

### <a name="when-i-select-a-sql-server-instance-are-future-databases-automatically-added"></a>Cuando selecciono una instancia de SQL Server, ¿se agregarán automáticamente las futuras bases de datos?

No. Al configurar la protección para una instancia de SQL Server, si selecciona la opción de nivel de servidor, se agregan todas las bases de datos. Si agrega las bases de datos a una instancia de SQL Server después de configurar la protección, deberá agregar manualmente las nuevas bases de datos para protegerlas. Las bases de datos no se incluyen de forma automática en la protección configurada.

### <a name="if-i-change-the-recovery-model-how-do-i-restart-protection"></a>Si cambio el modelo de recuperación ¿cómo reinicio la protección?

Desencadene una copia de seguridad completa. Las copias de seguridad de registros empiezan según lo previsto.

### <a name="can-i-protect-sql-always-on-availability-groups-where-the-primary-replica-is-on-premises"></a>¿Puedo proteger los grupos de disponibilidad Always On de SQL en los casos en los que la réplica principal está en una ubicación local?

No. Azure Backup protege los servidores de SQL Server que se ejecutan en Azure. Si se distribuye un grupo de disponibilidad entre máquinas locales y de Azure, se podrá proteger este solo si la réplica principal se ejecuta en Azure. Además, Azure Backup solo protege los nodos que se ejecutan en la misma región de Azure que el almacén de Recovery Services.

### <a name="can-i-protect-sql-always-on-availability-groups-which-are-spread-across-azure-regions"></a>¿Puedo proteger grupos de disponibilidad SQL AlwaysOn que se distribuyen entre las regiones de Azure?
El almacén de Azure Backup Recovery Services puede detectar y proteger todos los nodos que se encuentran en la misma región que el almacén de Recovery Services. Si tiene un grupo de disponibilidad SQL AlwaysON que abarca varias regiones de Azure, deberá configurar la copia de seguridad desde la región que tiene el nodo principal. Azure Backup será capaz de detectar y proteger todas las bases de datos del grupo de disponibilidad según la preferencia de copia de seguridad. Si no se cumple la preferencia de copia de seguridad, se producirá un error en las copias de seguridad y se emitirá la alerta de error.


## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Azure Backup, consulte el ejemplo de Azure PowerShell para realizar la copia de seguridad de máquinas virtuales cifradas.

> [!div class="nextstepaction"]
> [Copia de seguridad de una máquina virtual cifrada](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
