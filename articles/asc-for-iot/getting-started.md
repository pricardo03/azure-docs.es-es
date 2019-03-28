---
title: Empezar a usar Azure Security Center (ASC) para la versión preliminar de IoT | Microsoft Docs
description: Comience a comprender el flujo de trabajo básico de ASC para las características de IoT y el servicio.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 55c8d3b6-3126-4246-8d07-ef88fe5ea84f
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: a78eeda814b21d489eb957d44202b17c61d836d5
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541893"
---
# <a name="get-started-with-azure-security-center-asc-for-iot"></a>Empezar a trabajar con Azure Security Center (ASC) para IoT 

> [!IMPORTANT]
> ASC para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se proporciona una explicación de los distintos bloques de creación de los procesos de ASC para el servicio IoT y se explica cómo empezar a trabajar con [habilitar el servicio](quickstart-onboard-iot-hub.md). 

ASC para IoT puede integrarse sin problemas en su centro de IoT para proporcionar análisis de seguridad de la configuración del centro de IoT, identidad del dispositivo y dispositivos de centro de patrones de comunicación.
Para las capacidades mejoradas de seguridad, ASC para IoT proporciona basada en agente de recopilación de datos de seguridad de los dispositivos de IoT.

## <a name="asc-for-iot-seamless-iot-hub-integration"></a>ASC para la integración perfecta de IoT Hub de IoT

Al intentar proteger los dispositivos IoT individuales, se requiere la capacidad de recopilar datos directamente desde los dispositivos, o desde su red. Para admitir este esfuerzo, ASC para IoT ofrece un arsenal de agentes de seguridad de bajo impacto para proporcionar protección y supervisión de dispositivos.

En ASC para la versión preliminar de IoT, arquitectura de referencia para los agentes de seguridad de Windows y Linux en C# y C se proporcionan.
Los agentes de controlan la recopilación de eventos sin procesar desde el sistema operativo del dispositivo, agregación de eventos para reducir el costo y la configuración a través de un dispositivo gemelo de módulo.
Los mensajes de seguridad se envían a través de su centro de IoT en ASC para servicios de análisis de IoT.

## <a name="asc-for-iot-basics"></a>ASC para conceptos básicos de IoT

Elija el escenario de flujo de trabajo que mejor se adapte a sus requisitos de entorno y el dispositivo de IoT:

### <a name="get-started-with-asc-for-iot-seamless-iot-hub-integration"></a>Introducción a ASC para la integración perfecta de IoT Hub de IoT 

>[!Note]
>Este flujo de trabajo le permite usar el servicio sin utilizar ASC para los agentes de seguridad de IoT. 

Para habilitar la supervisión de su dispositivo de administración de identidades, dispositivo de nube y en la nube para patrones de comunicación de dispositivo, use la siguiente flujo de trabajo básico para las pruebas y para iniciar el servicio: 

1. [Habilitar ASC para el servicio de IoT en IoT Hub](quickstart-onboard-iot-hub.md)
1. Si el centro de IoT no tiene ningún dispositivo registrado, [registre un nuevo dispositivo](https://docs.microsoft.com/azure/iot-accelerators/quickstart-device-simulation-deploy).
1. [Creación de un módulo de seguridad ascforiot](quickstart-create-security-twin.md) para sus dispositivos. 
1. Definir el comportamiento normal de dispositivo y del sistema a través de [alertas personalizadas](quickstart-create-custom-alerts.md). 
1. Realice pruebas para comprobar el estado del servicio y el dispositivo del sistema. 
1. Explorar [alertas](concept-security-alerts.md), [recomendaciones](concept-recommendations.md), y [profundización mediante Log Analytics](how-to-security-data-access.md) mediante IoT Hub. 


### <a name="get-started-with-asc-for-iot-security-agents"></a>Introducción a ASC para los agentes de seguridad de IoT

Hacer uso de ASC para capacidades de seguridad mejorada de IoT, como la supervisión de las conexiones remotas, las aplicaciones activas, eventos de inicio de sesión y prácticas recomendadas de configuración del sistema operativo usando el siguiente flujo de trabajo básico para probar y habilitar el servicio: 

1. [Habilitar ASC para el servicio de IoT a IoT Hub](quickstart-onboard-iot-hub.md)
1. Si el centro de IoT no tiene ningún dispositivo registrado, [registre un nuevo dispositivo](https://docs.microsoft.com/azure/iot-accelerators/quickstart-device-simulation-deploy).
1. [Creación de un módulo de seguridad azureiotsecurity](quickstart-create-security-twin.md) para sus dispositivos.
1. Para instalar el agente en un dispositivo simulado de Azure en lugar de instalar en un dispositivo real, [flechas de una máquina Virtual de Azure nueva (VM)](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) en una zona de disponibilidad. 
1. [Implementar un ASC para el agente de seguridad de IoT](tutorial-deploy-linux-cs.md) en su dispositivo IoT o una nueva máquina virtual.
1. Siga las instrucciones para [trigger_events](https://aka.ms/iot-security-github-trigger-events) para ejecutar una simulación de un ataque inofensivo.
1. Compruebe ASC para IoT alertas en respuesta al ataque simulado en el paso anterior. 
    - Empezar a cinco minutos después de ejecutar la secuencia de comandos de comprobación.
1. Explorar [alertas](concept-security-alerts.md), [recomendaciones](concept-recommendations.md), y [profundización mediante Log Analytics](how-to-security-data-access.md) mediante IoT Hub. 

## <a name="next-steps"></a>Pasos siguientes

- Habilitar [ASC para IoT](quickstart-onboard-iot-hub.md)
- Configurar su [solución](quickstart-configure-your-solution.md)
- [Crear módulos de seguridad](quickstart-create-security-twin.md)
- Configurar [alertas personalizadas](quickstart-create-custom-alerts.md)
- [Implementar a un agente de seguridad](select-deploy-agent.md)
