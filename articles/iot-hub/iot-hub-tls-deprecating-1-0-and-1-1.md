---
title: Desuso de TLS 1.0 y 1.1 en IoT Hub y Device Provisioning Service (DPS) | Microsoft Docs
description: Instrucciones sobre el desuso de TLS 1.0 y 1.1 y los cifrados admitidos en IoT Hub y DPS.
author: rezasherafat
ms.author: rezas
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: 8e2f8fd7f99c359949b02959cc442f2f3d3ebfff
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2020
ms.locfileid: "77912161"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub-and-device-provisioning-service"></a>Desuso de TLS 1.0 y 1.1 en IoT Hub y en Device Provisioning Service

Para proporcionar el mejor cifrado, IoT Hub y Device Provisioning Service (DPS) se van a migrar a Seguridad de la capa de transporte (TLS) 1.2 como el mecanismo de cifrado elegido para servicios y dispositivos de IoT. Como tal, la compatibilidad heredada con TLS 1.0 y TLS 1.1, así como varios cifrados heredados no recomendados, quedarán en desuso **el 1 de julio de 2020**.


## <a name="impact"></a>Impacto
En función de las circunstancias y configuraciones específicas de los clientes, el desuso de TLS 1.0 y 1.1 y los cifrados heredados no recomendados pueden ser un cambio impactante en los servicios y dispositivos de IoT que se comunican con IoT Hub o DPS. En algunos casos, los dispositivos y servicios que no son compatibles con estos cambios no podrán conectarse a IoT Hub o DPS después de la fecha de cierre mencionada anteriormente.


## <a name="supported-ciphers"></a>Cifrados compatibles

La escala de tiempo para la disponibilidad de varios cifrados utilizados en el protocolo de enlace TLS es la siguiente:

* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (actualmente compatible)
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (será compatible durante la segunda mitad de 2020)
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (será compatible durante la segunda mitad de 2020)
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (será compatible durante la segunda mitad de 2020)


## <a name="customer-feedback"></a>Comentarios del cliente

Si bien el cumplimiento de TLS 1.2 es una de las mejores opciones de cifrado de todo el sector y se habilitará según lo planeado, nos gustaría seguir conociendo las opiniones de los clientes sobre sus implementaciones específicas y las dificultades para adoptar TLS 1.2. Con esta finalidad, puede enviar sus comentarios a [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com).
