---
title: Crear y administrar las reglas y puntos de conexión del servicio de red virtual de Azure Database for MariaDB mediante Azure Portal | Microsoft Docs
description: Crear y administrar las reglas y puntos de conexión del servicio de red virtual de Azure Database for MariaDB mediante Azure Portal
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 11/19/2018
ms.openlocfilehash: 77602898df7d783367ffd78328082c3edcaac83e
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53542346"
---
# <a name="create-and-manage-azure-database-for-mariadb-vnet-service-endpoints-and-vnet-rules-by-using-the-azure-portal"></a>Crear y administrar las reglas y puntos de conexión del servicio de red virtual de Azure Database for MariaDB mediante Azure Portal

> [!IMPORTANT]
> La característica de puntos de conexión del servicio de red virtual está en su versión preliminar pública.

Las reglas y los puntos de conexión de los servicios de red virtual (VNet) amplían el espacio de direcciones privadas de una red virtual al servidor de Azure Database for MariaDB. Para obtener información general sobre los puntos de conexión de servicio de red virtual de Azure Database for MariaDB, incluidas las limitaciones, consulte [Puntos de conexión de servicio de red virtual del servidor de Azure Database for MariaDB](concepts-data-access-security-vnet.md). Los puntos de conexión del servicio de red virtual están disponibles en todas las regiones admitidas para Azure Database for MariaDB.

> [!NOTE]
> La compatibilidad con puntos de conexión de servicio de red virtual solo existe para servidores de uso general y optimizados para memoria.

## <a name="create-a-vnet-rule-and-enable-service-endpoints"></a>Crear una regla de red virtual y habilitar los puntos de conexión de servicio

1. En la página del servidor de MariaDB, en el encabezado Configuración, haga clic en **Seguridad de conexión** para abrir la página de seguridad de conexión de Azure Database for MariaDB. A continuación, haga clic en **+ Agregar una red virtual existente**. Si no tiene ninguna red virtual, puede hacer clic en **+ Crear nueva red virtual** para crear una. Consulte [Quickstart: Creación de una red virtual mediante Azure Portal](../virtual-network/quick-create-portal.md)

   ![Azure Portal: haga clic en Seguridad de conexión](./media/howto-manage-vnet-portal/1-connection-security.png)

2. Escriba un nombre de regla de red virtual, seleccione la suscripción, el nombre de la subred y red virtual y, luego, haga clic en **Habilitar**. Esto habilita automáticamente puntos de conexión de servicio de red virtual en la subred mediante la etiqueta de servicio **Microsoft.SQL**.

   ![Azure Portal: configuración de red virtual](./media/howto-manage-vnet-portal/2-configure-vnet.png)

   La cuenta debe tener todos los permisos necesarios para crear una red virtual y un punto de conexión de servicio.

   Los puntos de conexión de servicio pueden configurarse en redes virtuales de forma independiente por un usuario con acceso de escritura a la red virtual.
    
   Para proteger los recursos de servicio de Azure en una red virtual, el usuario debe tener permiso en "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/" para las subredes que se agregan. De forma predeterminada, este permiso se incluye en los roles de administrador de servicios integrado y puede modificarse mediante la creación de roles personalizados.
    
   Obtenga más información sobre los [roles integrados](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) y la asignación de permisos específicos a [roles personalizados](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).
    
   Las redes virtuales y los recursos de servicio de Azure pueden encontrarse en la misma o en diferentes suscripciones. Si los recursos de servicio de Azure y de red virtual se encuentran en distintas suscripciones, los recursos deben estar en el mismo inquilino de Active Directory (AD).

   > [!IMPORTANT]
   > Se recomienda encarecidamente leer este artículo sobre las configuraciones y las consideraciones de puntos de conexión de servicio antes de configurarlos. **Punto de conexión de servicio de red virtual:** un [punto de conexión de servicio de Virtual Network](../virtual-network/virtual-network-service-endpoints-overview.md) es una subred cuyos valores de propiedad incluyen uno o más nombres formales de tipo de servicio de Azure. Los puntos de conexión de servicio de red virtual usan el nombre de tipo de servicio **Microsoft.Sql**, que hace referencia al servicio de Azure denominado SQL Database. Esta etiqueta de servicio también se aplica a los servicios Azure SQL Database, Azure Database for MariaDB, PostgreSQL y MySQL. Es importante tener en cuenta que, al aplicar la etiqueta de servicio **Microsoft.Sql** a un punto de conexión del servicio de red virtual, se configura el tráfico del punto de conexión de servicio de todos los servicios de Azure Database, incluidos los servidores de Azure SQL Database, Azure Database for PostgreSQL, Azure Database for MariaDB y Azure Database for MySQL de la subred.
   > 

3. Una vez habilitada, haga clic en **Aceptar** y verá que se habilitan los puntos de conexión de servicio de red virtual junto con una regla de red virtual.

   ![Se han habilitado los puntos de conexión de servicio de red virtual y se ha creado la regla de red virtual](./media/howto-manage-vnet-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre [configurar SSL en Azure Database for MariaDB](howto-configure-ssl.md).
- De forma similar, puede crear scripts para [habilitar puntos de conexión de servicio de red virtual y crear una regla de red virtual para Azure Database for MariaDB mediante la CLI de Azure](howto-manage-vnet-cli.md).
