---
title: archivo de inclusión
description: archivo de inclusión
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 07/24/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 103ad020b8d9f50ffe690f502b7cac08dab9237a
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640431"
---
* Una cuenta de Azure activa. (En caso de no tener ninguna, puede crear una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) en tan solo unos minutos).

* **Windows**

    * [Python  2.x o 3.x](https://www.python.org/downloads/). Asegúrese de usar la instalación de 32 bits o 64 bits en función del programa de instalación. Cuando se le solicite durante la instalación, asegúrese de agregar Python a la variable de entorno específica de la plataforma. Si usa Python 2.x, puede que necesite [instalar o actualizar *pip*, el sistema de administración de paquetes de Python](https://pip.pypa.io/en/stable/installing/).

    * Si usa el sistema operativo Windows, asegúrese de que tiene la versión adecuada del [paquete redistribuible de Visual C++](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) para permitir el uso de archivos DLL nativos de Python. Se recomienda usar la versión más reciente.

    * Si es necesario, instale el paquete [azure-iothub-device-client](https://pypi.org/project/azure-iothub-device-client/) mediante el comando   `pip install azure-iothub-device-client`.

    * Si es necesario, instale el paquete [azure-iothub-service-client](https://pypi.org/project/azure-iothub-service-client/) mediante el comando   `pip install azure-iothub-service-client`.

* **Mac OS**

    Por Mac OS, necesita Python 3.7.0 (o 2,7) + libboost-1.67 + curl 7.61.1 (todo instalado a través de Homebrew). Cualquier otra distribución o sistema operativo probablemente insertará versiones diferentes de boost y dependencias que no funcionarán, que darán lugar a un ImportError en el entorno en tiempo de ejecución.

    Los paquetes *pip* de `azure-iothub-service-client` y `azure-iothub-device-client` actualmente solo están disponibles para el sistema operativo Windows. Para Linux o Mac OS, consulte las secciones específicas de Mac OS y Linux en la publicación [Preparar el entorno de desarrollo para Python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md).

> [!NOTE]
> Se han producido varios informes de errores al importar iothub_client en un ejemplo. Para obtener más información sobre cómo tratar los problemas de **ImportError**, consulte [Dealing with ImportError issues](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues) (Tratamiento de problemas de ImportError).
