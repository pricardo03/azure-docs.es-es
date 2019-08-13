---
title: Información acerca de la versión preliminar del módulo de seguridad de Azure Security Center para IoT para IoT Edge | Microsoft Docs
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
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 6114fc768ad04ef812f6093d006ec9ad91b17af3
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596859"
---
# <a name="azure-iot-edge-security-module"></a>Módulo de seguridad de Azure Security Center for IoT

> [!IMPORTANT]
> El servicio Azure Security Center para IoT para IoT Edge, se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/) proporciona capacidades de enorme eficacia para administrar y procesar flujos de trabajo empresariales en el perímetro.
El papel esencial que IoT Edge desempeña en entornos de IoT lo hace especialmente atrayente para atacantes malintencionados.

El módulo de seguridad de Azure Security Center para IoT proporciona una solución de seguridad completa para dispositivos IoT Edge.
El módulo de Azure Security Center para IoT recopila, agrega y analiza los datos de seguridad sin procesar del sistema operativo y del sistema de contenedores para generar alertas y recomendaciones de seguridad que requieren acción.

Al igual que los agentes de seguridad de Azure Security Center para IoT para dispositivos IoT, el módulo de Azure Security Center para IoT Edge se puede personalizar en profundidad mediante su módulo gemelo.
Vea [Configurar agentes de seguridad](how-to-agent-configuration.md) para obtener más información.

El módulo de seguridad de Azure Security Center para IoT para IoT Edge ofrece las siguientes características:

- Recopila eventos de seguridad sin procesar del sistema operativo subyacente (Linux) y de los sistemas de contenedor de IoT Edge.
  
  Consulte [Configuración del agente de Azure Security Center para IoT](how-to-agent-configuration.md) para más información acerca de los recopiladores de datos de seguridad disponibles.

- Análisis de manifiestos de implementación de IoT Edge.

- Agrega eventos de seguridad sin procesar a los mensajes enviados a través de [IoT Edge Hub](https://docs.microsoft.com/azure/iot-edge/iot-edge-runtime#iot-edge-hub).

- Quita la configuración usando el módulo de seguridad gemelo.

  Consulte [Configuración del agente de Azure Security Center para IoT](how-to-agent-configuration.md) para más información.

El módulo de seguridad de Azure Security Center para IoT para IoT Edge se ejecuta en un modo con privilegios en IoT Edge.
El modo con privilegios es necesario para permitir que el módulo supervise el sistema operativo y otros módulos de IoT Edge.

## <a name="module-supported-platforms"></a>Plataformas admitidas por el módulo

El módulo de seguridad de Azure Security Center para IoT para IoT Edge solo está disponible en la actualidad para Linux. 

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha aprendido sobre la arquitectura y las funcionalidades del módulo de seguridad de Azure Security Center para IoT para IoT Edge.

Para continuar con la introducción a la implementación de Azure Security Center para IoT, lea los siguientes artículos:

- Implementar el [módulo de seguridad de IoT Edge](how-to-deploy-edge.md)
- Obtenga información sobre cómo [configurar el módulo de seguridad](how-to-agent-configuration.md)
- Revise los [requisitos previos del servicio](service-prerequisites.md) Azure Security Center para IoT
- Aprenda a [habilitar el servicio Azure Security Center para IoT en su instancia de IoT Hub](quickstart-onboard-iot-hub.md)
- Más información acerca del servicio en las [preguntas más frecuentes de Azure Security Center para IoT](resources-frequently-asked-questions.md)