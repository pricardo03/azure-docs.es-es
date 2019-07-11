---
title: Descripción del módulo de seguridad de Azure Security Center for IoT para IoT Edge | Microsoft Docs
description: Conozca la arquitectura y las capacidades del módulo de seguridad de Azure Security Center for IoT para IoT Edge.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: e78523ae-d70a-456a-818d-f8b1b025d7cb
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 4581f66a3401764237621bee86228aac724ec0af
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67616453"
---
# <a name="azure-iot-edge-security-module"></a>Módulo de seguridad de Azure Security Center for IoT

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla en cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/) proporciona capacidades de enorme eficacia para administrar y procesar flujos de trabajo empresariales en el perímetro.
El papel esencial que IoT Edge desempeña en entornos de IoT lo hace especialmente atrayente para atacantes malintencionados.

El módulo de seguridad de Azure Security Center (ASC) for IoT proporciona una solución de seguridad completa para dispositivos IoT Edge.
El módulo de ASC for IoT recopila, agrega y analiza los datos de seguridad sin procesar del sistema operativo y del sistema de contenedores para generar alertas y recomendaciones de seguridad que requieren acción.

Al igual que los agentes de seguridad de ASC for IoT de los dispositivos IoT, el módulo de ASC for IoT Edge se puede personalizar profundamente a través de su módulo gemelo.
Vea [Configurar agentes de seguridad](how-to-agent-configuration.md) para obtener más información.

El módulo de seguridad de ASC for IoT para IoT Edge ofrece las siguientes características:

- Recopila eventos de seguridad sin procesar del sistema operativo subyacente (Linux) y de los sistemas de contenedor de IoT Edge.
  
  Vea [Configurar agentes de seguridad](how-to-agent-configuration.md) para obtener más información sobre los recopiladores de datos de seguridad disponibles.

- Análisis de manifiestos de implementación de IoT Edge.

- Agrega eventos de seguridad sin procesar a los mensajes enviados a través de [IoT Edge Hub](https://docs.microsoft.com/azure/iot-edge/iot-edge-runtime#iot-edge-hub).

- Quita la configuración usando el módulo de seguridad gemelo.

  Vea [Configurar agentes de seguridad](how-to-agent-configuration.md) para obtener más información.

El módulo de seguridad de ASC for IoT para IoT Edge se ejecuta en un modo con privilegios en IoT Edge.
El modo con privilegios es necesario para permitir que el módulo supervise el sistema operativo y otros módulos de IoT Edge.

## <a name="agent-supported-platforms"></a>Plataformas compatibles con el agente

El módulo de seguridad de ASC for IoT para IoT Edge solo está disponible en Linux.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha conocido la arquitectura y las capacidades del módulo de seguridad de ASC for IoT para IoT Edge.

Para seguir obteniendo información sobre la implementación de ASC for IoT, lea los siguientes artículos:

- Implementar el [módulo de seguridad de IoT Edge](how-to-deploy-edge.md)
- Obtenga información sobre cómo [configurar el módulo de seguridad](how-to-agent-configuration.md)
- Repase los [requisitos previos de servicio](service-prerequisites.md) de ASC for IoT
- Obtenga información sobre cómo [habilitar el servicio ASC for IoT en IoT Hub](quickstart-onboard-iot-hub.md)
- Obtenga información sobre el servicio en las [preguntas más frecuentes de ASC for IoT](resources-frequently-asked-questions.md)