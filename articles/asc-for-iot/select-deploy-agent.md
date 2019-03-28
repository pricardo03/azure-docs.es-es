---
title: Seleccionar e implementar un ASC para vista previa del agente de IoT | Microsoft Docs
description: Obtenga información acerca cómo seleccionar e implementar ASC para los agentes de seguridad de IoT en dispositivos IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: 5ce583fe98acb7e0a4aaeb720cb2d5ed5eebb9e3
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541968"
---
# <a name="select-and-deploy-a-security-agent-on-your-iot-device"></a>Seleccionar e implementar a un agente de seguridad en el dispositivo de IoT

> [!IMPORTANT]
> ASC para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


ASC para IoT proporciona arquitecturas de referencia para los agentes de seguridad que supervisan y recopilan datos desde dispositivos IoT.

Los agentes de seguridad se desarrollan como proyectos de código abierto y están disponibles en dos modos: <br> [C](https://aka.ms/iot-security-github-c), y [ C# ](https://aka.ms/iot-security-github-cs).

## <a name="supported-platforms-for-asc-for-iot-agents"></a>Plataformas compatibles con ASC para agentes de IoT

La siguiente lista incluye todas las plataformas compatibles actualmente.  

|ASC para el agente de IoT |Sistema operativo |Arquitectura |
|--------------|------------|--------------|
|C|Ubuntu 16.04 |   x64|
|C|Ubuntu 18.04 |   x64|
|C|Debian 9 |   x64, x86|
|C#|Ubuntu 16.04    |x64|
|C#|Ubuntu 18.04    |x64|
|C#|Debian 9    |x64|
|C#|Windows Server 2016|    X64|
|C#|Compilación 17763 de Windows 10 IoT Core |x64|


## <a name="which-flavor-should-i-use"></a>¿El tipo se debe usar?

Tenga en cuenta las siguientes preguntas con respecto a los dispositivos de IoT:

- ¿Usa _Windows Server_ o _Windows IoT Core_? 

    Use [ASC para la instalación de IoT para Windows con el C#-agente de seguridad basado en](tutorial-deploy-windows-cs.md).

- ¿Usa una distribución de Linux con x86 arquitectura? 

    Use [ASC para la instalación de IoT para Linux con el agente de seguridad basada en C](tutorial-deploy-linux-c.md).
- ¿Usa una distribución de Linux con x64 arquitectura?

    Puede usar ambos modos. <br>
    [ASC para la instalación de IoT para Linux con el agente de seguridad basada en C](tutorial-deploy-linux-c.md) o [ASC para la instalación de IoT para Linux con la C#-agente de seguridad basado en](tutorial-deploy-linux-cs.md).

Ambos modos tienen el mismo conjunto de características y admiten las opciones de configuración similar. El agente de seguridad basada en C tiene un menor consumo de memoria.


## <a name="next-steps"></a>Pasos siguientes
- [Información general](overview.md)
- [Agentes de seguridad](security-agent-architecture.md)
- [Métodos de autenticación de seguridad del agente](concept-security-agent-authentication-methods.md)
- [ASC para IoT preguntas más frecuentes](resources-frequently-asked-questions.md)