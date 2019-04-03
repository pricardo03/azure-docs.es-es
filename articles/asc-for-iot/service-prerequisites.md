---
title: Azure Security Center para la versión preliminar de requisitos previos de IoT | Microsoft Docs
description: Detalles de todo lo necesario para empezar a trabajar con Azure Security Center para los requisitos previos del servicio de IoT.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 790cbcb7-1340-4cc1-9509-7b262e7c3181
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: f09d768c0c6c1d351f737b053da9fd3282867099
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2019
ms.locfileid: "58861696"
---
# <a name="azure-security-center-for-iot-prerequisites"></a>Azure Security Center para los requisitos previos de IoT

> [!IMPORTANT]
> Azure Security Center para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se proporciona una explicación de los diferentes bloques de creación de la Azure Security Center (ASC) para IoT service, lo que necesita para comenzar y conceptos básicos para ayudar a comprender el servicio. 

## <a name="minimum-requirements"></a>Requisitos mínimos

- Nivel estándar de IoT Hub
    - Rol de RBAC **propietario** privilegios 
- [Área de trabajo de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) 
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

1. Abra IoT Hub. 
2. Haga clic en **Descripción general**. 
3. Compruebe la ubicación indicada coincide con uno de los [servicio regiones admitidas](#supported-service-regions). 


## <a name="supported-platforms-for-agents"></a>Plataformas compatibles con los agentes 

ASC para agentes de IoT es compatible con una lista creciente de dispositivos y plataformas. Consulte la [admite la lista de plataformas](how-to-deploy-agent.md) para comprobar su biblioteca de dispositivos existente o planeada.  

## <a name="next-steps"></a>Pasos siguientes
- [Información general](overview.md)
- [Habilitar el servicio](quickstart-onboard-iot-hub.md)
- [ASC para IoT preguntas más frecuentes](resources-frequently-asked-questions.md)
- [Descripción de ASC para las alertas de IoT](concept-security-alerts.md)
