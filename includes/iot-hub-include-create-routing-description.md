---
title: archivo de inclusión
description: archivo de inclusión
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/05/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: b640a3cb9382ad72bb48e06c6a7074c96409e2e4
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186936"
---
<!-- description of message routing used in the Azure CLI, PowerShell, and RM routing articles.-->

Va a enrutar mensajes a diferentes recursos en función de propiedades que el dispositivo simulado adjunta al mensaje. Los mensajes que no tengan una ruta personalizada se envían al punto de conexión predeterminado (mensajes y eventos). En el siguiente tutorial, enviará mensajes a IoT Hub y verá cómo se enrutan a los distintos destinos.

|value |Resultado|
|------|------|
|level="storage" |Se escribe en Azure Storage.|
|level="critical" |Es escribe en una cola de Service Bus. Una aplicación lógica recupera el mensaje de la cola y usa Office 365 para enviarlo por correo electrónico.|
|default |Se muestran estos datos con Power BI.|

El primer paso es configurar el punto de conexión al que se enrutarán los datos. El segundo paso es configurar la ruta de mensajes que usa ese punto de conexión. Después de configurar el enrutamiento, puede ver los puntos de conexión y las rutas de mensajes en el portal.