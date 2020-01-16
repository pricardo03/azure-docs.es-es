---
title: Desuso de TLS 1.0 y 1.1 en IoT Hub y Device Provisioning Service (DPS) | Microsoft Docs
description: Instrucciones sobre el desuso de TLS 1.0 y 1.1 y los cifrados admitidos en IoT Hub y DPS.
author: rezas
ms.author: reza
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: f1ee3156e5639ae47d5bb323cf992586580668f5
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75476810"
---
# <a name="tls-10-and-11-deprecation-in-iot-hub-and-device-provisioning-service"></a>Desuso de TLS 1.0 y 1.1 en IoT Hub y en Device Provisioning Service

Para proporcionar el mejor cifrado, IoT Hub y Device Provisioning Service (DPS) se van a migrar a Seguridad de la capa de transporte (TLS) 1.2 como el mecanismo de cifrado elegido para servicios y dispositivos de IoT. Como tal, la compatibilidad heredada con TLS 1.0 y TLS 1.1, así como varios cifrados heredados no recomendados, quedarán en desuso **el 1 de julio de 2020**.


## <a name="impact"></a>Impacto
En función de las circunstancias y configuraciones específicas de los clientes, el desuso de TLS 1.0 y 1.1 y los cifrados heredados no recomendados pueden ser un cambio impactante en los servicios y dispositivos de IoT que se comunican con IoT Hub o DPS. En algunos casos, los dispositivos y servicios que no son compatibles con estos cambios no podrán conectarse a IoT Hub o DPS después de la fecha de cierre mencionada anteriormente.


## <a name="supported-ciphers"></a>Cifrados compatibles

Solo se permitirán los siguientes cifrados durante el protocolo de enlace TLS:

* TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
* TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
* TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
* TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384


## <a name="customer-feedback"></a>Comentarios del cliente

Si bien el cumplimiento de TLS 1.2 es una de las mejores opciones de cifrado de todo el sector y se habilitará según lo planeado, nos gustaría seguir conociendo las opiniones de los clientes sobre sus implementaciones específicas y las dificultades para adoptar TLS 1.2. Con esta finalidad, puede enviar sus comentarios a [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com).