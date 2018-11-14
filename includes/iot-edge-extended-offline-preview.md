---
title: archivo de inclusión
description: iot edge
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: kgremban
ms.openlocfilehash: be4d82577584e83e29f2511d51256fda0970e917
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51264377"
---
## <a name="enabling-extended-offline-operation-preview"></a>Habilitación de la operación sin conexión extendida (versión preliminar)
Con la versión [v1.0.4](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4) del entorno de ejecución de Edge, el dispositivo Edge y los dispositivos conectados a él pueden configurarse para una operación sin conexión extendida. 

Con esta funcionalidad, los módulos locales o dispositivos de nivel inferior pueden volver a autenticarse con el dispositivo Edge según sea necesario y comunicarse entre sí mediante el uso de mensajes y métodos incluso cuando están desconectados del centro de IoT. Consulte esta [entrada de blog](https://aka.ms/iot-edge-offline) y el [artículo de conceptos](../articles/iot-edge/offline-capabilities.md) para obtener más detalles y el ámbito de esta funcionalidad.

Para permitir el estado sin conexión extendido en un escenario de puerta de enlace, establezca una relación principal-secundaria entre el dispositivo perimetral y los dispositivos de nivel inferior que se conectarán a él.

1. Desde la hoja de detalles del dispositivo Edge en el portal de IoT Hub, haga clic en el botón **Manage Child Devices (preview)** (Administrar dispositivos secundarios [versión preliminar]) en la barra de comandos superior.

1. Haga clic en el botón **+ Agregar**.

1. En la lista de dispositivos, seleccione los dispositivos secundarios y utilice la flecha derecha para seleccionar los que desea agregar como secundarios.

1. Haga clic en **ACEPTAR** para continuar.

El dispositivo Edge y sus dispositivos secundarios están ahora habilitados para una operación sin conexión extendida.  