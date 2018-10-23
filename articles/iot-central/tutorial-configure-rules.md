---
title: Configuración de reglas y acciones en Azure IoT Central | Microsoft Docs
description: Este tutorial muestra cómo configurar las reglas basadas en la telemetría y acciones en la aplicación de Azure IoT Central como generador.
author: ankitgupta
ms.author: ankitgup
ms.date: 04/16/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 20dcad571a32638488ce111b5af55623f2305c57
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2018
ms.locfileid: "49310443"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central"></a>Tutorial: Configuración de reglas y acciones para el dispositivo en Azure IoT Central

*Este artículo se aplica a los administradores, operadores y compiladores.*

En este tutorial se crea una regla que envía un correo electrónico cuando la temperatura de un dispositivo de aire acondicionado conectado supera los 90&deg; F.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una regla basada en la telemetría
> * Agregar una acción

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, debe completar el tutorial [Definición de un nuevo tipo de dispositivo en la aplicación](tutorial-define-device-type.md) para crear la plantilla de dispositivo **Connected Aire Conditioner** con la que trabajar.

## <a name="create-a-telemetry-based-rule"></a>Crear una regla basada en la telemetría

1. Para agregar a la aplicación una nueva regla basada en la telemetría, en el menú de navegación izquierdo, elija **Device Explorer** (Explorador de dispositivos):

    ![Página Device Explorer (Explorador de dispositivos)](media/tutorial-configure-rules/explorerpage1.png)

    Verá la plantilla de dispositivo **Connected Aire Conditioner (1.0.0)** y el dispositivo **Connected Aire Conditioner-1** creado en el tutorial anterior.

2. Para empezar a personalizar el dispositivo de aire acondicionado conectado, elija el dispositivo que creó en el tutorial anterior:

    ![Página del aire acondicionado conectado](media/tutorial-configure-rules/builderdevicelist1.png)

3. Para empezar a agregar una regla en la vista **Rules** (Reglas), elija **Rules** (Reglas) y luego haga clic en **Edit Template** (Editar plantilla):

    ![Vista Rules (Reglas)](media/tutorial-configure-rules/builderedittemplate.png)

4. Para crear una regla de telemetría basada en el umbral, haga clic en **New Rule** (Nueva regla) y luego en **Telemetry** (Telemetría).

    ![Editar plantilla](media/tutorial-configure-rules/buildernewrule.png)

5. Para definir la regla, use la información de la tabla siguiente:

    | Configuración                                      | Valor                             |
    | -------------------------------------------- | ------------------------------    |
    | NOMBRE                                         | Alerta de temperatura de aire acondicionado |
    | Habilitación de reglas para todos los dispositivos de esta plantilla | Por                                |
    | Habilitar regla en este dispositivo                   | Por                                |
    | Condición                                    | La temperatura supera los 90°    |
    | Agregación                                  | None                              |

    ![Condición de regla de temperatura](media/tutorial-configure-rules/buildertemperaturerule1.png)

## <a name="add-an-action"></a>Agregar una acción

Al definir una regla, también se defina una acción que se ejecuta cuando se cumplen las condiciones de la regla. En este tutorial se agrega una acción para enviar una notificación de que se desencadenó la regla por correo electrónico.

1. Para agregar una **acción**, primero **guarde** la regla y luego desplácese hacia abajo en el panel **Configure Telemetry Rule** (Configurar regla de telemetría) y seleccione el signo **+** junto a **Actions** (Acciones) y luego seleccione **Email** (Correo electrónico):

    ![Acción de regla de temperatura](media/tutorial-configure-rules/builderaddaction1.png)

2. Para definir la acción, use la información de la tabla siguiente:

    | Configuración   | Valor                          |
    | --------- | ------------------------------ |
    | Para        | La dirección de correo electrónico propia             |
    | Notas     | La temperatura del aire acondicionado excedió el umbral. |

    > [!NOTE]
    > Para recibir una notificación por correo electrónico, la dirección debe ser un [identificador de usuario de la aplicación](howto-administer.md) y ese usuario debe haber iniciado sesión en la aplicación al menos una vez.

    ![Acción de temperatura del generador de aplicaciones](media/tutorial-configure-rules/buildertemperatureaction.png)

3. Elija **Guardar**. La regla se muestra en la página **Rules** (Reglas):

    ![Reglas del generador de aplicaciones](media/tutorial-configure-rules/builderrules1.png)

4. Seleccione **Listo** para salir del modo **Editar plantilla**.
 

## <a name="test-the-rule"></a>Prueba de la regla

Poco después de guardar la regla, esta se activa. Cuando se cumplan las condiciones definidas en la regla, la aplicación enviará un mensaje a la dirección de correo electrónico que especificó en la acción.

![Acción de enviar un correo electrónico](media/tutorial-configure-rules/email.png)

> [!NOTE]
> Una vez terminada la prueba, desactive la regla para dejar de recibir alertas en la bandeja de entrada. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Crear una regla basada en la telemetría
> * Agregar una acción

Ahora que ha definido una regla basada en umbrales, el siguiente paso sugerido es [personalizar las vistas del operador](tutorial-customize-operator.md).

Para más información sobre los diferentes tipos de reglas de Azure IoT Central y cómo parametrizar la definición de las reglas, consulte:
* [Creación de una regla de telemetría y configuración de las notificaciones](howto-create-telemetry-rules.md).
* [Creación de una regla de evento y configuración de las notificaciones](howto-create-event-rules.md).

<!-- Next tutorials in the sequence -->
