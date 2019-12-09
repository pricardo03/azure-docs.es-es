---
title: 'Inicio rápido: Configuración del SDK de Voz para la plataforma Python (servicio Voz)'
titleSuffix: Azure Cognitive Services
description: Use esta guía para configurar la plataforma para usar Python con el SDK del servicio de voz.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/09/2019
ms.author: erhopf
ms.openlocfilehash: 095c591f63cb228b48681ffc67499820631bbaf5
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74816467"
---
En esta guía se muestra cómo instalar el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para Python.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="supported-operating-systems"></a>Sistemas operativos compatibles

- El paquete del SDK de Voz de Python está disponible para estos sistemas operativos:
  - Windows: x64 y x86
  - Mac: macOS X versión 10.12 o posterior
  - Linux: Ubuntu 16.04, Ubuntu 18.04, Debian 9 en x64

## <a name="prerequisites"></a>Requisitos previos

- Las plataformas Linux admitidas requerirán la instalación de determinadas bibliotecas (`libssl` para la compatibilidad con la capa de sockets seguros y `libasound2` para la compatibilidad con el audio). Consulte a continuación su distribución para saber cuáles son los comandos necesarios para instalar las versiones correctas de estas bibliotecas.

  - En Ubuntu, ejecute los siguientes comandos para instalar los paquetes necesarios:

        ```sh
        sudo apt-get update
        sudo apt-get install build-essential libssl1.0.0 libasound2
        ```

  - En Debian 9, ejecute los siguientes comandos para instalar los paquetes necesarios:

        ```sh
        sudo apt-get update
        sudo apt-get install build-essential libssl1.0.2 libasound2
        ```

- En Windows, necesita [Microsoft Visual C++ Redistributable para Visual Studio 2019](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) para su plataforma. Tenga en cuenta que si es la primera vez que instala este paquete, puede que deba reiniciar Windows antes de seguir con esta guía.
- Y, por último, necesitará [Python 3.5 o posterior](https://www.python.org/downloads/). Para comprobar la instalación, abra un símbolo del sistema y escriba el comando `python --version` y compruebe el resultado. Si está instalado correctamente, obtendrá una respuesta "Python 3.5.1" o similar.

## <a name="install-the-speech-sdk-using-visual-studio-code"></a>Instalación del SDK de Voz mediante Visual Studio Code

1. Descargue e instale la última versión compatible de [Python](https://www.python.org/downloads/) para la plataforma, la versión 3.5 o posterior.
   - Los usuarios de Windows deben asegurarse de que seleccionan "Add Python to your PATH" (Agregar Python a su RUTA) durante el proceso de instalación.
1. Descargue e instale [Visual Studio Code](https://code.visualstudio.com/Download).
1. Abra Visual Studio Code e instale la extensión de Python. Seleccione **File** > **Preferences** > **Extensions** (Archivo > Preferencias > Extensiones) en el menú. Busque **Python** y haga clic en **Instalar**.

   ![Instalación de la extensión de Python](~/articles/cognitive-services/speech-service/media/sdk/qs-python-vscode-python-extension.png)

1. También desde dentro de Visual Studio Code, instale el paquete de Python del SDK de Voz desde la línea de comandos integrada:
   1. Abra un terminal (en los menús desplegables **Terminal** > **Nuevo terminal**)
   1. En el terminal que se abre, escriba el comando `python -m pip install azure-cognitiveservices-speech`.

Ya está listo para empezar a codificar en el SDK de Voz en Python y puede pasar a la sección [Pasos siguientes](#next-steps) a continuación. Si no está familiarizado con Visual Studio Code, consulte la [documentación más extensa sobre Visual Studio Code](https://code.visualstudio.com/docs). Para más información sobre Visual Studio Code y Python, consulte el [tutorial de Python para Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial).

## <a name="install-the-speech-sdk-using-the-command-line"></a>Instalación del SDK de Voz mediante la línea de comandos

Si no usa Visual Studio Code, el siguiente comando instala el paquete de Python desde [PyPI](https://pypi.org/) para el SDK de Voz. Si usa Visual Studio Code, pase a la siguiente subsección.

```sh
pip install azure-cognitiveservices-speech
```

Si está en macOS, es posible que tenga que ejecutar el siguiente comando para que funcione el comando `pip` anterior:

```sh
python3 -m pip install --upgrade pip
```

Una vez que haya usado correctamente `pip` para instalar `azure-cognitiveservices-speech`, puede usar el SDK de Voz importando el espacio de nombres en los proyectos de Python. Por ejemplo:

```py
import azure.cognitiveservices.speech as speechsdk
```

Esto se muestra con más detalle en los ejemplos de código que aparecen en la sección [Pasos siguientes](#next-steps) a continuación.

## <a name="support-and-updates"></a>Soporte técnico y actualizaciones

Las actualizaciones del paquete de Python del SDK de Voz se distribuirán mediante PyPI y se anunciarán en la página [Notas de la versión](~/articles/cognitive-services/speech-service/releasenotes.md).
Si hay disponible una nueva versión, puede actualizarse a ella con el comando `pip install --upgrade azure-cognitiveservices-speech`.
Para comprobar qué versión está instalada actualmente, inspeccione la variable `azure.cognitiveservices.speech.__version__`.

Si tiene un problema o falta una característica, consulte las [opciones de ayuda y soporte técnico](~/articles/cognitive-services/speech-service/support.md).

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [windows](../quickstart-list.md)]