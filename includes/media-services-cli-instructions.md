---
title: archivo de inclusión
description: archivo de inclusión
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 01/25/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: b335cf996de41f4eea15af1899a0c6654c2f679f
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104990"
---
## <a name="open-cli-shell"></a>Apertura de un shell de la CLI

Para ejecutar comandos de la CLI, se recomienda usar [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest). **Cloud Shell** es un shell interactivo gratuito que se puede usar para ejecutar los pasos de este artículo. Cloud Shell incluye herramientas comunes de Azure preinstaladas y configuradas para que las use con su cuenta. Solo hay que seleccionar el botón Copiar para copiar el código, pegarlo en Cloud Shell y presionar Entrar para que se ejecute. Existen varias maneras de abrir Cloud Shell:

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 

### <a name="login"></a>Inicio de sesión

Para empezar a usar el shell de la CLI (tanto en la nube como localmente), ejecute `az login` para crear una conexión con Azure.

Si la CLI puede abrir el explorador predeterminado, lo hará y cargará una página de inicio de sesión. De lo contrario, deberá abrir una página del explorador y seguir las instrucciones de la línea de comandos para especificar un código de autorización después de ir a https://aka.ms/devicelogin en el explorador.

### <a name="specify-location-of-files"></a>Especificación de la ubicación de los archivos

Muchos de los comandos de la CLI de Media Services permiten usar un parámetro con un nombre de archivo. 

Si va a usar **Azure Cloud Shell**, cargue el archivo en **Azure Cloud Shell**. El botón de carga y descarga de archivos se encuentra en la parte superior de la ventana del shell. Luego, haga referencia al archivo de esta forma: `@{FileName}.` 

![Carga de archivos]

Si usa la CLI de Azure localmente, especifique la ruta de acceso completa al de archivo. Por ejemplo, `@c:\tracks.json`.


[Carga de archivos]: ./media/media-services-cli/upload-download-files.png
