---
title: 'Creación de una aplicación de Python en Linux: Azure App Service | Microsoft Docs'
description: Implemente su primera aplicación Hola mundo de Python en Azure App Service en Linux en cuestión de minutos.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 02/08/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 14678ed789b611c0226d98fe11b3c9bacb993208
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "55991487"
---
# <a name="create-a-python-app-in-azure-app-service-on-linux-preview"></a>Creación de una aplicación de Python en Azure App Service en Linux (versión preliminar)

[App Service en Linux](app-service-linux-intro.md) proporciona un servicio de hospedaje web muy escalable y con aplicación automática de revisiones utilizando el sistema operativo Linux. En este inicio rápido se muestra cómo implementar una aplicación de Python sobre la imagen integrada de Python (versión preliminar) en App Service en Linux mediante la [CLI de Azure](/cli/azure/install-azure-cli).

Estos pasos se pueden realizar en este artículo con una máquina Mac, Windows o Linux.

![Aplicación de ejemplo que se ejecuta en Azure](media/quickstart-python/hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido:

* <a href="https://www.python.org/downloads/" target="_blank">Instalación de Python 3.7</a>
* <a href="https://git-scm.com/" target="_blank">Instalación de Git</a>

## <a name="download-the-sample-locally"></a>Descarga local del código

En la ventana de terminal, ejecute los comandos siguientes para clonar la aplicación de ejemplo en el equipo local y desplazarse al directorio con el código de ejemplo.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
cd python-docs-hello-world
```

El repositorio contiene un *application.py*, que indica a App Service que el repositorio contiene una aplicación de Flask. Para más información, consulte el artículo sobre [las personalizaciones y el proceso de inicio del contenedor](how-to-configure-python.md).

## <a name="run-the-app-locally"></a>Ejecución de la aplicación de forma local

Ejecute la aplicación localmente para ver cómo debería ser si se implementara en Azure. Abra una ventana de terminal y use los comandos siguientes para instalar las dependencias necesarias e iniciar el servidor de desarrollo integrado. 

```bash
# In Bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
FLASK_APP=application.py flask run

# In PowerShell
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
Set-Item Env:FLASK_APP ".\application.py"
flask run
```

Abra un explorador web y vaya a la aplicación de ejemplo en `http://localhost:5000/`.

Verá el mensaje **Hola mundo** desde la aplicación de ejemplo mostrada en la página.

![Aplicación de ejemplo que se ejecuta localmente](media/quickstart-python/hello-world-in-browser.png)

En la ventana de terminal, presione **Ctrl + C** para salir del servidor web.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="download-the-sample"></a>Descarga del ejemplo

En Cloud Shell, cree un directorio de inicio rápido y luego cambie a él.

```bash
mkdir quickstart

cd quickstart
```

A continuación, ejecute el comando siguiente para clonar el repositorio de la aplicación de ejemplo en el directorio de inicio rápido.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

Durante la ejecución, muestra información similar a la del ejemplo siguiente:

```bash
Cloning into 'python-docs-hello-world'...
remote: Enumerating objects: 43, done.
remote: Total 43 (delta 0), reused 0 (delta 0), pack-reused 43
Unpacking objects: 100% (43/43), done.
Checking connectivity... done.
```

## <a name="create-a-web-app"></a>Creación de una aplicación web

Cambie al directorio que contiene el código de ejemplo y ejecute el comando `az webapp up`.

En el siguiente ejemplo, reemplace <app_name > por un nombre único.

```bash
cd python-docs-hello-world

az webapp up -n <app_name>
```

Este comando puede tardar varios minutos en ejecutarse. Durante la ejecución, muestra información similar a la del ejemplo siguiente:

```json
The behavior of this command has been altered by the following extension: webapp
Creating Resource group 'appsvc_rg_Linux_CentralUS' ...
Resource group creation complete
Creating App service plan 'appsvc_asp_Linux_CentralUS' ...
App service plan creation complete
Creating app '<app_name>' ....
Webapp creation complete
Creating zip with contents of dir /home/username/quickstart/python-docs-hello-world ...
Preparing to deploy contents to app.
All done.
{
  "app_url": "https:/<app_name>.azurewebsites.net",
  "location": "Central US",
  "name": "<app_name>",
  "os": "Linux",
  "resourcegroup": "appsvc_rg_Linux_CentralUS ",
  "serverfarm": "appsvc_asp_Linux_CentralUS",
  "sku": "BASIC",
  "src_path": "/home/username/quickstart/python-docs-hello-world ",
  "version_detected": "-",
  "version_to_create": "python|3.7"
}
```

El comando `az webapp up` realiza las acciones siguientes:

- Crear un grupo de recursos predeterminado.

- Crear un plan de App Service predeterminado.

- Crear una aplicación con el nombre especificado.

- [Implementar con ZIP](https://docs.microsoft.com/azure/app-service/deploy-zip) archivos desde el directorio de trabajo actual a la aplicación.

## <a name="browse-to-the-app"></a>Navegación hasta la aplicación

Vaya a la aplicación implementada mediante el explorador web.

```bash
http://<app_name>.azurewebsites.net
```

El código de ejemplo de Python se ejecuta en App Service en Linux con una imagen integrada.

![Aplicación de ejemplo que se ejecuta en Azure](media/quickstart-python/hello-world-in-browser.png)

**¡Enhorabuena!** Ha implementado la primera aplicación de Python en App Service en Linux.

## <a name="update-locally-and-redeploy-the-code"></a>Actualización local y nueva implementación del código

En Cloud Shell, escriba `code application.py` para abrir el editor de Cloud Shell.

![Code application.py](media/quickstart-python/code-applicationpy.png)

 Realice un pequeño cambio en el texto en la llamada a `return`:

```python
return "Hello Azure!"
```

Guarde los cambios y salga del editor. Use el comando `^S` para guardar y `^Q` para salir.

Ahora va a volver a implementar la aplicación. Sustituya `<app_name>` por su aplicación.

```bash
az webapp up -n <app_name>
```

Una vez que la implementación haya finalizado, vuelva a la ventana del explorador que abrió en el paso **Navegación hasta la aplicación** y actualice la página.

![Aplicación de ejemplo actualizada que se ejecuta en Azure](media/quickstart-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-app"></a>Administración de la nueva aplicación de Azure

Vaya a <a href="https://portal.azure.com" target="_blank">Azure Portal</a> para administrar la aplicación que ha creado.

En el menú izquierdo, haga clic en **App Services** y, luego, en el nombre de la aplicación de Azure.

![Navegación en el portal a la aplicación de Azure](./media/quickstart-python/app-service-list.png)

Verá la página de información general de la aplicación. En este caso, puede realizar tareas de administración básicas como examinar, detener, iniciar, reiniciar y eliminar.

![Página de App Service en Azure Portal](media/quickstart-python/app-service-detail.png)

El menú izquierdo proporciona distintas páginas para configurar la aplicación. 

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Pasos siguientes

La imagen integrada de Python en App Service en Linux está actualmente en versión preliminar y se puede personalizar el comando que se usa para iniciar la aplicación. También se pueden crear aplicaciones Python de producción mediante un contenedor personalizado.

> [!div class="nextstepaction"]
> [Python con PostgreSQL](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Configuración de un comando de inicio personalizado](how-to-configure-python.md#customize-startup-command)

> [!div class="nextstepaction"]
> [Solución de problemas](how-to-configure-python.md#troubleshooting)

> [!div class="nextstepaction"]
> [Uso de imágenes personalizadas](tutorial-custom-docker-image.md)
