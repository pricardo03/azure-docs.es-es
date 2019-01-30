---
title: 'Comportamiento de un dispositivo simulado en la solución de supervisión remota: Azure | Microsoft Docs'
description: Este artículo describe cómo usar JavaScript para definir el comportamiento de un dispositivo simulado en la solución de supervisión remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: c2151a4b1eb2a853ed343f6720b4f53af5e5e449
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54449527"
---
# <a name="implement-the-device-model-behavior"></a>Implementación del comportamiento de modelo del dispositivo

El artículo [Descripción del esquema de modelo del dispositivo](iot-accelerators-remote-monitoring-device-schema.md) describe el esquema que define un modelo de dispositivo simulado. Ese artículo hace referencia a dos tipos de archivo de JavaScript que implementan el comportamiento de un dispositivo simulado:

- Archivos JavaScript de **estado** que se ejecutan a intervalos fijos para actualizar el estado interno del dispositivo.
- Archivos JavaScript de **Método** que se ejecutan cuando la solución invoca un método en el dispositivo.

> [!NOTE]
> Los comportamientos del modelo del dispositivo son solo para dispositivos simulados que se hospedan en el servicio de simulación del dispositivo. Si quiere crear un dispositivo real, consulte [Conectar el dispositivo al acelerador de la solución Supervisión remota](iot-accelerators-connecting-devices.md).

En este artículo, aprenderá a:

>[!div class="checklist"]
> * Controlar el estado de un dispositivo simulado
> * Definición de cómo responde un dispositivo simulado a la llamada a un método desde la solución Supervisión remota
> * Depurar los scripts

[!INCLUDE [iot-accelerators-device-behavior](../../includes/iot-accelerators-device-behavior.md)]

## <a name="next-steps"></a>Pasos siguientes

En este artículo se describe cómo definir el comportamiento de su propio modelo de dispositivo simulado personalizado. En este artículo le mostramos cómo:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Controlar el estado de un dispositivo simulado
> * Definición de cómo responde un dispositivo simulado a la llamada a un método desde la solución Supervisión remota
> * Depurar los scripts

Ahora que ha aprendido a especificar el comportamiento de un dispositivo simulado, el siguiente paso que se propone es aprender a [crear un dispositivo simulado](iot-accelerators-remote-monitoring-create-simulated-device.md).

Para más información sobre la solución de supervisión remota para los desarrolladores, consulte:

* [Guía de referencia para desarrolladores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Guía de solución de problemas para desarrolladores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->
