---
title: Uso de la dirección IP de IoT Hub | Microsoft Docs
description: Aprenda a consultar la dirección IP de IoT Hub y sus propiedades. La dirección IP de IoT Hub puede cambiar durante determinados escenarios, como la recuperación ante desastres o la conmutación por error regional.
author: philmea
ms.author: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/29/2019
ms.openlocfilehash: 7af40404550fb78af891563d8256f23620781b24
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71841529"
---
# <a name="understanding-the-ip-address-of-your-iot-hub"></a>Uso de la dirección IP de IoT Hub

La dirección IP de su centro de IoT es un punto de conexión público con equilibrio de carga para el centro y NO una dirección IP dedicada. La dirección viene determinada por el intervalo de direcciones de red de la región de Azure donde se implementa. Esta dirección IP está sujeta a cambios sin previo aviso. Las actualizaciones de red del centro de datos, la recuperación ante desastres del centro de datos o la conmutación por error regional de un centro de IoT pueden cambiar la dirección IP de IoT Hub. Consulte [Alta disponibilidad y recuperación ante desastres de IoT Hub](iot-hub-ha-dr.md) para más información sobre la conmutación por error regional y la disponibilidad de Azure IoT Hub.

La dirección IP de su centro de IoT cambia después de una conmutación por error a otra región. Para probar esta funcionalidad, puede seguir el tutorial [Realización de una conmutación por error manual de un centro de IoT](tutorial-manual-failover.md).

## <a name="discover-your-iot-hub-ip-address"></a>Detección de la dirección IP de IoT Hub

La dirección IP de IoT Hub se puede detectar mediante una búsqueda inversa de DNS en el CNAME ([*iot-hub-name*].azure-devices.net). Puede usar **nslookup** para comprobar la dirección IP de una instancia de IoT Hub:

```cmd/sh
nslookup {YourIoTHubName}.azure-devices.net
```

Esta dirección IP puede cambiar sin previo aviso. En un escenario de conmutación por error o de recuperación ante desastres, tendrá que sondear la dirección IP de IoT Hub en la región secundaria.

## <a name="outbound-firewall-rules-for-iot-hub"></a>Reglas de firewall de salida para IoT Hub

Pruebe a crear reglas de firewall y filtre por el dominio o el nombre de host de IoT Hub. Si solo puede permitir el tráfico saliente a direcciones específicas, sondee la dirección IP de IoT Hub periódicamente y actualice las reglas de firewall.

## <a name="support-for-ipv6"></a>Compatibilidad con IPv6 

Actualmente, IPv6 no se admite en IoT Hub.