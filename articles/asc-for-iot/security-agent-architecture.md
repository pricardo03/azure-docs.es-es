---
title: Descripción de Azure Security Center para la versión preliminar de arquitectura del agente de seguridad de IoT | Microsoft Docs
description: Conozca la arquitectura de seguridad del agente para los agentes utilizados en Azure Security Center para el servicio IoT.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: e78523ae-d70a-456a-818d-f8b1b025d7cb
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 3c05b7e9b1c6d1b9214da168f7abfcbb322f8f6d
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65192516"
---
# <a name="security-agent-reference-architecture"></a>Arquitectura de referencia de seguridad del agente

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Azure Security Center (ASC) para IoT proporciona la arquitectura de referencia para los agentes de seguridad que inicie sesión, procesarán, agregan y envían datos de seguridad a través de IoT hub.

Los agentes de seguridad están diseñados para funcionar en un entorno restringido de IoT y son muy personalizables en cuanto a los valores que se proporcionan en comparación con los recursos que consumen.

Los agentes de seguridad admiten las siguientes características:

- Recopilar eventos de seguridad sin procesar desde el sistema operativo subyacente (Linux, Windows). Para más información sobre los recopiladores de datos de seguridad disponibles, consulte [ASC para configurar el agente IoT](how-to-agent-configuration.md).

- Agregación de eventos de seguridad sin procesar en los mensajes enviados a través de IoT hub.

- Autenticar con la identidad del dispositivo existente, o una identidad de módulo dedicado. Consulte [métodos de autenticación de seguridad del agente](concept-security-agent-authentication-methods.md) para obtener más información.

- Configurar de forma remota mediante el uso de la **azureiotsecurity** módulo gemelo. Para obtener más información, consulte [configurar un ASC para el agente de IoT](how-to-agent-configuration.md).

ASC para los agentes de seguridad de IoT se desarrollan como proyectos de código abierto y están disponible en GitHub: 

- [ASC para el agente de IoT C](https://github.com/Azure/Azure-IoT-Security-Agent-C) 
- [ASC para IoT C#-agente basado en](https://github.com/Azure/Azure-IoT-Security-Agent-CS)

## <a name="agent-supported-platforms"></a>Plataformas compatibles del agente

ASC para IoT ofrece instalador diferentes agentes de 32 y 64 bits de Windows y el mismo para 32 bits y 64 bits de Linux. Asegúrese de que tiene el instalador del agente correcto para cada uno de los dispositivos según la tabla siguiente:

| 32 o 64 bits | Linux |  Windows |    Detalles|
|----------|----------------------------------------------|-------------|-------------------------------------------|
| 32bit  | C  | C#  ||
| 64bit  | C#o C           | C#      | Usar al agente de C para dispositivos con recursos mínimos|

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido acerca de ASC para arquitectura de seguridad del agente de IoT y los instaladores disponibles.

Para continuar la introducción a ASC para implementación de IoT, use los siguientes artículos:

- Comprender [métodos de autenticación de seguridad del agente](concept-security-agent-authentication-methods.md)
- Seleccionar e implementar un [agente de seguridad](how-to-deploy-agent.md)
- Revise la ASC para IoT [requisitos previos de servicio](service-prerequisites.md)
- Obtenga información sobre cómo [habilitar ASC para el servicio de IoT en IoT Hub](quickstart-onboard-iot-hub.md)
- Más información sobre el servicio desde el [ASC para IoT preguntas más frecuentes](resources-frequently-asked-questions.md)
