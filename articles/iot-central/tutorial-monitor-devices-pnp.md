---
title: Supervisión de los dispositivos en Azure IoT Central | Microsoft Docs
description: Como un operador, puede usar la aplicación de Azure IoT Central para supervisar los dispositivos.
author: dominicbetts
ms.author: dobett
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: cf50fd80bdbce5895bd1da39700d1c6e4cdcc230
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878786"
---
# <a name="tutorial-use-azure-iot-central-to-monitor-your-devices-preview-features"></a>Tutorial: Uso de Azure IoT Central para supervisar los dispositivos (características en versión preliminar)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Este tutorial le muestra, como un operador, cómo usar la aplicación de Microsoft Azure IoT Central para supervisar los dispositivos y cambiar la configuración.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Recibir una notificación
> * Investigar un problema
> * Corregir un problema

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, el desarrollador debería realizar los tres tutoriales del desarrollador para crear la aplicación de Azure IoT Central:

* [Definición de un tipo de dispositivo nuevo](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
* [Adición de un dispositivo](tutorial-add-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
* [Configuración de reglas y acciones para el dispositivo](tutorial-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)

## <a name="receive-a-notification"></a>Recibir una notificación

Azure IoT Central envía notificaciones acerca de los dispositivos como mensajes de correo electrónico. El desarrollador ha agregado una regla para enviar una notificación cuando la temperatura en un dispositivo sensor ambiental conectado supera un umbral. Compruebe los correos electrónicos enviados a la cuenta que el desarrollador ha elegido para recibir notificaciones.

Abra el mensaje de correo electrónico que recibió al final del tutorial [Configuración de reglas y acciones para el dispositivo](tutorial-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json). En el correo electrónico, seleccione el vínculo al dispositivo:

![Correo electrónico de notificación de alerta](media/tutorial-monitor-devices-pnp/email.png)

La vista **Dashboard** (Panel) del dispositivo simulado sensor ambiental que creó en los tutoriales anteriores se abre en el explorador:

![Dispositivo que desencadenó el mensaje de correo electrónico de notificación](media/tutorial-monitor-devices-pnp/dashboard.png)

## <a name="investigate-an-issue"></a>Investigar un problema

Los operadores pueden ver información sobre el dispositivo en las páginas **Dashboard** (Panel), **Environmental Sensor properties** (Propiedades del sensor ambiental) y**Commands** (Comandos). El generador personalizó las páginas **Dashboard** (Panel) y **Environmental Sensor properties** (Propiedades del sensor ambiental) para mostrar información importante sobre un dispositivo sensor ambiental conectado.

Elija la vista **Dashboard** (Panel) para ver información acerca del dispositivo.

El gráfico del panel muestra un trazado de la temperatura del dispositivo. También puede ver la temperatura de destino actual del dispositivo en el icono **Device properties** (Propiedades del dispositivo). Imaginemos que decide que la temperatura de destino es demasiado alta.

## <a name="remediate-an-issue"></a>Corregir un problema

Para hacer un cambio en el dispositivo, use la página **Environmental Sensor properties** (Propiedades del sensor ambiental):

1. Elija **Environmental Sensor properties** (Propiedades del sensor ambiental). Cambie el valor de **Brightness Level** (Nivel de brillo) a 10. Elija **Guardar** para actualizar el dispositivo. Cuando el dispositivo confirma el cambio en la configuración, el estado del valor cambia a **sincronizado**:

    ![Actualización de la configuración](media/tutorial-monitor-devices-pnp/change-settings.png)

2. Elija **Dashboard** (Panel) y compruebe el valor de la nueva propiedad:

    ![Panel del dispositivo actualizado](media/tutorial-monitor-devices-pnp/new-settings.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

* Recibir una notificación
* Investigar un problema
* Corregir un problema

Ahora que sabe cómo supervisar un dispositivo, el siguiente paso que se recomienda es:

> [!div class="nextstepaction"]
> [Configure rules and actions for your device](tutorial-configure-rules.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Configuración de reglas y acciones para el dispositivo).