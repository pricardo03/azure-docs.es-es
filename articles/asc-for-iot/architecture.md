---
title: Descripción de Azure Security Center para la versión preliminar de arquitectura de solución de IoT | Microsoft Docs
description: Obtenga información sobre el flujo de información en el centro de seguridad de Azure para el servicio IoT.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2019
ms.author: mlottner
ms.openlocfilehash: a0eb459391da65f8d0e2ae251809805924d07ad1
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58862372"
---
# <a name="azure-security-center-for-iot-architecture"></a>Azure Security Center para arquitectura de IoT

En este artículo se explica la arquitectura del sistema funcional de Azure Security Center (ASC) para la solución de IoT. 

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="asc-for-iot-components"></a>ASC para los componentes de IoT

ASC para IoT se compone de los siguientes componentes:
- Agentes de dispositivo
- Enviar mensaje de seguridad SDK
- Integración de IoT Hub
- Canalización de análisis
 
### <a name="asc-for-iot-workflow"></a>ASC para el flujo de trabajo de IoT

ASC para los agentes de dispositivo de IoT le permite reunir fácilmente los eventos de seguridad sin procesar de los dispositivos. Las conexiones IP, la creación del proceso, los inicios de sesión de usuario y otra información relacionada con la seguridad, pueden incluir eventos de seguridad sin procesar. ASC para los agentes de dispositivo de IoT también controla la agregación de eventos para ayudar a evitar el rendimiento de la red alta. Los agentes son muy personalizables, lo que se usan para tareas específicas, como enviar sólo información importante de lo SLA más rápido, o para la agregación de información de seguridad exhaustivas y el contexto en segmentos más grandes, evitar mayores costos de servicio.
 
Los agentes de dispositivo y otra aplicaciones utilizan el **Azure ASC enviar mensaje de seguridad SDK** para enviar información de seguridad a Azure IoT Hub. IoT Hub recoge esta información y lo reenvía a los procesos de ASC para el servicio IoT.

Una vez habilitada la ASC para el servicio IoT, además de los datos reenviados, IoT Hub envía también todas sus datos internos para el análisis ASC para IoT. Estos datos incluyen los registros de operaciones de nube a dispositivo, las identidades de dispositivo y configuración del centro. Toda esta información ayuda a crear los procesos de ASC para canalización de análisis de IoT.
 
ASC para canalización de análisis de IoT también recibe flujos de inteligencia de amenazas adicionales desde varios orígenes dentro de Microsoft y asociados. Los procesos de ASC para canalización de análisis completo de IoT funciona con todas las configuraciones cliente realizadas en el servicio (como alertas personalizadas y uso de la seguridad de envío mensajes SDK).
 
Uso de la canalización de análisis, ASC para IoT combina todas las secuencias de información para generar alertas y recomendaciones viables. La canalización contiene dos reglas personalizadas creadas por los investigadores de seguridad y expertos, así como búsqueda de desviación del análisis de comportamiento y el riesgo de dispositivo estándar de modelos de aprendizaje automático.
 
ASC para IoT recomendaciones y alertas (salida de canalización de análisis) se escribe en el área de trabajo de Log Analytics de cada cliente. Incluidos los eventos sin procesar en el área de trabajo, así como las alertas y recomendaciones permite las investigaciones de profundización y consultas con los detalles exactos de las actividades sospechosas que detecta.  

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido sobre la arquitectura básica y el flujo de trabajo de procesos de ASC para solución de IoT. Para obtener más información sobre los requisitos previos, cómo empezar a trabajar y habilitar la solución de seguridad en IoT Hub, consulte los artículos siguientes:

- [Requisitos previos del servicio](service-prerequisites.md)
- [Introducción](getting-started.md)
- [Configuración de una solución](quickstart-configure-your-solution.md)
- [Habilitar la seguridad en IoT Hub](quickstart-onboard-iot-hub.md)
- [ASC para IoT preguntas más frecuentes](resources-frequently-asked-questions.md)
- [ASC para las alertas de seguridad de IoT](concept-security-alerts.md)

