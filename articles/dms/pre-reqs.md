---
title: Información general sobre los requisitos previos para usar Azure Database Migration Service | Microsoft Docs
description: Obtenga información general sobre los requisitos previos para usar Azure Database Migration Service para realizar migraciones de bases de datos.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 05/29/2019
ms.openlocfilehash: 4e21014f7b4ed86846a100ed9a2b1cd4b0400974
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66304256"
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Información general sobre los requisitos previos para usar Azure Database Migration Service

Hay varios requisitos previos necesarios para asegurarse de que Azure Database Migration Service se ejecuta sin problemas al realizar migraciones de base de datos. Algunos de los requisitos previos se aplican en todos los escenarios (pares origen-destino) compatibles con el servicio, mientras que otros son exclusivos para un escenario específico.

Los requisitos previos asociados con el uso de Azure Database Migration Service se muestran en las secciones siguientes.

## <a name="prerequisites-common-across-migration-scenarios"></a>Requisitos previos comunes en los distintos escenarios de migración

Los requisitos de Azure Database Migration Service que son comunes en todos los escenarios de migración compatibles incluyen la necesidad de:

* Cree una red virtual de Azure para Azure Database Migration Service usando el modelo de implementación de Azure Resource Manager, que proporciona conectividad de sitio a sitio a los servidores de origen locales mediante [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) o [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
* Asegúrese de que las reglas del grupo de seguridad de red (NSG) de red virtual no bloquean los siguientes puertos de comunicación 443, 53, 9354, 445 y 12000. Para más información sobre el filtrado del tráfico con grupos de seguridad de red para redes virtuales de Azure, consulte el artículo [Planeamiento de redes virtuales](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
* Cuando se usa un dispositivo de firewall frente a las bases de datos de origen, puede que sea necesario agregar reglas de firewall para permitir que Azure Database Migration Service acceda a las bases de datos de origen para realizar la migración.
* Configurar su [Firewall de Windows para acceder al motor de base de datos](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Habilitar el protocolo TCP/IP, que se deshabilita de forma predeterminada durante la instalación de SQL Server Express, siguiendo las instrucciones del artículo [Habilitar o deshabilitar un protocolo de red de servidor](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).

    > [!IMPORTANT]
    > Creación de una instancia de Azure Database Migration Service requiere acceso a la configuración de red virtual que normalmente no están dentro del mismo grupo de recursos. Como resultado, el usuario que crea una instancia de DMS requiere el permiso en el nivel de suscripción. Para crear los roles necesarios, que puede asignar según sea necesario, ejecute el siguiente script:
    >
    > ```
    >
    > $readerActions = `
    > "Microsoft.DataMigration/services/*/read", `
    > "Microsoft.Network/networkInterfaces/ipConfigurations/read"
    >
    > $writerActions = `
    > "Microsoft.DataMigration/services/*/write", `
    > "Microsoft.DataMigration/services/*/delete", `
    > "Microsoft.DataMigration/services/*/action"
    >
    > $writerActions += $readerActions
    >
    > # TODO: replace with actual subscription IDs
    > $subScopes = ,"/subscriptions/00000000-0000-0000-0000-000000000000/","/subscriptions/11111111-1111-1111-1111-111111111111/"
    >
    > function New-DmsReaderRole() {
    > $aRole = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
    > $aRole.Name = "Azure Database Migration Reader"
    > $aRole.Description = "Lets you perform read only actions on DMS service/project/tasks."
    > $aRole.IsCustom = $true
    > $aRole.Actions = $readerActions
    > $aRole.NotActions = @()
    >
    > $aRole.AssignableScopes = $subScopes
    > #Create the role
    > New-AzRoleDefinition -Role $aRole
    > }
    >
    > function New-DmsContributorRole() {
    > $aRole = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
    > $aRole.Name = "Azure Database Migration Contributor"
    > $aRole.Description = "Lets you perform CRUD actions on DMS service/project/tasks."
    > $aRole.IsCustom = $true
    > $aRole.Actions = $writerActions
    > $aRole.NotActions = @()
    >
    >   $aRole.AssignableScopes = $subScopes
    > #Create the role
    > New-AzRoleDefinition -Role $aRole
    > }
    > 
    > function Update-DmsReaderRole() {
    > $aRole = Get-AzRoleDefinition "Azure Database Migration Reader"
    > $aRole.Actions = $readerActions
    > $aRole.NotActions = @()
    > Set-AzRoleDefinition -Role $aRole
    > }
    >
    > function Update-DmsConributorRole() {
    > $aRole = Get-AzRoleDefinition "Azure Database Migration Contributor"
    > $aRole.Actions = $writerActions
    > $aRole.NotActions = @()
    > Set-AzRoleDefinition -Role $aRole
    > }
    >
    > # Invoke above functions
    > New-DmsReaderRole
    > New-DmsContributorRole
    > Update-DmsReaderRole
    > Update-DmsConributorRole
    > ```

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>Requisitos previos para migrar SQL Server a Azure SQL Database

Además de los requisitos previos de Azure Database Migration Service que son comunes para todos los escenarios de migración, también hay requisitos previos que se aplican específicamente a un escenario o a otro.

Cuando se usa Azure Database Migration Service para realizar migraciones de SQL Server a Azure SQL Database, además de los requisitos previos comunes a todos los escenarios de migración, asegúrese de cumplir con estos requisitos previos adicionales:

* Crear una base de datos de Azure SQL siguiendo la información del artículo sobre cómo [crear una base de datos de Azure SQL en Azure Portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
* Descargar e instalar [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) versión 3.3 o posterior.
* Abra el Firewall de Windows para permitir que Azure Database Migration Service acceda a la instancia de SQL Server de origen que, de manera predeterminada, es el puerto TCP 1433.
* Si se ejecutan varias instancias con nombre de SQL Server con puertos dinámicos, puede que quiera habilitar el servicio SQL Browser y permitir el acceso al puerto UDP 1434 a través de los firewalls para que Azure Database Migration Service pueda conectarse a una instancia con nombre en el servidor de origen.
* Crear una [regla de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) de nivel de servidor para que el servidor de Azure SQL Database permita a Azure Database Migration Service acceder a las bases de datos de destino. Proporcionar el intervalo de subred de la red virtual usada para Azure Database Migration Service.
* Asegurarse de que las credenciales usadas para conectarse a la instancia de SQL Server de origen tenga permisos [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql).
* Asegurarse de que las credenciales usadas para conectarse a la base de datos de Azure SQL de destino tengan permisos CONTROL DATABASE en las bases de datos de Azure SQL de destino.

   > [!NOTE]
   > Para una lista completa de los requisitos previos necesarios para usar Azure Database Migration Service para realizar migraciones de SQL Server a Azure SQL Database, consulte el tutorial [Migración de SQL Server a Azure SQL Database](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql).
   > 

## <a name="prerequisites-for-migrating-sql-server-to-an-azure-sql-database-managed-instance"></a>Requisitos previos para migrar SQL Server en una instancia administrada de Azure SQL Database

* Crear una instancia administrada de Azure SQL Database siguiendo la información en el artículo [crear una instancia administrada de Azure SQL Database en Azure portal](https://aka.ms/sqldbmi).
* Abra los firewalls para permitir el tráfico de SMB en el puerto 445 en el intervalo de direcciones IP o el intervalo de subred de Azure Database Migration Service.
* Abra el Firewall de Windows para permitir que Azure Database Migration Service acceda a la instancia de SQL Server de origen que, de manera predeterminada, es el puerto TCP 1433.
* Si se ejecutan varias instancias con nombre de SQL Server con puertos dinámicos, puede que quiera habilitar el servicio SQL Browser y permitir el acceso al puerto UDP 1434 a través de los firewalls para que Azure Database Migration Service pueda conectarse a una instancia con nombre en el servidor de origen.
* Asegúrese de que los inicios de sesión usados para conectar la instancia de SQL Server de origen y la instancia administrada de destino son miembros del rol de servidor sysadmin.
* Cree un recurso compartido de red que pueda usar Azure Database Migration Service para hacer copia de seguridad de la base de datos de origen.
* Asegúrese de que la cuenta de servicio que ejecuta la instancia de SQL Server de origen tiene privilegios de escritura en el recurso compartido de red que haya creado, y que la cuenta del equipo del servidor de origen tiene acceso de lectura y escritura para el mismo recurso compartido.
* Anote un usuario de Windows (y su contraseña) que tenga privilegios de control total sobre el recurso compartido de red que creó anteriormente. Azure Database Migration Service suplanta la credencial de usuario para cargar los archivos de copia de seguridad en el contenedor de Azure Storage para la operación de restauración.
* Cree un contenedor de blobs y recupere su URI de SAS mediante los pasos del artículo [Administración de recursos Azure Blob Storage con el Explorador de Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container). Asegúrese de seleccionar todos los permisos (lectura, escritura, eliminación, lista) en la ventana de directiva al crear el URI de SAS.

   > [!NOTE]
   > Para acceder a una lista completa de los requisitos previos necesarios para usar Azure Database Migration Service para realizar migraciones de SQL Server a Azure SQL Database, consulte el tutorial [Migración de SQL Server a la Instancia administrada de Azure SQL Database](https://aka.ms/migratetomiusingdms).

## <a name="next-steps"></a>Pasos siguientes

Para información general sobre Azure Database Migration Service y la disponibilidad regional, consulte el artículo [¿Qué es la versión preliminar de Azure Database Migration Service?](dms-overview.md)
