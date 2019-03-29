---
title: ASC para vista previa de requisitos previos de IoT | Microsoft Docs
description: Detalles de todo lo necesario para empezar a trabajar con procesos de ASC para requisitos previos del servicio de IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 790cbcb7-1340-4cc1-9509-7b262e7c3181
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 0188dace0e666d4abfe31ac1c6c14d63c947c566
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2019
ms.locfileid: "58574759"
---
# <a name="asc-for-iot-prerequisites"></a>ASC para los requisitos previos de IoT

> [!IMPORTANT]
> ASC para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no se recomienda para las cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se proporciona una explicación de los diferentes bloques de creación de procesos de ASC para IoT service, lo que necesita para comenzar y conceptos básicos para ayudar a comprender el servicio. 

## <a name="minimum-requirements"></a>Requisitos mínimos

- Nivel estándar de IoT Hub
    - Rol de RBAC **propietario** privilegios 
- [El área de trabajo de log Analytics](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) 
- Azure Security Center (recomendado)
    - Aunque el uso de Azure Security Center es sólo una recomendación y no es un requisito, sin él, no podrá ver los otros recursos de Azure dentro de IoT Hub. 
 
## <a name="working-with-asc-for-iot-service"></a>Trabajar con procesos de ASC para el servicio IoT

ASC para información de IoT y los informes están disponibles mediante Azure IoT Hub y Azure Security Center. Para habilitar ASC para IoT en Azure IoT Hub, una cuenta con **propietario** se necesitan privilegios de nivel. Después de habilitar ASC para IoT en IoT Hub, ASC para IoT insights se muestran como el **seguridad** característica en Azure IoT Hub y como **IoT** en Azure Security Center. 

## <a name="supported-service-regions"></a>Regiones de servicios admitidos 

ASC para IoT es actualmente compatible con centros de IoT en las siguientes regiones de Azure:
  - Centro de EE. UU.
  - Norte de Europa
  - Sudeste asiático

## <a name="wheres-my-iot-hub"></a>¿Dónde está mi instancia de IoT Hub?

Compruebe la ubicación de centro de IoT para comprobar la disponibilidad del servicio antes de comenzar. 

1. Abra el IoT Hub. 
2. Haga clic en **Descripción general**. 
3. Compruebe la ubicación indicada coincide con uno de los [servicio regiones admitidas](#supported-service-regions). 


## <a name="supported-platforms-for-agents"></a>Plataformas compatibles con los agentes 

ASC para agentes de IoT es compatible con una lista creciente de dispositivos y plataformas. Consulte la [admite la lista de plataformas](how-to-deploy-agent.md) para comprobar su biblioteca de dispositivos existente o planeada.  

## <a name="next-steps"></a>Pasos siguientes
- [Información general](overview.md)
- [Habilitar el servicio](quickstart-onboard-iot-hub.md)
- [ASC para IoT preguntas más frecuentes](resources-frequently-asked-questions.md)
- [Descripción de ASC para las alertas de IoT](concept-security-alerts.md)
