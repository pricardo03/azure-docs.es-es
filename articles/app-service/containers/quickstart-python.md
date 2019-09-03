---
title: 'Creación de una aplicación de Python en Linux: Azure App Service | Microsoft Docs'
description: Implemente su primera aplicación Hola mundo de Python en Azure App Service en Linux en cuestión de minutos.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 08/23/2019
ms.author: cephalin
ms.openlocfilehash: d4f1cd5193f29e929c822966ca427c6e6ebf67de
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70070975"
---
# <a name="create-a-python-app-in-azure-app-service-on-linux"></a>Creación de una aplicación de Python en Azure App Service en Linux

En este inicio rápido se implementa una aplicación Python en [App Service en Linux](app-service-linux-intro.md) que proporciona un servicio de hospedaje de sitios web muy escalable y con aplicación automática de revisiones. En Azure Cloud Shell basado en el explorador interactivo, la interfaz de la línea de comandos de Azure (la [CLI de Azure](/cli/azure/install-azure-cli)) sirve para seguir los pasos de uso en un equipo Mac, Linux o Windows.

![Aplicación de ejemplo que se ejecuta en Azure](media/quickstart-python/hello-world-in-browser.png)

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido:

* <a href="https://www.python.org/downloads/" target="_blank">Instalación de Python 3.7</a>
* <a href="https://git-scm.com/" target="_blank">Instalación de Git</a>
* Una suscripción de Azure. Si no tiene una, cree [una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.

## <a name="download-the-sample-locally"></a>Descarga local del código

En una ventana de terminal, ejecute los comandos siguientes para clonar la aplicación de ejemplo en la máquina local y desplazarse al directorio con el código de ejemplo.

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

cd $HOME/quickstart
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

En el siguiente ejemplo, reemplace `<app-name>` por un nombre único global de aplicación (*los caracteres válidos son `a-z`, `0-9` y `-`* ).

```bash
cd python-docs-hello-world

az webapp up -n <app-name>
```

Este comando puede tardar varios minutos en ejecutarse. Durante la ejecución, muestra información similar a la del ejemplo siguiente:

```json
The behavior of this command has been altered by the following extension: webapp
Creating Resource group 'appsvc_rg_Linux_CentralUS' ...
Resource group creation complete
Creating App service plan 'appsvc_asp_Linux_CentralUS' ...
App service plan creation complete
Creating app '<app-name>' ....
Webapp creation complete
Creating zip with contents of dir /home/username/quickstart/python-docs-hello-world ...
Preparing to deploy contents to app.
All done.
{
  "app_url": "https:/<app-name>.azurewebsites.net",
  "location": "Central US",
  "name": "<app-name>",
  "os": "Linux",
  "resourcegroup": "appsvc_rg_Linux_CentralUS ",
  "serverfarm": "appsvc_asp_Linux_CentralUS",
  "sku": "BASIC",
  "src_path": "/home/username/quickstart/python-docs-hello-world ",
  "version_detected": "-",
  "version_to_create": "python|3.7"
}
```

[!INCLUDE [AZ Webapp Up Note](../../../includes/app-service-web-az-webapp-up-note.md)]

## <a name="browse-to-the-app"></a>Navegación hasta la aplicación

Vaya a la aplicación implementada mediante el explorador web.

```bash
http://<app-name>.azurewebsites.net
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

Vuelva a implementar la aplicación con el comando [`az webapp up`](/cli/azure/webapp#az-webapp-up). Sustituya el nombre de la aplicación por `<app-name>` y especifique una ubicación para `<location-name>` (mediante uno de los valores que muestra el comando [`az account list-locations`](/cli/azure/appservice?view=azure-cli-latest.md#az-appservice-list-locations)).

```bash
az webapp up -n <app-name> -l <location-name>
```

Una vez que la implementación haya finalizado, vuelva a la ventana del explorador que abrió en el paso **Navegación hasta la aplicación** y actualice la página.

![Aplicación de ejemplo actualizada que se ejecuta en Azure](media/quickstart-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-app"></a>Administración de la nueva aplicación de Azure

Vaya a <a href="https://portal.azure.com" target="_blank">Azure Portal</a> para administrar la aplicación que ha creado.

En el menú izquierdo, seleccione **App Services** y, después, el nombre de la aplicación de Azure.

![Navegación en el portal a la aplicación de Azure](./media/quickstart-python/app-service-list.png)

Verá la página de información general de la aplicación. En este caso, puede realizar tareas de administración básicas como examinar, detener, iniciar, reiniciar y eliminar.

![Página de App Service en Azure Portal](media/quickstart-python/app-service-detail.png)

El menú izquierdo proporciona distintas páginas para configurar la aplicación. 

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Aplicación web Python (Django) con PostgreSQL](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Configuración de una aplicación de Python](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Tutorial: Ejecución de la aplicación Python en un contenedor personalizado](tutorial-custom-docker-image.md)
