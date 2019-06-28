---
title: Seleccionar e implementar el agente de Azure Security Center for IoT, versión preliminar | Microsoft Docs
description: Aprenda a seleccionar e implementar agentes de seguridad de Azure Security Center for IoT en dispositivos IoT.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: 5c76cce5a3d9add8a1dcd8acc439a70c4a8655a9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65200564"
---
# <a name="select-and-deploy-a-security-agent-on-your-iot-device"></a>Seleccionar e implementar un agente de seguridad en el dispositivo IoT

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Security Center (ASC) for IoT proporciona arquitecturas de referencia para los agentes de seguridad que supervisan y recopilan datos de dispositivos IoT.
Vea [Arquitectura de referencia del agente de seguridad](security-agent-architecture.md) para obtener más información.

Los agentes de seguridad se desarrollan como proyectos de código abierto y están disponibles en dos tipos: <br> [C](https://aka.ms/iot-security-github-c) y [C#](https://aka.ms/iot-security-github-cs).

En este artículo, aprenderá a: 
> [!div class="checklist"]
> * Comparar tipos de agente de seguridad
> * Descubrir las plataformas de agente admitidas
> * Elegir el tipo de agente correcto para su solución

## <a name="understand-security-agent-options"></a>Descripción de las opciones de agente de seguridad

Cada tipo de agente de seguridad de ASC for IoT ofrece el mismo conjunto de características y admite unas opciones de configuración similares. 

El agente de seguridad basado en C tiene una superficie de memoria menor y es la opción ideal en dispositivos con pocos recursos disponibles. 

|     | Agente de seguridad basado en C | Agente de seguridad basado en C# |
| --- | ----------- | --------- |
| Código abierto | Disponible con [licencia de MIT](https://en.wikipedia.org/wiki/MIT_License) en [Github](https://aka.ms/iot-security-github-cs) | Disponible con [licencia de MIT](https://en.wikipedia.org/wiki/MIT_License) en [Github](https://aka.ms/iot-security-github-c) |
| Lenguaje de desarrollo    | C | C# |
| ¿Admite plataformas Windows? | Sin | Sí |
| Requisitos previos de Windows | --- | [WMI](https://docs.microsoft.com/windows/desktop/wmisdk/) |
| ¿Admite plataformas Linux? | Sí, x64 y x86 | Sí, solo x64 |
| Requisitos previos de Linux | libunwind8, libcurl3, uuid-runtime, auditd, audispd-plugins | libunwind8, libcurl3, uuid-runtime, auditd, audispd-plugins, sudo, netstat, iptables |
| Superficie de disco | 10,5 MB | 90 MB |
| Superficie de memoria (promedio) | 5,5 MB | 33 MB |
| [Autenticación](concept-security-agent-authentication-methods.md) en IoT Hub | Sí | Sí |
| [Recopilación](how-to-agent-configuration.md#supported-security-events) de datos de seguridad | Sí | Sí |
| Agregación de datos | Sí | Sí |
| Configuración remota a través de un [módulo de seguridad gemelo](concept-security-module.md) | Sí | Sí |


## <a name="choose-an-agent-flavor"></a>Elegir un tipo de agente 

Responda las siguientes preguntas sobre sus dispositivos IoT para seleccionar al agente correcto:

- ¿Usa _Windows Server_ o _Windows IoT Core_? 

    [Implementación de un agente de seguridad basado en C# para Windows](how-to-deploy-windows-cs.md)

- ¿Usa una distribución Linux con arquitectura x86? 

    [Implementación de un agente de seguridad basado en C para Linux](how-to-deploy-linux-c.md)

- ¿Usa una distribución Linux con arquitectura x64?

    Puede usar cualquier tipo de agente. <br>
    [Implementación de un agente de seguridad basado en C para Linux](how-to-deploy-linux-c.md) y/o [Implementación de un agente de seguridad basado en C# para Linux](how-to-deploy-linux-cs.md)

Ambos tipos de agente ofrecen el mismo conjunto de características y admiten opciones de configuración similares.
Vea [Comparación de agentes de seguridad](how-to-deploy-agent.md#understand-security-agent-options) para obtener más información.

## <a name="supported-platforms"></a>Plataformas compatibles

La siguiente lista incluye todas las plataformas admitidas actualmente.

|Agente de ASC for IoT |Sistema operativo |Arquitectura |
|--------------|------------|--------------|
|C|Ubuntu 16.04 |   x64|
|C|Ubuntu 18.04 |   x64|
|C|Debian 9 |   x64, x86|
|C#|Ubuntu 16.04    |x64|
|C#|Ubuntu 18.04    |x64|
|C#|Debian 9    |x64|
|C#|Windows Server 2016|    X64|
|C#|Compilación 17763 de Windows 10 IoT Core |x64|

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las opciones de configuración, prosiga con la guía de procedimientos para configurar el agente. 
> [!div class="nextstepaction"]
> [Tutorial: Configurar agentes de seguridad](./how-to-agent-configuration.md)
