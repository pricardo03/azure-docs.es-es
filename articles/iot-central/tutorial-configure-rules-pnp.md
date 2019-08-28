---
title: Configuración de reglas y acciones en Azure IoT Central | Microsoft Docs
description: Este tutorial muestra cómo configurar las reglas basadas en la telemetría y acciones en la aplicación de Azure IoT Central como generador.
author: dominicbetts
ms.author: dobett
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: c2aa6edfe7ce9b670ea1015e2da72f3ecc48c9ec
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878836"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central-preview-features"></a>Tutorial: Configuración de reglas y acciones para el dispositivo en Azure IoT Central (características en versión preliminar)

*Este artículo se aplica a los administradores, operadores y compiladores.*

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

En este tutorial se crea una regla que envía un correo electrónico cuando la temperatura de sensor ambiental supera los 90&deg; F.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una regla basada en la telemetría
> * Agregar una acción

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, debe completar el tutorial [Definición de un nuevo tipo de dispositivo en la aplicación](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) para crear la plantilla de dispositivo **Environment Sensor** (Sensor ambiental) con la que trabajar.

## <a name="create-a-telemetry-based-rule"></a>Crear una regla basada en la telemetría

1. Para agregar una regla basada en telemetría a la aplicación, en el menú de navegación izquierdo, seleccione **Rules** (Reglas).

1. Para crear una regla, seleccione **+ New** (+ Nuevo). Luego, elija **Telemetry** (Telemetría).

1. Especifique **Environmental temperature** (Temperatura ambiental) como nombre de la regla.

1. En la sección **Scope** (Ámbito), seleccione **Environment Sensor** (Sensor ambiental) como plantilla de dispositivo. El ámbito al que se aplica esta regla. Para agregar más criterios de filtro, use **+ Filter** (+ Filtro).

1. En la sección **Condition** (Condición), defina lo que desencadena la regla. Use la siguiente información para definir una condición basada en los datos de telemetría de temperatura:

    | Campo                                        | Valor                             |
    | -------------------------------------------- | ------------------------------    |
    | Medición                                  | Temperatura                       |
    | Operator                                     | es mayor que                   |
    | Valor                                        | 90                                |

    Para agregar más condiciones, seleccione **+ Condition** (+ Condición).

    ![Crear condición de regla](./media/tutorial-configure-rules-pnp/condition.png)

1. Para agregar una acción para que se ejecute cuando se desencadene la regla, seleccione **+ Action** (+ Acción) y elija **Email** (Correo electrónico).

1. Use la información de la tabla siguiente para definir la acción:

    | Configuración   | Valor                                             |
    | --------- | ------------------------------------------------- |
    | Para        | La dirección de correo electrónico propia                                |
    | Notas     | La temperatura ambiental superó el umbral. |

    > [!NOTE]
    > Para recibir una notificación por correo electrónico, la dirección debe ser un [identificador de usuario de la aplicación](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) y ese usuario debe haber iniciado sesión en la aplicación al menos una vez.

    ![Crear acción de regla](./media/tutorial-configure-rules-pnp/action.png)

1. Seleccione **Guardar**. La regla se muestra en la página **Reglas**.

## <a name="test-the-rule"></a>Prueba de la regla

Poco después de guardar la regla, esta se activa. Cuando se cumplan las condiciones definidas en la regla, la aplicación enviará un mensaje a la dirección de correo electrónico que especificó en la acción.

> [!NOTE]
> Una vez terminada la prueba, desactive la regla para dejar de recibir alertas en la bandeja de entrada.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

* Crear una regla basada en la telemetría
* Agregar una acción

Ahora que ha definido una regla basada en umbral, el siguiente paso que se recomienda dar es:

> [!div class="nextstepaction"]
> [Creación de una regla de evento y configuración de las notificaciones](howto-create-event-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).
