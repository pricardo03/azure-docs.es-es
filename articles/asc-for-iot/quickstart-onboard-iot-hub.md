---
title: Habilitación del servicio Azure Security Center for IoT en IoT Hub, versión preliminar | Microsoft Docs
description: Aprenda a activar el servicio Azure Security Center for IoT en IoT Hub.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 670e6d2b-e168-4b14-a9bf-51a33c2a9aad
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/16/2019
ms.author: mlottner
ms.openlocfilehash: f81fb7aeed1b704ebdd82c1f5b83c33a4b05e9ca
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618001"
---
# <a name="quickstart-enable-service-in-iot-hub"></a>Inicio rápido: Habilitación del servicio en IoT Hub

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se explica cómo habilitar el servicio de la versión preliminar de Azure Security Center for IoT en IoT Hub.  

> [!NOTE]
> Actualmente, Azure Security Center for IoT solo admite el nivel Estándar de IoT Hub.
> Azure Security Center for IoT es una solución para un centro de conectividad individual. Si necesita varios centros de conectividad, se requieren varias soluciones. 

## <a name="prerequisites-for-enabling-the-service"></a>Requisitos previos para habilitar el servicio

- Área de trabajo de Log Analytics
  - De forma predeterminada ASC for IoT almacena dos tipos de información en el área de trabajo de Log Analytics: **alertas de seguridad** y **recomendaciones**. 
  - Puede elegir agregar almacenamiento de un tipo de información adicional, **eventos sin procesar**. Tenga en cuenta que almacenar **eventos sin procesar** en Log Analytics conlleva costos de almacenamiento adicionales. 
- IoT Hub (nivel estándar)
- Cumplimiento de todos [requisitos previos de servicio](service-prerequisites.md) 
- Regiones de servicio admitidas
  - Centro de EE. UU.
  - Norte de Europa
  - Sudeste asiático

## <a name="enable-asc-for-iot-on-your-iot-hub"></a>Habilitación de ASC for IoT en IoT Hub 

Para habilitar la seguridad en IoT Hub, haga lo siguiente: 

1. Abra **IoT Hub** en Azure Portal. 
2. En el menú **Security** (Seguridad), haga clic en **Overview** (Información general) y, a continuación, haga clic en **Start preview** (Iniciar vista previa). 
3. Elija **Enable IoT Security** (Habilitar seguridad de IoT). 
4. Especifique sus datos del área de trabajo de Log Analytics. 
   - Si opta por almacenar **eventos sin procesar**, además de los tipos de información predeterminada de almacenamiento, deje el conmutador de **eventos sin procesar** en la posición **On** (Activar). 
   - Si opta por habilitar la **recopilación de gemelos colección**, deje el conmutador de **recopilación de gemelos** en la posición **On** (Activar). 
5. Haga clic en **Save**(Guardar). 

Felicidades. Ha completado la habilitación de ASC for IoT en IoT Hub. 

## <a name="next-steps"></a>Pasos siguientes

Pase al siguiente artículo para aprender a configurar una solución...

> [!div class="nextstepaction"]
> [Configuración de una solución](quickstart-configure-your-solution.md)
