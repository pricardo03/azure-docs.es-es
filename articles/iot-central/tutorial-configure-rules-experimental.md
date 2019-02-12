---
title: Configuración de reglas y acciones en Azure IoT Central | Microsoft Docs
description: Este tutorial muestra cómo configurar las reglas basadas en la telemetría y acciones en la aplicación de Azure IoT Central como generador.
author: ankitscribbles
ms.author: ankitgup
ms.date: 01/28/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: d8a5ca6285624720e23a4986917ab5e715f6ebfa
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55768020"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central-new-ui-design"></a>Tutorial: Configuración de reglas y acciones para el dispositivo en Azure IoT Central (nuevo diseño de la interfaz de usuario)

*Este artículo se aplica a los administradores, operadores y compiladores.*

En este tutorial se crea una regla que envía un correo electrónico cuando la temperatura de un dispositivo de aire acondicionado conectado supera los 90&deg; F.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una regla basada en la telemetría
> * Agregar una acción

[!INCLUDE [iot-central-experimental-note](../../includes/iot-central-experimental-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, debe completar el tutorial [Definición de un nuevo tipo de dispositivo en la aplicación](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) para crear la plantilla de dispositivo **Connected Aire Conditioner** con la que trabajar.

## <a name="create-a-telemetry-based-rule"></a>Crear una regla basada en la telemetría

1. Para agregar una nueva regla basada en telemetría a la aplicación, en el menú de navegación izquierdo, elija **Device Templates** (Plantillas de dispositivos):

    ![Página Plantillas de dispositivo](media/tutorial-configure-rules-experimental/templatespage1.png)

    Verá la plantilla del dispositivo **Connected Aire Conditioner (1.0.0)** que creó en el tutorial anterior.

2. Para personalizar la plantilla del dispositivo, haga clic en la plantilla **Connected Air Conditioner** que creó en el tutorial anterior.

3. Para agregar una regla basada en telemetría, en la vista **Reglas**, elija **Reglas**, haga clic en **+ Nueva regla**y elija **Telemetría**:

    ![Vista Rules (Reglas)](media/tutorial-configure-rules-experimental/newrule.png)

5. Para definir la regla, use la información de la tabla siguiente:

    | Configuración                                      | Valor                             |
    | -------------------------------------------- | ------------------------------    |
    | NOMBRE                                         | Alerta de temperatura de aire acondicionado |
    | Habilitación de reglas para todos los dispositivos de esta plantilla | Por                                |
    | Condición                                    | La temperatura supera los 90°    |
    | Agregación                                  | None                              |

    ![Condición de regla de temperatura](media/tutorial-configure-rules-experimental/temperaturerule.png)

    A continuación, haga clic en **Guardar**.

## <a name="add-an-action"></a>Agregar una acción

Al definir una regla, también se defina una acción que se ejecuta cuando se cumplen las condiciones de la regla. En este tutorial, creará una regla con una acción que envía una notificación por correo electrónico.

1. Para agregar una **acción**, en primer lugar **guarde** la regla y, después, desplácese hacia abajo en el panel **Configurar regla Telemetría**. Elija **+** junto a **Acciones**y, después, elija **Correo electrónico**:

    ![Acción de regla de temperatura](media/tutorial-configure-rules-experimental/addaction.png)

2. Para definir la acción, use la información de la tabla siguiente:

    | Configuración   | Valor                          |
    | --------- | ------------------------------ |
    | Para        | La dirección de correo electrónico propia             |
    | Notas     | La temperatura del aire acondicionado excedió el umbral. |

    > [!NOTE]
    > Para recibir una notificación por correo electrónico, la dirección debe ser un [identificador de usuario de la aplicación](howto-administer-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) y ese usuario debe haber iniciado sesión en la aplicación al menos una vez.

    ![Acción de Temperature](media/tutorial-configure-rules-experimental/temperatureaction.png)

3. Haga clic en **Save**(Guardar). La regla se muestra en la página **Reglas**.

## <a name="test-the-rule"></a>Prueba de la regla

Poco después de guardar la regla, esta se activa. Cuando se cumplan las condiciones definidas en la regla, la aplicación enviará un mensaje a la dirección de correo electrónico que especificó en la acción.

![Acción de enviar un correo electrónico](media/tutorial-configure-rules-experimental/email.png)

> [!NOTE]
> Una vez terminada la prueba, desactive la regla para dejar de recibir alertas en la bandeja de entrada.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Crear una regla basada en la telemetría
> * Agregar una acción

Ahora que ha definido una regla basada en umbral, el siguiente paso sugerido es [personalizar las vistas del operador](tutorial-customize-operator-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

Para más información sobre los diferentes tipos de reglas de Azure IoT Central y cómo parametrizar la definición de las reglas, consulte:
* [Creación de una regla de telemetría y configuración de las notificaciones](howto-create-telemetry-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).
* [Creación de una regla de evento y configuración de las notificaciones](howto-create-event-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

<!-- Next tutorials in the sequence -->
