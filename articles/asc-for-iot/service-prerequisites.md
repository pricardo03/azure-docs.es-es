---
title: Requisitos previos de Azure Security Center para IoT | Microsoft Docs
description: Detalles de todo lo necesario para empezar a conocer los requisitos previos del servicio Azure Security Center for IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 790cbcb7-1340-4cc1-9509-7b262e7c3181
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/28/2019
ms.author: mlottner
ms.openlocfilehash: cc2dc3a190e3ad06bdc048f2a5770eae2a6990ec
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596818"
---
# <a name="azure-security-center-for-iot-prerequisites"></a>Requisitos previos de Azure Security Center for IoT

En este artículo proporciona una explicación sobre los diferentes bloques de creación del servicio Azure Security Center (ASC) para IoT, lo que se necesita para comenzar, y explica igualmente los conceptos básicos para ayudar a conocer el servicio. 

## <a name="minimum-requirements"></a>Requisitos mínimos

- Nivel estándar de IoT Hub
    - Privilegios de nivel **Propietario** del rol RBAC 
- [Área de trabajo de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) 
- Azure Security Center (recomendado)
    - El uso de Azure Security Center es una recomendación, no un requisito. Sin Azure Security Center, no podrá ver los demás recursos de Azure en IoT Hub. 
 
## <a name="working-with-asc-for-iot-service"></a>Trabajar con el servicio ASC for IoT

La información y los informes de ASC for IoT están disponibles mediante Azure IoT Hub y Azure Security Center. Para habilitar ASC for IoT en Azure IoT Hub, se necesita una cuenta con privilegios de nivel **Propietario**. Después de habilitar ASC for IoT en IoT Hub, la información de ASC for IoT se muestra como la característica **Seguridad** en Azure IoT Hub y como **IoT** en Azure Security Center. 

## <a name="supported-service-regions"></a>Regiones de servicio admitidas 

Actualmente, ASC for IoT es compatible con IoT Hub de las siguientes regiones de Azure:
  - Centro de EE. UU.  
  - East US 
  - Este de EE. UU. 2
  - Centro occidental de EE.UU.
  - Oeste de EE. UU.
  - Oeste de EE. UU. 2
  - Centro-Sur de EE. UU.
  - Centro-Norte de EE. UU
  - Centro de Canadá
  - Este de Canadá 
  - Europa del Norte    
  - Sur de Brasil
  - Centro de Francia  
  - Oeste de Reino Unido 
  - Sur de Reino Unido 2
  - Europa occidental 
  - Norte de Europa 
  - Oeste de Japón  
  - Este de Japón  
  - Sudeste de Australia
  - Este de Australia
  - Asia oriental   
  - Sudeste asiático
  - Corea Central
  - Corea del Sur 
  - India Central
  - Sur de la India
  
## <a name="wheres-my-iot-hub"></a>¿Dónde está mi IoT Hub?

Antes de comenzar, compruebe la ubicación del IoT Hub para confirmar la disponibilidad del servicio. 

1. Abra IoT Hub. 
2. Haga clic en **Descripción general**. 
3. Compruebe que la ubicación que aparece coincide con una de las [regiones de servicio admitidas](#supported-service-regions). 


## <a name="supported-platforms-for-agents"></a>Plataformas compatibles con los agentes 

Los agentes de ASC for IoT admiten una lista cada vez mayor de dispositivos y plataformas. Vea la [lista de plataformas admitidas](how-to-deploy-agent.md) para consultar su biblioteca de dispositivos prevista o existente.  

## <a name="next-steps"></a>Pasos siguientes
- Lea la [introducción](overview.md) a la seguridad de Azure IoT
- Aprenda a [habilitar el servicio](quickstart-onboard-iot-hub.md)
- Lea las [preguntas más frecuentes sobre Azure Security Center para IoT](resources-frequently-asked-questions.md)
- Explore la [información sobre Alertas de seguridad de Azure Security Center para IoT](concept-security-alerts.md)
