---
title: Requisitos previos de Azure Security Center for IoT, versión preliminar | Microsoft Docs
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
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 198459887ff19b16e897b2a8dde55bca1217c8ac
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67616446"
---
# <a name="azure-security-center-for-iot-prerequisites"></a>Requisitos previos de Azure Security Center for IoT

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se explican los diferentes bloques de creación del servicio Azure Security Center (ASC) for IoT, lo que se necesita para comenzar y los conceptos básicos para ayudar a conocer el servicio. 

## <a name="minimum-requirements"></a>Requisitos mínimos

- Nivel estándar de IoT Hub
    - Privilegios de nivel **Propietario** del rol RBAC 
- [Área de trabajo de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) 
- Azure Security Center (recomendado)
    - Aunque el uso de Azure Security Center es solo una recomendación, no un requisito, sin él no podrá ver los otros recursos de Azure dentro de IoT Hub. 
 
## <a name="working-with-asc-for-iot-service"></a>Trabajar con el servicio ASC for IoT

La información y los informes de ASC for IoT están disponibles mediante Azure IoT Hub y Azure Security Center. Para habilitar ASC for IoT en Azure IoT Hub, se necesita una cuenta con privilegios de nivel **Propietario**. Después de habilitar ASC for IoT en IoT Hub, la información de ASC for IoT se muestra como la característica **Seguridad** en Azure IoT Hub y como **IoT** en Azure Security Center. 

## <a name="supported-service-regions"></a>Regiones de servicio admitidas 

Actualmente, ASC for IoT es compatible con IoT Hub de las siguientes regiones de Azure:
  - Centro de EE. UU.
  - Norte de Europa
  - Sudeste asiático

## <a name="wheres-my-iot-hub"></a>¿Dónde está mi IoT Hub?

Antes de comenzar, compruebe la ubicación del IoT Hub para confirmar la disponibilidad del servicio. 

1. Abra IoT Hub. 
2. Haga clic en **Descripción general**. 
3. Compruebe que la ubicación que aparece coincide con una de las [regiones de servicio admitidas](#supported-service-regions). 


## <a name="supported-platforms-for-agents"></a>Plataformas compatibles con los agentes 

Los agentes de ASC for IoT admiten una lista cada vez mayor de dispositivos y plataformas. Vea la [lista de plataformas admitidas](how-to-deploy-agent.md) para consultar su biblioteca de dispositivos prevista o existente.  

## <a name="next-steps"></a>Pasos siguientes
- [Información general](overview.md)
- [Habilitar el servicio](quickstart-onboard-iot-hub.md)
- [ASC for IoT FAQ](resources-frequently-asked-questions.md) (Preguntas más frecuentes sobre ASC for IoT)
- [Conocer las alertas de ASC for IoT](concept-security-alerts.md)
