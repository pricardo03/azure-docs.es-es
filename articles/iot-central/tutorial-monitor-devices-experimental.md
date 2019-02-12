---
title: Supervisión de los dispositivos en Azure IoT Central | Microsoft Docs
description: Como un operador, puede usar la aplicación de Azure IoT Central para supervisar los dispositivos.
author: dominicbetts
ms.author: dobett
ms.date: 02/01/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 9480565643887b5a9a4d644ba3173b365eaea29c
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55767780"
---
# <a name="tutorial-use-azure-iot-central-to-monitor-your-devices"></a>Tutorial: Uso de Azure IoT Central para supervisar los dispositivos

Este tutorial le muestra, como un operador, cómo usar la aplicación de Microsoft Azure IoT Central para supervisar los dispositivos y cambiar la configuración.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Recibir una notificación
> * Investigar un problema
> * Corregir un problema

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, el desarrollador debería realizar los tres tutoriales del desarrollador para crear la aplicación de Azure IoT Central:

* [Definición de un tipo de dispositivo nuevo](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
* [Configuración de reglas y acciones para el dispositivo](tutorial-configure-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
* [Personalización de la vista del operador](tutorial-customize-operator-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)

## <a name="receive-a-notification"></a>Recibir una notificación

Azure IoT Central envía notificaciones acerca de los dispositivos como mensajes de correo electrónico. El desarrollador ha agregado una regla para enviar una notificación cuando la temperatura en un dispositivo acondicionador de aire conectado supera un umbral. Compruebe los correos electrónicos enviados a la cuenta que el desarrollador ha elegido para recibir notificaciones.

Abra el mensaje de correo electrónico que recibió al final del tutorial [Configuración de reglas y acciones para el dispositivo](tutorial-configure-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json). En el correo electrónico, elija **Haga clic aquí para abrir el dispositivo**:

![Correo electrónico de notificación de alerta](media/tutorial-monitor-devices-experimental/email.png)

La página **Device** (Dispositivo) del dispositivo simulado **Connected Air Conditioner-1** (Acondicionador de aire conectado-1) que creó en los tutoriales anteriores se abre en el explorador:

![Dispositivo que desencadenó el mensaje de correo electrónico de notificación](media/tutorial-monitor-devices-experimental/sourcedevice.png)

## <a name="investigate-an-issue"></a>Investigar un problema

Como un operador, puede ver información sobre el dispositivo en las páginas **Measurements** (Medidas), **Settings** (Configuración), **Properties** (Propiedades), **Rules** (Reglas) y  **Dashboard** (Panel). El desarrollador ha personalizado el **Panel** para mostrar información relevante acerca de un dispositivo acondicionador de aire conectado.

Elija la vista **Dashboard** (Panel) para ver información acerca del dispositivo.

![Panel del dispositivo](media/tutorial-monitor-devices-experimental/initial_screen.png)

El gráfico del panel muestra un trazado de la temperatura del dispositivo. También puede ver la temperatura de destino actual del dispositivo en el icono **Set target temperature** (Establecer temperatura de destino). Imaginemos que decide que la temperatura de destino es demasiado alta.

## <a name="remediate-an-issue"></a>Corregir un problema

Para cambiar la temperatura de destino del dispositivo, use la página **Settings** (Configuración):

1. Elija **Settings** (Configuración). Cambie **Set Temperature** (Establecer temperatura) a 75. Elija **Update** (Actualizar) para enviar la nueva temperatura de destino al dispositivo. Cuando el dispositivo confirma el cambio de configuración, el estado del valor cambia a **sincronizado**:

    ![Actualización de la configuración](media/tutorial-monitor-devices-experimental/change_settings.png)

2. Elija **Dashboard** (Panel) y compruebe el nuevo valor de configuración:

    ![Panel del dispositivo actualizado](media/tutorial-monitor-devices-experimental/new_settings.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="nextstepaction"]
> * Recibir una notificación
> * Investigar un problema
> * Corregir un problema

Ahora que sabe cómo supervisar un dispositivo, el siguiente paso que se sugiere es [agregar un dispositivo](tutorial-add-device-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).