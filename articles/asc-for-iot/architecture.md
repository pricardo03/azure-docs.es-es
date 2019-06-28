---
title: Descripción de la arquitectura de la solución de Azure Security Center para IoT en versión preliminar | Microsoft Docs
description: Obtenga información sobre el flujo de información en el servicio Azure Security Center para IoT.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2019
ms.author: mlottner
ms.openlocfilehash: 3c775c8cb99b056c7f468c028a80dd0b27ae6438
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65200692"
---
# <a name="azure-security-center-for-iot-architecture"></a>Arquitectura de Azure Security Center para IoT

En este artículo se explica la arquitectura del sistema funcional de la solución Azure Security Center (ASC) para IoT. 

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="asc-for-iot-components"></a>Componentes de ASC para IoT

ASC para IoT consta de los siguientes componentes:
- Agentes de dispositivo
- SDK de Send Security Message
- Integración de IoT Hub
- Canalización de Analytics
 
### <a name="asc-for-iot-workflow"></a>Flujo de trabajo de ASC para IoT

Los agentes de dispositivo de ASC para IoT le permiten reunir fácilmente los eventos de seguridad sin procesar de los dispositivos. Los eventos de seguridad sin procesar pueden incluir las conexiones de IP, la creación de procesos, los inicios de sesión de los usuarios y otra información relacionada con la seguridad. Además, los agentes de dispositivo de ASC para IoT controlan la agregación de eventos para ayudar a evitar una alta tasa de procesamiento de la red. Los agentes son altamente personalizables, lo que le permite usarlos para tareas específicas, como enviar solo información importante a los SLA más rápidos, o para agregar información de seguridad exhaustiva y contexto en segmentos más grandes, lo que evita mayores costos de servicio.
 
Los agentes de dispositivo y otra aplicaciones usan el **SDK de Send Security Message de Azure ASC** para enviar información de seguridad a Azure IoT Hub. IoT Hub recoge esta información y la reenvía al servicio de ASC para IoT.

Una vez que el servicio de ASC para IoT está habilitado, además de los datos reenviados, IoT Hub envía también todos los datos internos para el análisis por parte de ASC para IoT. Estos datos incluyen los registros de operaciones de dispositivos a la nube, las identidades de los dispositivos la y configuración del Hub. Toda esta información ayuda a crear la canalización de análisis de ASC para IoT.
 
La canalización de análisis de ASC para IoT también recibe flujos adicionales de inteligencia de amenazas desde varios orígenes dentro de Microsoft y partners de Microsoft. La canalización de análisis completa de ASC para IoT funciona con cada configuración del cliente realizada en el servicio (como alertas personalizadas y uso del SDK de Send Security Message).
 
Con la canalización de análisis, ASC para IoT combina todos los flujos de información para generar alertas y recomendaciones viables. La canalización contiene reglas personalizadas creadas tanto por investigadores de seguridad como por expertos, así como modelos de Machine Learning en busca de desviaciones del comportamiento estándar del dispositivo y análisis de riesgos.
 
Las recomendaciones y alertas de ASC para IoT (salida de la canalización de análisis) se escriben en el área de trabajo de Log Analytics de cada cliente. Incluir los eventos sin procesar en el área de trabajo, así como las alertas y recomendaciones, permite investigaciones y consultas de profundización con los detalles exactos de las actividades sospechosas detectadas.  

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido sobre la arquitectura y el flujo de trabajo básicos de la solución ASC para IoT. Para más información sobre los requisitos previos, cómo empezar a trabajar y habilitar la solución de seguridad en IoT Hub, consulte los artículos siguientes:

- [Requisitos previos del servicio](service-prerequisites.md)
- [Introducción](getting-started.md)
- [Configuración de una solución](quickstart-configure-your-solution.md)
- [Habilitar la seguridad en IoT Hub](quickstart-onboard-iot-hub.md)
- [Preguntas más frecuentes sobre ASC para IoT](resources-frequently-asked-questions.md)
- [Alertas de seguridad de ASC para IoT](concept-security-alerts.md)

