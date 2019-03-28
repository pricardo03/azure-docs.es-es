---
title: Descripción de ASC para vista previa de arquitectura del agente de seguridad de IoT | Microsoft Docs
description: Conozca la arquitectura de seguridad del agente para los agentes utilizados en los procesos de ASC para el servicio IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: e78523ae-d70a-456a-818d-f8b1b025d7cb
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 7c8f256cf91a479c45f21b933efdb6a5e0212796
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541878"
---
# <a name="security-agent-reference-architecture"></a>Arquitectura de referencia de seguridad del agente

> [!IMPORTANT]
> ASC para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no se recomienda para las cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


ASC para IoT proporciona la arquitectura de referencia para los agentes de seguridad que inicie sesión, procesarán, agregan y envían datos de seguridad a través de IoT hub.

Los agentes de seguridad están diseñados para funcionar en un entorno restringido de IoT y son muy personalizables en cuanto a los valores que se proporcionan en comparación con los recursos que consumen.

Los agentes de seguridad admiten las siguientes características de solución de IoT:

- Recopilar eventos de seguridad sin procesar desde el sistema operativo subyacente (Linux, Windows). Para más información sobre los recopiladores de datos de seguridad disponibles, consulte [ASC para configurar el agente IoT](concept-agent-configuration.md).

- Agregación de eventos de seguridad sin procesar en los mensajes enviados a través de IoT hub.

- Autenticar con la identidad del dispositivo existente, o una identidad de módulo dedicado. Consulte [métodos de autenticación de seguridad del agente](concept-security-agent-authentication-methods.md) para obtener más información.

- Configurar de forma remota mediante el uso de la **ascforiot** módulo gemelo. Para obtener más información, consulte [configurar un ASC para el agente de IoT](concept-agent-configuration.md).

ASC para los agentes de seguridad de IoT se desarrollan como proyectos de código abierto y están disponible en GitHub: 

- [IoT-ASC-Agent-C](https://github.com/Azure/IoT-ASC-Agent-C) 
- [IoT-ASC-Agent-CS](https://github.com/Azure/IoT-ASC-Agent-CS)

## <a name="agent-supported-platforms"></a>Plataformas compatibles del agente

ASC para IoT ofrece instalador diferentes agentes de 32 y 64 bits de Windows y el mismo para 32 bits y 64 bits de Linux. Asegúrese de que tiene el instalador del agente correcto para cada uno de los dispositivos según la tabla siguiente:

| 32 o 64 bits | Linux |  Windows |    Detalles|
|----------|----------------------------------------------|-------------|-------------------------------------------|
| 32bit  | C  | C#  ||
| 64bit  | C#o C           | C#      | Usar al agente de C para dispositivos con recursos mínimos|

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido acerca de ASC para arquitectura de seguridad del agente de IoT y los instaladores disponibles.

Para continuar la introducción a ASC para implementación de IoT, use los siguientes artículos:


- Revise la ASC para IoT [requisitos previos de servicio](service-prerequisites.md)
- Obtenga información sobre cómo [habilitar ASC para el servicio de IoT en IoT Hub](quickstart-onboard-iot-hub.md)
- Use el inicio rápido para [configurar la solución](quickstart-configure-your-solution.md)
- Comprender [métodos de autenticación de seguridad del agente](concept-security-agent-authentication-methods.md)
- Seleccionar e implementar un [agente de seguridad](select-deploy-agent.md)
- Más información sobre el servicio desde el [ASC para IoT preguntas más frecuentes](resources-frequently-asked-questions.md)