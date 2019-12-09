---
title: 'Tutorial: Supervisión de los dispositivos en Azure IoT Central'
description: Este tutorial le muestra, como operador, cómo usar la aplicación de Azure IoT Central para supervisar los dispositivos.
author: dominicbetts
ms.author: dobett
ms.date: 11/13/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: c04793f22e146491efdffc81f28e1719542af054
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74705860"
---
# <a name="tutorial-use-azure-iot-central-to-monitor-your-devices"></a>Tutorial: Uso de Azure IoT Central para supervisar los dispositivos

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

Este tutorial le muestra, como un operador, cómo usar la aplicación de Microsoft Azure IoT Central para supervisar los dispositivos y cambiar la configuración.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Recibir una notificación
> * Investigar un problema
> * Corregir un problema

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, el desarrollador debería realizar los tres tutoriales del desarrollador para crear la aplicación de Azure IoT Central:

* [Definición de un tipo de dispositivo nuevo](tutorial-define-device-type.md)
* [Configuración de reglas y acciones para el dispositivo](tutorial-configure-rules.md)
* [Personalización de la vista del operador](tutorial-customize-operator.md)

## <a name="receive-a-notification"></a>Recibir una notificación

Azure IoT Central envía notificaciones acerca de los dispositivos como mensajes de correo electrónico. El desarrollador ha agregado una regla para enviar una notificación cuando la temperatura en un dispositivo acondicionador de aire conectado supera un umbral. Compruebe los correos electrónicos enviados a la cuenta que el desarrollador ha elegido para recibir notificaciones.

Abra el mensaje de correo electrónico que recibió al final del tutorial [Configuración de reglas y acciones para el dispositivo](tutorial-configure-rules.md). En el correo electrónico, seleccione el vínculo al dispositivo situado junto a **Nombre de dispositivo** en la sección **Detalles**:

![Correo electrónico de notificación de alerta](media/tutorial-monitor-devices/email.png)

La página **Device** (Dispositivo) del dispositivo simulado **Connected Air Conditioner-1** (Acondicionador de aire conectado-1) que creó en los tutoriales anteriores se abre en el explorador:

![Dispositivo que desencadenó el mensaje de correo electrónico de notificación](media/tutorial-monitor-devices/sourcedevice.png)

## <a name="investigate-an-issue"></a>Investigar un problema

Como un operador, puede ver información sobre el dispositivo en las páginas **Measurements** (Medidas), **Settings** (Configuración), **Properties** (Propiedades), **Rules** (Reglas) y  **Dashboard** (Panel). El desarrollador ha personalizado el **Panel** para mostrar información relevante acerca de un dispositivo acondicionador de aire conectado.

Elija la vista **Dashboard** (Panel) para ver información acerca del dispositivo.

![Panel del dispositivo](media/tutorial-monitor-devices/initial_screen.png)

El gráfico del panel muestra un trazado de la temperatura del dispositivo. También puede ver la temperatura de destino actual del dispositivo en el icono **Device properties** (Propiedades del dispositivo). Imaginemos que decide que la temperatura de destino es demasiado alta.

## <a name="remediate-an-issue"></a>Corregir un problema

Para cambiar la temperatura de destino del dispositivo, use la página **Settings** (Configuración):

1. Elija **Settings** (Configuración). Cambie **Set Temperature** (Establecer temperatura) a 75. Elija **Update** (Actualizar) para enviar la nueva temperatura de destino al dispositivo. Cuando el dispositivo confirma el cambio de configuración, el estado del valor cambia a **sincronizado**:

    ![Actualización de la configuración](media/tutorial-monitor-devices/change_settings.png)

2. Elija **Dashboard** (Panel) y compruebe el nuevo valor de configuración:

    ![Panel del dispositivo actualizado](media/tutorial-monitor-devices/new_settings.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="nextstepaction"]
> * Recibir una notificación
> * Investigar un problema
> * Corregir un problema

Ahora que sabe cómo supervisar un dispositivo, el siguiente paso que se sugiere es [agregar un dispositivo](tutorial-add-device.md).
