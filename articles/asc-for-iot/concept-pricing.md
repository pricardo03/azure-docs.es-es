---
title: Comprender la versión preliminar de costos de Azure Security Center para IoT | Microsoft Docs
description: Obtenga información sobre los costos asociados con Azure Security Center para IoT y cómo se puede controlar.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: ef839708-4574-4a40-bc45-07005f8e9daf
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: 00666e27c9251248aa6ecff75d88908baabf71f3
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64919729"
---
# <a name="pricing-and-associated-costs"></a>Precios y costos asociados

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo explica Azure Security Center (ASC) para el modelo de precios de IoT, se resume todos los costes asociados y se explica cómo administrarlas.

## <a name="pricing"></a>Precios

La ASC para IoT, modelo de precios está formado por dos partes y se factura una vez que un centro de IoT es [habilitado](quickstart-onboard-iot-hub.md) en ASC para IoT:

- Costo por dispositivo - capacidades de seguridad incorporadas en función de análisis de registros de IoT Hub.

- Costo por mensaje: capacidades de seguridad mejorada basada en mensajes de seguridad de dispositivos IoT Edge u hoja.

  >[!Note]
  > Los mensajes de seguridad también cargará el consumo de cuota en IoT Hub.

Para obtener más información, consulte [precios de Security Center](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="associated-costs"></a>Costos asociados

ASC para IoT tiene dos tipos de los costos asociados, que no forman parte de los precios directa:

- Consumo de cuota de IoT Hub

- Los costos de almacenamiento del análisis de registro

Puede reducir los costos asociados si deciden no utilizar determinadas características, cambiando la configuración.

Para cambiar la configuración:

1. Abra IoT Hub.

2. En **seguridad**, haga clic en **Introducción**.

3. Haga clic en **Configuración**.

En la tabla siguiente proporciona un resumen de los costos asociados y las implicaciones de cada opción.

|     | Uso | Comentario |
| --- | --- | --- |
| **Consumo de cuota de IoT Hub** |  |
| [Exportar dispositivos](https://docs.microsoft.com/azure/iot-hub/iot-hub-bulk-identity-mgmt#export-devices) trabajo (exportar gemelo) | Una vez al día | Deshabilitar _gemelas de la colección de metadatos_ |
| **Almacenamiento de log Analytics** |  |
| Las alertas y la recomendación de dispositivo| Recomendación de seguridad y las alertas generadas por el servicio | No es opcional |
| Datos sin procesar de seguridad| Datos de seguridad sin procesar de los dispositivos de IoT, recopilados por los agentes de seguridad | Deshabilitar _almacenar eventos de seguridad de dispositivo sin formato_ |

>[!Important]
> Rechazarlo tiene implicaciones serias a las características de seguridad disponibles.
  
| Deshabilitar envío | Implicaciones |
| --- | --- |
| _Colección de metadatos gemelos_ | Deshabilitar [alertas personalizadas](quickstart-create-custom-alerts.md) |
| | Deshabilitar las recomendaciones de manifiesto de IoT Edge |
| | Deshabilitar alertas y recomendaciones basada en identidades de dispositivo |
| _Eventos de seguridad del dispositivo sin formato de Store_ | Obtener más información sobre recomendaciones de línea base del sistema operativo de dispositivo no está disponibles |
| | Los detalles de [alerta](concept-security-alerts.md) y [recomendación](concept-recommendations.md) no están disponibles las investigaciones |


## <a name="see-also"></a>Vea también

- Acceso a su [datos sin procesar de seguridad](how-to-security-data-access.md)
- [Investigar un dispositivo](how-to-investigate-device.md)
- Conocer y explorar [recomendaciones de seguridad](concept-recommendations.md)
- Conocer y explorar [las alertas de seguridad](concept-security-alerts.md)
