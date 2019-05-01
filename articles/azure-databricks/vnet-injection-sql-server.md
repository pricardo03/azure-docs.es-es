---
title: Consulta de un contenedor de Docker de Linux de SQL Server en una red virtual desde un bloc de notas de Azure Databricks
description: En este artículo se describe cómo implementar Azure Databricks a la red virtual, también conocida como inserción de red virtual.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: 345e07fac30f4ad0c8e9918cb8a1ff0fb8aeb811
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60770830"
---
# <a name="tutorial-query-a-sql-server-linux-docker-container-in-a-virtual-network-from-an-azure-databricks-notebook"></a>Tutorial: Consulta de un contenedor de Docker de Linux de SQL Server en una red virtual desde un bloc de notas de Azure Databricks

Este tutorial le enseña cómo integrar Azure Databricks con un contenedor de Docker de Linux de SQL Server en una red virtual. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Implementar un área de trabajo de Azure Databricks en una red virtual
> * Instalar una máquina virtual Linux en una red pública
> * Instalación de Docker
> * Instale a Microsoft SQL Server en el contenedor de docker de Linux
> * Consultar el servidor de SQL con JDBC desde un cuaderno de Databricks

## <a name="prerequisites"></a>Requisitos previos

* Crear un [área de trabajo de Databricks en una red virtual](quickstart-create-databricks-workspace-vnet-injection.md).

