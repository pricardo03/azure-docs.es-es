---
title: Introducción al uso de Azure Security Center for IoT, versión preliminar | Microsoft Docs
description: Primeros pasos para entender el flujo de trabajo básico de las características y el servicio Azure Security Center for IoT.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 55c8d3b6-3126-4246-8d07-ef88fe5ea84f
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: bdc5e858286d7db03281408cf3ed00e0760e0c3a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65200644"
---
# <a name="get-started-with-azure-security-center-for-iot"></a>Introducción a Azure Security Center for IoT 

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se explican los diferentes bloques de creación del servicio Azure Security Center (ASC) for IoT. También se describen los primeros pasos para [habilitar el servicio](quickstart-onboard-iot-hub.md). 

ASC for IoT se integra sin problemas en su IoT Hub para proporcionar análisis de seguridad de la configuración de IoT Hub, la identidad de dispositivos y los patrones de comunicación entre el IoT Hub y el dispositivo.
Para lograr unas capacidades mejoradas de seguridad, ASC for IoT permite la recopilación basada en agente de datos de seguridad procedentes de dispositivos IoT.

## <a name="asc-for-iot-seamless-iot-hub-integration"></a>Integración perfecta de ASC for IoT e IoT Hub

Cuando se intentan proteger dispositivos IoT individuales, es necesario poder recopilar datos directamente desde los dispositivos, o bien desde la red. Para hacer esto posible, ASC for IoT ofrece un arsenal de agentes de seguridad de bajo impacto que proporcionan protección y supervisión de dispositivos.

En ASC for IoT, versión preliminar, se proporciona arquitectura de referencia para los agentes de seguridad de Windows y Linux tanto en C# y como en C.
Los agentes controlan la recopilación de eventos sin procesar procedentes del sistema operativo del dispositivo, la agregación de eventos para reducir el coste y la configuración a través de un módulo gemelo del dispositivo.
Los mensajes de seguridad se envían a través del IoT Hub a los servicios de análisis de ASC for IoT.

## <a name="asc-for-iot-basics"></a>Conceptos básicos de ASC for IoT

Elija el escenario de flujo de trabajo que mejor encaje con sus requisitos de entorno y de dispositivo IoT:

### <a name="get-started-with-asc-for-iot-seamless-iot-hub-integration"></a>Introducción a la integración perfecta de ASC for IoT e IoT Hub 

>[!Note]
>Este flujo de trabajo permite usar el servicio prescindiendo de los agentes de seguridad de ASC for IoT. 

Para permitir la supervisión de la administración de identidades de dispositivo y los patrones de comunicación entre el dispositivo y la nube y viceversa, use el siguiente flujo de trabajo básico para realizar pruebas e iniciar el servicio: 

1. [Habilite el servicio ASC for IoT en IoT Hub](quickstart-onboard-iot-hub.md).
1. Si IoT Hub no tiene ningún dispositivo registrado, [registre uno nuevo](https://docs.microsoft.com/azure/iot-accelerators/quickstart-device-simulation-deploy).
1. [Cree un módulo de seguridad azureiotsecurity](quickstart-create-security-twin.md) para sus dispositivos. 
1. Defina el comportamiento normal de dispositivo y del sistema a través de [alertas personalizadas](quickstart-create-custom-alerts.md). 
1. Realice pruebas en el sistema para comprobar el estado del servicio y del dispositivo. 
1. Explore las [alertas](concept-security-alerts.md), las [recomendaciones](concept-recommendations.md) y [perfeccione su uso de Log Analytics](how-to-security-data-access.md) mediante IoT Hub. 


### <a name="get-started-with-asc-for-iot-security-agents"></a>Introducción a los agentes de seguridad de ASC for IoT

Haga uso de las capacidades de seguridad mejoradas de ASC for IoT, como la supervisión de conexiones remotas, aplicaciones activas, eventos de inicio de sesión y procedimientos recomendados de configuración del sistema operativo. Para ello, use el siguiente flujo de trabajo básico para probar y habilitar el servicio: 

1. [Habilite el servicio ASC for IoT en IoT Hub](quickstart-onboard-iot-hub.md).
1. Si IoT Hub no tiene ningún dispositivo registrado, [registre uno nuevo](https://docs.microsoft.com/azure/iot-accelerators/quickstart-device-simulation-deploy).
1. [Cree un módulo de seguridad azureiotsecurity](quickstart-create-security-twin.md) para sus dispositivos.
1. Para instalar el agente en un dispositivo simulado de Azure en lugar de hacerlo en un dispositivo real, [prepare una máquina virtual de Azure nueva](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) en una zona de disponibilidad. 
1. [Implemente un agente de seguridad de ASC for IoT](how-to-deploy-linux-cs.md) en el dispositivo IoT o en una máquina virtual nueva.
1. Siga las instrucciones de [trigger_events](https://aka.ms/iot-security-github-trigger-events) para llevar a cabo una simulación de un ataque inofensivo.
1. Compruebe las alertas de ASC for IoT en respuesta al ataque simulado del paso anterior. No las compruebe hasta que hayan transcurrido cinco minutos desde la ejecución del script.
1. Explore las [alertas](concept-security-alerts.md), las [recomendaciones](concept-recommendations.md) y [perfeccione su uso de Log Analytics](how-to-security-data-access.md) mediante IoT Hub. 

## <a name="next-steps"></a>Pasos siguientes

- Habilitar [ASC for IoT](quickstart-onboard-iot-hub.md)
- Configurar la [solución](quickstart-configure-your-solution.md)
- [Creación de módulos de seguridad](quickstart-create-security-twin.md)
- Configurar [alertas personalizadas](quickstart-create-custom-alerts.md)
- [Implementación de un agente de seguridad](how-to-deploy-agent.md)
