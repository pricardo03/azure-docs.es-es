---
title: Creación y administración de reglas de firewall de Azure Database for MySQL mediante la CLI de Azure
description: En este artículo se describe cómo crear y administrar reglas de firewall de Azure Database for MySQL mediante la línea de comandos de la CLI de Azure.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 04/09/2018
ms.openlocfilehash: dca7d09a5358f5e8b4025dc5e35e4465e21d77a2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "61458474"
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-cli"></a>Creación y administración de reglas de firewall de Azure Database for MySQL mediante la CLI de Azure
Las reglas de firewall de nivel de servidor pueden usarse para administrar el acceso a una base de datos de Azure Database for MySQL desde una dirección IP específica o un intervalo de direcciones IP. Con los comandos de la CLI de Azure adecuados, puede crear, actualizar, eliminar, enumerar y mostrar reglas de firewall para administrar el servidor. Para obtener información general de la base de datos de Azure para MySQL firewalls, consulte [-Azure Database for MySQL reglas de firewall de servidor](./concepts-firewall-rules.md).

Reglas virtuales Network (VNet) también pueden usarse para proteger el acceso a su servidor. Obtenga más información sobre [reglas mediante la CLI de Azure y los puntos de conexión de servicio creación y administración de red Virtual](howto-manage-vnet-using-cli.md).

## <a name="prerequisites"></a>Requisitos previos
* [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).
* Un [servidor de Azure Database for MySQL y una base de datos](quickstart-create-mysql-server-database-using-azure-cli.md).

## <a name="firewall-rule-commands"></a>Comandos de reglas de firewall:
El comando **az mysql server firewall-rule** se utiliza desde la CLI de Azure para crear, eliminar, enumerar, mostrar y actualizar reglas de firewall.

Comandos:
- **create**: cree una regla de firewall de servidor de Azure para MySQL.
- **delete**: elimine una regla de firewall de servidor de Azure para MySQL.
- **list**: enumere las reglas de firewall de servidor de Azure para MySQL.
- **show**: muestre los detalles de una regla de firewall de servidor de Azure para MySQL.
- **update**: actualice una regla de firewall de servidor de Azure para MySQL.

## <a name="sign-in-to-azure-and-list-your-azure-database-for-mysql-servers"></a>Inicie sesión en Azure y de lista de la base de datos de Azure para servidores de MySQL
Conéctese de forma segura a la CLI de Azure con su cuenta de Azure mediante el comando **az login**.

1. En la línea de comandos, ejecute el siguiente comando:
    ```azurecli
    az login
    ```
   Este comando generará un código que se usará en el paso siguiente.