* Instalar [Ubuntu para Windows](https://www.microsoft.com/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab).

* Descargue [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017).

## <a name="create-a-linux-virtual-machine"></a>Creación de una máquina virtual con Linux

1. En el portal de Azure, seleccione el icono de **máquinas virtuales**. A continuación, seleccione **+ agregar**.

    ![Agregar nueva máquina virtual de Azure](./media/vnet-injection-sql-server/add-virtual-machine.png)

2. En el **Fundamentos** ficha, elija Ubuntu Server 16.04 LTS. Cambiar el tamaño de máquina virtual a B1ms, que tiene una VCPU y 2 GB de RAM. El requisito mínimo para un contenedor de Docker de Linux SQL Server es de 2 GB. Nombre de usuario y contraseña, elija a administrador.

    ![Pestaña aspectos básicos de la nueva configuración de máquina virtual](./media/vnet-injection-sql-server/create-virtual-machine-basics.png)

3. Navegue hasta la **redes** ficha. Elija la red virtual y la subred pública que incluye el clúster de Azure Databricks. Seleccione **revisión + crear**, a continuación, **crear** para implementar la máquina virtual.

    ![Pestaña red de nueva configuración de máquina virtual](./media/vnet-injection-sql-server/create-virtual-machine-networking.png)

4. Una vez completada la implementación, vaya a la máquina virtual. Observe la dirección IP pública y la red Virtual/subred en la **Introducción**. Seleccione el **dirección IP pública**

    ![Información general de la máquina virtual](./media/vnet-injection-sql-server/virtual-machine-overview.png)

5. Cambiar el **asignación** a **estático** y escriba un **etiqueta de nombre DNS**. Seleccione **guardar**y reinicie la máquina virtual.

    ![Configuración de dirección IP pública](./media/vnet-injection-sql-server/virtual-machine-staticip.png)

6. Seleccione el **redes** pestaña **configuración**. Tenga en cuenta que el grupo de seguridad de red que se creó durante la implementación de Azure Databricks está asociado con la máquina virtual. Seleccione **Agregar regla de puerto de entrada**.

7. Agregar una regla para abrir el puerto 22 para SSH. Use la configuración siguiente:
    
    |Configuración|Valor sugerido|DESCRIPCIÓN|
    |-------|---------------|-----------|
    |Origen|Direcciones IP|Las direcciones IP se especifica que el tráfico entrante desde un origen específico se permitirá o denegará por esta regla de dirección IP.|
    |Direcciones IP de origen|< la dirección ip pública\>|Escriba el la dirección IP pública. Puede encontrar la dirección IP pública, visite [bing.com](https://www.bing.com/) y buscando **"Mi IP"**.|
    |Intervalos de puertos de origen|*|Permitir el tráfico de cualquier puerto.|
    |Destino|Direcciones IP|Las direcciones IP se especifica que el tráfico saliente para un origen específico se permitirá o denegará por esta regla de dirección IP.|
    |Direcciones IP de destino|< la ip pública de la máquina virtual\>|Escriba la dirección IP pública de la máquina virtual. Puede encontrarlo en el **Introducción** página de la máquina virtual.|
    |Intervalos de puertos de destino|22|Abra el puerto 22 para SSH.|
    |Prioridad|290|Asigne a la regla una prioridad.|
    |NOMBRE|ssh-databricks-tutorial-vm|Asigne un nombre de la regla.|


    ![Agregar regla de seguridad de entrada para el puerto 22](./media/vnet-injection-sql-server/open-port.png)

8. Agregar una regla para abrir el puerto 1433 para SQL con la siguiente configuración:

    |Configuración|Valor sugerido|DESCRIPCIÓN|
    |-------|---------------|-----------|
    |Origen|Direcciones IP|Las direcciones IP se especifica que el tráfico entrante desde un origen específico se permitirá o denegará por esta regla de dirección IP.|
    |Direcciones IP de origen|10.179.0.0/16|Escriba el intervalo de direcciones de la red virtual.|
    |Intervalos de puertos de origen|*|Permitir el tráfico de cualquier puerto.|
    |Destino|Direcciones IP|Las direcciones IP se especifica que el tráfico saliente para un origen específico se permitirá o denegará por esta regla de dirección IP.|
    |Direcciones IP de destino|< la ip pública de la máquina virtual\>|Escriba la dirección IP pública de la máquina virtual. Puede encontrarlo en el **Introducción** página de la máquina virtual.|
    |Intervalos de puertos de destino|1433|Abra el puerto 22 para SQL Server.|
    |Prioridad|300|Asigne a la regla una prioridad.|
    |NOMBRE|sql-databricks-tutorial-vm|Asigne un nombre de la regla.|

    ![Agregar regla de seguridad de entrada para el puerto 1433](./media/vnet-injection-sql-server/open-port2.png)

## <a name="run-sql-server-in-a-docker-container"></a>Ejecute SQL Server en un contenedor de Docker

1. Abra [Ubuntu para Windows](https://www.microsoft.com/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab), o cualquier otra herramienta que le permitirá conectarse mediante SSH en la máquina virtual. Vaya a la máquina virtual en Azure portal y seleccione **Connect** para obtener el comando SSH tiene que conectarse.

    ![Conexión a la máquina virtual](./media/vnet-injection-sql-server/vm-ssh-connect.png)

2. Escriba el comando en el terminal de Ubuntu y escriba la contraseña de administrador que creó al configurar la máquina virtual.

    ![Ubuntu terminal SSH inicio de sesión](./media/vnet-injection-sql-server/vm-login-terminal.png)

3. Use el siguiente comando para instalar a Docker en la máquina virtual.

    ```bash
    sudo apt-get install docker.io
    ```

    Comprobar la instalación de Docker con el siguiente comando:

    ```bash
    sudo docker --version
    ```

4. Instale la imagen.

    ```bash
    sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
    ```

    Compruebe las imágenes.

    ```bash
    sudo docker images
    ```

5. Ejecute el contenedor de la imagen.

    ```bash
    sudo docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=Password1234' -p 1433:1433 --name sql1  -d mcr.microsoft.com/mssql/server:2017-latest
    ```

    Compruebe que el contenedor se está ejecutando.

    ```bash
    sudo docker ps -a
    ```

## <a name="create-a-sql-database"></a>Creación de una base de datos SQL

1. Abra SQL Server Management Studio y conéctese al servidor mediante el nombre del servidor y la autenticación de SQL. Es el nombre de usuario de inicio de sesión **SA** y la contraseña es la contraseña establecida en el comando de Docker. La contraseña en el comando de ejemplo es `Password1234`.

    ![Conectarse a SQL Server utilizando SQL Server Management Studio](./media/vnet-injection-sql-server/ssms-login.png)

2. Una vez que se ha conectado correctamente, seleccione **nueva consulta** y escriba el siguiente fragmento de código para crear una base de datos, una tabla e insertar algunos registros de la tabla.

    ```SQL
    CREATE DATABASE MYDB;
    GO
    USE MYDB;
    CREATE TABLE states(Name VARCHAR(20), Capitol VARCHAR(20));
    INSERT INTO states VALUES ('Delaware','Dover');
    INSERT INTO states VALUES ('South Carolina','Columbia');
    INSERT INTO states VALUES ('Texas','Austin');
    SELECT * FROM states
    GO
    ```

    ![Consulta para crear una base de datos de SQL Server](./media/vnet-injection-sql-server/create-database.png)

## <a name="query-sql-server-from-azure-databricks"></a>Consulta SQL Server desde Azure Databricks

1. Desplácese hasta el área de trabajo de Azure Databricks y compruebe que ha creado un clúster como parte de los requisitos previos. A continuación, seleccione **crear un cuaderno**. Asigne un nombre, seleccione al Bloc de notas *Python* como el lenguaje y seleccione el clúster que creó.

    ![Nueva configuración de cuaderno de Databricks](./media/vnet-injection-sql-server/create-notebook.png)

2. Use el siguiente comando para hacer ping a la dirección IP interna de la máquina virtual de SQL Server. Este ping debería completarse correctamente. Si no es así, compruebe que el contenedor se está ejecutando y revise la configuración de grupo (NSG) de seguridad de red.

    ```python
    %sh
    ping 10.179.64.4
    ```

    También puede usar el comando nslookup para revisar.

    ```python
    %sh
    nslookup databricks-tutorial-vm.westus2.cloudapp.azure.com
    ```

3. Una vez que haya hacer ping correctamente en el servidor SQL Server, puede consultar la base de datos y tablas. Ejecute la siguiente versión de python de código:

    ```python
    jdbcHostname = "10.179.64.4"
    jdbcDatabase = "MYDB"
    userName = 'SA'
    password = 'Password1234'
    jdbcPort = 1433
    jdbcUrl = "jdbc:sqlserver://{0}:{1};database={2};user={3};password={4}".format(jdbcHostname, jdbcPort, jdbcDatabase, userName, password)

    df = spark.read.jdbc(url=jdbcUrl, table='states')
    display(df)
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos, el área de trabajo de Azure Databricks y todos los recursos relacionados. Si elimina el trabajo evitará que se generen facturas innecesarias. Si planea usar el área de trabajo de Azure Databricks en el futuro, puede detener el clúster y reiniciarlo más tarde. Si no va a seguir usando esta área de trabajo de Azure Databricks, elimine todos los recursos creados en este tutorial mediante los siguientes pasos:

1. En el menú de la izquierda de Azure Portal, haga clic en **Grupos de recursos** y en el nombre del grupo de recursos que creó.

2. En la página del grupo de recursos, seleccione **Eliminar**, escriba el nombre del recurso que quiere eliminar en el cuadro de texto y seleccione **Eliminar** de nuevo.

## <a name="next-steps"></a>Pasos siguientes

Avance al siguiente artículo para obtener información sobre cómo extraer, transformar y cargar datos mediante Azure Databricks.
> [!div class="nextstepaction"]
> [Tutorial: Extracción, transformación y carga de datos mediante Azure Databricks](databricks-extract-load-sql-data-warehouse.md)
