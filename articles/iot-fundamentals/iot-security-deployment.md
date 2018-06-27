---
title: Protección de la implementación de Internet de las cosas | Microsoft Docs
description: En este artículo se detalla cómo proteger la implementación de IoT
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 01/17/2018
ms.author: dobett
ms.openlocfilehash: b90d916514e7b5ce566d4823b44d4b4ef575224d
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2018
ms.locfileid: "35774001"
---
[!INCLUDE [iot-secure-your-deployment](../../includes/iot-secure-your-deployment.md)]

## <a name="iot-solution-accelerator-cipher-suites"></a>Conjuntos de cifrado de aceleradores de soluciones de IoT

Los aceleradores de soluciones de IoT admiten los siguientes conjuntos de cifrado, en este orden.

| Conjunto de cifrado | Length |
| --- | --- |
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 (eq. RSA de 7680 bits) FS |256 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (eq. RSA de 3072 bits) FS |128 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (eq. RSA de 7680 bits) FS |256 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (eq. RSA de 3072 bits) FS |128 |
| TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384 (0x9d) |256 |
| TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256 (0x9c) |128 |
| TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA256 (0x3d) |256 |
| TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0x3c) |128 |
| TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA (0x35) |256 |
| TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA (0x2f) |128 |
| TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA (0xa) |112 |

## <a name="see-also"></a>Otras referencias

Puede informarse sobre la seguridad en IoT Hub en [Control del acceso a IoT Hub][lnk-devguide-security] en la Guía del desarrollador de IoT Hub.

[lnk-devguide-security]: /azure/iot-hub/iot-hub-devguide-security.md