2. Use un explorador web para abrir la página [https://aka.ms/devicelogin](https://aka.ms/devicelogin) y escriba el código.

3. En el símbolo del sistema, inicie sesión con sus credenciales de Azure.

4. Después de que se autorice el inicio de sesión, se imprimirá una lista de suscripciones en la consola. Copie el identificador de la suscripción deseada para establecer la suscripción actual que se va a usar. Use el comando [az account set](/cli/azure/account#az-account-set).
    ```azurecli-interactive
    az account set --subscription <your subscription id>
    ```

5. Si no está seguro de los nombres, muestre los servidores de Azure Database for MySQL para su suscripción y grupo de recursos. Use el comando [az mysql server list](/cli/azure/mysql/server#az-mysql-server-list).

    ```azurecli-interactive
    az mysql server list --resource-group myresourcegroup
    ```

   Anote el valor del atributo name de la lista, que se necesitará para especificar con qué servidor MySQL desea trabajar. Si es necesario, confirme los detalles de dicho servidor con el atributo name para confirmar que el nombre es correcto. Use el comando [az mysql server show](/cli/azure/mysql/server#az-mysql-server-show).

    ```azurecli-interactive
    az mysql server show --resource-group myresourcegroup --name mydemoserver
    ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>Lista de reglas de firewall en el servidor de Azure Database for MySQL 
Utilizando el nombre del servidor y el nombre del grupo de recursos, enumere las reglas de firewall existentes en el servidor. Use el comando [az mysql server firewall list](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-list).  Tenga en cuenta que el atributo de nombre de servidor se especifica en el modificador **--server** y no en el modificador **--name**. 
```azurecli-interactive
az mysql server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
La salida enumera las reglas existentes, si existen, en formato JSON (de forma predeterminada). Puede usar el modificador **--output table** para obtener los resultados en un formato de la tabla más legible.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-a-firewall-rule-on-azure-database-for-mysql-server"></a>Creación de una regla de firewall en el servidor de Azure Database for MySQL
Con el nombre del servidor de Azure MySQL y el nombre del grupo de recursos, cree una nueva regla de firewall en el servidor. Use el comando [az mysql server firewall create](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-create). Dé un nombre a la regla y a las direcciones IP inicial y final (para proporcionar acceso a un intervalo de direcciones IP) para la regla.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```

Para permitir el acceso a una única dirección IP, proporcione la misma dirección IP como la dirección IP inicial y final, como en este ejemplo.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```

Para permitir que las aplicaciones de las direcciones IP de Azure se conecten al servidor Azure Database for MySQL, proporcione la dirección IP 0.0.0.0 como IP inicial e IP final, como en este ejemplo.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server mysql --name "AllowAllWindowsAzureIps" --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Esta opción configura el firewall para permitir todas las conexiones de Azure, incluidas las de las suscripciones de otros clientes. Al seleccionar esta opción, asegúrese de que los permisos de usuario y el inicio de sesión limiten el acceso solamente a los usuarios autorizados.
> 

Si se realiza correctamente, en la salida de cada comando create se mostrarán los detalles de la regla de firewall que ha creado, en formato JSON (de forma predeterminada). Si se produce un error, la salida muestra el texto del mensaje de error.

## <a name="update-a-firewall-rule-on-azure-database-for-mysql-server"></a>Actualización de una regla de firewall en el servidor de Azure Database for MySQL 
Con el nombre del servidor de Azure MySQL y el nombre del grupo de recursos, actualice una regla de firewall existente en el servidor. Use el comando [az mysql server firewall update](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-update). Proporcione el nombre de una regla de firewall existente como entrada, así como los atributos de dirección IP inicial y final que se van a actualizar.
```azurecli-interactive
az mysql server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Cuando se realiza correctamente, la salida del comando muestra los detalles de la regla de firewall que ha actualizado, de forma predeterminada en formato JSON. Si se produce un error, la salida muestra el texto del mensaje de error.

> [!NOTE]
> Si la regla de firewall no existe, el comando update la crea.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>Presentación de los detalles de las reglas de firewall en el servidor de Azure Database for MySQL
Con el nombre del servidor de Azure MySQL y el nombre del grupo de recursos, muestre los detalles de la regla de firewall existente en el servidor. Use el comando [az mysql server firewall show](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-show). Proporcione como entrada el nombre de una regla de firewall existente.
```azurecli-interactive
az mysql server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
Cuando se realiza correctamente, la salida del comando muestra los detalles de la regla de firewall que ha especificado, en formato JSON (de forma predeterminada). Si se produce un error, la salida muestra el texto del mensaje de error.

## <a name="delete-a-firewall-rule-on-azure-database-for-mysql-server"></a>Eliminación de una regla de firewall en el servidor de Azure Database for MySQL
Con el nombre del servidor de Azure MySQL y el nombre del grupo de recursos, quite una regla de firewall existente del servidor. Use el comando [az mysql server firewall delete](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-delete). Proporcione el nombre de una regla de firewall existente.
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
Cuando se realiza correctamente, no hay ninguna salida. En caso de error, se muestra el texto del mensaje de error.

## <a name="next-steps"></a>Pasos siguientes
- Para más información, consulte [Reglas de firewall del servidor de Azure Database para MySQL](./concepts-firewall-rules.md).
- [Creación y administración de reglas de firewall de Azure Database for MySQL mediante Azure Portal](./howto-manage-firewall-using-portal.md).
- Proteger aún más el acceso a su servidor [reglas mediante la CLI de Azure y los puntos de conexión de servicio creación y administración de red Virtual](howto-manage-vnet-using-cli.md).