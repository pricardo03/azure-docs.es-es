---
title: Creación y configuración de un servidor de Azure Database for MySQL con Ansible
description: Obtenga información acerca de cómo usar Ansible para crear y configurar un servidor de Azure Database for MySQL
ms.service: azure
keywords: ansible, azure, devops, bash, cuaderno de estrategias, mysql, base de datos
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/23/2018
ms.openlocfilehash: 63472cf9c4b6b16f74ececfb6c6e61cf5f89ff9d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58095405"
---
# <a name="create-and-configure-an-azure-database-for-mysql-server-by-using-ansible"></a>Creación y configuración de un servidor de Azure Database for MySQL con Ansible
[Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) es un servicio administrado que se usa para ejecutar, administrar y escalar bases de datos MySQL de alta disponibilidad en la nube. Ansible permite automatizar la implementación y la configuración de recursos en el entorno. 

En esta guía de inicio rápido se muestra cómo usar Ansible para crear un servidor de Azure Database for MySQL y configurar su regla de firewall. Puede finalizar dichas tareas en unos cinco minutos mediante Azure Portal.

## <a name="prerequisites"></a>Requisitos previos
- **Suscripción a Azure**: si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Se requiere Ansible 2.7 para ejecutar los siguientes cuadernos de estrategias de ejemplo en este tutorial. 

## <a name="create-a-resource-group"></a>Crear un grupo de recursos
Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure.  

En el ejemplo siguiente, se crea un grupo de recursos denominado **myResourceGroup** en la ubicación **eastus**:

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

Guarde el cuaderno de estrategias anterior como **rg.yml**. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:
```bash
ansible-playbook rg.yml
```

## <a name="create-a-mysql-server-and-database"></a>Creación de un servidor de MySQL y una base de datos
En el ejemplo siguiente se crea un servidor de MySQL llamado **mysqlserveransible** y una instancia de Azure Database for MySQL denominada **mysqldbansible**. Se trata de un servidor Gen 5 de uso general con un núcleo virtual. 

El valor de **mysqlserver_name** debe ser único. Para comprender cuáles son los valores válidos por región y nivel consulte la [documentación sobre planes de tarifa](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers). Reemplace `<server_admin_password>` por una contraseña.

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

Guarde el cuaderno de estrategias anterior como **mysql_create.yml**. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:
```bash
ansible-playbook mysql_create.yml
```

## <a name="configure-a-firewall-rule"></a>Configuración de una regla de firewall
Una regla de firewall de nivel de servidor permite que una aplicación externa se conecte al servidor a través del firewall del servicio MySQL de Azure. Un ejemplo de aplicación externa es la herramienta de línea de comandos **mysql** o MySQL Workbench.
En el ejemplo siguiente se crea una regla de firewall denominada **extenalaccess** que permite las conexiones desde cualquier dirección IP externa. 

Escriba sus propios valores para **startIpAddress** (dirección IP de inicio) y **endIpAddress** (dirección IP final). Utilice el intervalo de direcciones IP que se correspondan con aquella desde la que se está conectando. 

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

En este caso, se usa el módulo **azure_rm_resource** para realizar esta tarea. Permite el uso directo de la API REST.

Guarde el cuaderno de estrategias anterior como **mysql_firewall.yml**. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:
```bash
ansible-playbook mysql_firewall.yml
```

## <a name="connect-to-the-server-by-using-the-command-line-tool"></a>Conexión al servidor mediante la herramienta de línea de comandos
Puede [descargar MySQL](https://dev.mysql.com/downloads/) e instalarlo en el equipo. En su lugar, también puede seleccionar el botón **Pruébelo** de los ejemplos de código o el botón **>_** de la barra de herramientas situada en la parte superior derecha de Azure Portal y abrir **Azure Cloud Shell**.

Escriba los comandos siguientes: 

1. Conéctese al servidor mediante la herramienta de línea de comandos **mysql**:
   ```azurecli-interactive
   mysql -h mysqlserveransible.mysql.database.azure.com -u mysqladmin@mysqlserveransible -p
   ```

2. Para ver el estado del servidor:
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
En el ejemplo siguiente se realizan consultas en los servidores de MySQL de **myResourceGroup** y posteriormente en todas las bases de datos de los servidores:

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

Guarde el cuaderno de estrategias anterior como **mysql_query.yml**. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:

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

También verá la siguiente salida para la base de datos de MySQL:
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

Guarde el cuaderno de estrategias anterior como **rg_delete.yml**. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:
```bash
ansible-playbook rg_delete.yml
```

Si desea eliminar solo el servidor de MySQL recién creado, ejecute el siguiente ejemplo:

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

Guarde el cuaderno de estrategias anterior como **mysql_delete.yml**. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:
```bash
ansible-playbook mysql_delete.yml
```

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Ansible en Azure](https://docs.microsoft.com/azure/ansible/)
