---
title: Uso del servicio Azure IoT Hub Device Provisioning para aprovisionar dispositivos en instancias de IoT Hub con equilibrio de carga | Microsoft Docs
description: Aprovisionamiento automático de dispositivos mediante DPS en instancias de IoT Hub con equilibrio de carga en Azure Portal
author: sethmanheim
ms.author: sethm
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: d0a3720fe729d5e260bbe5b0902460c8c7cfc7cb
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629633"
---
# <a name="provision-devices-across-load-balanced-iot-hubs"></a>Aprovisionamiento de dispositivos en instancias de IoT Hub con equilibrio de carga

Este tutorial muestra cómo aprovisionar dispositivos para varias instancias de IoT Hub con equilibrio de carga mediante el servicio Device Provisioning (DPS). En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Usar Azure Portal para aprovisionar un segundo dispositivo en un segundo centro de IoT 
> * Agregar una entrada de la lista de inscripción en el segundo dispositivo
> * Establecer la directiva de asignación de DPS en **distribución uniforme**
> * Vincular la nueva instancia de IoT Hub a DPS

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>requisitos previos

Este tutorial se basa en el tutorial anterior, [Provision the device to an IoT hub using the Azure IoT Hub Device Provisioning Service](tutorial-provision-device-to-hub.md) (Aprovisionamiento del dispositivo en un centro de IoT mediante el servicio IoT Hub Device Provisioning).

## <a name="use-the-azure-portal-to-provision-a-second-device-to-a-second-iot-hub"></a>Uso de Azure Portal para aprovisionar un segundo dispositivo en un segundo centro de IoT

Siga los pasos del tutorial [Provision the device to an IoT hub using the Azure IoT Hub Device Provisioning Service](tutorial-provision-device-to-hub.md) para aprovisionar un segundo dispositivo en otro centro de IoT (Aprovisionamiento del dispositivo en un centro de IoT mediante el servicio IoT Hub Device Provisioning).

## <a name="add-an-enrollment-list-entry-to-the-second-device"></a>Agregar una entrada de la lista de inscripción en el segundo dispositivo

La lista de inscripción le indica al DPS qué método de atestación (el método para confirmar una identidad de dispositivo) se está usando con el dispositivo. El siguiente paso es agregar una entrada de la lista de inscripción para el segundo dispositivo. 

1. En la página de su DPS, haga clic en **Manage enrollments** (Administrar inscripciones). Aparece la página **Add enrollment list entry** (Agregar entrada de la lista de inscripción). 
2. Haga clic en **Agregar** en la parte superior de la página.
2. Rellene los campos y, a continuación, haga clic en **Guardar**.

## <a name="set-the-dps-allocation-policy"></a>Establecimiento de la directiva de asignación de DPS

La directiva de asignación es una configuración del servicio Device Provisioning que determina cómo se asignan los dispositivos a una instancia de IoT Hub. Hay tres directivas de asignación admitidas: 

1. **Latencia más baja**: los dispositivos se aprovisionan en una instancia de IoT Hub en función del centro con la latencia más baja en el dispositivo.
2. **Distribución uniformemente ponderada** (predeterminada): las instancias de IoT Hub vinculadas tienen la misma probabilidad de tener dispositivos aprovisionados para ellos. Esta es la configuración predeterminada. Si va a aprovisionar dispositivos para un único centro de IoT Hub, puede mantener esta configuración. 
3. **Configuración estática a través de la lista de inscripción**: la especificación del centro de IoT deseado en la lista de inscripción tiene prioridad sobre la directiva de asignación del nivel del DPS.

Siga estos pasos para establecer la directiva de asignación:

1. Para establecer la directiva de asignación, en la página del DPS haga clic en **Manage allocation policy** (Administrar directiva de asignación).
2. Establezca la directiva de asignación en **Distribución uniformemente ponderada**.
3. Haga clic en **Save**(Guardar).

## <a name="link-the-new-iot-hub-to-dps"></a>Vincular la nueva instancia de IoT Hub a DPS

Vincule el DPS y la instancia de IoT Hub para que el DPS pueda registrar los dispositivos en esa instancia.

1. En la página **Todos los recursos**, haga clic en el DPS que creó anteriormente.
2. En la página del DPS, haga clic en **Linked IoT hubs** (Centros de IoT vinculados).
3. Haga clic en **Agregar**.
4. En la página **Add link to IoT hub** (Agregar vínculo a centro de IoT), utilice los botones de radio para especificar si el centro de IoT vinculado se encuentra en la suscripción actual o en una suscripción diferente. A continuación, elija el nombre del centro de IoT en el cuadro **Centro de IoT**.
5. Haga clic en **Save**(Guardar).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Usar Azure Portal para aprovisionar un segundo dispositivo en un segundo centro de IoT 
> * Agregar una entrada de la lista de inscripción en el segundo dispositivo
> * Establecer la directiva de asignación de DPS en **distribución uniforme**
> * Vincular la nueva instancia de IoT Hub a DPS

<!-- Advance to the next tutorial to learn how to 
 Replace this .md
> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)
-->
