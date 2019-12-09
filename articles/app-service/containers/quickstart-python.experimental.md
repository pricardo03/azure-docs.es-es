---
title: 'Inicio rápido: Creación de una aplicación en Python para Linux'
description: Para empezar a trabajar con aplicaciones de Linux en Azure App Service, debe implementar su primera aplicación de Python en un contenedor Linux en App Service.
author: msangapu-msft
ms.topic: quickstart
ms.date: 08/23/2019
ms.custom: seodec18
experimental: false
experiment_id: 1e304dc9-5add-4b
ms.openlocfilehash: 7905b086ac390243f2fe02697f68c9b1c05ca28e
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74688994"
---
# <a name="create-a-python-app-in-azure-app-service-on-linux"></a>Creación de una aplicación de Python en Azure App Service en Linux

En este inicio rápido se implementa una aplicación Python en [App Service en Linux](app-service-linux-intro.md) que proporciona un servicio de hospedaje de sitios web muy escalable y con aplicación automática de revisiones. En Azure Cloud Shell basado en el explorador interactivo, la interfaz de la línea de comandos de Azure (la [CLI de Azure](/cli/azure/install-azure-cli)) sirve para seguir los pasos de uso en un equipo Mac, Linux o Windows.

![Aplicación de ejemplo que se ejecuta en Azure](media/quickstart-python/hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

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

En el siguiente ejemplo, reemplace *\<app_name>* por un nombre único global de aplicación (los caracteres válidos son `a-z`, `0-9` y `-`).

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

## <a name="update-and-redeploy-the-code"></a>Actualización del código y nueva implementación

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

> [!div class="nextstepaction"]
> [Tutorial: Aplicación de Python con PostgreSQL](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Configuración de una aplicación de Python](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Tutorial: Implementar desde el repositorio de contenedor privado](tutorial-custom-docker-image.md)
