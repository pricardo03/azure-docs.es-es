---
title: archivo de inclusión
description: archivo de inclusión
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 06/25/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: bacafdc8f7fd8e206335f3be0a086df1c54f1081
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38746716"
---
Cree una identidad para el dispositivo simulado, con el fin de que pueda comunicarse con su centro de IoT. Dado que los dispositivos de IoT Edge se comportan y se pueden administrar de manera diferente a los dispositivos de IoT típicos, declare que este es un dispositivo de IoT Edge desde el principio. 

1. En Azure Portal, navegue hasta el centro de IoT.
1. Seleccione **IoT Edge** y, a continuación, **Agregar dispositivo IoT Edge**.

   ![Agregar dispositivo de IoT Edge](./media/iot-edge-register-device/add-device.png)

1. Asigne el dispositivo simulado un identificador de dispositivo único.
1. Seleccione **Guardar** para agregar su dispositivo.
1. Seleccione el dispositivo nuevo en la lista de dispositivos.
1. Copie el valor de **Cadena de conexión: clave principal** y guárdelo. Este valor se usará para configurar el runtime de IoT Edge en la sección siguiente. 

