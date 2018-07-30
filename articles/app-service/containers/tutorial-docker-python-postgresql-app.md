---
title: Compilación de una aplicación web Python y PostgreSQL en Azure App Service | Microsoft Docs
description: Aprenda a ejecutar una aplicación Python controlada por datos en Azure, con conexión a una base de datos PostgreSQL.
services: app-service\web
documentationcenter: python
author: berndverst
manager: jeconnoc
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 07/13/2018
ms.author: beverst;cephalin
ms.custom: mvc
ms.openlocfilehash: 20b549914daf71c0d23235b5c20ebb6f14367471
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2018
ms.locfileid: "39172041"
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Compilación de una aplicación web Docker Python con PostgreSQL en Azure

Web App for Containers proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático. En este tutorial se muestra cómo crear una aplicación web Python controlada por datos con PostgreSQL como back-end de base de datos. Cuando haya terminado, tendrá una aplicación Python Flask que se ejecuta en un contenedor Docker en [App Service en Linux](app-service-linux-intro.md).

![Aplicación Docker Python Flask en App Service en Linux](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una base de datos PostgreSQL en Azure
> * Conexión de una aplicación Python a PostgreSQL
> * Implementación de la aplicación en Azure
> * Actualizar el modelo de datos y volver a implementar la aplicación
> * Administrar la aplicación en Azure Portal

También puede seguir los pasos de este artículo en macOS. Las instrucciones de Linux y Windows son las mismas en la mayoría de los casos, pero en este tutorial no se detallan las diferencias.
 
[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial:

1. [Instalación de Git](https://git-scm.com/)
1. [Instalación de Python](https://www.python.org/downloads/)
1. [Instalación y ejecución de PostgreSQL](https://www.postgresql.org/download/)
1. [Instalación de Docker Community Edition](https://www.docker.com/community-edition)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Prueba de la instalación local de PostgreSQL y creación de una base de datos

En una ventana de terminal local, ejecute `psql` para conectarse al servidor local de PostgreSQL.

```bash
sudo -u postgres psql
```

Si la conexión se realiza correctamente, significa que la base de datos PostgreSQL está en ejecución. De lo contrario, asegúrese de que se ha iniciado la base de datos PostgreSQL local, para lo que debe seguir los pasos que se indican en [Downloads - PostgreSQL Core Distribution](https://www.postgresql.org/download/) (Descargas - Distribución del núcleo de PostgreSQL).

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

Ejecute los siguientes comandos para clonar el repositorio de ejemplo y vaya a la versión *0.1-initialapp*.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

Este repositorio de ejemplo contiene una aplicación [Flask](http://flask.pocoo.org/). 

### <a name="run-the-app-locally"></a>Ejecución de la aplicación de forma local

Instale los paquetes necesarios e inicie la aplicación.

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
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

![Aplicación Python Flask que se ejecuta de forma local](./media/tutorial-docker-python-postgresql-app/local-app.png)

La aplicación Flask de ejemplo almacena datos de usuario en la base de datos. Si el registro del usuario se realiza correctamente, la aplicación escribe datos en la base de datos de PostgreSQL local.

Para detener el servidor de Flask en cualquier momento, escriba Ctrl+C en el terminal. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-production-postgresql-database"></a>Creación de una base de datos PostgreSQL de producción

En este paso, creará una base de datos PostgreSQL en Azure. Cuando la aplicación se implementa en Azure, utiliza esta base de datos en la nube.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### <a name="create-an-azure-database-for-postgresql-server"></a>Creación de un servidor de Azure Database for PostgreSQL

Cree un servidor de PostgreSQL con el comando [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az_postgres_server_create) en Cloud Shell.

En el siguiente comando de ejemplo, reemplace *\<postgresql_name>* por un nombre de servidor único y *\<admin_username>* y *\<admin_password>* por las credenciales de usuario deseadas. El nombre del servidor se usa como parte del punto de conexión de PostgreSQL (`https://<postgresql_name>.postgres.database.azure.com`), por lo que debe ser único en todos los servidores de Azure. Las credenciales de usuario son para la cuenta de usuario administrador de base de datos. 

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --location "West Europe" --admin-user <admin_username> --admin-password <admin_password> --sku-name GP_Gen4_2
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
    "capacity": 100,
    "family": null,
    "name": "PGSQLS3M100",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

### <a name="create-a-firewall-rule-for-the-postgresql-server"></a>Creación de una regla de firewall para el servidor de PostgreSQL

En Cloud Shell, ejecute el siguiente comando de la CLI de Azure para permitir el acceso a la base de datos desde todas las direcciones IP. Cuando las direcciones IP de inicio y fin están establecidas en `0.0.0.0`, el firewall solo se abre para otros recursos de Azure. 

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAzureIPs
```

> [!TIP] 
> Puede ser incluso más restrictivo con su regla de firewall [usando solo las direcciones IP de salida que utiliza su aplicación](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).
>

En Cloud Shell, ejecute de nuevo el comando para permitir el acceso a la base de datos desde el equipo local y reemplace *\<you_ip_address>* por [la dirección IP IPv4 local](https://whatismyipaddress.com/). 

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=<you_ip_address> --end-ip-address=<you_ip_address> --name AllowLocalClient
```

## <a name="connect-python-app-to-production-database"></a>Conexión de la aplicación Python a la base de datos de producción

En este paso, conectará la aplicación Flask de ejemplo al servidor de Azure Database for PostgreSQL que ha creado.

### <a name="create-empty-database-and-user-access"></a>Creación del acceso de usuario y la base de datos vacía

En Cloud Shell, conéctese a la base de datos mediante la ejecución de `psql`. Cuando se le pida la contraseña de administrador, use la misma contraseña que especificó en [Creación de un servidor de Azure Database for PostgreSQL](#create-an-azure-database-for-postgresql-server).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Cree la base de datos y el usuario desde la CLI de PostgreSQL.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Escriba `\q` para salir del cliente de PostgreSQL.

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

![Aplicación Python Flask que se ejecuta de forma local](./media/tutorial-docker-python-postgresql-app/local-app.png)

## <a name="upload-app-to-a-container-registry"></a>Carga de la aplicación en un registro de contenedor

En este paso, creará una imagen de Docker y la cargará en Azure Container Registry. También puede usar registros conocidos, como Docker Hub.

### <a name="build-the-docker-image-and-test-it"></a>Compilación y prueba de la imagen de Docker

En la ventana del terminal local, compile la imagen de Docker.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

Docker muestra una confirmación de que se ha creado la imagen correctamente.

```bash
Successfully built 7548f983a36b
```

En la raíz del repositorio, agregue un archivo de variables de entorno denominado _db.env_ y, a continuación, agréguele las siguientes variables de entorno de base de datos. La aplicación se conecta a la base de datos de producción de Azure Database for PostgreSQL.

```text
DBHOST=<postgresql_name>.postgres.database.azure.com
DBUSER=manager@<postgresql_name>
DBNAME=eventregistration
DBPASS=supersecretpass
```

Ejecute la imagen localmente en un contenedor de Docker. El siguiente comando especifica el archivo de variables de entorno y asigna el puerto 5000 predeterminado de Flask al puerto 5000 local.

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

El resultado es similar a lo que vio anteriormente. No obstante, ya no es necesario efectuar la migración de base de datos inicial, por lo que se omite.

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

La base de datos ya contiene el registro que creó anteriormente.

![Aplicación Python Flask basada en contenedor Docker que se ejecuta de forma local](./media/tutorial-docker-python-postgresql-app/local-docker.png)

Ahora que ha comprobado que el contenedor funciona localmente, elimine _db.env_. En Azure App Service, usará la configuración de la aplicación para definir las variables de entorno.  

### <a name="create-an-azure-container-registry"></a>Creación de una instancia de Azure Container Registry

En Cloud Shell, cree un registro en Azure Container Registry con el siguiente comando. Reemplace *\<registry_name>* por un nombre de registro único.

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

### <a name="retrieve-registry-credentials"></a>Recuperación de las credenciales de registro

En Cloud Shell, ejecute los comandos siguientes para recuperar las credenciales de registro. Las necesitará para insertar y extraer las imágenes.

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

En la salida, verá dos contraseñas. Anote el nombre de usuario (que es el nombre del registro de forma predeterminada) y la primera contraseña.

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "<registry_password>"
    },
    {
      "name": "password2",
      "value": "<registry_password2>"
    }
  ],
  "username": "<registry_name>"
}
```

### <a name="upload-docker-image-to-registry"></a>Carga de la imagen de Docker en el registro

En la ventana de terminal local, inicie sesión en el nuevo registro con `docker`. Cuando se le solicite, proporcione la contraseña que ha recuperado.

```bash
docker login <registry_name>.azurecr.io -u <registry_name>
```

Inserte la imagen de Docker en el registro.

```bash
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="create-web-app-with-uploaded-image"></a>Creación de la aplicación web con la imagen cargada

En este paso, creará una aplicación en Azure App Service y la configurará para usar la imagen de Docker cargada en Azure Container Registry.

### <a name="create-an-app-service-plan"></a>Creación de un plan de App Service

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Creación de una aplicación web

En Cloud Shell, cree una aplicación web en el plan de App Service *myAppServicePlan* con el comando [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create).

En el siguiente comando, reemplace el marcador de posición *\<app_name>* por un nombre de aplicación único. Este nombre forma parte de la dirección URL predeterminada de la aplicación web, por lo que el nombre debe ser único en todas las aplicaciones de Azure App Service.

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-container-image-name "<registry_name>.azurecr.io/flask-postgresql-sample"
```

Cuando se ha creado la aplicación web, la CLI de Azure muestra información similar a la del ejemplo siguiente:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-environment-variables"></a>Configuración de las variables de entorno

En este mismo tutorial, ya ha definido variables de entorno para conectarse a su base de datos de PostgreSQL.

En App Service, las variables de entorno se establecen como _valores de aplicación_ mediante el comando [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set).

En el siguiente ejemplo se especifican los datos de conexión de la base de datos en forma de valores de configuración de aplicación. También se usa la variable *WEBSITES_PORT* al puerto del contenedor 5000, lo que permite que el contenedor reciba el tráfico HTTP en el puerto 80.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" WEBSITES_PORT=5000
```

### <a name="configure-custom-container-deployment"></a>Configuración de la implementación de contenedor personalizada

Aunque ya haya especificado el nombre de la imagen de contenedor, debe especificar la dirección URL de registro personalizada y las credenciales de usuario. En Cloud Shell, ejecute el comando [az webapp config container set](/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set).

```azurecli-interactive
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

En Cloud Shell, reinicie la aplicación. El reinicio garantiza que se aplican todas las opciones de configuración y que el contenedor más reciente se extrae del registro.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-to-the-azure-web-app"></a>Navegación hasta la aplicación web de Azure 

Vaya a la aplicación web implementada. 

```bash 
http://<app_name>.azurewebsites.net 
```

> [!NOTE]
> La aplicación web tarda algún tiempo en iniciarse porque el contenedor se debe descargar y ejecutar la primera vez que se solicita la aplicación. Si ve un error al principio después de mucho tiempo, actualice la página.

Ve a los invitados que registró anteriormente y que se guardaron en la base de datos de producción de Azure en el paso anterior.

![Aplicación Python Flask basada en contenedor Docker que se ejecuta de forma local](./media/tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**¡Enhorabuena!** Va a ejecutar una aplicación Python en Web App for Containers.

## <a name="update-data-model-and-redeploy"></a>Actualización del modelo de datos y nueva implementación

En este paso, se agrega el número de asistentes al registro de cada evento mediante la actualización del modelo `Guest`.

En la ventana de terminal local, extraiga del repositorio la versión *0.2-migration* con el siguiente comando de Git:

```bash
git checkout tags/0.2-migration
```

En esta versión ya se han realizado los cambios necesarios en las vistas, los controladores y el modelo. También incluye una migración de base de datos generada mediante *alembic* (`flask db migrate`). Puede ver todos los cambios realizados mediante el siguiente comando de Git:

```bash
git diff 0.1-initialapp 0.2-migration
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

![Aplicación Python Flask basada en contenedor Docker que se ejecuta de forma local](./media/tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Publicación de los cambios en Azure

En la ventana de terminal local, compile la nueva imagen de Docker e insértela en el registro.

```bash
cd ..
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

En Cloud Shell, reinicie la aplicación para asegurarse de que el contenedor más reciente se ha extraído del registro.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

Vaya a la aplicación web de Azure y vuelva a probar la nueva funcionalidad. Cree otro registro de eventos.

```bash 
http://<app_name>.azurewebsites.net 
```

![Aplicación Docker Python Flask en Azure App Service](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Administración de la aplicación web de Azure

Vaya a [Azure Portal](https://portal.azure.com) para ver la aplicación web que ha creado.

En el menú izquierdo, haga clic en **App Services**, a continuación, haga clic en el nombre de la aplicación web de Azure.

![Navegación desde el portal a la aplicación web de Azure](./media/tutorial-docker-python-postgresql-app/app-resource.png)

De forma predeterminada, el portal muestra la página **Información general**. Esta página proporciona una visión del funcionamiento de la aplicación. En este caso, también puede realizar tareas de administración básicas como examinar, detener, iniciar, reiniciar y eliminar. Las pestañas del lado izquierdo de la página muestran las diferentes páginas de configuración que puede abrir.

![Página de App Service en Azure Portal](./media/tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a>Pasos siguientes

Vaya al siguiente tutorial para aprender a asignar un nombre DNS personalizado a una aplicación web.

> [!div class="nextstepaction"]
> [Asignar un nombre DNS personalizado a Azure Web Apps](../app-service-web-tutorial-custom-domain.md)
