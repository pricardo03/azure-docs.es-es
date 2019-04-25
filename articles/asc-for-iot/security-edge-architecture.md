---
title: Descripción de Azure Security Center para el módulo de seguridad de IoT para IoT Edge | Microsoft Docs
description: Comprender la arquitectura y capacidades de Azure Security Center para el módulo de seguridad de IoT para IoT Edge.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: e78523ae-d70a-456a-818d-f8b1b025d7cb
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: c6ac3d9dbbb16caed51243fea852adea541b9f04
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60505416"
---
# <a name="azure-iot-edge-security-module"></a>Módulo de seguridad de Azure IoT Edge

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no se recomienda para worklo§1ads de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Azure IoT Edge](https://docs.microsoft.com/en-us/azure/iot-edge/) proporciona estas eficaces funciones para administrar y realizar flujos de trabajo empresariales en el perímetro.
La parte de clave que IoT Edge desempeña en entornos de IoT facilitan especialmente atractivo para actores malintencionados.

Azure Security Center (ASC) para el módulo de seguridad de IoT proporciona una solución de seguridad completa para el IoT Edge de dispositivos.
ASC para el módulo de IoT se recopila, se agrega y analiza los datos de seguridad sin procesar desde el sistema operativo y el sistema de contenedores en alertas y recomendaciones de seguridad que requieren acción.

Similar a ASC para los agentes de seguridad de IoT para dispositivos de IoT, los procesos de ASC para el módulo de IoT Edge son muy personalizable a través de su módulo gemelo.
Consulte [configurar el agente](how-to-agent-configuration.md) para obtener más información.

ASC para el módulo de seguridad de IoT para IoT Edge ofrece las siguientes características:

- Recopila eventos de seguridad sin procesar desde el sistema operativo subyacente (Linux) y los sistemas de contenedor de IoT Edge.
  
  Consulte [ASC para configurar el agente IoT](how-to-agent-configuration.md) para obtener más información sobre los recopiladores de datos de seguridad disponibles.

- Análisis de manifiestos de implementación de IoT Edge.

- Agrega los eventos de seguridad sin procesar en los mensajes enviados a través de [IoT Edge Hub](https://docs.microsoft.com/en-us/azure/iot-edge/iot-edge-runtime#iot-edge-hub).

- Quitar la configuración mediante el uso del módulo gemelo de seguridad.

  Consulte [configurar un ASC para el agente de IoT](how-to-agent-configuration.md) para obtener más información.

ASC para el módulo de seguridad de IoT para IoT Edge se ejecuta en un modo con privilegios en IoT Edge.
Se requiere el modo con privilegios para permitir que el módulo para supervisar el sistema operativo y otros módulos de IoT Edge.

## <a name="agent-supported-platforms"></a>Plataformas compatibles del agente

ASC para el módulo de seguridad de IoT para IoT Edge solo está disponible para Linux.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido sobre la arquitectura y capacidades de ASC para el módulo de seguridad de IoT para IoT Edge.

Para continuar la introducción a ASC para implementación de IoT, use los siguientes artículos:

- Implementar [módulo de seguridad de IoT Edge](how-to-deploy-edge.md)
- Obtenga información sobre cómo [configurar el módulo de seguridad](how-to-agent-configuration.md)
- Revise la ASC para IoT [requisitos previos de servicio](service-prerequisites.md)
- Obtenga información sobre cómo [habilitar ASC para el servicio de IoT en IoT Hub](quickstart-onboard-iot-hub.md)
- Más información sobre el servicio desde el [ASC para IoT preguntas más frecuentes](resources-frequently-asked-questions.md)