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
ms.openlocfilehash: bb0b140dd1f42cae1d5d4bb670af8780d66c1f80
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60768638"
---
Cree una identidad para el dispositivo simulado, con el fin de que pueda comunicarse con su centro de IoT. Dado que los dispositivos de IoT Edge se comportan y se pueden administrar de manera diferente a los dispositivos de IoT típicos, declare que este es un dispositivo de IoT Edge desde el principio. 

1. En Azure Portal, vaya hasta el centro de IoT.
1. Seleccione **IoT Edge** y, a continuación, **Agregar dispositivo IoT Edge**.

   ![Agregar dispositivo de IoT Edge](./media/iot-edge-register-device/add-device.png)

1. Asigne el dispositivo simulado un identificador de dispositivo único.
1. Seleccione **Guardar** para agregar su dispositivo.
1. Seleccione el dispositivo nuevo en la lista de dispositivos.
1. Copie el valor de **Cadena de conexión: clave principal** y guárdelo. Este valor se usará para configurar el runtime de IoT Edge en la sección siguiente. 

