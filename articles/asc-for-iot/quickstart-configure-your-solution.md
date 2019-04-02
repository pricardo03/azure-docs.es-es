---
title: Configuración de la solución ASC for IoT, versión preliminar | Microsoft Docs
description: Aprenda a configurar una solución de IoT de un extremo a otro mediante ASC for IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: ae2207e8-ac5b-4793-8efc-0517f4661222
ms.service: ascforiot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 3ba8b1a099dc204730813754fdfc4b0b8ce45303
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541762"
---
# <a name="quickstart-configure-your-iot-solution"></a>Inicio rápido: Configuración de una solución de IoT

> [!IMPORTANT]
> ASC for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se explica cómo realizar la configuración inicial de una solución de seguridad de IoT mediante ASC for IoT. 

## <a name="asc-for-iot"></a>ASC for IoT

ASC for IoT proporciona una seguridad completa, de un extremo a otro, para las soluciones de IoT que usen Azure.

Con ASC for IoT. se puede supervisar toda una solución de IoT desde un solo panel, así como presentar todos los dispositivos de IoT, las plataformas de IoT y los recursos de back-end de Azure.

Una vez que se habilita en IoT Hub, ASC for IoT identifica automáticamente otros servicios de Azure, que también estén conectados a IoT Hub y que estén relacionados con la solución de IoT.

Además de la detección automática de relaciones, se pueden elegir los otros recursos de Azure que se etiquetan como parte de la solución de IoT.
Las opciones que se seleccionen permiten agregar suscripciones completas, grupos de recursos o recursos individuales.

Después de definir todas las relaciones de los recursos, ASC for IoT aprovecha Azure Security Center para generar alertas y recomendaciones de seguridad relativas a estos recursos.

## <a name="add-azure-resources-to-your-iot-solution"></a>Adición de recursos de Azure a una solución de IoT

Para agregar un recurso nuevo a la solución de IoT, siga estos pasos: 

1. Abra **IoT Hub** en Azure Portal. 
2. Seleccione y abra **Resources** (Recursos) en **Security**  (Seguridad) en el menú izquierdo. 
3. Seleccione **Add resources** (Agregar recursos).
4. Elija los recursos que pertenezcan a su solución de IoT.
4. Haga clic en **Agregar**. 

Felicidades. Ha agregado un nuevo recurso a la solución de IoT.

ASC for IoT ahora supervisa los recursos que acaba de agregar y muestra alertas y recomendaciones de seguridad pertinentes como parte de la solución de IoT.

## <a name="next-steps"></a>Pasos siguientes

Pase al siguiente artículo para aprender a crear módulos de seguridad...

> [!div class="nextstepaction"]
> [Creación de módulos de seguridad](quickstart-create-security-twin.md)