---
title: archivo de inclusión
description: archivo de inclusión
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 01/28/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 8439da94c770bee313a1ae1d1da5df30683cd2ad
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155711"
---
## <a name="cli-shell"></a>Shell de la CLI

Para ejecutar comandos de la CLI, se recomienda usar [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest). **Cloud Shell** es un shell interactivo gratuito que se puede usar para ejecutar los pasos de este artículo. Cloud Shell incluye herramientas comunes de Azure preinstaladas y configuradas para que las use con su cuenta. Ofrece la flexibilidad de poder elegir la experiencia de shell que mejor se adapte a su forma de trabajar. Los usuarios de Linux pueden elegir una experiencia de Bash, mientras que los usuarios de Windows pueden optar por PowerShell.

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 

### <a name="login"></a>Inicio de sesión

Para empezar a usar el shell de la CLI (tanto en la nube como localmente), ejecute `az login` para crear una conexión con Azure.

Si la CLI puede abrir el explorador predeterminado, lo hará y cargará una página de inicio de sesión. De lo contrario, deberá abrir una página del explorador y seguir las instrucciones de la línea de comandos para especificar un código de autorización después de ir a https://aka.ms/devicelogin en el explorador.

### <a name="specify-location-of-files"></a>Especificación de la ubicación de los archivos

Muchos de los comandos de la CLI de Media Services permiten usar un parámetro con un nombre de archivo. Si usa **Cloud Shell**, puede cargar el archivo para su unidad en la nube (con Bash o PowerShell). 

![Carga de archivos]

Si usa una CLI local o **Cloud Shell**, deberá especificar la ruta de acceso de archivo según el sistema operativo o Cloud Shell (Bash o PowerShell) que esté usando. A continuación se muestran algunos ejemplos:

Ruta de acceso relativa al archivo de metadatos (todos los sistemas operativos)

* `@"mytestfile.json"`
* `@"../mytestfile.json"`

Ruta de acceso absoluta del archivo en el sistema operativo Windows y Linux o Mac

* `@ "/usr/home/mytestfile.json"`
*   `@"c:\tmp\user\mytestfile.json"`

Use `{file}` si el comando pide una ruta de acceso al archivo. Por ejemplo, `az ams transform create -a amsaccount -g resourceGroup -n custom --preset .\customPreset.json`. <br/> Use `@{file}` si el comando va a cargar el archivo especificado. Por ejemplo, `az ams account-filter create -a amsaccount -g resourceGroup -n filterName --tracks @tracks.json`.

[Carga de archivos]: ./media/media-services-cli/upload-download-files.png
