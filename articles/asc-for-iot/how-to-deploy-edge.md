---
title: Implementar un ASC para el módulo de IoT Edge | Microsoft Docs
description: Obtenga información acerca de cómo implementar ASC para el agente de seguridad de IoT en IoT Edge.
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
ms.openlocfilehash: 87094b265a0c30c0349d64e4b956224525c04f74
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2019
ms.locfileid: "58580499"
---
# <a name="deploy-security-module-on-your-iot-edge-device"></a>Implementar en módulo de seguridad en el dispositivo de IoT Edge

> [!IMPORTANT]
> ASC para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

ASC para IoT **azureiotsecurity** módulo proporciona una solución de seguridad completa para el dispositivo IoT Edge.
Módulo de seguridad recopila, agrega y analiza los datos sin procesar de seguridad del sistema del sistema operativo y el contenedor en las alertas y recomendaciones de seguridad que requieren acción.
Para obtener más información, consulte [módulo de seguridad de IoT Edge](security-edge-architecture.md).

En esta guía, obtendrá información sobre cómo implementar un módulo de seguridad en el dispositivo de IoT Edge.

## <a name="deploy-security-module"></a>Implementar en módulo de seguridad

Siga estos pasos para implementar un ASC para el módulo de seguridad de IoT para IoT Edge.

### <a name="prerequisites"></a>Requisitos previos

1. Asegúrese de que el dispositivo está [registrado como un dispositivo IoT Edge](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-register-device-portal).

1. Los procesos de ASC para el módulo de IoT Edge requiere la [AuditD framework](https://linux.die.net/man/8/auditd) instalada en el dispositivo de Edge.

   Instale el marco de trabajo ejecutando el comando siguiente en el dispositivo de Edge:
   
   `apt-get install auditd audispd-plugins`

### <a name="deployment-using-azure-portal"></a>Implementación mediante Azure portal

1. Abra **Marketplace** en Azure portal.

1. Busque **seguridad de azure iot** y haga clic en **seguridad de Azure IoT**.

   ![](media/howto/edge_onboarding_7.png)

1. Haga clic en **Create**(Crear).

1. Elija su **suscripción**, **IoT Hub** y **nombre de dispositivo de IoT Edge**, a continuación, haga clic en **crear**.

1. Haga clic en **siguiente** dos veces a **revisión implementación**.

1. Asegúrese de que **edgeHub.settings.createOptions** está configurado como sigue:

   `"createOptions": "{\"HostConfig\":{\"PortBindings\":{\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}]}}}"`

1. Haga clic en **enviar** para completar la implementación.


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las opciones de configuración, seguir la Guía de procedimientos de configuración del módulo. 
> [!div class="nextstepaction"]
> [Configuración del módulo de guía de procedimientos](./how-to-agent-configuration.md)