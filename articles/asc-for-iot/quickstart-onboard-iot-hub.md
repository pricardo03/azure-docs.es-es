---
title: Habilitación del servicio Azure Security Center para IoT en IoT Hub | Microsoft Docs
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
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 3d9c5352a90d5bcacbaf27b7b62be61fc404e87a
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71299492"
---
# <a name="quickstart-onboard-azure-security-center-for-iot-service-in-iot-hub"></a>Inicio rápido: Incorporación del servicio Azure Security Center para IoT en IoT Hub

En este artículo se explica cómo habilitar el servicio Azure Security Center para IoT en la instancia existente de IoT Hub. Si actualmente no tiene una instancia de IoT Hub, consulte [Creación de una instancia de IoT Hub mediante Azure Portal](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal) para comenzar. 

> [!NOTE]
> Actualmente, Azure Security Center for IoT solo admite el nivel Estándar de IoT Hub.
> Azure Security Center for IoT es una solución para un centro de conectividad individual. Si necesita varios centros de conectividad, se requieren varias soluciones de Azure Security Center para IoT. 

## <a name="prerequisites-for-enabling-the-service"></a>Requisitos previos para habilitar el servicio

- Área de trabajo de Log Analytics
  - De forma predeterminada, Azure Security Center para IoT almacena dos tipos de información en el área de trabajo de Log Analytics: **alertas de seguridad** y **recomendaciones**. 
  - Puede elegir agregar almacenamiento de un tipo de información adicional, **eventos sin procesar**. Tenga en cuenta que almacenar **eventos sin procesar** en Log Analytics conlleva costos de almacenamiento adicionales. 
- IoT Hub (nivel estándar)
- Cumplimiento de todos [requisitos previos de servicio](service-prerequisites.md) 

## <a name="enable-azure-security-center-for-iot-on-your-iot-hub"></a>Habilitación de Azure Security Center para IoT en IoT Hub 

Para habilitar la seguridad en IoT Hub, haga lo siguiente: 

1. Abra **IoT Hub** en Azure Portal. 
1. En el menú **Seguridad**, haga clic en **Secure your IoT solution** (Proteger la solución de IoT).
1. Deje la opción **Habilitar** seleccionada como valor predeterminado. 
1. Seleccione el área de trabajo de Log Analytics.
1. Especifique los datos del área de trabajo de Log Analytics. 
   - Si opta por habilitar la **recopilación de gemelos colección**, deje el conmutador de **recopilación de gemelos** en la posición **On** (Activar).
   - Si opta por almacenar **eventos sin procesar**, además de los tipos de información predeterminada de almacenamiento, seleccione los **eventos de seguridad del dispositivo sin procesar** en Log Analytics. Deje el **evento sin procesar** en la posición **On** (Activar). 
    
1. Haga clic en **Save**(Guardar). 

Felicidades. Ha completado la habilitación de Azure Security Center para IoT en IoT Hub. 

## <a name="next-steps"></a>Pasos siguientes

Pase al siguiente artículo para configurar una solución...

> [!div class="nextstepaction"]
> [Configuración de una solución](quickstart-configure-your-solution.md)


