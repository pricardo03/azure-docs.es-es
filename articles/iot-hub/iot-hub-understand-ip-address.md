---
title: Uso de la dirección IP de IoT Hub | Microsoft Docs
description: Aprenda a consultar la dirección IP de IoT Hub y sus propiedades. La dirección IP de IoT Hub puede cambiar durante determinados escenarios, como la recuperación ante desastres o la conmutación por error regional.
author: philmea
ms.author: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 11/21/2019
ms.openlocfilehash: 5c77fb30ef60c1ad82d0a87442bc8af186c54321
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74383913"
---
# <a name="iot-hub-ip-addresses"></a>Direcciones IP de IoT Hub

Los prefijos de dirección IP de IoT Hub se publican periódicamente con la [etiqueta de servicio](../virtual-network/service-tags-overview.md) *AzureIoTHub*. Para garantizar un funcionamiento correcto, los dispositivos IoT deben tener conectividad de salida a los prefijos de dirección enumerados en la etiqueta de servicio *AzureIoTHub*. Los servicios de aplicaciones IoT deben tener además conectividad de salida a los prefijos de dirección enumerados en la etiqueta de servicio *EventHub*.


## <a name="best-practices"></a>Procedimientos recomendados

* Los prefijos de dirección IP de IoT Hub están sujetos a cambios. Estos cambios se publican periódicamente mediante etiquetas de servicio antes de entrar en vigor. Por lo tanto, es importante que desarrolle procesos para recuperar y usar con regularidad las etiquetas de servicio más recientes. Este proceso se puede automatizar mediante la [API de detección de etiquetas de servicio](../virtual-network/service-tags-overview.md#service-tags-in-on-premises).
* Use la etiqueta *AzureIoTHub.[nombre de la región]* para identificar prefijos de dirección IP que usan los puntos de conexión de IoT Hub en una región determinada. Para tener en cuenta la recuperación ante desastres de un centro de datos o la [conmutación por error regional](iot-hub-ha-dr.md), asegúrese de que también esté habilitada la conectividad a los prefijos de dirección IP de la región del par de replicación geográfica de su instancia de IoT Hub.


## <a name="support-for-ipv6"></a>Compatibilidad con IPv6 

Actualmente, IPv6 no se admite en IoT Hub.