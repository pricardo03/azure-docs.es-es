---
title: archivo de inclusión
description: archivo de inclusión
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 07/17/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 47d46cf4b400e29de8c526f750a10f57d6400220
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "68402671"
---
<!-- This contains intro text for the "Get an IoT hub connection string" section in the iot-hub-lang-lang-schedule-jobs.md files-->

En este artículo se creará un servicio de back-end que programa un trabajo para que invoque un método directo en un dispositivo, programa un trabajo para actualizar el dispositivo gemelo y supervisa el progreso de cada trabajo. Para realizar estas operaciones, el servicio necesita los permisos de **lectura del registro** y **escritura en el registro**. De manera predeterminada, todas las instancias de IoT Hub se crean con una directiva de acceso compartido denominada **registryReadWrite** que concede estos permisos.
