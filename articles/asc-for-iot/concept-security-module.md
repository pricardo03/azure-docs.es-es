---
title: Descripción de los módulos gemelos de seguridad de Azure Security Center for IoT, versión preliminar | Microsoft Docs
description: Sepa en qué consiste el concepto de módulo gemelo de seguridad y cómo se usa en Azure Security Center for IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: a5c25cba-59a4-488b-abbe-c37ff9b151f9
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 8f964fd2b121c6232081673666f5babb160e1721
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618309"
---
# <a name="security-module"></a>Módulo de seguridad

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se explica el modo en que Azure Security Center (ASC) for IoT usa módulos y dispositivos gemelos. 

## <a name="device-twins"></a>Dispositivos gemelos

En el caso de las soluciones de IoT integradas en Azure, los dispositivos gemelos desempeñan un rol clave tanto en la administración de dispositivos como en la automatización de procesos.  

ASC for IoT ofrece integración completa con la plataforma de administración de dispositivos de IoT existente, lo que permite no solo administrar el estado de seguridad de los dispositivos, sino también hacer uso de las funcionalidades de control de dispositivos existentes. La integración se consigue usando el mecanismo gemelo de IoT Hub.  

Obtenga más información sobre el concepto de [dispositivos gemelos](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins) en Azure IoT Hub. 

## <a name="security-module-twins"></a>Módulos de seguridad gemelos

ASC for IoT conserva un módulo gemelo de seguridad por cada dispositivo que haya en el servicio.
Este módulo contiene toda la información relevante relativa a la seguridad de cada dispositivo específico de la solución.
En un módulo de seguridad gemelo dedicado se conservan las propiedades de seguridad del dispositivo para que la comunicación sea más segura y para permitir unas actualizaciones y mantenimiento que requieren menos recursos.  

Vea [Creación de un módulo gemelo](quickstart-create-security-twin.md) y [Configurar agentes de seguridad](how-to-agent-configuration.md) para obtener información sobre cómo crear, personalizar y configurar el módulo gemelo. Vea [Descripción de módulos gemelos](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins) para obtener más información sobre el concepto de módulo gemelo en IoT Hub. 
 

## <a name="see-also"></a>Otras referencias
- [ASC for IoT, versión preliminar](overview.md)
- [Implementación de agentes de seguridad](how-to-deploy-agent.md)
- [Métodos de autenticación del agente de seguridad](concept-security-agent-authentication-methods.md)