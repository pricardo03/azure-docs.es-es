---
title: Seleccione e implementar Azure Security Center para la versión preliminar del agente de IoT | Microsoft Docs
description: Obtenga información acerca cómo seleccionar e implementar Azure Security Center para los agentes de seguridad de IoT en dispositivos IoT.
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
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65200564"
---
# <a name="select-and-deploy-a-security-agent-on-your-iot-device"></a>Seleccionar e implementar a un agente de seguridad en el dispositivo de IoT

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Security Center (ASC) para IoT proporciona arquitecturas de referencia para los agentes de seguridad que supervisan y recopilan datos desde dispositivos IoT.
Consulte [arquitectura de referencia de seguridad del agente](security-agent-architecture.md) para obtener más información.

Los agentes se desarrollan como proyectos de código abierto y están disponibles en dos modos: <br> [C](https://aka.ms/iot-security-github-c), y [ C# ](https://aka.ms/iot-security-github-cs).

En este artículo, aprenderá a: 
> [!div class="checklist"]
> * Comparar versiones de agente de seguridad
> * Descubra las plataformas de agentes admitidos
> * Elija el tipo de agente correcto para su solución

## <a name="understand-security-agent-options"></a>Comprender las opciones de seguridad del agente

Cada ASC para el tipo de agente de seguridad de IoT ofrece el mismo conjunto de características y es compatible con las opciones de configuración similar. 

El agente de seguridad basada en C tiene un menor consumo de memoria y es la opción ideal para los dispositivos con menos recursos disponibles. 

|     | Agente de seguridad basada en C | C#-agente de seguridad basado en |
| --- | ----------- | --------- |
| Código abierto | Disponible en [licencia MIT](https://en.wikipedia.org/wiki/MIT_License) en [Github](https://aka.ms/iot-security-github-cs) | Disponible en [licencia MIT](https://en.wikipedia.org/wiki/MIT_License) en [Github](https://aka.ms/iot-security-github-c) |
| Lenguaje de desarrollo    | C | C# |
| ¿Plataformas admitidas de Windows? | Sin  | Sí |
| Requisitos previos de Windows | --- | [WMI](https://docs.microsoft.com/windows/desktop/wmisdk/) |
| ¿Plataformas compatibles de Linux? | Sí, x64 y x86 | Sí, solo x64 |
| Requisitos previos de Linux | libunwind8, libcurl3, uuid en tiempo de ejecución, auditd, audispd complementos | libunwind8, libcurl3, uuid en tiempo de ejecución, auditd, audispd complementos, sudo, netstat, iptables |
| Superficie de disco | 10.5 MB | 90MB |
| Superficie de memoria (en promedio) | 5.5 MB | 33MB |
| [Autenticación](concept-security-agent-authentication-methods.md) a IoT Hub | Sí | Sí |
| Datos de seguridad [colección](how-to-agent-configuration.md#supported-security-events) | Sí | Sí |
| Agregación de datos | Sí | Sí |
| Configuración remota a través de [gemelo de módulo de seguridad](concept-security-module.md) | Sí | Sí |


## <a name="choose-an-agent-flavor"></a>Elija un tipo de agente 

Responda a las siguientes preguntas acerca de los dispositivos de IoT para seleccionar al agente correcto:

- ¿Usa _Windows Server_ o _Windows IoT Core_? 

    [Implementar un C#-agente de seguridad de Windows basado en](how-to-deploy-windows-cs.md).

- ¿Usa una distribución de Linux con x86 arquitectura? 

    [Implementar un agente de seguridad basada en C para Linux](how-to-deploy-linux-c.md).

- ¿Usa una distribución de Linux con x64 arquitectura?

    Puede usar cualquier tipo de agente. <br>
    [Implementar un agente de seguridad basada en C para Linux](how-to-deploy-linux-c.md) o [implementar un C#-basándose security agent para Linux](how-to-deploy-linux-cs.md).

Ambos modos de agente ofrecen el mismo conjunto de características y admiten opciones de configuración similar.
Consulte [comparación de seguridad del agente](how-to-deploy-agent.md#understand-security-agent-options) para obtener más información.

## <a name="supported-platforms"></a>Plataformas compatibles

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

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las opciones de configuración, seguir la Guía de procedimientos para configurar el agente. 
> [!div class="nextstepaction"]
> [Configuración del agente de guía de procedimientos](./how-to-agent-configuration.md)
