---
title: Creación y administración de reglas y puntos de conexión de servicio de red virtual de Azure Database for PostgreSQL mediante la CLI de Azure | Microsoft Docs
description: En este artículo se describe cómo crear y administrar reglas y puntos de conexión de servicio de red virtual de Azure Database for PostgreSQL mediante la línea de comandos de la CLI de Azure.
services: postgresql
author: mbolz
ms.author: mbolz
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 08/15/2018
ms.openlocfilehash: 6f69c412aa7322326205e32b5df35aed2faa4649
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971525"
---
# <a name="create-and-manage-azure-database-for-postgresql-vnet-service-endpoints-using-azure-cli"></a>Creación y administración de puntos de conexión de servicio de red virtual de Azure Database for PostgreSQL mediante la CLI de Azure
Las reglas y los puntos de conexión de servicios de red virtual (VNet) amplían el espacio de direcciones privadas de una red virtual al servidor de Azure Database for PostgreSQL. Con los comandos de la interfaz de la línea de comandos (CLI) de Azure adecuados, puede crear, actualizar, eliminar, enumerar y mostrar reglas y puntos de conexión de servicio de red virtual para administrar el servidor. Para obtener información general sobre los puntos de conexión de servicio de red virtual de Azure Database for PostgreSQL, incluidas las limitaciones, consulte [Azure Database for PostgreSQL Server VNet service endpoints](concepts-data-access-and-security-vnet.md) (Puntos de conexión de servicio de red virtual del servidor de Azure Database for PostgreSQL). Los puntos de conexión de servicio de red virtual están disponibles en todas las regiones admitidas para Azure Database for PostgreSQL.

## <a name="prerequisites"></a>Requisitos previos
Para seguir esta guía, necesitará:
- Instale [la CLI de Azure](/cli/azure/install-azure-cli) o use Azure Cloud Shell en el explorador.
- Un [servidor y una base de datos de Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md).

> [!NOTE]
> La compatibilidad con puntos de conexión de servicio de red virtual solo existe para servidores de uso general y optimizados para memoria.

## <a name="configure-vnet-service-endpoints-for-azure-database-for-postgresql"></a>Configuración de puntos de conexión de servicio de red virtual para Azure Database for PostgreSQL
Los comandos [az network vnet](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest) se usan para configurar redes virtuales.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0 o posterior de la CLI de Azure. Para ver la versión instalada, ejecute el comando `az --version`. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

Si ejecuta localmente la CLI, debe iniciar sesión en su cuenta mediante el comando [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest). Anote la propiedad **id** de la salida del comando para el nombre de la suscripción correspondiente.
```azurecli-interactive
az login
```

Si tiene varias suscripciones, elija la suscripción adecuada en la que se debe facturar el recurso. Seleccione el identificador de suscripción específico en su cuenta mediante el comando [az account set](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-set). Sustituya la propiedad **id** de la salida **az login** para su suscripción en el marcador de posición de id. de suscripción.

- La cuenta debe tener todos los permisos necesarios para crear una red virtual y un punto de conexión de servicio.

Los puntos de conexión de servicio pueden configurarse en redes virtuales de forma independiente por un usuario con acceso de escritura a la red virtual.

Para proteger los recursos de servicio de Azure a una red virtual, el usuario debe tener permiso "Microsoft.Network/JoinServicetoaSubnet" para las subredes que se van a agregar. De forma predeterminada, este permiso se incluye en los roles de administrador de servicios integrado y puede modificarse mediante la creación de roles personalizados.

Obtenga más información sobre los [roles integrados](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) y la asignación de permisos específicos a [roles personalizados](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).

Las redes virtuales y los recursos de servicio de Azure pueden encontrarse en la misma o en diferentes suscripciones. Si los recursos de servicio de Azure y de red virtual se encuentran en distintas suscripciones, los recursos deben estar en el mismo inquilino de Active Directory (AD).

> [!IMPORTANT]
> Se recomienda encarecidamente que lea este artículo sobre las configuraciones y las consideraciones de los puntos de conexión de servicio antes de ejecutar el script de ejemplo siguiente, o configurar los puntos de conexión de servicio. **Punto de conexión de servicio de Virtual Network:** un [punto de conexión de servicio de Virtual Network](../virtual-network/virtual-network-service-endpoints-overview.md) es una subred cuyos valores de propiedad incluyen uno o más nombres formales de tipo de servicio de Azure. Los puntos de conexión de servicio de red virtual usan el nombre de tipo de servicio **Microsoft.Sql**, que hace referencia al servicio de Azure denominado SQL Database. Esta etiqueta de servicio también se aplica a los servicios Azure SQL Database, Azure Database for PostgreSQL y MySQL. Es importante tener en cuenta que, al aplicar la etiqueta de servicio **Microsoft.Sql** a un punto de conexión de servicio de red virtual, se configura el tráfico de punto de conexión de servicio de todos los servicios de Azure Database, incluidos los servidores de Azure SQL Database, Azure Database for PostgreSQL y Azure Database for MySQL de la subred. 
> 

### <a name="sample-script-to-create-an-azure-database-for-postgresql-database-create-a-vnet-vnet-service-endpoint-and-secure-the-server-to-the-subnet-with-a-vnet-rule"></a>Script de ejemplo para crear una base de datos de Azure Database for PostgreSQL, crear una red virtual, un punto de conexión de servicio de red virtual y proteger el servidor a la subred con una regla de red virtual
En este script de ejemplo, cambie las líneas resaltadas para personalizar el nombre de usuario de administrador y la contraseña. Reemplace el identificador de suscripción usado en el comando `az account set --subscription` por su propio identificador de suscripción.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/create-postgresql-server.sh?highlight=5,20 "Create an Azure Database for PostgreSQL, VNet, VNet service endpoint, and VNet rule.")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación
Después de ejecutar el script de ejemplo, se puede usar el comando siguiente para quitar el grupo de recursos y todos los recursos asociados.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/delete-postgresql.sh "Delete the resource group.")]
