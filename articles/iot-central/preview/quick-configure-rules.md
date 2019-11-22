---
title: Configuración de reglas y acciones en Azure IoT Central | Microsoft Docs
description: En esta guía de inicio rápido se muestra, como un generador, cómo configurar las reglas y las acciones basadas en la telemetría en la aplicación de Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 10/24/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 7e2047ee824c3dc8b6387f7879757a1e7be7a4a9
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896663"
---
# <a name="quickstart-configure-rules-and-actions-for-your-device-in-azure-iot-central-preview-features"></a>Inicio rápido: Configuración de reglas y acciones para el dispositivo en Azure IoT Central (características en versión preliminar)

*Este artículo se aplica a los administradores, operadores y compiladores.*

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

En esta guía de inicio rápido, creará una regla que enviará un correo cuando la temperatura de un dispositivo de sensor ambiental supere 90&deg; F.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, debe completar los dos inicios rápidos anteriores [Crear una aplicación de Azure IoT Central](./quick-deploy-iot-central.md) y [Agregar un dispositivo simulado a la aplicación de IoT Central](./quick-create-pnp-device.md) para crear la plantilla de dispositivo **Sensor del ambiental** con la que trabajar.

## <a name="create-a-telemetry-based-rule"></a>Crear una regla basada en la telemetría

1. Para agregar una nueva regla basada en telemetría a la aplicación, en el panel izquierdo, seleccione **Reglas**.

1. Para crear una regla, seleccione **+ New** (+ Nuevo).

1. Especifique **Environmental temperature** (Temperatura ambiental) como nombre de la regla.

1. En la sección **Dispositivos de destino**, seleccione el **Sensor ambiental** como plantilla de dispositivo. Esta opción filtra los dispositivos a los que se aplica la regla por tipo de plantilla de dispositivo. Puede agregar más criterios de filtro al elegir **+ Filtro**.

1. En la sección **Condiciones**, se define lo que desencadena la regla. Use la siguiente información para definir una condición basada en los datos de telemetría de temperatura:

    | Campo                                        | Valor                             |
    | -------------------------------------------- | ------------------------------    |
    | Medición                                  | Temperatura                       |
    | Operator                                     | es mayor que                   |
    | Valor                                        | 90                                |

    Para agregar más condiciones, seleccione **+ Condition** (+ Condición).

    ![Crear condición de regla](./media/quick-configure-rules/condition.png)

1. Para agregar una acción de correo para que se ejecute cuando se desencadene la regla, seleccione **+Correo**.

1. Use la información de la tabla siguiente para definir la acción:

    | Configuración   | Valor                                             |
    | --------- | ------------------------------------------------- |
    | Nombre para mostrar | Adición de una acción de correo                          |
    | Para        | La dirección de correo electrónico propia                                |
    | Notas     | La temperatura ambiental superó el umbral. |

    > [!NOTE]
    > Para recibir una notificación por correo electrónico, la dirección debe ser un [identificador de usuario de la aplicación](howto-administer.md) y ese usuario debe haber iniciado sesión en la aplicación al menos una vez.

    ![Crear acción de regla](./media/quick-configure-rules/action.png)

1. Seleccione **Guardar**. La regla se muestra en la página **Reglas**.

## <a name="test-the-rule"></a>Prueba de la regla

Poco después de guardar la regla, esta se activa. Cuando se cumplan las condiciones definidas en la regla, la aplicación enviará un mensaje a la dirección de correo electrónico que especificó en la acción.

> [!NOTE]
> Una vez terminada la prueba, desactive la regla para dejar de recibir alertas en la bandeja de entrada.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a hacer lo siguiente:

* Crear una regla basada en la telemetría
* Agregar una acción

Para más información sobre la supervisión de dispositivos conectados a la aplicación, continúe con la guía de inicio rápido:

> [!div class="nextstepaction"]
> [Uso de Azure IoT Central para supervisar los dispositivos](quick-monitor-devices.md).
