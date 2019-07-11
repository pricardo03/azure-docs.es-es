---
title: Descripción del módulo de seguridad de la arquitectura de agente de seguridad de Azure Security Center for IoT | Microsoft Docs
description: Conozca la arquitectura de agente de seguridad de los agentes que se usan en el servicio Azure Security Center for IoT.
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
ms.openlocfilehash: 7f4ca9a2689ab9e09b4ebff903e757f5c352b556
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67616580"
---
# <a name="security-agent-reference-architecture"></a>Arquitectura de referencia del agente de seguridad

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Azure Security Center (ASC) for IoT proporciona una arquitectura de referencia a agentes de seguridad que registran, procesan, agregan y envían datos de seguridad a través de IoT Hub.

Los agentes de seguridad están diseñados para funcionar en un entorno de IoT restringido, y son tremendamente personalizables en cuanto a todo el valor que aportan en comparación con los recursos que consumen.

Los agentes de seguridad permiten realizar lo siguiente:

- Recopilar eventos de seguridad sin procesar del sistema operativo subyacente (Linux o Windows). Vea [Configurar agentes de seguridad](how-to-agent-configuration.md) para obtener más información sobre los recopiladores de datos de seguridad disponibles.

- Agregar eventos de seguridad sin procesar a los mensajes enviados a través de IoT Hub.

- Autenticarse con la identidad del dispositivo existente, o con una identidad de módulo dedicada. Vea [Métodos de autenticación del agente de seguridad](concept-security-agent-authentication-methods.md) para obtener más información.

- Establecer configuraciones de forma remota mediante el módulo gemelo **azureiotsecurity**. Vea [Configurar agentes de seguridad](how-to-agent-configuration.md) para obtener más información.

Los agentes de seguridad de ASC for IoT se desarrollan como proyectos de código abierto y están disponibles en GitHub: 

- [Agente basado en C de ASC for IoT](https://github.com/Azure/Azure-IoT-Security-Agent-C) 
- [Agente basado en C# de ASC for IoT](https://github.com/Azure/Azure-IoT-Security-Agent-CS)

## <a name="agent-supported-platforms"></a>Plataformas compatibles con el agente

ASC for IoT ofrece diferentes instaladores de agente de 32 y 64 bits de Windows, y lo mismo para 32 bits y 64 bits de Linux. Asegúrese de que tiene el instalador de agente correcto para cada uno de los dispositivos, según la siguiente tabla:

| 32 o 64 bits | Linux | Windows |    Detalles|
|----------|----------------------------------------------|-------------|-------------------------------------------|
| 32 bits  | C  | C#  ||
| 64 bits  | C# o C           | C#      | Use el agente de C en dispositivos con recursos mínimos.|

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha conocido la arquitectura de agentes de seguridad de ASC for IoT, así como los instaladores disponibles.

Para seguir obteniendo información sobre la implementación de ASC for IoT, lea los siguientes artículos:

- Conozca los [métodos de autenticación del agente de seguridad](concept-security-agent-authentication-methods.md)
- Seleccione e implemente un [agente de seguridad](how-to-deploy-agent.md)
- Repase los [requisitos previos de servicio](service-prerequisites.md) de ASC for IoT
- Obtenga información sobre cómo [habilitar el servicio ASC for IoT en IoT Hub](quickstart-onboard-iot-hub.md)
- Obtenga información sobre el servicio en las [preguntas más frecuentes de ASC for IoT](resources-frequently-asked-questions.md)
