---
title: Adición de un dispositivo real a una aplicación de Azure IoT Central | Microsoft Docs
description: Como operador, puede agregar un dispositivo real a una aplicación de Azure IoT Central.
author: sarahhubbard
ms.author: sahubbar
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 7191c98cfc522068dbea576a8f81776ae4301da8
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878866"
---
# <a name="tutorial-add-a-simulated-device-to-your-azure-iot-central-application-preview-features"></a>Tutorial: Incorporación de un dispositivo simulado a una aplicación de Azure IoT Central (características en versión preliminar)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

En este tutorial se muestra cómo agregar y configurar un dispositivo a una aplicación de Microsoft Azure IoT Central.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Agregar un nuevo dispositivo simulado
> * Usar un dispositivo simulado en la experiencia de creación

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, el generador debería realizar el primer tutorial para desarrolladores para crear la aplicación de Azure IoT Central:

* [Definición de un tipo de dispositivo nuevo](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (obligatorio)

## <a name="add-a-simulated-device"></a>Adición de un dispositivo simulado

Para agregar un dispositivo real a la aplicación, use la plantilla de dispositivo **Environmental Sensor** (Sensor ambiental) que creó en el tutorial [Definición de un tipo de dispositivo nuevo](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

1. Para agregar un nuevo dispositivo como un operador, elija **Devices**  (Dispositivos) en el menú de navegación izquierdo. La pestaña **Devices** (Dispositivos) muestra **All devices** (Todos los dispositivos) y la plantilla de dispositivo **Environmental Sensor** (Sensor ambiental).

1. Para agregar un dispositivo de sensor ambiental simulado, seleccione **+ New** (+ Nuevo). Use el valor de **Device ID** (Id. de dispositivo) que se sugiere o escriba el **identificador de su dispositivo** en minúsculas. También puede escribir el nombre del nuevo dispositivo. Conmute el botón de alternancia **Simulated** (Simulado) a **On** (Activado) y seleccione **Create** (Crear).

    ![Dispositivo simulado](./media/tutorial-add-device-pnp/simulated-device.png)

Ya puede interactuar con las vistas que creó el generador para la plantilla de dispositivo mediante datos simulados.

## <a name="use-a-simulated-device-to-improve-views"></a>Uso de un dispositivo simulado para mejorar las vistas

Después de crear un dispositivo simulado, el generador puede usarlo para mejorar y desarrollar las vistas de la plantilla de dispositivo.

1. En la vista del dispositivo, copie el valor de **Device ID** (Id. de dispositivo) dispositivo simulado que creó.

1. Elija la pestaña **Device templates** (Plantillas de dispositivo) en el menú de navegación izquierdo y seleccione la plantilla **Environmental Sensor** (Sensor ambiental).

1. Seleccione la vista que desea editar o cree una. Haga clic en **Configure preview device**(Configurar dispositivo de versión preliminar). Aquí puede elegir entre no tener dispositivo de versión preliminar y usar un dispositivo real que puede configurar para las pruebas o desde un dispositivo existente que se haya agregado a IoT Central.

1. Elija **Select from a running device** (Seleccionar desde un dispositivo en ejecución) y escríbalo en el **identificador de dispositivo** de del dispositivo simulado que ha copiado.

1. Elija **Aplicar**. Ahora puede ver el mismo dispositivo simulado en la experiencia de creación de las vistas de plantilla de dispositivos. Esta vista es especialmente útil para gráficos y otras visualizaciones.

    ![Configurar dispositivo de versión preliminar](./media/tutorial-add-device-pnp/configure-preview.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

* Agregar un nuevo dispositivo simulado
* Usar un dispositivo simulado en la experiencia de creación

Ahora que ha conectado un dispositivo simulado a una aplicación de Azure IoT Central, estos son los siguientes pasos sugeridos.

Como operador, puede obtener información sobre:

> [!div class="nextstepaction"]
> [Configuración de reglas](tutorial-configure-rules.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)

Como desarrollador de dispositivos, puede obtener información sobre:

> [!div class="nextstepaction"]
> [Connect an MXChip IoT DevKit device to your Microsoft IoT Central application](howto-connect-devkit-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Conexión de un dispositivo MXChip IoT DevKit a una aplicación de Microsoft IoT Central)



