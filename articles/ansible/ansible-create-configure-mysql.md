---
title: Creación y configuración de un servidor de Azure Database for MySQL mediante Ansible (versión preliminar)
description: Aprenda a usar Ansible para crear y configurar un servidor de Azure Database for MySQL.
ms.service: ansible
keywords: ansible, azure, devops, bash, cuaderno de estrategias, mysql, base de datos
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/23/2018
ms.openlocfilehash: 508274d11a9693d28a9b3a01bd6ebbd7198e8711
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47586707"
---
# <a name="create-and-configure-an-azure-database-for-mysql-server-using-ansible-preview"></a>Creación y configuración de un servidor de Azure Database for MySQL mediante Ansible (versión preliminar)
[Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) es un servicio administrado que se usa para ejecutar, administrar y escalar bases de datos MySQL de alta disponibilidad en la nube. Esta guía de inicio rápido muestra cómo crear un servidor de Azure Database for MySQL en unos cinco minutos mediante Azure Portal. 

Ansible permite automatizar la implementación y la configuración de recursos en un entorno. En este artículo se muestra cómo usar Ansible para crear un servidor de Azure Database for MySQL y configurar su regla de firewall en cinco minutos. 

## <a name="prerequisites"></a>Requisitos previos
- **Suscripción a Azure**: si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Se requiere Ansible 2.7 para ejecutar los siguientes cuadernos de estrategias de ejemplo en este tutorial. Para instalar la versión RC de Ansible 2.7, ejecute `sudo pip install ansible[azure]==2.7.0rc2`. Ansible 2.7 se publicará en octubre de 2018. Después de esa fecha, no es necesario especificar la versión aquí porque la versión predeterminada será la 2.7.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos
Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure.  

En el ejemplo siguiente, se crea un grupo de recursos denominado **myResourceGroup** en la ubicación **eastus**.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
```

Guarde el cuaderno de estrategias anterior como *rg.yml*. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:
```bash
ansible-playbook rg.yml
```

## <a name="create-mysql-server-and-database"></a>Creación de base de datos y servidor de MySQL
En el ejemplo siguiente se crea un servidor de MySQL llamado **mysqlserveransible** y una instancia de Azure Database for MySQL denominada **mysqldbansible**. Se trata de un servidor Gen 5 Basic de uso general con 1 núcleo virtual. Tenga en cuenta que el valor de **mysqlserver_name** debe ser único y si desea comprender cuáles son los valores válidos por región y nivel consulte la [documentación sobre planes de tarifa](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers). 

Escriba su propio `<server_admin_password>`:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus
    mysqlserver_name: mysqlserveransible
    mysqldb_name: mysqldbansible
    admin_username: mysqladmin
    admin_password: <server_admin_password> 
  tasks:
    - name: Create MySQL Server
      azure_rm_mysqlserver:
        resource_group: "{{ resource_group }}"
        name: "{{ mysqlserver_name }}"
        sku:
          name: B_Gen5_1
          tier: Basic
        location: "{{ location }}"
        version: 5.6
        enforce_ssl: True
        admin_username: "{{ admin_username }}"
        admin_password: "{{ admin_password }}"
        storage_mb: 51200
    - name: Create instance of MySQL Database
      azure_rm_mysqldatabase:
        resource_group: "{{ resource_group }}"
        server_name: "{{ mysqlserver_name }}"
        name: "{{ mysqldb_name }}"
```

Guarde el cuaderno de estrategias anterior como *mysql_create.yml*. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:
```bash
ansible-playbook mysql_create.yml
```

## <a name="configure-firewall-rule"></a>Configurar de la regla de firewall
Una regla de firewall de nivel de servidor permite que una aplicación externa, como una herramienta de línea de comandos de **mysql** o MySQL Workbench se conecte al servidor a través del firewall del servicio Azure MySQL. En el ejemplo siguiente se crea una regla de firewall denominada **extenalaccess** que permite las conexiones desde cualquier dirección IP externa. 

Especifique su propia **startIpAddress** y **endIpAddress** con el rango de direcciones IP que se corresponda con aquella desde la que se está conectando. 

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    mysqlserver_name: mysqlserveransible
  tasks:
  - name: Open firewall to access MySQL Server from outside
    azure_rm_resource:
      api_version: '2017-12-01'
      resource_group: "{{ resource_group }}"
      provider: dbformysql
      resource_type: servers
      resource_name: "{{ mysqlserver_name }}"
      subresource:
        - type: firewallrules
          name: externalaccess
      body:
        properties:
          startIpAddress: "0.0.0.0"
          endIpAddress: "255.255.255.255"
