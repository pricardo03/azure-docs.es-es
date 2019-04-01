---
title: Comprender la versión preliminar de gemelos de Azure Security Center para el módulo de seguridad de IoT | Microsoft Docs
description: Obtenga información sobre el concepto de los módulos gemelos de seguridad y cómo se usan en Azure Security Center para IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: a5c25cba-59a4-488b-abbe-c37ff9b151f9
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 56bc93973bd2c080b13b97f1344cc165f742e757
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758402"
---
# <a name="security-module"></a>Módulo de seguridad

> [!IMPORTANT]
> Azure Security Center para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se explica cómo Azure Security Center (ASC) para IoT usa módulos y los dispositivos gemelos. 

## <a name="device-twins"></a>Dispositivos gemelos

Para las soluciones de IoT compiladas en Azure, los dispositivos gemelos desempeñan un papel clave en la administración de dispositivos y automatización de procesos.  

ASC para IoT ofrece integración completa con la plataforma de administración de dispositivos de IoT existente, lo que permite administrar el estado de seguridad del dispositivo, así como hacer uso de las capacidades de control de dispositivos existentes. La integración se consigue haciendo uso de IoT Hub gemelas mecanismo.  

Más información sobre el concepto de [dispositivos gemelos](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins) en Azure IoT Hub. 

## <a name="security-module-twins"></a>Módulos gemelos de seguridad

ASC para IoT mantiene a un módulo gemelo de seguridad para cada dispositivo en el servicio.
El módulo gemelo de seguridad contiene toda la información relevante para la seguridad de los dispositivos para cada dispositivo específico en la solución.
Propiedades de seguridad del dispositivo se mantienen en un gemelo de módulo de seguridad dedicado para la comunicación segura y para habilitar las actualizaciones y mantenimiento que requieren menos recursos.  

Consulte [gemelo de módulo de seguridad crear](quickstart-create-security-twin.md) y [configurar agentes de seguridad](how-to-agent-configuration.md) para obtener información sobre cómo crear, personalizar y configurar el gemelo. Consulte [módulos gemelos de comprensión](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins) para obtener más información sobre el concepto de módulos gemelos en IoT Hub. 
 

## <a name="see-also"></a>Vea también
- [ASC para la versión preliminar de IoT](overview.md)
- [Implementar a agentes de seguridad](how-to-deploy-agent.md)
- [Métodos de autenticación de seguridad del agente](concept-security-agent-authentication-methods.md)