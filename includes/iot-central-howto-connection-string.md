---
title: archivo de inclusión
description: archivo de inclusión
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 04/09/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: eae4b1e919270413d4ca6627964fad9c7f5bd9bf
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59805166"
---
1. Use el `dps-keygen` utilidad de línea de comandos para generar una cadena de conexión:

    Para instalar el [clave de la utilidad de generador](https://github.com/Azure/dps-keygen), ejecute el siguiente comando:

    ```cmd/sh
    npm i -g dps-keygen
    ```

1. Para generar una cadena de conexión, ejecute el comando siguiente con los detalles de conexión que anotó anteriormente:

    ```cmd/sh
    dps-keygen -di:<Device ID> -dk:<Primary or Secondary Key> -si:<Scope ID>
    ```

1. Copie la cadena de conexión desde el `dps-keygen` salida para usarla en el código del dispositivo.