```

> [!NOTE]
> Las conexiones a Azure Database for MySQL se comunican a través del puerto 3306. Si intenta conectarse desde una red corporativa, es posible que no se permita el tráfico saliente a través del puerto 3306. En ese caso no podrá conectarse al servidor, salvo que el departamento de TI abra el puerto 3306.
> 

El módulo **azure_rm_resource** se usa para realizar esta tarea que permite el uso directo de la API REST.

Guarde el cuaderno de estrategias anterior como *mysql_firewall.yml*. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:
```bash
ansible-playbook mysql_firewall.yml
```

## <a name="connect-to-the-server-using-command-line-tool"></a>Conexión al servidor con la herramienta de la línea de comandos
Puede descargarlo desde [aquí](https://dev.mysql.com/downloads/) e instalarlo en su equipo. En su lugar, también puede hacer clic en el botón **Pruébelo** de los códigos de ejemplo o en el botón `>_` de la barra de herramientas situada en la parte superior derecha de Azure Portal e iniciar **Azure Cloud Shell**.

Escriba los comandos siguientes: 

1. Conéctese al servidor con la herramienta de la línea de comandos **mysql**:
```azurecli-interactive
 mysql -h mysqlserveransible.mysql.database.azure.com -u mysqladmin@mysqlserveransible -p
```

2. Ver el estado del servidor:
```sql
 mysql> status
```

Si todo se ha realizado correctamente, la herramienta de la línea de comandos debe mostrar el siguiente texto:

```
demo@Azure:~$ mysql -h mysqlserveransible.mysql.database.azure.com -u mysqladmin@mysqlserveransible -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65233
Server version: 5.6.39.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.7.23, for Linux (x86_64) using  EditLine wrapper

Connection id:          65233
Current database:
Current user:           mysqladmin@13.76.42.93
SSL:                    Cipher in use is AES256-SHA
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         5.6.39.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             mysqlserveransible.mysql.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    utf8
Conn.  characterset:    utf8
TCP port:               3306
Uptime:                 36 min 21 sec

Threads: 5  Questions: 559  Slow queries: 0  Opens: 96  Flush tables: 3  Open tables: 10  Queries per second avg: 0.256
--------------
```

## <a name="using-facts-to-query-mysql-servers"></a>Uso de hechos para realizar consultas en los servidores de MySQL
En el ejemplo siguiente se realizan consultas en los servidores de MySQL de **myResourceGroup** y posteriormente en todas las bases de datos del servidor:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    mysqlserver_name: mysqlserveransible
  tasks:
    - name: Query MySQL Servers in current resource group
      azure_rm_mysqlserver_facts:
        resource_group: "{{ resource_group }}"
      register: mysqlserverfacts

    - name: Dump MySQL Server facts
      debug:
        var: mysqlserverfacts

    - name: Query MySQL Databases
      azure_rm_mysqldatabase_facts:
        resource_group: "{{ resource_group }}"
        server_name: "{{ mysqlserver_name }}"
      register: mysqldatabasefacts

    - name: Dump MySQL Database Facts
      debug:
        var: mysqldatabasefacts
```

Guarde el cuaderno de estrategias anterior como *mysql_query*.yml. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:

```bash
ansible-playbook mysql_query.yml
```

A continuación, verá la siguiente salida para el servidor de MySQL: 
```json
"servers": [
    {
        "admin_username": "mysqladmin",
        "enforce_ssl": false,
        "fully_qualified_domain_name": "mysqlserveransible.mysql.database.azure.com",
        "id": "/subscriptions/685ba005-af8d-4b04-8f16-a7bf38b2eb5a/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/mysqlserveransible",
        "location": "eastus",
        "name": "mysqlserveransible",
        "resource_group": "myResourceGroup",
        "sku": {
            "capacity": 1,
            "family": "Gen5",
            "name": "B_Gen5_1",
            "tier": "Basic"
        },
        "storage_mb": 5120,
        "user_visible_state": "Ready",
        "version": "5.6"
    }
]
```

Y también verá la siguiente salida para la base de datos de MySQL:
```json
"databases": [
    {
        "charset": "utf8",
        "collation": "utf8_general_ci",
        "name": "information_schema",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransible"
    },
    {
        "charset": "latin1",
        "collation": "latin1_swedish_ci",
        "name": "mysql",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransibler"
    },
    {
        "charset": "latin1",
        "collation": "latin1_swedish_ci",
        "name": "mysqldbansible",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransible"
    },
    {
        "charset": "utf8",
        "collation": "utf8_general_ci",
        "name": "performance_schema",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransible"
    }
]
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no necesita estos recursos, puede eliminarlos mediante la ejecución del ejemplo siguiente. Este elimina un grupo de recursos denominado **myResourceGroup**. 

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        state: absent
```

Guarde el cuaderno de estrategias anterior como *rg_delete*.yml. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:
```bash
ansible-playbook rg_delete.yml
```

Si solo desea eliminar el servidor de MySQL recién creado, puede hacerlo ejecutando el ejemplo siguiente:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    mysqlserver_name: mysqlserveransible
  tasks:
    - name: Delete MySQL Server
      azure_rm_mysqlserver:
        resource_group: "{{ resource_group }}"
        name: "{{ mysqlserver_name }}"
        state: absent
```

Guarde el cuaderno de estrategias anterior como *mysql_delete.yml*. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:
```bash
ansible-playbook mysql_delete.yml
```

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Ansible en Azure](https://docs.microsoft.com/azure/ansible/)