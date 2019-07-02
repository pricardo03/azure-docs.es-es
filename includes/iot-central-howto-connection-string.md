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
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186441"
---
1. Use la utilidad de línea de comandos `dps-keygen` para generar una cadena de conexión:

    Para instalar la utilidad [generadora de claves](https://github.com/Azure/dps-keygen), ejecute el comando siguiente:

    ```cmd/sh
    npm i -g dps-keygen
    ```

1. Para generar una cadena de conexión, ejecute el siguiente comando con los detalles de conexión que anotó anteriormente:

    ```cmd/sh
    dps-keygen -di:<Device ID> -dk:<Primary or Secondary Key> -si:<Scope ID>
    ```

1. Copie la cadena de conexión desde la salida de `dps-keygen` para usarla en el código del dispositivo.