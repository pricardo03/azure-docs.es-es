---
title: Compilación de una aplicación web Python y PostgreSQL en Azure App Service | Microsoft Docs
description: Aprenda a ejecutar una aplicación Python controlada por datos en Azure, con conexión a una base de datos PostgreSQL.
services: app-service\web
documentationcenter: python
author: cephalin
manager: jeconnoc
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 09/28/2018
ms.author: beverst;cephalin
ms.custom: mvc
ms.openlocfilehash: f4ce197d541b8573e38fd85dcebb575c8ee99f59
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435793"
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Compilación de una aplicación web Docker Python con PostgreSQL en Azure

[App Service en Linux](app-service-linux-intro.md) proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático. En este tutorial se muestra cómo crear una aplicación web Python controlada por datos con PostgreSQL como back-end de base de datos. Cuando haya terminado, tendrá una aplicación Python Flask que se ejecuta en un contenedor Docker en App Service en Linux.

![Aplicación Docker Python Flask en App Service en Linux](./media/tutorial-python-postgresql-app/docker-flask-in-azure.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una base de datos PostgreSQL en Azure
> * Conexión de una aplicación Python a PostgreSQL
> * Implementación de la aplicación en Azure
> * Visualización de los registros de diagnóstico
> * Actualizar el modelo de datos y volver a implementar la aplicación
> * Administrar la aplicación en Azure Portal

También puede seguir los pasos de este artículo en macOS. Las instrucciones de Linux y Windows son las mismas en la mayoría de los casos, pero en este tutorial no se detallan las diferencias.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial:

1. [Instalación de Git](https://git-scm.com/)
1. [Instalación de Python](https://www.python.org/downloads/)
1. [Instalación y ejecución de PostgreSQL](https://www.postgresql.org/download/)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Prueba de la instalación local de PostgreSQL y creación de una base de datos

En una ventana de terminal local, ejecute `psql` para conectarse al servidor local de PostgreSQL.

```bash
sudo -u postgres psql postgres
```

Si recibe un mensaje de error similar a `unknown user: postgres`, la instalación de PostgreSQL puede configurarse con su nombre de usuario registrado. Pruebe con el comando siguiente en su lugar.

```bash
psql postgres
```

Si la conexión se realiza correctamente, significa que la base de datos PostgreSQL está en ejecución. De lo contrario, asegúrese de que se ha iniciado la base de datos PostgreSQL local, para lo que debe seguir las instrucciones para su sistema operativo que se indican en [Downloads - PostgreSQL Core Distribution](https://www.postgresql.org/download/) (Descargas - Distribución del núcleo de PostgreSQL).

Cree una base de datos denominada *eventregistration* y configure un usuario de base de datos independiente denominado *manager* cuya contraseña será *supersecretpass*.

```sql
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Escriba `\q` para salir del cliente de PostgreSQL.

<a name="step2"></a>

## <a name="create-local-python-app"></a>Creación de una aplicación Python local

En este paso, configurará el proyecto de Python Flask local.

### <a name="clone-the-sample-app"></a>Clonación de la aplicación de ejemplo

Abra la ventana de terminal y use `CD` para cambiar a un directorio de trabajo.

Ejecute los comandos siguientes para clonar el repositorio de ejemplo.

```bash
git clone https://github.com/Azure-Samples/flask-postgresql-app.git
cd flask-postgresql-app
```

Este repositorio de ejemplo contiene una aplicación [Flask](http://flask.pocoo.org/).

### <a name="run-the-app-locally"></a>Ejecución de la aplicación de forma local

Instale los paquetes necesarios e inicie la aplicación.

```bash
# Bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run

# PowerShell
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
Set-Item Env:FLASK_APP ".\app.py"
DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Cuando la aplicación se haya cargado completamente, verá algo similar al siguiente mensaje:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Vaya a `http://localhost:5000` en un explorador. Haga clic en **Register!** (Registrarse) y cree un usuario de prueba.

![Aplicación Python Flask que se ejecuta de forma local](./media/tutorial-python-postgresql-app/local-app.png)

La aplicación Flask de ejemplo almacena datos de usuario en la base de datos. Si el registro del usuario se realiza correctamente, la aplicación escribe datos en la base de datos de PostgreSQL local.

Para detener el servidor de Flask en cualquier momento, escriba Ctrl+C en el terminal.

## <a name="create-a-production-postgresql-database"></a>Creación de una base de datos PostgreSQL de producción

En este paso, creará una base de datos PostgreSQL en Azure. Cuando la aplicación se implementa en Azure, utiliza esta base de datos en la nube.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)]

### <a name="create-an-azure-database-for-postgresql-server"></a>Creación de un servidor de Azure Database for PostgreSQL

Cree un servidor de PostgreSQL con el comando [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) en Cloud Shell.

En el siguiente comando de ejemplo, reemplace *\<postgresql_name>* por un nombre de servidor único y *\<admin_username>* y *\<admin_password>* por las credenciales de usuario deseadas. Las credenciales de usuario son para la cuenta de administrador de base de datos. El nombre del servidor se usa como parte del punto de conexión de PostgreSQL (`https://<postgresql_name>.postgres.database.azure.com`), por lo que debe ser único en todos los servidores de Azure.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --location "West Europe" --admin-user <admin_username> --admin-password <admin_password> --sku-name B_Gen4_1
```

Cuando se crea la base de datos de Azure para el servidor de PostgreSQL, la CLI de Azure muestra información similar a la del siguiente ejemplo:

```json
{
  "administratorLogin": "<admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 1,
    "family": "Gen4",
    "name": "B_Gen4_1",
    "size": null,
    "tier": "Basic"
  },
  < JSON data removed for brevity. >
}
```

> [!NOTE]
> Recuerde \<nombre_de_usuario_admin> y \<contraseña_admin> para más tarde. Debe iniciar sesión en el servidor de Postgre y sus bases de datos.


### <a name="create-firewall-rules-for-the-postgresql-server"></a>Creación de reglas de firewall para el servidor de PostgreSQL

En Cloud Shell, ejecute los siguientes comandos de la CLI de Azure para permitir el acceso a la base de datos desde los recursos de Azure.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAllAzureIPs
```

> [!NOTE]
> Esta configuración permite conexiones de red desde todas las direcciones IP dentro de la red Azure. Para su uso en producción, pruebe a configurar las reglas de firewall más restrictivas posibles [con solo las direcciones IP de salida que utiliza la aplicación](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).

En Cloud Shell, ejecute de nuevo el comando para permitir el acceso desde el equipo local y reemplace *\<su_dirección_ip>* por [la dirección IP IPv4 local](http://www.whatsmyip.org/).

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=<your_ip_address> --end-ip-address=<your_ip_address> --name AllowLocalClient
```

## <a name="connect-python-app-to-production-database"></a>Conexión de la aplicación Python a la base de datos de producción

En este paso, conectará la aplicación Flask de ejemplo al servidor de Azure Database for PostgreSQL que ha creado.

### <a name="create-empty-database-and-user-access"></a>Creación del acceso de usuario y la base de datos vacía

En la ventana del terminal local, conéctese a la base de datos mediante la ejecución del siguiente comando. Cuando se le pida la contraseña de administrador, use la misma contraseña que especificó en [Creación de un servidor de Azure Database for PostgreSQL](#create-an-azure-database-for-postgresql-server).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Al igual que en su servidor local de Postgres, cree la base de datos y el usuario en el servidor Postgres de Azure.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Escriba `\q` para salir del cliente de PostgreSQL.

> [!NOTE]
> Es un procedimiento recomendado crear usuarios de bases de datos con permisos restringidos para aplicaciones específicas, en lugar de utilizar el usuario administrador. En este ejemplo, el usuario `manager` tiene plenos privilegios para _solo_ la base de datos `eventregistration`.

### <a name="test-app-connectivity-to-production-database"></a>Prueba de la conectividad de la aplicación a la base de datos de producción

De nuevo en la ventana del terminal local, ejecute los siguientes comandos para ejecutar la migración de la base de datos de Flask y el servidor de Flask.

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Cuando la aplicación se haya cargado completamente, verá algo similar al siguiente mensaje:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Vaya a http://localhost:5000 en un explorador. Haga clic en **Register!** (Registrarse) y cree un registro de prueba. Ya escribe datos en la base de datos de Azure.

![Aplicación Python Flask que se ejecuta de forma local](./media/tutorial-python-postgresql-app/local-app.png)

## <a name="deploy-to-azure"></a>Implementar en Azure

En este paso, se implementará la aplicación Python conectada a Postgres en Azure App Service.

### <a name="configure-repository"></a>Configuración del repositorio

El motor de implementación de Git en App Service invoca la automatización `pip` cuando hay un archivo _application.py_ en la raíz del repositorio. En este tutorial, dejará que el motor de implementación ejecute la automatización por usted. En la ventana del terminal local, vaya a la raíz del repositorio, cree un archivo _application.py_ ficticio y confirme los cambios.

```bash
cd ..
touch application.py
git add .
git commit -m "ensure azure automation"
```

### <a name="configure-a-deployment-user"></a>Configuración de un usuario de implementación

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Creación de un plan de App Service 

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Creación de una aplicación web 

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-python-linux-no-h.md)]

### <a name="configure-environment-variables"></a>Configuración de las variables de entorno

En este mismo tutorial, ya ha definido variables de entorno para conectarse a su base de datos de PostgreSQL.

En App Service, las variables de entorno se establecen como _valores de aplicación_ mediante el comando [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) en Cloud Shell.

En el siguiente ejemplo se especifican los datos de conexión de la base de datos en forma de valores de configuración de aplicación. 

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration"
```

### <a name="push-to-azure-from-git"></a>Inserción en Azure desde Git

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash 
Counting objects: 5, done. 
Delta compression using up to 4 threads. 
Compressing objects: 100% (5/5), done. 
Writing objects: 100% (5/5), 489 bytes | 0 bytes/s, done. 
Total 5 (delta 3), reused 0 (delta 0) 
remote: Updating branch 'master'. 
remote: Updating submodules. 
remote: Preparing deployment for commit id '6c7c716eee'. 
remote: Running custom deployment command... 
remote: Running deployment command... 
remote: Handling node.js deployment. 
. 
. 
. 
remote: Deployment successful. 
To https://<app_name>.scm.azurewebsites.net/<app_name>.git 
 * [new branch]      master -> master 
```  

### <a name="configure-entry-point"></a>Configuración del punto de entrada

De forma predeterminada, la imagen incorporada busca un archivo _wsgi.py_ o _application.py_ en el directorio raíz como punto de entrada, pero el punto de entrada es _app/app.py_. El archivo _application.py_ que agregó anteriormente está vacío y no hace nada.

En Cloud Shell, ejecute el comando [`az webapp config set`](/cli/azure/webapp/config?view=azure-cli-latest#az-webapp-config-set) para establecer un script de inicio personalizado.

```azurecli-interactive
az webapp config set --name <app_name> --resource-group myResourceGroup --startup-file "gunicorn '--bind=0.0.0.0' --chdir /home/site/wwwroot/app app:app"
```

El parámetro `--startup-file` toma un comando personalizado o la ruta de acceso al archivo que contiene el comando personalizado. El comando personalizado debe tener el siguiente formato:

```
gunicorn '--bind=0.0.0.0' --chdir /home/site/wwwroot/<subdirectory> <module>:<variable>
```

En el comando personalizado, `--chdir` es necesario si el punto de entrada no está en el directorio raíz, y `<subdirectory>` es el subdirectorio. `<module>` es el nombre del archivo _.py_ y `<variable>` es la variable del módulo que representa la aplicación web.

### <a name="browse-to-the-azure-web-app"></a>Navegación hasta la aplicación web de Azure

Vaya a la aplicación web implementada. Tarda algún tiempo en iniciarse porque el contenedor se debe descargar y ejecutar la primera vez que se solicita la aplicación. Si la página agota el tiempo de espera o muestra un mensaje de error, espere unos minutos y actualice la página.

```bash
http://<app_name>.azurewebsites.net
```

Ve a los invitados que registró anteriormente y que se guardaron en la base de datos de producción de Azure en el paso anterior.

![Aplicación Python Flask que se ejecuta en Azure](./media/tutorial-python-postgresql-app/docker-app-deployed.png)

**¡Enhorabuena!** Va a ejecutar una aplicación Python en App Service para Linux.

## <a name="access-diagnostic-logs"></a>Acceso a los registros de diagnóstico

Dado que la aplicación Python se ejecuta en un contenedor, App Service on Linux le permite acceder a los registros de la consola generados desde el interior del contenedor. Para buscar los registros, vaya a esta dirección URL:

```
https://<app_name>.scm.azurewebsites.net/api/logs/docker
```

Debería ver dos objetos JSON, cada uno con una propiedad `href`. Un objeto `href` apunta a los registros de la consola Docker (termina con `_docker.log`) y el otro objeto `href` apunta a los registros de la consola generados desde el interior del contenedor de Python. 

```json
[  
   {  
      "machineName":"RD0003FF61ACD0_default",
      "lastUpdated":"2018-09-27T16:48:17Z",
      "size":4766,
      "href":"https://<app_name>.scm.azurewebsites.net/api/vfs/LogFiles/2018_09_27_RD0003FF61ACD0_default_docker.log",
      "path":"/home/LogFiles/2018_09_27_RD0003FF61ACD0_default_docker.log"
   },
   {  
      "machineName":"RD0003FF61ACD0",
      "lastUpdated":"2018-09-27T16:48:19Z",
      "size":2589,
      "href":"https://<app_name>.scm.azurewebsites.net/api/vfs/LogFiles/2018_09_27_RD0003FF61ACD0_docker.log",
      "path":"/home/LogFiles/2018_09_27_RD0003FF61ACD0_docker.log"
   }
]
```

Copie el valor `href` que desee en una ventana del explorador para ir a los registros. Los registros no se transmiten en secuencias, por lo que es posible que experimente algún retraso. Para ver nuevos registros, actualice la página del explorador.

## <a name="update-data-model-and-redeploy"></a>Actualización del modelo de datos y nueva implementación

En este paso, se agrega el número de asistentes al registro de cada evento mediante la actualización del modelo `Guest` y después se vuelve a implementar la actualización en Azure.

En la ventana de terminal local, extraiga del repositorio la rama `modelChange` con el siguiente comando de Git:

```bash
git checkout origin/modelChange -- .
```

Esta retirada ya realiza los cambios necesarios en el modelo, las vistas y los controladores. También incluye una migración de base de datos generada mediante *alembic* (`flask db migrate`). Puede ver todos los cambios mediante el siguiente comando de Git:

```bash
git diff master origin/modelChange
```

### <a name="test-your-changes-locally"></a>Prueba de los cambios localmente

En la ventana de terminal local, ejecute los siguientes comandos para probar los cambios localmente mediante la ejecución del servidor de Flask.

```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Vaya a http://localhost:5000 en el explorador para ver los cambios. Cree un registro de prueba.

![Aplicación Python Flask basada en contenedor Docker que se ejecuta de forma local](./media/tutorial-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Publicación de los cambios en Azure

En la ventana del terminal local, confirme todos los cambios en Git e inserte los cambios de código en Azure.

```bash 
git add . 
git commit -m "updated data model" 
git push azure master 
``` 

Vaya a la aplicación web de Azure y vuelva a probar la nueva funcionalidad. Asegúrese de actualizar la página.

```bash
http://<app_name>.azurewebsites.net
```

![Aplicación Docker Python Flask en Azure App Service](./media/tutorial-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-web-app-in-the-azure-portal"></a>Administración de una aplicación web en Azure Portal

Vaya a [Azure Portal](https://portal.azure.com) para ver la aplicación web que ha creado.

En el menú izquierdo, haga clic en **App Services**, a continuación, haga clic en el nombre de la aplicación web de Azure.

![Navegación desde el portal a la aplicación web de Azure](./media/tutorial-python-postgresql-app/app-resource.png)

De forma predeterminada, el portal muestra la página **Información general**. Esta página proporciona una visión del funcionamiento de la aplicación. En este caso, también puede realizar tareas de administración básicas como examinar, detener, iniciar, reiniciar y eliminar. Las pestañas del lado izquierdo de la página muestran las diferentes páginas de configuración que puede abrir.

![Página de App Service en Azure Portal](./media/tutorial-python-postgresql-app/app-mgmt.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Creación de una base de datos PostgreSQL en Azure
> * Conexión de una aplicación Python a PostgreSQL
> * Implementación de la aplicación en Azure
> * Visualización de los registros de diagnóstico
> * Actualizar el modelo de datos y volver a implementar la aplicación
> * Administrar la aplicación en Azure Portal

Vaya al siguiente tutorial para aprender a asignar un nombre DNS personalizado a una aplicación web.

> [!div class="nextstepaction"]
> [Configuración de una imagen de Python integrada](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Asignar un nombre DNS personalizado a Azure Web Apps](../app-service-web-tutorial-custom-domain.md)

