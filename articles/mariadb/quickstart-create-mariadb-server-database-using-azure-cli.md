---
title: 'Guía de inicio rápido: Creación de un servidor de Azure Database for MariaDB mediante la CLI de Azure'
description: En esta guía de inicio rápido se describe cómo usar la CLI de Azure para crear un servidor de Azure Database for MariaDB en un grupo de recursos de Azure.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 09/24/2018
ms.custom: mvc
ms.openlocfilehash: 9e87dacb80aa7fc5f5b073e631fc06ae74b3ad00
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46996611"
---
# <a name="create-an-azure-database-for-mariadb-server-using-azure-cli"></a>Creación de un servidor de Azure Database for MariaDB mediante la CLI de Azure
En esta guía de inicio rápido se describe cómo usar la CLI de Azure para crear un servidor de Azure Database for MariaDB en un grupo de recursos de Azure en unos cinco minutos. La CLI de Azure se usa para crear y administrar recursos de Azure desde la línea de comandos o en scripts.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0]( /cli/azure/install-azure-cli). 

Si tiene varias suscripciones, elija la adecuada donde se encuentre el recurso o para la cual se facture. Seleccione un identificador de suscripción específico en su cuenta mediante el comando [az account set](/cli/azure/account#az-account-set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos
Cree un [grupo de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) con el comando [az group create](/cli/azure/group#az-group-create). Un grupo de recursos es un contenedor lógico en el que se implementan y se administran recursos de Azure como un grupo.

En el ejemplo siguiente, se crea un grupo de recursos denominado `myresourcegroup` en la ubicación `westus`.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mariadb-server"></a>Creación de un servidor de Azure Database for MariaDB
Cree un servidor de Azure Database for MariaDB con el comando **[az mariadb server create](/cli/azure/mariadb/server#az-mariadb-server-create)**. Un servidor puede administrar varias bases de datos. Normalmente se usa una base de datos independiente para cada proyecto o para cada usuario.

**Configuración** | **Valor de ejemplo** | **Descripción**
---|---|---
name | mydemoserver | Elija un nombre único que identifique el servidor de Azure Database for MariaDB. El nombre del servidor solo puede contener letras minúsculas, números y el carácter de guion (-). Debe contener entre 3 y 63 caracteres.
resource-group | myresourcegroup | Especifique el nombre del grupo de recursos de Azure.
sku-name | GP_Gen5_2 | El nombre de la SKU. Sigue la convención {plan de tarifa}_{generación de procesos}_{núcleos virtuales} en forma abreviada. En esta misma tabla puede obtener más información acerca del parámetro sku-name.
backup-retention | 7 | Cuánto tiempo deben conservarse las copias de seguridad. La unidad es días. El intervalo es 7-35. 
geo-redundant-backup | Disabled | Si se deben habilitar las copias de seguridad con redundancia geográfica en este servidor o no. Valores permitidos: Enabled, Disabled.
location | westus | La ubicación de Azure para el servidor.
ssl-enforcement | Enabled | Si debe ssl debe habilitarse, o no, en este servidor. Valores permitidos: Enabled, Disabled.
storage-size | 51200 | La capacidad de almacenamiento del servidor (la unidad es megabytes). El tamaño de almacenamiento válido un mínimo de 5120 MB y aumenta en incrementos de 1024 MB. Consulte el documento de los [planes de tarifa](./concepts-pricing-tiers.md) para más información acerca de los límites de tamaño de almacenamiento. 
version | 10.2 | La versión del motor principal de MariaDB.
admin-user | myadmin | El nombre del usuario del inicio de sesión del administrador. No puede ser **azure_superuser**, **admin**, **administrator**, **root**, **guest** o **public**.
admin-password | Password123 | La contraseña del usuario administrador. Debe tener entre 8 y 128 caracteres. La contraseña debe contener caracteres de tres de las siguientes categorías: letras en mayúsculas del alfabeto inglés, letras en minúscula del alfabeto inglés, números y caracteres no alfanuméricos.

El valor del parámetro sku-name sigue la convención {plan de tarifa}\_{generación de proceso}\_{núcleos virtuales} como en los ejemplos siguientes:
+ `--sku-name B_Gen5_4` se asigna a Básico, Gen 5 y 4 núcleos virtuales.
+ `--sku-name GP_Gen5_32` se asigna a De uso general, Gen 5 y 32 núcleos virtuales.
+ `--sku-name MO_Gen5_2` se asigna a Optimizado para memoria, Gen 5 y 2 núcleos virtuales.

Para comprender cuáles son los valores válidos por región y nivel consulte la documentación sobre [planes de tarifa](./concepts-pricing-tiers.md).

En el ejemplo siguiente se crea un servidor llamado `mydemoserver` en la región Oeste de EE. UU., en el grupo de recursos `myresourcegroup` y con el inicio de sesión de administrador de servidor `myadmin`. Se trata de un servidor **Gen 5** de **uso general** con 2 **núcleos virtuales**. El nombre de un servidor se asigna al nombre DNS y, por tanto, debe ser único a nivel global en Azure. Sustituya `<server_admin_password>` por su propio valor.
```azurecli-interactive
az mariadb server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 10.2
```

## <a name="configure-firewall-rule"></a>Configurar de la regla de firewall
Cree una regla de firewall de nivel de servidor de Azure Database for MariaDB con el comando **[az mariadb server firewall-rule create](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-create)**. Una regla de firewall de nivel de servidor permite que una aplicación externa, como la herramienta de línea de comandos **mysql** o MySQL Workbench, se conecte al servidor a través del firewall del servicio Azure MariaDB. 

En el ejemplo siguiente se crea una regla de firewall denominada `AllowMyIP` que permite las conexiones desde una dirección IP específica, 192.168.0.1. Sustituya la dirección IP o rango de direcciones IP que se correspondan con aquella desde la que se está conectando. 

```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
```

> [!NOTE]
> Las conexiones a Azure Database for MariaDB se comunican mediante puerto 3306. Si intenta conectarse desde una red corporativa, es posible que no se permita el tráfico saliente a través del puerto 3306. En ese caso no podrá conectarse al servidor, salvo que el departamento de TI abra el puerto 3306.
> 

## <a name="configure-ssl-settings"></a>Configuración de SSL
De manera predeterminada, se exigen conexiones SSL entre su servidor y las aplicaciones cliente. Así se garantiza de forma predeterminada la seguridad de los datos "en movimiento" al cifrar el flujo de datos que circula por Internet. Para hacer que esta guía de inicio rápido sea más fácil, se han deshabilitado las conexiones SSL para su servidor. Esto no se recomienda en servidores de producción. Para más información, consulte [Configuración de la conectividad SSL en la aplicación para conectarse de forma segura a Azure Database for MariaDB](./howto-configure-ssl.md).

En el ejemplo siguiente se deshabilita la aplicación de SSL en su servidor de MariaDB.
 
 ```azurecli-interactive
 az mariadb server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a>Obtención de la información de conexión

Para conectarse al servidor, debe proporcionar las credenciales de acceso y la información del host.

```azurecli-interactive
az mariadb server show --resource-group myresourcegroup --name mydemoserver
```

El resultado está en formato JSON. Tome nota de los valores de **fullyQualifiedDomainName** y **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.mariadb.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMariaDB/servers/mydemoserver",
  "location": "westus",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 2,
    "family": "Gen5",
    "name": "GP_Gen5_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageMb": 5120
  },
  "tags": null,
  "type": "Microsoft.DBforMariaDB/servers",
  "userVisibleState": "Ready",
  "version": "10.2"
}
```

## <a name="connect-to-server-using-mysql-command-line"></a>Conéctese al servidor mediante la línea de comandos mysql
Conéctese al servidor mediante la herramienta de línea de comandos **mysql**. La herramienta de línea de comandos se puede descargar de [aquí](https://dev.mysql.com/downloads/) e instalarla en su equipo. En su lugar, también puede hacer clic en el botón **Pruébelo** de los códigos de ejemplo o en el botón `>_` de la barra de herramientas situada en la parte superior derecha de Azure Portal e iniciar **Azure Cloud Shell**.

Escriba los comandos siguientes: 

1. Conéctese al servidor con la herramienta de la línea de comandos **mysql**:
```azurecli-interactive
 mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p
```

2. Vea el estado del servidor en el símbolo del sistema de mysql >:
```sql
status
```
Si todo se ha realizado correctamente, la herramienta de la línea de comandos debe mostrar el siguiente texto:

```bash
C:\Users\>mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.39.0 MariaDB Server

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.7.23, for Linux (x86_64)

Connection id:          64681
Current database:
Current user:           myadmin@40.118.201.21
SSL:                    Cipher in use is AES256-SHA
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         5.6.39.0 MariaDB Server
Protocol version:       10
Connection:             mydemoserver.mariadb.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    utf8
Conn.  characterset:    utf8
TCP port:               3306
Uptime:                 1 day 3 hours 28 min 50 sec

Threads: 10  Questions: 29002  Slow queries: 0  Opens: 33  Flush tables: 3  Open tables: 1  Queries per second avg: 0.293
--------------

mysql>
```

> [!TIP]
> Para otros comandos, consulte el [capítulo 4.5.1 del Manual de referencia de MySQL 5.7](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

## <a name="connect-to-server-using-mysql-workbench"></a>Conexión al servidor con MySQL Workbench
1.  Inicie la aplicación MySQL Workbench en el equipo cliente. Puede descargar e instalar MySQL Workbench desde [aquí](https://dev.mysql.com/downloads/workbench/).

2.  En el cuadro de diálogo **Setup New Connection** (Establecer nueva conexión), escriba la siguiente información en la pestaña **Parámetros**:

   ![Configuración de una conexión nueva](./media/quickstart-create-mariadb-server-database-using-azure-cli/setup-new-connection.png)

| **Configuración** | **Valor sugerido** | **Descripción** |
|---|---|---|
|   Nombre de la conexión | Mi conexión | Especifique una etiqueta para esta conexión (puede ser cualquier cosa) |
| Método de conexión | elija Estándar (TCP/IP) | Use el protocolo TCP/IP para conectarse a Azure Database for MariaDB |
| Nombre de host | mydemoserver.mariadb.database.azure.com | Nombre del servidor que anotó anteriormente. |
| Port | 3306 | Se usa el puerto predeterminado para MariaDB. |
| Nombre de usuario | myadmin@mydemoserver | El inicio de sesión de administrador del servidor que anotó anteriormente. |
| Contraseña | **** | Use la contraseña de la cuenta de administrador que configuró anteriormente. |

Haga clic en **Probar conexión** para probar si todos los parámetros están configurados correctamente.
Ahora puede hacer clic en la conexión para conectarse correctamente al servidor.

## <a name="clean-up-resources"></a>Limpieza de recursos
Si no necesita estos recursos para otra guía de inicio rápido o tutorial, puede eliminarlos con el siguiente comando: 

```azurecli-interactive
az group delete --name myresourcegroup
```

Si solo desea eliminar el servidor recién creado, puede ejecutar el comando **[az mariadb server delete](/cli/azure/mariadb/server#az-mariadb-server-delete)**.
```azurecli-interactive
az mariadb server delete --resource-group myresourcegroup --name mydemoserver
```

<!--
## Next steps

> [!div class="nextstepaction"]
> [Design a MariaDB Database with Azure CLI](./tutorial-design-database-using-cli.md)
-->