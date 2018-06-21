---
title: Copia de seguridad de base de datos de SQL Server en Azure | Microsoft Docs
description: En este tutorial se explica cómo se realiza la copia de seguridad de SQL Server en Azure. En este tutorial también se explica cómo se realiza la recuperación de SQL Server.
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
ms.date: 6/1/2018
ms.author: markgal;anuragm
ms.custom: ''
ms.openlocfilehash: 4ae64fefb58840214104a4e1cb338ec404fac1a8
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "35235420"
---
# <a name="back-up-sql-server-database-in-azure"></a>Copia de seguridad de base de datos de SQL Server en Azure

Las bases de datos SQL Server son cargas de trabajo críticas que requieren un bajo objetivo de punto de recuperación (RPO) y retención a largo plazo. Azure Backup ofrece una solución de copia de seguridad de SQL Server que requiere una infraestructura cero, lo que significa que no hay ningún servidor de copia de seguridad complejo, agente de administración o almacenamiento de copia de seguridad para administrarlos. Azure Backup ofrece una administración centralizada para las copias de seguridad de todos los servidores SQL o incluso cargas de trabajo distintas.

 En este artículo, aprenderá lo siguiente:

> [!div class="checklist"]
> * Requisitos previos para la copia de seguridad de SQL Server en Azure
> * Crear y usar un almacén de Recovery Services
> * Configurar copias de seguridad de bases de datos SQL Server
> * Establecer una directiva de copia de seguridad o retención para los puntos de recuperación
> * Cómo restaurar la base de datos

Antes de comenzar los procedimientos de este artículo, debe disponer de una base de datos de SQL Server en ejecución en Azure. [Puede usar máquinas virtuales del catálogo de soluciones de SQL para crear rápidamente una base de datos de SQL Server](../sql-database/sql-database-get-started-portal.md).

## <a name="public-preview-limitations"></a>Limitaciones de la versión preliminar pública

Los elementos siguientes son las limitaciones conocidas de la versión preliminar pública.

