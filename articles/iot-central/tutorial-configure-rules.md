---
title: Configuración de reglas y acciones en Azure IoT Central | Microsoft Docs
description: Este tutorial muestra cómo configurar las reglas basadas en la telemetría y acciones en la aplicación de Azure IoT Central como generador.
author: ankitscribbles
ms.author: ankitgup
ms.date: 06/09/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 30bfd3b119b3ae4badbda50747158e8b08a9dd08
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69875518"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central"></a>Tutorial: Configuración de reglas y acciones para el dispositivo en Azure IoT Central

*Este artículo se aplica a los administradores, operadores y compiladores.*

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

En este tutorial se crea una regla que envía un correo electrónico cuando la temperatura de un dispositivo de aire acondicionado conectado supera los 90&deg; F.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una regla basada en la telemetría
> * Agregar una acción

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, debe completar el tutorial [Definición de un nuevo tipo de dispositivo en la aplicación](tutorial-define-device-type.md) para crear la plantilla de dispositivo **Connected Aire Conditioner** con la que trabajar.

## <a name="create-a-telemetry-based-rule"></a>Crear una regla basada en la telemetría

1. Para agregar una nueva regla basada en telemetría a la aplicación, en el menú de navegación izquierdo, seleccione **Device Templates** (Plantillas de dispositivos):

    ![Página Plantillas de dispositivo](media/tutorial-configure-rules/templatespage1.png)

    Verá la plantilla del dispositivo **Connected Aire Conditioner (1.0.0)** que creó en el tutorial anterior.

2. Para personalizar la plantilla del dispositivo, seleccione la plantilla **Connected Air Conditioner** que creó en el tutorial anterior.

3. Para agregar una regla basada en telemetría, en la vista **Reglas**, seleccione **Reglas**, **+ Nueva regla** y **Telemetría**:

    ![Vista Rules (Reglas)](media/tutorial-configure-rules/newrule.png)

5. Para definir la regla, use la información de la tabla siguiente:

    | Configuración                                      | Valor                             |
    | -------------------------------------------- | ------------------------------    |
    | NOMBRE                                         | Alerta de temperatura de aire acondicionado |
    | Habilitación de reglas para todos los dispositivos de esta plantilla | Por                                |
    | Condición                                    | La temperatura supera los 90°    |
    | Agregación                                  | None                              |

    ![Condición de regla de temperatura](media/tutorial-configure-rules/temperaturerule.png)

    Después, seleccione **Guardar**.

## <a name="add-an-action"></a>Agregar una acción

Al definir una regla, también se defina una acción que se ejecuta cuando se cumplen las condiciones de la regla. En este tutorial, creará una regla con una acción que envía una notificación por correo electrónico.

1. Para agregar una **acción**, en primer lugar **guarde** la regla y, después, desplácese hacia abajo en el panel **Configurar regla Telemetría**. Elija **+** junto a **Acciones**y, después, elija **Correo electrónico**:

    ![Acción de regla de temperatura](media/tutorial-configure-rules/addaction.png)

2. Para definir la acción, use la información de la tabla siguiente:

    | Configuración   | Valor                          |
    | --------- | ------------------------------ |
    | Para        | La dirección de correo electrónico propia             |
    | Notas     | La temperatura del aire acondicionado excedió el umbral. |

    > [!NOTE]
    > Para recibir una notificación por correo electrónico, la dirección debe ser un [identificador de usuario de la aplicación](howto-administer.md) y ese usuario debe haber iniciado sesión en la aplicación al menos una vez.

    ![Acción de Temperature](media/tutorial-configure-rules/temperatureaction.png)

3. Seleccione **Guardar**. La regla se muestra en la página **Reglas**.

## <a name="test-the-rule"></a>Prueba de la regla

Poco después de guardar la regla, esta se activa. Cuando se cumplan las condiciones definidas en la regla, la aplicación enviará un mensaje a la dirección de correo electrónico que especificó en la acción.

> [!NOTE]
> Una vez terminada la prueba, desactive la regla para dejar de recibir alertas en la bandeja de entrada.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Crear una regla basada en la telemetría
> * Agregar una acción

Ahora que ha definido una regla basada en umbral, el siguiente paso sugerido es [personalizar las vistas del operador](tutorial-customize-operator.md).

Para más información sobre los diferentes tipos de reglas de Azure IoT Central y cómo parametrizar la definición de las reglas, consulte:
* [Creación de una regla de telemetría y configuración de las notificaciones](howto-create-telemetry-rules.md).
* [Creación de una regla de evento y configuración de las notificaciones](howto-create-event-rules.md).

<!-- Next tutorials in the sequence -->