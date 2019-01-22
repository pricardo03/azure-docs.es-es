---
title: Introducción a Azure IoT Edge | Microsoft Docs
description: Introducción al servicio Azure IoT Edge
author: kgremban
manager: philmea
ms.reviewer: chipalost
ms.service: iot-edge
services: iot-edge
ms.topic: overview
ms.date: 06/12/2018
ms.author: kgremban
ms.custom: mvc
ms.openlocfilehash: 6bd72fd91190c9ed54b4dfd3e7b1e957c375bd4c
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2019
ms.locfileid: "54230567"
---
# <a name="what-is-azure-iot-edge"></a>¿Qué es Azure IoT Edge?

Azure IoT Edge mueve el análisis en la nube y lógica de negocios personalizada a los dispositivos para que su organización pueda centrarse en la información empresarial en lugar de en la administración de los datos. Habilite el escalado real de la solución al configurar el software de IoT, implementarlo en dispositivos a través de contenedores estándares y supervisar todo desde la nube.

>[!NOTE]
>Azure IoT Edge está disponible en el nivel gratuito y estándar de IoT Hub. El nivel gratuito está pensado únicamente para pruebas y evaluación. Para más información acerca de los niveles básico y estándar, consulte el artículo sobre [cómo elegir el nivel de IoT Hub correcto](../iot-hub/iot-hub-scaling.md).

Los análisis son un valor añadido empresarial para las soluciones de IoT, pero no es necesario que todos los análisis estén en la nube. Si desea que un dispositivo responda a emergencias lo antes posible, puede realizar la detección de anomalías en el propio dispositivo. Análogamente, si desea reducir los costos de ancho de banda y evitar la transferencia de terabytes de datos sin procesar, puede realizar la limpieza y la agregación de datos localmente. A continuación, envíe la información a la nube. 

Azure IoT Edge está formado por tres componentes:
* Los módulos de IoT Edge son contenedores que ejecutan servicios de Azure, de terceros o código propio del usuario. Se implementan en los dispositivos de IoT Edge y se ejecutan en ellos. 
* El entorno de tiempo de ejecución de IoT Edge se ejecuta en todos los dispositivos de IoT Edge y administra los módulos que se implementan en cada dispositivo. 
* Una interfaz basada en la nube permite supervisar y administrar los dispositivos de IoT Edge de forma remota.

## <a name="iot-edge-modules"></a>Módulos de IoT Edge

Los módulos de IoT Edge son unidades de ejecución implementadas actualmente como contenedores compatibles con Docker, que ejecutan la lógica de negocios en los dispositivos perimetrales. Se pueden configurar varios módulos para que se comuniquen entre sí al crear una canalización de procesamiento de datos. Puede desarrollar módulos personalizados o empaquetar determinados servicios de Azure en módulos que proporcionen información sin conexión y en el dispositivo perimetral. 

### <a name="artificial-intelligence-on-the-edge"></a>Inteligencia artificial en los dispositivos perimetrales

Azure IoT Edge permite implementar el procesamiento de eventos complejos, el aprendizaje automático, el reconocimiento de imágenes y otros tipos de inteligencia artificial de gran valor sin necesidad de escribirla internamente. Los servicios de Azure como Azure Functions, Azure Stream Analytics y Azure Machine Learning se pueden ejecutar de manera local mediante Azure IoT Edge; sin embargo, esto no se limita únicamente a los servicios de Azure. Cualquier persona puede crear módulos de inteligencia artificial y ponerlos a disposición de la comunidad. 

### <a name="bring-your-own-code"></a>Traiga su propio código

Cuando desee implementar su propio código en los dispositivos, Azure IoT Edge también lo admite. Azure IoT Edge aplica el mismo modelo de programación que los demás servicios de IoT de Azure. El mismo código se puede ejecutar en un dispositivo o en la nube. Azure IoT Edge es compatible con Windows y Linux, por lo que podrá codificar para la plataforma de su elección. Admite Java, .NET Core 2.0, Node.js, C y Python, por lo que los desarrolladores pueden crear código en un lenguaje que ya conozcan y usar la lógica de negocios existente.

## <a name="iot-edge-runtime"></a>Entorno de tiempo de ejecución de IoT Edge

El entorno de tiempo de ejecución de Azure IoT Edge permite lógica de la nube y personalizada en dispositivos de IoT Edge. Se encuentra en el dispositivo de IoT Edge y realiza operaciones de administración y comunicación. El entorno de tiempo de ejecución realiza varias funciones:

* Instalación y actualización de las cargas de trabajo en el dispositivo.
* Mantenimiento de los estándares de seguridad de Azure IoT Edge en el dispositivo.
* Garantía de que los módulos de IoT Edge están siempre en ejecución.
* Notificación del mantenimiento del módulo a la nube para la supervisión remota.
* Administración de la comunicación entre los dispositivos hoja descendentes y un dispositivo IoT Edge, entre los módulos de un dispositivo IoT Edge y entre un dispositivo IoT Edge y la nube.

![El entorno de tiempo de ejecución de IoT Edge envía información e informes a IoT Hub](./media/about-iot-edge/runtime.png)

La manera de usar los dispositivos de Azure IoT Edge es decisión suya. El runtime a menudo se usa para implementar inteligencia artificial en puertas de enlace que agregan y procesan datos de otros dispositivos locales, pero este modelo de implementación es solo una de las opciones disponibles. Los dispositivos de hoja también pueden ser dispositivos de Azure IoT Edge, independientemente de si están conectados a una puerta de enlace o directamente a la nube.

El entorno de tiempo de ejecución de Azure IoT Edge se ejecuta en un gran conjunto de dispositivos de IoT para habilitar el su uso de numerosas maneras distintas. Admite los sistemas operativos Windows y Linux y abstrae los detalles del hardware. Use un dispositivo más pequeño que un Raspberry Pi 3 si no procesa muchos datos, o utilice un servidor industrializado para ejecutar cargas de trabajo que consumen numerosos recursos.

## <a name="iot-edge-cloud-interface"></a>Interfaz en la nube de IoT Edge

Administrar el ciclo de vida del software para los dispositivos de la empresa es complicado. Administrar el ciclo de vida del software para millones de dispositivos de IoT heterogéneos es incluso más difícil. Las cargas de trabajo deben crearse y configurarse para un determinado tipo de dispositivo, implementarse a escala en los millones de dispositivos de la solución y supervisarse para detectar los comportamientos incorrectos de los dispositivos. Estas actividades no se pueden realizar en cada dispositivo y deben realizarse a escala.

Azure IoT Edge se integra perfectamente con los aceleradores de soluciones de IoT de Azure para conseguir un plan de control que se adapte a las necesidades de la solución. Los servicios en la nube le permiten:

* Crear y configurar una carga de trabajo que se ejecute en un tipo específico de dispositivo.
* Enviar una carga de trabajo a un conjunto de dispositivos.
* Supervisar las cargas de trabajo que se ejecutan en los dispositivos de campo.

![La telemetría y las acciones de los dispositivos se coordinan con la nube](./media/about-iot-edge/cloud-interface.png)

## <a name="next-steps"></a>Pasos siguientes

Pruebe estos conceptos al [implementar IoT Edge en un dispositivo simulado](quickstart.md).

 