- La máquina virtual SQL requiere conectividad a Internet para acceder a direcciones IP públicas de Azure. Para obtener información más detallada, vea la sección [Establecimiento de la conectividad de red](backup-azure-sql-database.md#establish-network-connectivity).
- Puede proteger hasta 2000 bases de datos SQL en un almacén de Recovery Services. Las bases de datos SQL adicionales deben almacenarse en un almacén de Recovery Services independiente.
- [La copia de seguridad de grupos de disponibilidad distribuidos tiene limitaciones](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017).
- No se admiten instancias de clúster de conmutación por error (FCI) de SQL.
- Use Azure Portal para configurar Azure Backup para proteger las bases de datos SQL Server. La compatibilidad con Azure PowerShell, la CLI y las API de REST no está disponible actualmente.

## <a name="supported-azure-geos"></a>Replicaciones geográficas de Azure admitidas

- Sudeste de Australia (ASE) 
- Sur de Brasil (BRS)
- Centro de Canadá (CNC)
- Este de Canadá (CE)
- Centro de EE. UU. (CUS)
- Asia Oriental (EA)
- Australia Oriental (AE) 
- Este de EE. UU. (EUS)
- Este de EE. UU. 2 (EUS2)
- Este de Japón (JPE)
- Oeste de Japón (JPE)
- India central (INC) 
- India del Sur (INS)
- Centro de Corea del Sur (KRC)
- Corea del Sur (KRS)
- Centro-norte de EE. UU. (NCUS) 
- Europa del Norte (NE) 
- Centro-sur de EE. UU. (SCUS) 
- Asia Suroriental (SEA)
- Sur de Reino Unido (UKS) 
- Oeste de Reino Unido (UKW) 
- Europa Occidental (WE) 
- Oeste de EE. UU. (WUS)
- Centro-oeste de EE. UU. (WCUS)
- Oeste de Estados Unidos 2 (WUS 2) 

## <a name="supported-operating-systems-and-versions-of-sql-server"></a>Sistemas operativos y versiones de SQL Server compatibles

Los siguientes sistemas operativos y versiones compatibles de SQL Server se aplican a las máquinas virtuales de Azure incluidas en el catálogo de soluciones de SQL y no a aquellas que no están en dicho catálogo (donde SQL Server se instala manualmente).

### <a name="supported-operating-systems"></a>Sistemas operativos compatibles

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

Linux no se admite actualmente.

### <a name="supported-versionseditions-of-sql-server"></a>Versiones y ediciones compatibles de SQL Server

- SQL 2012 Enterprise, Standard, Web, Developer, Express
- SQL 2014 Enterprise, Standard, Web, Developer, Express
- SQL 2016 Enterprise, Standard, Web, Developer, Express
- SQL 2017 Enterprise, Standard, Web, Developer, Express


## <a name="prerequisites-for-using-azure-backup-to-protect-sql-server"></a>Requisitos previos para usar Azure Backup para proteger SQL Server 

Para poder realizar copias de seguridad de la base de datos SQL Server, primero debe comprobar si reúne las siguientes condiciones. :

- Identifique o [cree un almacén de Recovery Services](backup-azure-sql-database.md#create-a-recovery-services-vault) en la misma región o configuración regional que la máquina virtual que hospeda SQL Server.
- [Compruebe los permisos en la máquina virtual](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms) necesarios para realizar la copia de seguridad de las bases de datos SQL.
- [La máquina virtual SQL tiene conectividad de red](backup-azure-sql-database.md#establish-network-connectivity).

> [!NOTE]
> Puede tener solo una solución de copia de seguridad a la vez para realizar copias de seguridad de las bases de datos SQL Server. Deshabilite cualquier otra copia de seguridad de SQL antes de utilizar esta característica; las otras copias de seguridad interferirán y producirán un error. Puede habilitar Azure Backup para máquinas virtuales IaaS con la copia de seguridad de SQL sin que se produzca ningún conflicto. 
>

Si se dan estas condiciones en su entorno, continúe con la sección de [configuración del almacén para proteger una base de datos SQL](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database). Si no cumple ninguno de los requisitos previos, continúe leyendo esta sección.


## <a name="establish-network-connectivity"></a>Establecimiento de conectividad de red

Para todas las operaciones, la máquina virtual SQL necesita conectividad a las direcciones IP públicas de Azure. Se producirá un error en las operaciones con máquinas virtuales SQL (como detección de base de datos, configuración de copias de seguridad, copias de seguridad programadas, restauración de puntos de recuperación, etc.) si no existe conectividad con las direcciones IP públicas. Use alguna de las siguientes opciones para ofrecer una ruta de acceso clara para el tráfico de copia de seguridad:

- Incluya los intervalos de IP del centro de datos de Azure en una lista blanca: para incluir en una lista blanca los intervalos de IP del centro de datos de Azure, consulte la [página del centro de descargas para obtener información detallada sobre los intervalos de IP junto con instrucciones](https://www.microsoft.com/download/details.aspx?id=41653). 
- Implemente un servidor proxy HTTP para enrutar el tráfico: cuando haga copias de seguridad de una base de datos SQL en una máquina virtual, la extensión de copia de seguridad de la máquina virtual utiliza las API HTTPS para enviar comandos de administración a Azure Backup y datos a Azure Storage. La extensión de copia de seguridad también usa Azure Active Directory (AAD) para la autenticación. Enrute el tráfico de extensión de copia de seguridad de estos tres servicios a través del proxy HTTP, ya que es el único componente configurado para acceder a la red pública de Internet.

Los inconvenientes entre las opciones son: capacidad de administración, control granular y costo.

> [!NOTE]
>Las etiquetas de servicio para Azure Backup deben estar disponibles con la disponibilidad general.
>

| Opción | Ventajas | Desventajas |
| ------ | ---------- | ------------- |
| Creación de una lista blanca con intervalos IP | Sin costos adicionales. <br/> Para abrir el acceso en un grupo de seguridad de red, use el cmdlet **Set-AzureNetworkSecurityRule**. | Es complejo de administrar, ya que los intervalos de IP afectados cambian con el tiempo. <br/>Proporciona acceso a la totalidad de Azure, no solo al almacenamiento.|
| Usar un servidor proxy HTTP   | Se permite un control detallado en el proxy sobre las direcciones URL de almacenamiento. <br/>Un único punto de acceso a Internet para las máquinas virtuales. <br/> No están sujetas a cambios de direcciones IP de Azure. | Costes adicionales de ejecutar una máquina virtual con el software de proxy. |

## <a name="set-permissions-for-non-marketplace-sql-vms"></a>Definición de permisos para máquinas virtuales SQL no incluidas en el catálogo de soluciones

Para hacer una copia de seguridad de una máquina virtual, Azure Backup requiere la instalación de la extensión **AzureBackupWindowsWorkload**. Si usa máquinas virtuales de Azure Marketplace, vaya directamente a [Detección de bases de datos SQL Server](backup-azure-sql-database.md#discover-sql-server-databases). Si no se creó la máquina virtual que hospeda las bases de datos SQL desde Azure Marketplace, complete la siguiente sección para instalar la extensión y establecer los permisos apropiados. Además de la extensión **AzureBackupWindowsWorkload**, Azure Backup requiere privilegios de administrador del sistema de SQL para proteger las bases de datos SQL. Al detectar las bases de datos en la máquina virtual, Azure Backup crea una cuenta, NT Service\AzureWLBackupPluginSvc. Para que Azure Backup detecte bases de datos SQL, la cuenta NT Service\AzureWLBackupPluginSvc debe disponer de permisos de administrador del sistema de SQL y para iniciar sesión en SQL. En el procedimiento siguiente se explica cómo proporcionar estos permisos.

Para configurar permisos:

1. En [Azure Portal](https://portal.azure.com), abra el almacén de Recovery Services que se va a utilizar para proteger las bases de datos SQL.
2. En el menú del panel del almacén, haga clic en **+ Copia de seguridad** para abrir el menú **Objetivo de Backup**.

   ![Haga clic en + Copia de seguridad para abrir el menú Objetivo de Backup.](./media/backup-azure-sql-database/open-backup-menu.png)

3. En el menú **Objetivo de Backup**, en el menú **¿Dónde se ejecuta su carga de trabajo?**, deje **Azure** como opción predeterminada.

4. En el menú **¿De qué quiere hacer una copia de seguridad?**, expanda el menú desplegable y seleccione **SQL Server en VM de Azure**.

    ![Haga clic en + Copia de seguridad para abrir el menú Objetivo de Backup.](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    En el menú **Objetivo de Backup** se muestran dos pasos nuevos: **Detección de bases de datos en máquinas virtuales** y **Configuración de copia de seguridad**. **Detección de bases de datos en máquinas virtuales** inicia una búsqueda de máquinas virtuales de Azure.

    ![Se muestran los nuevos pasos de objetivo de Backup](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

5. Haga clic en **Iniciar detección** para buscar las máquinas virtuales sin protección en la suscripción. Dependiendo del número de máquinas virtuales sin protección de la suscripción, puede llevar bastante tiempo recorrer todas las máquinas virtuales.

    ![Copia de seguridad pendiente](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Una vez que se detecta una máquina virtual sin protección, aparece en la lista. Las máquinas virtuales no protegidas se enumeran por su grupo de recursos y el nombre de máquina virtual. Es posible que varias máquinas virtuales tengan el mismo nombre. Sin embargo, las máquinas virtuales con el mismo nombre pertenecen a distintos grupos de recursos. Si una máquina virtual esperada no aparece en la lista, vea si la máquina virtual ya está protegida en un almacén.

6. En la lista de máquinas virtuales, seleccione la máquina virtual que contiene la base de datos SQL de la que desea realizar la copia de seguridad y haga clic en **Detectar bases de datos**. 

    El proceso de detección instala la extensión **AzureBackupWindowsWorkload** en la máquina virtual. La extensión permite al servicio Azure Backup comunicarse con la máquina virtual, para que pueda realizar copias de seguridad de las bases de datos SQL. Una vez instalada la extensión, Azure Backup crea la cuenta de servicio virtual de Windows, **NT Service\AzureWLBackupPluginSvc**, en la máquina virtual. La cuenta de servicio virtual requiere permisos de administrador del sistema de SQL. Durante el proceso de instalación de la cuenta de servicio, si aparece el error **UserErrorSQLNoSysadminMembership**, consulte la sección [Corrección de permisos de administrador del sistema de SQL](backup-azure-sql-database.md#fixing-sql-sysadmin-permissions).

    El área de notificaciones muestra el progreso de la detección de bases de datos. Según la cantidad de bases de datos que haya en la máquina virtual, puede que el trabajo tarde un tiempo en completarse. Cuando se detectan las bases de datos seleccionadas, aparece un mensaje de operación correcta.

    ![mensaje de notificación de una implementación correcta](./media/backup-azure-sql-database/notifications-db-discovered.png)

Una vez asociada la base de datos con el almacén de Recovery Services, el paso siguiente consiste en [configurar la copia de seguridad](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="fixing-sql-sysadmin-permissions"></a>Corrección de permisos de administrador del sistema de SQL

Durante el proceso de instalación, si ve el error **UserErrorSQLNoSysadminMembership**, inicie sesión en SQL Server Management Studio (SSMS) con una cuenta que tenga permisos de administrador del sistema de SQL. A menos que necesite permisos especiales, debe ser capaz de usar la autenticación de Windows para que se reconozca la cuenta.

1. En el servidor SQL Server, abra la carpeta **Seguridad/Inicios de sesión**.

    ![Abra las carpetas de seguridad e inicio de sesión y de SQL Server para ver las cuentas](./media/backup-azure-sql-database/security-login-list.png)

2. En la carpeta Inicios de sesión, haga clic con el botón derecho y seleccione **Nuevo inicio de sesión** y, en el cuadro de diálogo Inicio de sesión - Nuevo, haga clic en **Buscar**.

    ![Abrir Buscar en el cuadro de diálogo Inicio de sesión - Nuevo](./media/backup-azure-sql-database/new-login-search.png)

3. Como la cuenta de servicio virtual de Windows, **NT Service\AzureWLBackupPluginSvc**, ya se ha creado durante el registro de la máquina virtual y la fase de detección de SQL, escriba el nombre de la cuenta tal y como aparece en el cuadro de diálogo **Enter the object name to select** (Escribir el nombre de objeto para seleccionar). Haga clic en **Comprobar nombres** para resolver el nombre. 

    ![Haga clic en el botón Comprobar nombres para resolver el nombre de servicio desconocido](./media/backup-azure-sql-database/check-name.png)

4. Haga clic en **Aceptar** para cerrar el cuadro de diálogo Seleccionar usuario o grupo.

5. En el cuadro de diálogo **Roles de servidor**, asegúrese de que el rol **administrador del sistema** está seleccionado. Después haga clic en **Aceptar** para cerrar **Inicio de sesión - Nuevo**.

    ![Asegúrese de que el rol del servidor administrador del sistema está seleccionado](./media/backup-azure-sql-database/sysadmin-server-role.png)

    Ahora deben existir los permisos necesarios.

6. Aunque se ha corregido el error de permisos, deberá asociar la base de datos con el almacén de Recovery Services. En la lista **Servidores protegidos** de Azure Portal, haga clic con el botón derecho en el servidor que presenta el error y seleccione **Volver a detectar bases de datos**.

    ![Verifique que el servidor tiene los permisos adecuados](./media/backup-azure-sql-database/check-erroneous-server.png)

    El área de notificaciones muestra el progreso de la detección de bases de datos. Según la cantidad de bases de datos que haya en la máquina virtual, puede que el trabajo tarde un tiempo en completarse. Cuando no se encuentran las bases de datos seleccionadas, aparece un mensaje de operación correcta en el área de notificaciones.

    ![mensaje de notificación de una implementación correcta](./media/backup-azure-sql-database/notifications-db-discovered.png)

Una vez asociada la base de datos con el almacén de Recovery Services, el paso siguiente consiste en [configurar la copia de seguridad](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

[!INCLUDE [Section explaining how to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>Detección de bases de datos SQL Server

Azure Backup puede detectar todas las bases de datos en una instancia de SQL Server, para poder protegerlas según sus requisitos de copia de seguridad. Utilice el procedimiento siguiente para identificar la máquina virtual que hospeda las bases de datos SQL. Una vez identificada la máquina virtual, Azure Backup instala una extensión ligera para detectar las bases de datos SQL Server.

1. Inicie sesión en su suscripción en [Azure Portal](https://portal.azure.com/).
2. En el menú izquierdo, seleccione **Todos los servicios**.

    ![Elija la opción Todos los servicios en el menú principal](./media/backup-azure-sql-database/click-all-services.png) <br/>

3. En el cuadro de diálogo Todos los servicios, escriba *Recovery Services*. En cuanto empiece a escribirlo, la entrada filtra la lista de recursos. Cuando vea la opción **Almacenes de Recovery Services**, selecciónela.

    ![Escriba Recovery Services en el cuadro de diálogo Todos los servicios](./media/backup-azure-sql-database/all-services.png) <br/>

    Aparece la lista de almacenes de Recovery Services de la suscripción. 

4. En la lista de almacenes de Recovery Services, seleccione el almacén que desea usar para proteger las bases de datos SQL.

5. En el menú del panel del almacén, haga clic en **+ Copia de seguridad** para abrir el menú **Objetivo de Backup**.

   ![Haga clic en + Copia de seguridad para abrir el menú Objetivo de Backup.](./media/backup-azure-sql-database/open-backup-menu.png)

6. En el menú **Objetivo de Backup**, en el menú **¿Dónde se ejecuta su carga de trabajo?**, deje **Azure** como opción predeterminada.

7. En el menú **¿De qué quiere hacer una copia de seguridad?**, expanda el menú desplegable y seleccione **SQL Server en VM de Azure**.

    ![Haga clic en + Copia de seguridad para abrir el menú Objetivo de Backup.](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Una vez realizada la selección, en el menú **Objetivo de Backup** se muestran dos pasos: Detección de bases de datos en máquinas virtuales y Configuración de copia de seguridad. 

    ![Se muestran los nuevos pasos de objetivo de Backup](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

8. Haga clic en **Iniciar detección** para buscar las máquinas virtuales sin protección en la suscripción. Dependiendo del número de máquinas virtuales sin protección de la suscripción, puede llevar bastante tiempo recorrer todas las máquinas virtuales.

    ![Copia de seguridad pendiente](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Una vez que se detecta una máquina virtual sin protección, aparece en la lista. Varias máquinas virtuales pueden tener el mismo nombre. Sin embargo, varias máquinas virtuales con el mismo nombre pertenecen a distintos grupos de recursos. Las máquinas virtuales no protegidas se enumeran por su grupo de recursos y el nombre de máquina virtual. Si una máquina virtual esperada no aparece en la lista, vea si la máquina virtual ya está protegida en un almacén.

9. En la lista de máquinas virtuales, seleccione la casilla de la máquina virtual que contiene las bases de datos SQL que desea proteger y haga clic en **Detectar bases de datos**.

    Azure Backup detecta todas las bases de datos SQL de la máquina virtual. Para obtener información sobre lo que ocurre durante la fase de detección de base de datos, vea la sección siguiente [Operaciones back-end durante la detección de bases de datos SQL](backup-azure-sql-database.md#backend-operations-when-discovering-sql-databases). Tras detectar las bases de datos SQL, ya puede [configurar el trabajo de copia de seguridad](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="backend-operations-when-discovering-sql-databases"></a>Operaciones back-end durante la detección de bases de datos SQL

Cuando usa la herramienta **Detectar bases de datos**, Azure Backup ejecuta las siguientes operaciones en segundo plano:

- Registra la máquina virtual en el almacén de Recovery Services para la copia de seguridad de la carga de trabajo. La copia de seguridad de la máquina virtual registrada solo puede realizarse en este almacén de Recovery Services. 

- Instala la extensión **AzureBackupWindowsWorkload** en la máquina virtual. La copia de seguridad de una base de datos SQL es una solución sin agente, es decir, con la extensión instalada en la máquina virtual, no se instala ningún agente en la base de datos SQL.

- Crea la cuenta de servicio, **NT Service\AzureWLBackupPluginSvc**, en la máquina virtual. Todas las operaciones de copia de seguridad y restauración utilizan la cuenta de servicio. **NT Server\AzureWLBackupPluginSvc** necesita permisos de administrador del sistema de SQL. Todas las máquinas virtuales del catálogo de soluciones de SQL incorporan la extensión SqlIaaSExtension, y AzureBackupWindowsWorkload usa SQLIaaSExtension para obtener automáticamente los permisos necesarios. Si la máquina virtual no tiene la extensión SqlIaaSExtension instalada, se produce un error en la operación de detección de base de datos y se obtiene el mensaje de error **UserErrorSQLNoSysAdminMembership**. Para agregar el permiso de administrador del sistema para poder realizar copias de seguridad, siga las instrucciones de [configuración de los permisos de Azure Backup para máquinas virtuales SQL no incluidas en el catálogo de soluciones](backup-azure-sql-database.md#set-permissions-for-non--marketplace-sql-vms).

    ![Seleccione la máquina virtual y la base de datos](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup-for-sql-server-database"></a>Configuración de copia de seguridad para bases de datos SQL Server

Azure Backup ofrece servicios de administración para proteger las bases de datos SQL Server y administrar trabajos de copia de seguridad. Las funcionalidades de administración y supervisión dependen del almacén de Recovery Services. 

> [!NOTE]
> Puede tener solo una solución de copia de seguridad a la vez para realizar copias de seguridad de las bases de datos SQL Server. Deshabilite cualquier otra copia de seguridad de SQL antes de utilizar esta característica; las otras copias de seguridad interferirán y producirán un error. Puede habilitar Azure Backup para máquinas virtuales IaaS con la copia de seguridad de SQL sin que se produzca ningún conflicto. 
>

Para configurar la protección de la base de datos SQL:

1. Abra el almacén de Recovery Services registrado con la máquina virtual SQL.

2. En el menú del panel del almacén, haga clic en **+ Copia de seguridad** para abrir el menú **Objetivo de Backup**.

    ![Haga clic en + Copia de seguridad para abrir el menú Objetivo de Backup.](./media/backup-azure-sql-database/open-backup-menu.png)

3. En el menú **Objetivo de Backup**, en el menú **¿Dónde se ejecuta su carga de trabajo?**, deje **Azure** como opción predeterminada.

4. En el menú **¿De qué quiere hacer una copia de seguridad?**, expanda el menú desplegable y seleccione **SQL Server en VM de Azure**.

    ![Haga clic en + Copia de seguridad para abrir el menú Objetivo de Backup.](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Una vez realizada la selección, en el menú **Objetivo de Backup** se muestran dos pasos: Detección de bases de datos en máquinas virtuales y Configuración de copia de seguridad. Si ha leído este artículo de forma ordenada, ya habrá detectado las máquinas virtuales no protegidas, y este almacén estará registrado con una máquina virtual. Ahora está listo para configurar la protección de las bases de datos SQL.

5. En el menú Objetivo de Backup, haga clic en **Configurar copia de seguridad**.

    ![Se muestran los nuevos pasos de objetivo de Backup](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    El servicio Azure Backup muestra todas las instancias de SQL con bases de datos independientes, además de los grupos de disponibilidad AlwaysOn de SQL. Para ver las bases de datos independientes en la instancia de SQL, haga clic en el botón de contenido adicional que se encuentra junto al nombre de instancia para ver las bases de datos. En las imágenes siguientes se muestran ejemplos de una instancia independiente y un grupo de disponibilidad AlwaysOn.

    > [!NOTE]
    > Se realizan copias de seguridad completas y diferenciales desde el nodo principal, ya que la plataforma SQL tiene dicha limitación. Se puede producir la copia de seguridad de registros en función de las preferencias de copia de seguridad. Debido a esta limitación, debe registrarse el nodo principal.
    >

    ![Lista de bases de datos en instancias de SQL](./media/backup-azure-sql-database/discovered-databases.png)

    Haga clic en el botón de contenido adicional que se encuentra junto a los grupos de disponibilidad AlwaysOn para ver la lista de bases de datos.

    ![Lista de bases de datos del grupo de disponibilidad AlwaysOn](./media/backup-azure-sql-database/discovered-database-availability-group.png)

6. En la lista de bases de datos, seleccione todo lo que desea proteger y luego haga clic en **Aceptar**.

    ![Seleccione varias bases de datos para protegerlas](./media/backup-azure-sql-database/select-multiple-database-protection.png)

    Puede seleccionar hasta cincuenta bases de datos al mismo tiempo. Si desea proteger más de cincuenta bases de datos, realice varios pases. Después de proteger las cincuenta primeras bases de datos, repita este paso para proteger el siguiente conjunto de bases de datos.
    > [!Note] 
    > Para optimizar las cargas de copia de seguridad, Azure Backup divide grandes trabajos de copia de seguridad en varios lotes. El número máximo de bases de datos de un trabajo de copia de seguridad es cincuenta.
    >
    >

7. Para crear o elegir una directiva de copia de seguridad, en el menú **Copia de seguridad**, seleccione **Directiva de copia de seguridad** para abrir el menú.

    ![Seleccionar la opción Directiva de copia de seguridad](./media/backup-azure-sql-database/select-backup-policy.png)

8. En el menú desplegable **Elegir directiva de copia de seguridad**, elija la directiva de copia de seguridad y haga clic en **Aceptar**. Para obtener información sobre cómo crear su propia directiva de copia de seguridad, vea la sección [Definición de una directiva de copia de seguridad](backup-azure-sql-database.md#define-a-backup-policy).

    ![Elija una directiva de copia de seguridad en el menú desplegable](./media/backup-azure-sql-database/select-backup-policy-steptwo.png)

    En el menú Directiva de copia de seguridad del menú desplegable **Elegir directiva de copia de seguridad**, puede seleccionar: 
    - la directiva HourlyLogBackup predeterminada, 
    - una directiva de copia de seguridad existente creada previamente para SQL,
    - para [definir una nueva directiva](backup-azure-sql-database.md#define-a-backup-policy) basada en el objetivo de punto de recuperación (RPO) y en la duración de retención. 

    > [!Note]
    > Azure Backup admite la retención a largo plazo basada en el esquema de copia de seguridad abuelo-padre-hijo para optimizar el consumo de almacenamiento back-end al mismo tiempo que se cumplen los requisitos de conformidad.
    >

9. Una vez que haya elegido una directiva de copia de seguridad, en el **menú Copia de seguridad**, haga clic en **Habilitar copia de seguridad**.

    ![Habilitar la directiva de copia de seguridad elegida](./media/backup-azure-sql-database/enable-backup-button.png)

    Puede realizar el seguimiento del progreso de la configuración en el área de notificaciones del portal.

    ![Ver el área de notificaciones](./media/backup-azure-sql-database/notifications-area.png)


### <a name="define-a-backup-policy"></a>Definición de una directiva de copia de seguridad

Una directiva de copia de seguridad define una matriz del momento en que se realizan las copias de seguridad y durante cuánto tiempo se retienen. Puede usar Azure Backup para programar tres tipos de copia de seguridad de bases de datos SQL:

* Copia de seguridad completa: una copia de seguridad completa de la base de datos realiza una copia de seguridad de toda la base de datos. Una copia de seguridad completa contiene todos los datos de una base de datos específica o de un conjunto de archivos o grupos de archivos y suficientes registros para recuperar esos datos. A lo sumo, puede desencadenar una copia de seguridad completa al día. Puede elegir realizar una copia de seguridad completa en un intervalo diario o semanal. 
* Copia de seguridad diferencial: la copia de seguridad diferencial se basa en la copia de seguridad de datos completa anterior más reciente. Una copia de seguridad diferencial captura solo los datos que han cambiado desde la copia de seguridad completa. A lo sumo, puede desencadenar una copia de seguridad diferencial al día. No se puede configurar una copia de seguridad completa y una copia de seguridad diferencial en el mismo día.
* Copia de seguridad del registro de transacciones: una copia de seguridad de registros permite realizar la restauración a un momento dado hasta un segundo específico. A lo sumo, puede configurar las copias de seguridad del registro de transacciones cada 15 minutos.

La directiva se crea al nivel de almacén de Recovery Services. Si tiene varios almacenes, estos pueden usar la misma directiva de copia de seguridad, pero debe aplicar la directiva de copia de seguridad a cada almacén. Al crear una directiva de copia de seguridad, la copia de seguridad completa diaria es la predeterminada. Puede agregar una copia de seguridad diferencial, pero solo si cambia las copias de seguridad completas para que se realicen semanalmente. En el siguiente procedimiento se explica cómo crear una directiva de copia de seguridad para un servidor SQL Server en una máquina virtual de Azure.

Para crear una directiva de copia de seguridad

1. En el menú desplegable **Elegir directiva de copia de seguridad** del menú Directiva de copia de seguridad, seleccione **Crear nueva**.

   ![crear una directiva de copia de seguridad](./media/backup-azure-sql-database/create-new-backup-policy.png)

    El menú Directiva de copia de seguridad cambia para proporcionar los campos necesarios para cualquier nueva política de copia de seguridad de SQL Server.

   ![nuevos campos de directiva de copia de seguridad](./media/backup-azure-sql-database/blank-new-policy.png)

2. En **Nombre de directiva**, proporcione un nombre. 

3. Una copia de seguridad completa es obligatoria. Puede aceptar los valores predeterminados para la copia de seguridad completa, o haga clic en **Copia de seguridad completa** para editar la directiva.

    ![nuevos campos de directiva de copia de seguridad](./media/backup-azure-sql-database/full-backup-policy.png)

    En la directiva de copia de seguridad completa, elija una frecuencia diaria o semanal. Si selecciona la frecuencia diaria, elija la hora y zona horaria, cuando se inicie el trabajo de copia de seguridad. Si elige copias de seguridad completas diarias, no se pueden crear copias de seguridad diferenciales.

   ![configuración de intervalo diario](./media/backup-azure-sql-database/daily-interval.png)

    Si selecciona la frecuencia semanal, elija el día de la semana, la hora y la zona horaria, cuando se inicie el trabajo de copia de seguridad.

   ![configuración de intervalo semanal](./media/backup-azure-sql-database/weekly-interval.png)

4. De forma predeterminada, se seleccionan todas las opciones de duración de retención (diaria, semanal, mensual y anual). Desactive los límites de la duración de retención que no quiera usar y defina los intervalos que desea usar. En el menú de la directiva de copia de seguridad completa, haga clic en **Aceptar** para aceptar la configuración.

   ![configuración del intervalo de la duración de retención](./media/backup-azure-sql-database/retention-range-interval.png)

    Los puntos de recuperación se etiquetan para la retención, en función de su duración de retención. Por ejemplo, si selecciona la frecuencia diaria, solo se desencadena una copia de seguridad completa cada día. En función de la retención semanal, la copia de seguridad del día específico se etiqueta y se retiene en función de la duración de retención semanal. La duración de retención mensual y anual se comportan de forma similar.

5. Para agregar una directiva de copia de seguridad diferencial, haga clic en **Copia de seguridad diferencial** para abrir su menú. 

   ![Abrir directiva diferencial](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

    En el menú de directiva de copia de seguridad diferencial, seleccione **Habilitar** para abrir los controles de frecuencia y retención. A lo sumo, puede desencadenar una copia de seguridad diferencial al día.
    > [!Important] 
    > Como máximo, las copias de seguridad diferenciales se pueden retener durante 180 días. Si necesita más tiempo de retención, debe usar copias de seguridad completas; no puede usar copias de seguridad diferenciales.
    >

   ![Editar directiva diferencial](./media/backup-azure-sql-database/enable-differential-backup-policy.png)

    Haga clic en **Aceptar** para guardar la directiva y volver al menú principal de la directiva de copia de seguridad.

6. Para agregar una directiva de copia de seguridad del registro de transacciones, haga clic en **Copia de seguridad de registros** para abrir su menú. En el menú Copia de seguridad de registros, seleccione **Habilitar** y establezca los controles de frecuencia y retención. Las copia de seguridad de registros pueden realizarse cada 15 minutos y se pueden retener durante un período máximo de 35 días. Haga clic en **Aceptar** para guardar la directiva y volver al menú principal de la directiva de copia de seguridad.

   ![Editar directiva de copia de seguridad de registros](./media/backup-azure-sql-database/log-backup-policy-editor.png)

7. Elija si desea habilitar la compresión de copia de seguridad de SQL. La compresión está deshabilitada de manera predeterminada.

    En el back-end, Azure Backup usa la compresión de copia de seguridad nativa de SQL.

8. Cuando haya realizado todas las modificaciones a la directiva de copia de seguridad, haga clic en **Aceptar**. 

   ![duración de retención diferencial](./media/backup-azure-sql-database/differential-backup-policy.png)

## <a name="restore-a-sql-database"></a>Restauración de una base de datos SQL

Azure Backup proporciona funcionalidad para restaurar las bases de datos individuales a una fecha u hora específicas, hasta un segundo específico, con las copias de seguridad del registro de transacciones. En función de los tiempos de restauración proporcionados, Azure Backup determina automáticamente si la copia de seguridad será completa o diferencial y también la cadena de copia de seguridad de registros necesaria para restaurar los datos.

Como alternativa, puede seleccionar una copia de seguridad completa o diferencial específica para restaurar a un punto de recuperación específico en lugar de a una hora específica.

Para restaurar una base de datos

1. Abra el almacén de Recovery Services registrado con la máquina virtual SQL.

2. En el panel del almacén, seleccione los elementos de copia de seguridad de **uso** para abrir el menú Elementos de copia de seguridad.

    ![Haga clic en + Copia de seguridad para abrir el menú Objetivo de Backup.](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. En el menú **Elementos de copia de seguridad**, seleccione el tipo de administración de copia de seguridad **SQL en máquina virtual de Azure**. 

    ![Haga clic en + Copia de seguridad para abrir el menú Objetivo de Backup.](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    La lista de elementos de copia de seguridad se ajusta para mostrar la lista de base de datos SQL. 

4. En la lista de bases de datos SQL, seleccione la base de datos que desea restaurar.

    ![Seleccionar SQL en la máquina virtual de Azure en la lista](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Al seleccionar la base de datos, se abre el menú. Este menú proporciona los detalles de copia de seguridad de la base de datos, incluidos:

    * los puntos de restauración más antiguos y más recientes,
    * el estado de copia de seguridad de registros de las 24 últimas horas (para bases de datos con el modelo de recuperación optimizado para cargas masivas de registros y el modelo de recuperación optimizado para cargas completas de registros, si está definida la configuración para copia de seguridad del registro de transacciones).

5. En el menú de la base de datos seleccionada, haga clic en **Restaurar BD** para abrir el menú de restauración.

    ![seleccionar restaurar base de datos](./media/backup-azure-sql-database/restore-db-button.png)

    El menú **Restaurar** se abre, y también el menú **Restaurar configuración**. El menú **Restaurar configuración** es el primer paso para configurar la restauración. En este menú, seleccione dónde desea restaurar los datos. Las opciones son:
    * Ubicación alternativa: use esta opción si desea restaurar la base de datos en una ubicación alternativa al tiempo que mantiene la base de datos de origen original.
    * Sobrescribir la base de datos: restaura los datos en la misma instancia de SQL Server que el origen. El efecto de esto es que se sobrescribe la base de datos original.

    > [!Important]
    > Si la base de datos seleccionada pertenece a un grupo de disponibilidad AlwaysOn, SQL no permite sobrescribir la base de datos. En este caso, solo está habilitada la opción **Ubicación alternativa**.
    >

    ![Haga clic en Configurar para abrir el menú Restaurar configuración](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Restauración a una ubicación alternativa

Este procedimiento le guía en la restauración de datos a una ubicación alternativa. Si desea sobrescribir la base de datos al restaurar, vaya a la sección [Restauración y sobrescritura de la base de datos](backup-azure-sql-database.md#restore-and-overwrite-the-database). En este procedimiento se asume que tiene el almacén de Recovery Services abierto y que se encuentra en el menú Restaurar configuración. Si no es así, empiece con la sección [Restauración de una base de datos SQL](backup-azure-sql-database.md#restore-a-sql-database).

El menú desplegable **Servidor** solo muestra los servidores SQL Server registrados con el almacén de Recovery Services. Si el servidor que desea no está en la lista **Servidor**, vea la sección [Detección de bases de datos SQL Server](backup-azure-sql-database.md#discover-sql-server-databases) para encontrar el servidor. Durante el proceso de detección de bases de datos, todos los servidores nuevos se registran en el almacén de Recovery Services.

1. En el menú **Restaurar configuración**:

    * Seleccione **Ubicación alternativa**.
    * Para **Servidor**, elija la instancia de SQL Server en la que desea restaurar la base de datos.
    * En el menú desplegable **Instancia**, elija una instancia de SQL.
    * En el cuadro de diálogo **Nombre de la base de datos restaurada**, proporcione el nombre de la base de datos de destino.
    * Si procede, seleccione **Overwrite if the DB with the same name already exists on selected SQL instance** (Sobrescribir si ya existe una base de datos con el mismo nombre en la instancia de SQL seleccionada).
    * Haga clic en **Aceptar** para completar la configuración del destino y elegir un punto de restauración.

    ![Seleccionar ubicación alternativa, instancia y nombre en el menú Restaurar configuración](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. En el menú **Seleccionar punto de restauración**, puede elegir un registro (punto en el tiempo) o un punto de restauración completo y diferencial. Si desea restaurar a un registro específico en un momento dado, continúe con este paso. Si desea restaurar un punto de restauración completo o diferencial, vaya directamente al paso 3.

    ![Menú Seleccionar punto de restauración](./media/backup-azure-sql-database/recovery-point-menu.png)

    La restauración a un momento dado solo está disponible para copias de seguridad de registros con el modelo de recuperación optimizado para cargas masivas de registros y el modelo de recuperación optimizado para cargas completas de registros. Para restaurar a un momento dado específico:

    1. Seleccione **Registro (punto en el tiempo)** como la opción de restauración.

        ![elegir tipo de punto de restauración](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. En **Fecha y hora de restauración**, haga clic en el icono del calendario para abrir el calendario. Las fechas en negrita contienen puntos de recuperación, y la fecha actual está resaltada. Seleccione una fecha en el calendario con puntos de recuperación. No puede seleccionar las fechas sin ningún punto de recuperación.

        ![Abrir calendario](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Una vez seleccionada la fecha, el gráfico de línea cronológica muestra los puntos de recuperación disponibles en un rango continuo.

    3. Con el gráfico de línea cronológica o el cuadro de diálogo Tiempo, especifique un tiempo específico para el punto de recuperación y haga clic en **Aceptar** para completar el paso Punto de restauración.
    
       ![Abrir calendario](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        El menú **Seleccionar punto de restauración** se cierra y el menú **Configuración avanzada** se abre.

       ![menú de configuración avanzada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. En el menú **Configuración avanzada**:

        * Para mantener la base de datos no operativa después de la restauración, en el menú **Restaurar con NORECOVERY**, seleccione **Habilitado**.
        * Si desea cambiar la ubicación de restauración en el servidor de destino, proporcione una nueva ruta de acceso en la columna **Destino**.
        * Haga clic en **Aceptar** para aprobar la configuración y cierre **Configuración avanzada**.

    5. En el menú **Restaurar**, haga clic en **Restaurar** para iniciar el trabajo de restauración. En el área de notificaciones, puede seguir el progreso. También puede realizar un seguimiento del progreso de los trabajos de restauración de base de datos.

       ![menú de configuración avanzada](./media/backup-azure-sql-database/restore-job-notification.png)

3. En el menú **Seleccionar punto de restauración**, elija un punto de recuperación. Puede elegir registros (punto en el tiempo) o completo y diferencial. Si desea restaurar un registro a un momento dado, vuelva al paso 2. Este paso permite restaurar un punto de restauración completo o diferencial específico. Con esta opción, puede ver todos los puntos de recuperación completos y diferenciales de los 30 últimos días. Si desea ver puntos de recuperación anteriores a 30 días, haga clic en **Filtrar** para abrir el menú **Filtrar puntos de restauración**. Si elige un punto de recuperación diferencial, Azure Backup restaura primero el punto de recuperación completa adecuado y, a continuación, aplica el punto de recuperación diferencial seleccionado.

    ![Menú Seleccionar punto de restauración](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. En el menú **Seleccionar punto de restauración**, seleccione **Completo y diferencial**.

       ![Menú Seleccionar punto de restauración](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        Aparece la lista de puntos de recuperación disponibles.

    2. En la lista de puntos de recuperación, seleccione un punto de recuperación y haga clic en **Aceptar** para completar el procedimiento Punto de restauración. 

        ![Elija el punto de recuperación completa](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        El menú **Punto de restauración** se cierra y el menú **Configuración avanzada** se abre.

        ![menú de configuración avanzada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. En el menú **Configuración avanzada**:

        * Para mantener la base de datos no operativa después de la restauración, en el menú **Restaurar con NORECOVERY**, seleccione **Habilitado**. **Restaurar con NORECOVERY** está deshabilitado de forma predeterminada.
        * Si desea cambiar la ubicación de restauración en el servidor de destino, proporcione una nueva ruta de acceso en la columna **Destino**.
        * Haga clic en **Aceptar** para aprobar la configuración y cierre **Configuración avanzada**.

    4. En el menú **Restaurar**, haga clic en **Restaurar** para iniciar el trabajo de restauración. En el área de notificaciones, puede seguir el progreso. También puede realizar un seguimiento del progreso de los trabajos de restauración de base de datos.

       ![menú de configuración avanzada](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-and-overwrite-the-database"></a>Restauración y sobrescritura de la base de datos

Este procedimiento le guía en la restauración de datos y la sobrescritura de la base de datos. Si desea restaurar a una ubicación alternativa, vaya a la sección [Restauración a una ubicación alternativa](backup-azure-sql-database.md#restore-to-an-alternate-location). En este procedimiento se asume que tiene el almacén de Recovery Services abierto y que se encuentra en el menú **Restaurar configuración** (vea la imagen siguiente). Si no es así, empiece con la sección [Restauración de una base de datos SQL](backup-azure-sql-database.md#restore-a-sql-database).

![Haga clic en Configurar para abrir el menú Restaurar configuración](./media/backup-azure-sql-database/restore-overwrite-db.png)

El menú desplegable **Servidor** solo muestra los servidores SQL Server registrados con el almacén de Recovery Services. Si el servidor que desea no está en la lista **Servidor**, vea la sección [Detección de bases de datos SQL Server](backup-azure-sql-database.md#discover-sql-server-databases) para encontrar el servidor. Durante el proceso de detección de bases de datos, todos los servidores nuevos se registran en el almacén de Recovery Services.

1. En el menú **Restaurar configuración**, seleccione **Sobrescribir la base de datos** y haga clic en **Aceptar** para completar la configuración del destino. 

   ![haga clic en Sobrescribir la base de datos](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

    Los cuadros de diálogo **Servidor**, **Instancia** y **Nombre de la base de datos restaurada** no son necesarios.

2. En el menú **Seleccionar punto de restauración**, puede elegir un registro (punto en el tiempo) o un punto de restauración completo y diferencial. Si desea restaurar a un registro en un momento dado, continúe con este paso. Si desea restaurar un punto de restauración completo y diferencial, vaya directamente al paso 3.

    ![Menú Seleccionar punto de restauración](./media/backup-azure-sql-database/recovery-point-menu.png)

    La restauración a un momento dado solo está disponible para copias de seguridad de registros con el modelo de recuperación optimizado para cargas masivas de registros y el modelo de recuperación optimizado para cargas completas de registros. Para elegir una restauración a un momento dado a un segundo específico:

    1. Seleccione **Registro (punto en el tiempo)** como la opción de restauración.

        ![elegir tipo de punto de restauración](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. En **Fecha y hora de restauración**, haga clic en el icono del calendario para abrir el calendario. Las fechas en negrita contienen puntos de recuperación, y la fecha actual está resaltada. Seleccione una fecha en el calendario con puntos de recuperación. No puede seleccionar las fechas sin ningún punto de recuperación.

        ![Abrir calendario](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Una vez seleccionada la fecha, el gráfico de línea cronológica muestra los puntos de recuperación disponibles.

    3. Con el gráfico de línea cronológica o el cuadro de diálogo Tiempo, especifique un tiempo específico para el punto de recuperación y haga clic en **Aceptar** para completar el paso Punto de restauración.
    
       ![Abrir calendario](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        El menú **Seleccionar punto de restauración** se cierra y el menú **Configuración avanzada** se abre.

       ![menú de configuración avanzada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. En el menú **Configuración avanzada**:

        * Para mantener la base de datos no operativa después de la restauración, en el menú **Restaurar con NORECOVERY**, seleccione **Habilitado**.
        * Si desea cambiar la ubicación de restauración en el servidor de destino, proporcione una nueva ruta de acceso en la columna **Destino**.
        * Haga clic en **Aceptar** para aprobar la configuración y cierre **Configuración avanzada**.

    5. En el menú **Restaurar**, haga clic en **Restaurar** para iniciar el trabajo de restauración. En el área de notificaciones, puede seguir el progreso. También puede realizar un seguimiento del progreso de los trabajos de restauración de base de datos.

       ![menú de configuración avanzada](./media/backup-azure-sql-database/restore-job-notification.png)

3. En el menú **Seleccionar punto de restauración**, elija un punto de recuperación. Puede elegir registros (punto en el tiempo) o completo y diferencial. Si desea restaurar un registro a un momento dado, vuelva al paso 2. Este paso permite restaurar un punto de restauración completo o diferencial específico. Con esta opción, puede ver todos los puntos de recuperación completos y diferenciales de los 30 últimos días. Si desea ver puntos de recuperación anteriores a 30 días, haga clic en **Filtrar** para abrir el menú **Filtrar puntos de restauración**. Si elige un punto de recuperación diferencial, Azure Backup restaura primero el punto de recuperación completa adecuado y, a continuación, aplica el punto de recuperación diferencial seleccionado.

    ![Menú Seleccionar punto de restauración](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. En el menú **Seleccionar punto de restauración**, seleccione **Completo y diferencial**.

       ![Menú Seleccionar punto de restauración](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        Aparece la lista de puntos de recuperación disponibles.

    2. En la lista de puntos de recuperación, seleccione un punto de recuperación y haga clic en **Aceptar** para completar el procedimiento Punto de restauración. 

        ![Elija el punto de recuperación completa](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        El menú **Punto de restauración** se cierra y el menú **Configuración avanzada** se abre.

        ![menú de configuración avanzada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. En el menú **Configuración avanzada**:

        * Para mantener la base de datos no operativa después de la restauración, en el menú **Restaurar con NORECOVERY**, seleccione **Habilitado**. **Restaurar con NORECOVERY** está deshabilitado de forma predeterminada.
        * Si desea cambiar la ubicación de restauración en el servidor de destino, proporcione una nueva ruta de acceso en la columna **Destino**.
        * Haga clic en **Aceptar** para aprobar la configuración y cierre **Configuración avanzada**.

    4. En el menú **Restaurar**, haga clic en **Restaurar** para iniciar el trabajo de restauración. En el área de notificaciones, puede seguir el progreso. También puede realizar un seguimiento del progreso de los trabajos de restauración de base de datos.

       ![menú de configuración avanzada](./media/backup-azure-sql-database/restore-job-notification.png)


## <a name="manage-azure-backup-operations-for-sql-on-azure-vms"></a>Administración de operaciones de Azure Backup para SQL en máquinas virtuales de Azure

En esta sección se ofrece información sobre las diversas operaciones de administración de Azure Backup disponibles para SQL en máquinas virtuales de Azure. Existen las siguientes operaciones de alto nivel:

* Supervisión de trabajos
* Alertas de copias de seguridad
* Detención de la protección en una base de datos SQL
* Reanudación de la protección en una base de datos SQL
* Activación de un trabajo de copia de seguridad ad hoc
* Anulación del registro de un servidor SQL

### <a name="monitor-jobs"></a>Supervisión de trabajos

Azure Backup utiliza API nativas de SQL para todas las operaciones de copia de seguridad. Con las API nativas, puede capturar toda la información de la [tabla del conjunto de copias de seguridad de SQL](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) en la base de datos msdb. Además, Azure Backup muestra todos los trabajos desencadenados manualmente o ad hoc en el portal Trabajos de copia de seguridad. Los trabajos disponibles en el portal incluyen: todas las operaciones de configuración de la copia de seguridad, las operaciones de restauración, las operaciones de registro y detección de bases de datos y las operaciones de detención de copia de seguridad. Todos los trabajos programados también se pueden supervisar con Log Analytics de OMS. Con el uso de Log Analytics se elimina el desorden de los trabajos y, además, se ofrece una flexibilidad granular para supervisar o filtrar trabajos específicos.

![menú de configuración avanzada](./media/backup-azure-sql-database/jobs-list.png)

### <a name="backup-alerts"></a>Alertas de copias de seguridad

Con copias de seguridad de registros con una frecuencia de 15 minutos, en ocasiones, la supervisión de los trabajos de copia de seguridad puede resultar tediosa. Azure Backup ha previsto para esta posible situación tediosa el envío de alertas por correo electrónico desencadenadas por cualquier error de copia de seguridad. Las alertas se consolidan en el nivel de base de datos por código de error. Por ejemplo, si una base de datos tiene varios errores de copia de seguridad, en lugar de recibir una alerta por cada error, recibirá un correo electrónico del primer error. Puede iniciar sesión en Azure Portal para supervisar los errores posteriores de dicha base de datos. 

Para supervisar las alertas de copia de seguridad:

1. Inicie sesión en su suscripción a Azure en [Azure Portal](https://portal.azure.com).

2. Abra el almacén de Recovery Services registrado con la máquina virtual SQL.

3. En el menú del almacén de Recovery Services, seleccione **Alertas y eventos**. 

   ![menú de configuración avanzada](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. En el menú **Alertas y eventos**, seleccione **Alertas de copias de seguridad** para ver la lista de alertas.

   ![menú de configuración avanzada](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

### <a name="stop-protection-on-a-sql-server-database"></a>Detención de la protección en una base de datos SQL Server

Si se detiene la protección de una base de datos SQL Server, Azure Backup pregunta si desea retener los punto de recuperación. Hay dos formas de dejar de proteger la base de datos SQL:

* Detener todos los trabajos futuros de copia de seguridad y eliminar todos los puntos de recuperación.
* Detener todos los trabajos futuros de copia de seguridad pero dejar los puntos de recuperación. 

Dejar los punto de recuperación conlleva un costo, ya que a los puntos de recuperación para SQL se les aplica el cargo por los precios de instancias protegidas de SQL, además del almacenamiento consumido. Para más información sobre los precios de Azure Backup para SQL, vea la [página de precios de Azure Backup](https://azure.microsoft.com/pricing/details/backup/). Para detener la protección de la base de datos:

1. Abra el almacén de Recovery Services registrado con la máquina virtual SQL.

2. En el panel del almacén, seleccione los elementos de copia de seguridad de **uso** para abrir el menú Elementos de copia de seguridad.

    ![Haga clic en + Copia de seguridad para abrir el menú Objetivo de Backup.](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. En el menú **Elementos de copia de seguridad**, seleccione el tipo de administración de copia de seguridad **SQL en máquina virtual de Azure**. 

    ![Haga clic en + Copia de seguridad para abrir el menú Objetivo de Backup.](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    La lista de elementos de copia de seguridad se ajusta para mostrar la lista de base de datos SQL. 

4. En la lista de bases de datos SQL, seleccione la base de datos que desea dejar de proteger.

    ![Seleccionar SQL en la máquina virtual de Azure en la lista](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Al seleccionar la base de datos, se abre el menú. 

5. En el menú de la base de datos seleccionada, haga clic en **Detener copia de seguridad** para dejar de proteger la base de datos.

    ![seleccionar restaurar base de datos](./media/backup-azure-sql-database/stop-db-button.png)

    Se abre el menú **Detener copia de seguridad**.

6. En el menú **Detener copia de seguridad**, elija Retener datos de copia de seguridad o Eliminar datos de la copia de seguridad. Si lo desea, puede indicar un motivo para detener la protección y proporcionar un comentario.

    ![seleccionar restaurar base de datos](./media/backup-azure-sql-database/stop-backup-button.png)

7. Haga clic en **Detener copia de seguridad** para dejar de proteger la base de datos. 

### <a name="resume-protection-for-a-sql-database"></a>Reanudación de la protección en una base de datos SQL

Si seleccionó la opción **Retener datos de copia de seguridad** al detener la protección de la base de datos SQL, se puede reanudar la protección. Si no se retuvieron los datos de la copia de seguridad, la protección no se puede reanudar. 

1. Para reanudar la protección de la base de datos SQL, abra el elemento de copia de seguridad y haga clic en **Reanudar copia de seguridad**.

    ![reanudar la protección de la base de datos](./media/backup-azure-sql-database/resume-backup-button.png)

   Se abre el menú Directiva de copia de seguridad.

2. En el menú **Directiva de copia de seguridad**, seleccione una directiva y haga clic en **Guardar**.

### <a name="trigger-an-adhoc-backup"></a>Activación de una copia de seguridad ad hoc

Puede desencadenar una copia de seguridad ad hoc siempre que lo desee. Hay cuatro tipos de copias de seguridad ad hoc. Para obtener información detallada, vea el artículo [Tipos de copias de seguridad de SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/backup-overview-sql-server?view=sql-server-2017#types-of-backups).

* Copia de seguridad completa
* Copiar solo copia de seguridad completa
* Copia de seguridad diferencial
* Copia de seguridad de registros

### <a name="unregister-a-sql-server"></a>Anulación del registro de un servidor SQL

Para anular el registro de un servidor SQL después de quitar la protección, pero antes de eliminar el almacén

1. Abra el almacén de Recovery Services registrado con la máquina virtual SQL.

2. En la sección **Administrar** del menú del almacén, haga clic en **Infraestructura de copia de seguridad**.  

   ![reanudar la protección de la base de datos](./media/backup-azure-sql-database/backup-infrastructure-button.png)

3. En la sección **Servidores de administración**, haga clic en **Servidores protegidos**.

   ![reanudar la protección de la base de datos](./media/backup-azure-sql-database/protected-servers.png)

    Se abre el menú Servidores protegidos. 

4. En el menú **Servidores protegidos**, seleccione el servidor del que desea anular el registro. Si desea eliminar el almacén, debe anular el registro de todos los servidores.

5. En el menú Servidores protegidos, haga clic con el botón derecho en el servidor protegido y seleccione **Eliminar**. 

   ![reanudar la protección de la base de datos](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de Azure Backup, consulte el ejemplo de PowerShell sobre la copia de seguridad de máquinas virtuales cifradas.

> [!div class="nextstepaction"]
> [Copia de seguridad de máquinas virtuales cifradas](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
