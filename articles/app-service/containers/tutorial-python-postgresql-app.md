---
title: 'Compilación de una aplicación web Python con PostgreSQL en Linux: Azure App Service | Microsoft Docs'
description: Aprenda a ejecutar una aplicación Python controlada por datos en Azure, con conexión a una base de datos PostgreSQL.
services: app-service\web
documentationcenter: python
author: cephalin
manager: jeconnoc
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 11/29/2018
ms.author: beverst;cephalin
ms.custom: seodec18
ms.openlocfilehash: 8846ec386ad1776172ae1949b5e0f26e03ddf1df
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53337996"
---
# <a name="build-a-python-and-postgresql-web-app-in-azure-app-service"></a>Compilación de una aplicación web Python y PostgreSQL en Azure App Service

[App Service en Linux](app-service-linux-intro.md) proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático. En este tutorial se muestra cómo crear una aplicación web Python controlada por datos con PostgreSQL como back-end de base de datos. Cuando haya terminado, tendrá una aplicación Django que se ejecuta en App Service en Linux.

![Aplicación Python Django en App Service en Linux](./media/tutorial-python-postgresql-app/django-admin-azure.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una base de datos PostgreSQL en Azure
> * Conexión de una aplicación Python a PostgreSQL
> * Implementación de la aplicación en Azure
> * Visualización de los registros de diagnóstico
> * Administrar la aplicación en Azure Portal

También puede seguir los pasos de este artículo en macOS. Las instrucciones de Linux y Windows son las mismas en la mayoría de los casos, pero en este tutorial no se detallan las diferencias.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial:

1. [Instalación de Git](https://git-scm.com/)
2. [Instalación de Python](https://www.python.org/downloads/)
3. [Instalación y ejecución de PostgreSQL](https://www.postgresql.org/download/)

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

Cree una base de datos denominada *pollsdb* y configure un usuario de base de datos independiente denominado *manager* cuya contraseña será *supersecretpass*.

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

Escriba `\q` para salir del cliente de PostgreSQL.

<a name="step2"></a>

## <a name="create-local-python-app"></a>Creación de una aplicación Python local

En este paso, configurará el proyecto de Python Django local.

### <a name="clone-the-sample-app"></a>Clonación de la aplicación de ejemplo

Abra la ventana de terminal y use `CD` para cambiar a un directorio de trabajo.

Ejecute los comandos siguientes para clonar el repositorio de ejemplo.

```bash
git clone https://github.com/Azure-Samples/djangoapp.git
cd djangoapp
```

Este repositorio de ejemplo contiene una aplicación [Django](https://www.djangoproject.com/). Es la misma aplicación controlada por datos que obtendría si realiza el [tutorial de introducción que se encuentra en la documentación de Django](https://docs.djangoproject.com/en/2.1/intro/tutorial01/). En este tutorial no se enseña Django en sí, pero se muestra cómo implementar y ejecutar una aplicación Django (o cualquier otra aplicación Python controlada por datos) en App Service.

### <a name="configure-environment"></a>Configuración del entorno

Cree un entorno virtual de Python y use un script para establecer la configuración de la conexión de base de datos.

```bash
# Bash
python3 -m venv venv
source venv/bin/activate
source ./env.sh

# PowerShell
py -3 -m venv venv
venv\scripts\activate
.\env.ps1
```

Las variables de entorno definidas en *env.sh* y *env.ps1* se usan en _azuresite/settings.py_ para definir la configuración de la base de datos.

### <a name="run-app-locally"></a>Ejecución de la aplicación de forma local

Instale los paquetes necesarios, [ejecute migraciones de Django](https://docs.djangoproject.com/en/2.1/topics/migrations/) y [cree un usuario administrador](https://docs.djangoproject.com/en/2.1/intro/tutorial02/#creating-an-admin-user).

```bash
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
```

Una vez creado el usuario administrador, ejecute el servidor de Django.

```bash
python manage.py runserver
```

Cuando la aplicación se haya cargado completamente, verá algo similar al siguiente mensaje:

```bash
Performing system checks...

System check identified no issues (0 silenced).
October 26, 2018 - 10:54:59
Django version 2.1.2, using settings 'azuresite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

Vaya a `http://localhost:8000` en un explorador. Verá el mensaje `No polls are available.`. 

Vaya a `http://localhost:8000/admin` e inicie sesión mediante el usuario administrador que creó en el último paso. Haga clic en **Agregar** junto a **Preguntas** y cree una pregunta de sondeo con algunas opciones.

![Aplicación Python Django que se ejecuta de forma local](./media/tutorial-python-postgresql-app/django-admin-local.png)

Vaya a `http://localhost:8000` de nuevo y vea la pregunta de sondeo que se muestra.

La aplicación Django de ejemplo almacena datos de usuario en la base de datos. Si la adición de la pregunta de sondeo se realiza correctamente, la aplicación escribe datos en la base de datos de PostgreSQL local.

Para detener el servidor de Django en cualquier momento, escriba Ctrl+C en el terminal.

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

En Cloud Shell, ejecute de nuevo el comando para permitir el acceso desde el equipo local y reemplace *\<su_dirección_ip>* por [la dirección IP IPv4 local](https://www.whatsmyip.org/).

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=<your_ip_address> --end-ip-address=<your_ip_address> --name AllowLocalClient
```

## <a name="connect-python-app-to-production-database"></a>Conexión de la aplicación Python a la base de datos de producción

En este paso, conectará la aplicación Django de ejemplo al servidor de Azure Database for PostgreSQL que ha creado.

### <a name="create-empty-database-and-user-access"></a>Creación del acceso de usuario y la base de datos vacía

En Cloud Shell, conéctese a la base de datos mediante la ejecución del comando anterior. Cuando se le pida la contraseña de administrador, use la misma contraseña que especificó en [Creación de un servidor de Azure Database for PostgreSQL](#create-an-azure-database-for-postgresql-server).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Al igual que en su servidor local de Postgres, cree la base de datos y el usuario en el servidor Postgres de Azure.

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

Escriba `\q` para salir del cliente de PostgreSQL.

> [!NOTE]
> Es un procedimiento recomendado crear usuarios de bases de datos con permisos restringidos para aplicaciones específicas, en lugar de utilizar el usuario administrador. En este ejemplo, el usuario `manager` tiene plenos privilegios para _solo_ la base de datos `pollsdb`.

### <a name="test-app-connectivity-to-production-database"></a>Prueba de la conectividad de la aplicación a la base de datos de producción

En la ventana del terminal local, cambie las variables de entorno de la base de datos (que configuró anteriormente con la ejecución de *env.sh* o *env.ps1*):

```bash
# Bash
export DBHOST="<postgresql_name>.postgres.database.azure.com"
export DBUSER="manager@<postgresql_name>"
export DBNAME="pollsdb"
export DBPASS="supersecretpass"

# PowerShell
$Env:DBHOST = "<postgresql_name>.postgres.database.azure.com"
$Env:DBUSER = "manager@<postgresql_name>"
$Env:DBNAME = "pollsdb"
$Env:DBPASS = "supersecretpass"
```

Ejecute la migración de Django a la base de datos de Azure y cree un usuario administrador.

```bash
python manage.py migrate
python manage.py createsuperuser
```

Una vez creado el usuario administrador, ejecute el servidor de Django.

```bash
python manage.py runserver
```

Vaya a `http://localhost:8000` de nuevo. Verá el mensaje `No polls are available.` otra vez. 

Vaya a `http://localhost:8000/admin` e inicie sesión con el usuario administrador de antes y cree una pregunta de sondeo como ya hizo anteriormente.

![Aplicación Python Django que se ejecuta de forma local](./media/tutorial-python-postgresql-app/django-admin-local.png)

Vaya a `http://localhost:8000` de nuevo y vea la pregunta de sondeo que se muestra. La aplicación ya escribe datos en la base de datos de Azure.

## <a name="deploy-to-azure"></a>Implementar en Azure

En este paso, se implementará la aplicación Python conectada a Postgres en Azure App Service.

### <a name="configure-repository"></a>Configuración del repositorio

Django valida el encabezado `HTTP_HOST` en las solicitudes entrantes. Para que la aplicación Django funcione en App Service, debe agregar el nombre de dominio completo de la aplicación a los hosts permitidos. Abra _azuresite/settings.py_ y busque el valor `ALLOWED_HOSTS`. Cambie la línea a:

```python
ALLOWED_HOSTS = [os.environ['WEBSITE_SITE_NAME'] + '.azurewebsites.net', '127.0.0.1'] if 'WEBSITE_SITE_NAME' in os.environ else []
```

Como Django no admite [archivos estáticos en producción](https://docs.djangoproject.com/en/2.1/howto/static-files/deployment/), tendrá que habilitarlo manualmente. Para este tutorial, puede usar [WhiteNoise](https://whitenoise.evans.io/en/stable/). El paquete WhiteNoise ya está incluido en el archivo _requirements.txt_. Solo tiene que configurar Django para utilizarlo. 

En _azuresite/settings.py_, busque el valor `MIDDLEWARE` y agregue el middleware `whitenoise.middleware.WhiteNoiseMiddleware` a la lista, justo debajo del middleware `django.middleware.security.SecurityMiddleware`. El valor `MIDDLEWARE` debe tener este aspecto:

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',
    ...
]
```

Al final de _azuresite/settings.py_, agregue las líneas siguientes.

```python
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'

STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
```

Para más información sobre cómo configurar WhiteNoise, consulte la [documentación de WhiteNoise](https://whitenoise.evans.io/en/stable/).

> [!IMPORTANT]
> La sección de configuración de la base de datos ya sigue el procedimiento recomendado de seguridad que aconseja usar variables de entorno. Para conocer todas las recomendaciones de implementación, consulte [Documentación de Django: Lista de comprobación de implementación](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/).


Confirme los cambios en el repositorio.

```bash
git commit -am "configure for App Service"
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
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="pollsdb"
```

### <a name="push-to-azure-from-git"></a>Inserción en Azure desde Git

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash 
Counting objects: 7, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (7/7), done.
Writing objects: 100% (7/7), 775 bytes | 0 bytes/s, done.
Total 7 (delta 4), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '6520eeafcc'.
remote: Generating deployment script.
remote: Running deployment command...
remote: Python deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
. 
. 
. 
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git 
   06b6df4..6520eea  master -> master
```  

El servidor de implementación de App Service ve el archivo _requirements.txt_ en la raíz del repositorio y ejecuta la administración de paquetes de Python automáticamente después de `git push`.

### <a name="browse-to-the-azure-web-app"></a>Navegación hasta la aplicación web de Azure

Vaya a la aplicación web implementada. Tarda algún tiempo en iniciarse porque el contenedor se debe descargar y ejecutar la primera vez que se solicita la aplicación. Si la página agota el tiempo de espera o muestra un mensaje de error, espere unos minutos y actualice la página.

```bash
http://<app_name>.azurewebsites.net
```

Debería ver la pregunta de sondeo que creó anteriormente. 

App Service detecta un proyecto de Django en el repositorio mediante la búsqueda de un archivo _wsgi.py_ en cada subdirectorio, archivo que `manage.py startproject` crea de forma predeterminada. Cuando encuentra el archivo, carga la aplicación de Django. Para más información sobre cómo carga App Service aplicaciones Python, consulte [Configuración de una imagen de Python integrada](how-to-configure-python.md).

Vaya a `<app_name>.azurewebsites.net` e inicie sesión mediante el mismo usuario administrador que creó. Si lo desea, intente crear algunas preguntas de sondeo más.

![Aplicación Python Django que se ejecuta de forma local](./media/tutorial-python-postgresql-app/django-admin-azure.png)

**¡Enhorabuena!** Va a ejecutar una aplicación Python en App Service para Linux.

## <a name="access-diagnostic-logs"></a>Acceso a los registros de diagnóstico

En App Service en Linux, las aplicaciones se ejecutan dentro de un contenedor a partir de una imagen de Docker predeterminada. Puede acceder a los registros de consola generados desde el contenedor. Para obtener los registros, en primer lugar active registro de contenedores ejecutando el siguiente comando en Cloud Shell:

```azurecli-interactive
az webapp log config --name <app_name> --resource-group myResourceGroup --docker-container-logging filesystem
```

Una vez que se active el registro de contenedor, ejecute el siguiente comando para ver el flujo del registro:

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
```

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
> * Administrar la aplicación en Azure Portal

Vaya al siguiente tutorial para aprender a asignar un nombre DNS personalizado a una aplicación web.

> [!div class="nextstepaction"]
> [Asignar un nombre DNS personalizado a Azure Web Apps](../app-service-web-tutorial-custom-domain.md)

> [!div class="nextstepaction"]
> [Configuración una imagen de Python integrada y solución de problemas](how-to-configure-python.md)

