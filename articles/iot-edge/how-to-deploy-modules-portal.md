---
title: 'Implementación de módulos desde Azure Portal: Azure IoT Edge | Microsoft Docs'
description: Uso de Azure Portal para implementar módulos en un dispositivo de IoT Edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/19/2019
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 9d7729dce5419c5813de3c4dfce55c40098f5988
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60595237"
---
# <a name="deploy-azure-iot-edge-modules-from-the-azure-portal"></a>Implementación de módulos de Azure IoT Edge desde Azure Portal

Una vez que ha creado módulos de IoT Edge con su lógica empresarial, querrá implementarlos en sus dispositivos para usarlos en el perímetro. Si tiene varios módulos que funcionan conjuntamente para recopilar y procesar datos, puede implementarlos todos a la vez y declarar las reglas de enrutamiento que los conectan.

Este artículo muestra cómo le puede ayudar Azure Portal en la creación de un manifiesto de implementación y en la inserción de la implementación en un dispositivo de IoT Edge. Para información sobre la creación de una implementación dirigida a varios dispositivos en función de sus etiquetas compartidas, consulte [Implementación y supervisión de módulos de IoT Edge a escala](how-to-deploy-monitor.md).

## <a name="prerequisites"></a>Requisitos previos

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) en la suscripción de Azure.
* Un [dispositivo de IoT Edge](how-to-register-device-portal.md) que tenga instalado el entorno de ejecución de Azure IoT Edge.

## <a name="select-your-device"></a>Selección del dispositivo

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub.
1. Seleccione **IoT Edge** en el menú.
1. Haga clic en el identificador del dispositivo de destino en la lista de dispositivos.
1. Seleccione **Set modules** (Establecer módulos).

## <a name="configure-a-deployment-manifest"></a>Configuración de un manifiesto de implementación

Un manifiesto de implementación es un documento JSON que describe qué módulos se van a implementar, cómo fluyen los datos entre los módulos y las propiedades deseadas de los módulos gemelos. Para más información sobre los manifiestos de implementación y cómo crearlos, consulte [Descripción de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

Azure Portal tiene un asistente que le guía en la creación del manifiesto de implementación, en lugar de crear el documento JSON de forma manual. Consta de tres pasos: **Adición de módulos**, **Especificación de rutas** y **Revisión de la implementación**.

### <a name="add-modules"></a>Adición de módulos

1. En la sección **Configuración del registro** de la página, proporcione las credenciales para acceder a cualquier registro del contenedor privado que contiene las imágenes del módulo.

1. En la sección **Módulos de implementación** de la página, seleccione **Agregar**.

1. Fíjese en los tipos de módulos en la lista desplegable:

   * **Módulo de IoT Edge**: la opción predeterminada.
   * **Módulo de Azure Stream Analytics**: solo módulos generados a partir de una carga de trabajo de Azure Stream Analytics.
   * **Módulo de Azure Machine Learning**: solo imágenes de modelo generadas a partir de un área de trabajo de Azure Machine Learning.

1. Seleccione **Módulo IoT Edge**.

1. Proporcione un nombre para el módulo y, a continuación, especifique la imagen de contenedor. Por ejemplo: 

   * **Nombre**: tempSensor
   * **Identificador URI de la imagen**: mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0

1. Rellene los campos opcionales si es necesario. Para más información sobre las opciones de creación del contenedor, la directiva de reinicio y el estado deseado, consulte [Propiedades deseadas de EdgeAgent](module-edgeagent-edgehub.md#edgeagent-desired-properties). Para más información sobre el módulo gemelo, consulte [Definición o actualización de las propiedades deseadas](module-composition.md#define-or-update-desired-properties).

1. Seleccione **Guardar**.

1. Repita los pasos 2 a 6 para agregar módulos adicionales a la implementación.

1. Seleccione **Siguiente** para ir a la sección de rutas.

### <a name="specify-routes"></a>Especificación de rutas

De forma predeterminada, el asistente proporciona una ruta llamada **route** y definida como **FROM /\* INTO $upstream**, que significa que cualquier mensaje de salida de cualquier módulo se envía al IoT Hub.  

Agregue o actualice las rutas con la información de [Declaración de rutas](module-composition.md#declare-routes) y, a continuación, seleccione **Siguiente** para continuar con la sección de revisión.

### <a name="review-deployment"></a>Revisión de la implementación

La sección de revisión le muestra el manifiesto de implementación en formato JSON que se ha creado en función de las selecciones de las dos secciones anteriores. Observe que hay dos módulos declarados que no ha agregado: **$edgeAgent** y **$edgeHub**. Estos dos módulos constituyen el [entorno de ejecución de Azure IoT Edge](iot-edge-runtime.md) y son valores predeterminados necesarios en todas las implementaciones.

Revise la información de implementación y seleccione **Enviar**.

## <a name="view-modules-on-your-device"></a>Visualización de módulos en el dispositivo

Una vez que los módulos se han implementado en el dispositivo, puede verlos todos en la página **Detalles del dispositivo** del portal. Esta página muestra el nombre de cada módulo implementado, así como información útil, como el estado de la implementación y el código de salida.

## <a name="deploy-modules-from-azure-marketplace"></a>Implementación de módulos desde Azure Marketplace

Azure Marketplace es un mercado de aplicaciones y servicios en línea en el que puede examinar una amplia gama de aplicaciones y soluciones empresariales que están certificadas y optimizadas para ejecutarse en Azure, incluyendo los [módulos de IoT Edge](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules). También puede acceder a Azure Marketplace desde la sección **Crear un recurso** de Azure Portal.

Puede instalar un módulo IoT Edge desde Azure Marketplace o desde Azure Portal:

1. Encuentre un módulo e inicie el proceso de implementación.

   * Azure Portal: Encuentre un módulo y seleccione **Crear**.

   * Azure Marketplace:

     1. Encuentre un módulo y seleccione **Obtenerla ahora**.
     1. Para confirmar las condiciones de uso y la directiva de privacidad del proveedor, seleccione **Continuar**.

1. Elija la suscripción y la instancia de IoT Hub a la que está asociado el dispositivo de destino.

1. Elija **Implementar en un dispositivo**.

1. Escriba el nombre del dispositivo o seleccione **Buscar dispositivo** para buscar entre los dispositivos registrados en el centro.

1. Seleccione **Crear** para continuar el proceso estándar de configurar un manifiesto de implementación, lo que incluye agregar otros módulos si lo desea. Los detalles del nuevo módulo, como el URI de la imagen, las opciones de creación y las propiedades deseadas están predefinidas pero se pueden cambiar.

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [implementar y supervisar módulos de IoT Edge a escala](how-to-deploy-monitor.md)
