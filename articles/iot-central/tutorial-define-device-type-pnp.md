---
title: Definición de un nuevo tipo de dispositivo en Azure IoT Central | Microsoft Docs
description: Este tutorial le muestra, como desarrollador, cómo crear un tipo de dispositivo en la aplicación de Azure IoT Central. Se definen la telemetría, el estado, las propiedades y los comandos del tipo.
author: dominicbetts
ms.author: dobett
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: a622aa85d1d0a9dcd5d5ad9b2b30e7a3120ea974
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878636"
---
# <a name="tutorial-define-a-new-device-type-in-your-azure-iot-central-application-preview-features"></a>Tutorial: Definición de un nuevo tipo de dispositivo en la aplicación Azure IoT Central (característica en versión preliminar)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Este tutorial le muestra, como desarrollador, cómo usar una plantilla de dispositivo para definir un nuevo tipo de dispositivo en la aplicación de Microsoft Azure IoT Central. Una plantilla de dispositivo define las funcionalidades del dispositivo. Entre estas se incluyen la telemetría que envía el dispositivo, las propiedades del dispositivo y los comandos a los que responde un dispositivo.

En este tutorial, se crea una plantilla de dispositivo **Environment Sensor**. Un dispositivo sensor de entorno:

* Envía datos de telemetría, como la temperatura.
* Notifica propiedades específicas del dispositivo, como el nivel de brillo.
* Responde a comandos como, por ejemplo, activar y desactivar.
* Notifica propiedades genéricas del dispositivo, como la versión de firmware y el número de serie.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una plantilla de dispositivo
> * Importar un modelo de funcionalidad del dispositivo
> * Crear propiedades de la nube
> * Definir una visualización para la telemetría del dispositivo
> * Publicar la plantilla de dispositivo
> * Crear un dispositivo simulado para la plantilla de dispositivo
> * Ver el dispositivo simulado

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará una aplicación de Azure IoT Central. Si realizó la guía de inicio rápido [Creación de una aplicación de Azure IoT Central](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json), puede volver a usar la aplicación que creó en dicha guía. En caso contrario, complete los pasos siguientes para crear una aplicación de Azure IoT Central vacía:

1. Vaya a la página [Application Manager](https://aka.ms/iotcentral) (Administrador de aplicaciones) de Azure IoT Central.

1. Si tiene una suscripción a Azure, inicie sesión con las credenciales que usa para acceder a ella; si no la tiene, inicie sesión con una cuenta de Microsoft:

    ![Incorporación de la cuenta de la organización](./media/tutorial-define-device-type-pnp/sign-in.png)

1. Para empezar a crear una nueva aplicación de Azure IoT Central, seleccione **New Application** (Nueva aplicación).

Para crear una aplicación de Azure IoT Central que use características en versión preliminar, lo que incluye IoT Plug and Play, siga estos pasos:

1. Elija **Versión de evaluación**. No necesita una suscripción a Azure para crear una aplicación de evaluación.

    Para más información acerca de los directorios y las suscripciones, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

1. Elija **Preview application** (Aplicación de versión preliminar).

1. Puede elegir un nombre de aplicación descriptivo, como **Contoso Air Conditioners**. Azure IoT Central genera un prefijo de dirección URL único. Puede cambiar este prefijo de dirección URL por algo más fácil de recordar.

1. Si va a crear una aplicación de prueba, debe proporcionar los detalles de contacto.

1. Seleccione **Crear**.

    ![Página de creación de una aplicación de Azure IoT Central](./media/tutorial-define-device-type-pnp/iotcentralcreate.png)

    Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

También necesita una copia local del archivo **EnvironmentalSensorInline.capabilitymodel.json** que contenga el modelo de funcionalidad del dispositivo [IoT Plug and Play](https://aka.ms/iot-pnp-docs). Puede descargarla [aquí](https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensorInline.capabilitymodel.json). Haga clic con el botón derecho en la página y seleccione **Guardar como**.

Después de descargar el archivo, ábralo en un editor de texto y reemplace las dos instancias de `<YOUR_COMPANY_NAME_HERE>` por su nombre. Use solo los caracteres a-z, A-Z, 0-9 y el carácter de subrayado.

## <a name="create-a-template"></a>Creación de una plantilla

Como desarrollador, puede crear y editar plantillas de dispositivo de la aplicación. Después de publicar una plantilla de dispositivo, puede generar un dispositivo simulado o conectar dispositivos reales que implementan la plantilla de dispositivo. Los dispositivos simulados permiten probar el comportamiento de la aplicación antes de conectar un dispositivo real.

Para agregar una nueva plantilla de dispositivo a la aplicación, debe ir a la página **Device Templates** (Plantillas de dispositivo). Para ello, seleccione la pestaña **Device Templates** (Plantillas de dispositivo) en el menú de navegación izquierdo.

![Página Plantillas de dispositivo](./media/tutorial-define-device-type-pnp/devicedefinitions.png)

### <a name="add-a-device-capability-model"></a>Adición de un modelo de funcionalidad del dispositivo

Hay varias opciones para crear un modelo de funcionalidad del dispositivo en IoT Central. Puede optar por crear un modelo personalizado desde cero, importarlo desde un archivo, seleccionarlo en el catálogo de dispositivos o conectar un dispositivo IoT Plug and Play mediante una primera conexión del dispositivo en la que el modelo de funcionalidad del dispositivo se ha publicado en el repositorio público. En este tutorial, importará un modelo de funcionalidad del dispositivo desde un archivo.

En los pasos siguientes se muestra cómo importar el modelo de funcionalidad de un dispositivo **Environmental Sensor**. Estos dispositivos envían telemetría a la aplicación, por ejemplo, los datos de temperatura:

1. Para agregar una nueva plantilla de dispositivo, seleccione **+ New** (+ Nuevo) en la página **Device Templates** (Plantillas de dispositivo).

1. En la lista de modelos de funcionalidad del dispositivo, elija **Custom** (Personalizado).

1. Escriba **Environmental Sensor** como nombre de la plantilla de dispositivo.

1. Elija **Import Capability Model** (Importar modelo de funcionalidad) para crear un modelo de funcionalidad del dispositivo a partir de un archivo JSON. Vaya a la carpeta donde guardó el archivo **EnvironmentalSensorInline.capabilitymodel.json** en la máquina local. Seleccione el archivo **EnvironmentalSensorInline.capabilitymodel.json** y, luego, la opción de importación. El modelo de funcionalidad del sensor ambiental incluye las interfaces **Environmental Sensor** (Sensor ambiental) e **Device Information** (Información del dispositivo):

    ![Modelo de funcionalidad del dispositivo del sensor ambiental](./media/tutorial-define-device-type-pnp/newdevicecapabilitymodel.png)

    Estas interfaces definen las funcionalidades de un dispositivo **Environmental Sensor**. Entre las funcionalidades se incluyen la telemetría que envía un dispositivo, las propiedades que notifica un dispositivo y los comandos a los que responde un dispositivo.

### <a name="add-cloud-properties"></a>Adición de propiedades de la nube

Una plantilla de dispositivo puede incluir propiedades de la nube. Las propiedades de la nube solo existen en la aplicación IoT Central y nunca se envían a un dispositivo ni se reciben de él.

1. Seleccione **Cloud Properties** (Propiedades de nube) y, luego, **+ Add Cloud Property** (+ Agregar propiedad de la nube). Use la información de la tabla siguiente para agregar una propiedad de la nube a la plantilla de dispositivo.

    | Display Name (Nombre para mostrar)      | Tipo semántico | Esquema |
    | ----------------- | ------------- | ------ |
    | Fecha de la última revisión | None          | Date   |
    | Nombre del cliente     | None          | Cadena |

1. Seleccione **Save** (Guardar) para guardar los cambios.

    ![Propiedades de la nube](media/tutorial-define-device-type-pnp/cloudproperties.png)

### <a name="add-customizations"></a>Adición de personalizaciones

Use personalizaciones cuando necesite modificar una interfaz o agregar características específicas de IoT Central a una funcionalidad que no requiera controlar la versión del modelo de funcionalidad del dispositivo. Se pueden personalizar los campos cuando el modelo de funcionalidad esté en estado borrador o publicado. Y solo se pueden personalizar aquellos que no interrumpan la compatibilidad de la interfaz. Por ejemplo, puede:

- Personalizar el nombre para mostrar y las unidades de una funcionalidad
- Agregar un color predeterminado para usarlo cuando el valor aparezca en un gráfico
- Especificar los valores inicial, mínimo y máximo de una propiedad

No se puede personalizar el tipo o el nombre de la funcionalidad.


## <a name="create-views"></a>Creación de vistas

Como desarrollador, puede personalizar la aplicación para mostrar información pertinente sobre el dispositivo sensor ambiental a un operador. Las personalizaciones permiten al operador administrar estos dispositivo conectados a la aplicación. Puede crear dos tipos de vistas para que un operador interactúe con los dispositivos:

* Formularios para ver y editar las propiedades del dispositivo y la nube
* Paneles para visualizar los dispositivos

### <a name="generate-default-views"></a>Generación de vistas predeterminadas

La generación de vistas predeterminadas es una forma rápida de empezar a visualizar la información importante del dispositivo. Puede generar hasta tres vistas predeterminadas para la plantilla de dispositivo:

* La vista **Commands** (Comandos) permite que el operador envíe comandos al dispositivo.
* La vista **Overview** (Información general) usa gráficos y métricas para mostrar la telemetría del dispositivo.
* La vista **About** (Acerca de) muestra las propiedades del dispositivo.

Después de seleccionar **Generate default views** (Generar vistas predeterminadas), las vistas se agregan automáticamente a la sección **Views** (Vistas) de la plantilla de dispositivo.

### <a name="configure-a-view-to-visualize-devices"></a>Configuración de una vista para visualizar los dispositivos

Un panel de dispositivos permite a un operador visualizar un dispositivo mediante gráficos y métricas. Como generador, puede definir qué información se muestra en el panel del dispositivo. Puede definir varios paneles para los dispositivos. Para crear un panel y visualizar la telemetría del sensor ambiental, seleccione **Views** (Vistas) y, luego, **Visualizing the Device** (Visualización del dispositivo):

1. Todas las propiedades del dispositivo, las propiedades de la nube, la telemetría y las opciones estáticas se muestran en el panel derecho. Puede arrastrar y colocar cualquiera de estos elementos en la vista. Arrastre una propiedad a la vista. Puede configurar el icono mediante el icono de engranaje.

1. Para agregar un gráfico que represente la telemetría, seleccione **Humidity** (Humedad) y **Temperature** (Temperatura) y, luego, **Combine** (Combinar). Para ver este gráfico en otro formato, como un gráfico circular o un gráfico de barras, seleccione el botón **Change Visualization** (Cambiar visualización) situado en la parte superior del icono.

1. Seleccione **Save** (Guardar) para guardar la vista:

Puede agregar más iconos que muestren otras propiedades o valores de telemetría. También puede agregar texto estático, vínculos e imágenes. Para mover o cambiar el tamaño de un icono en el panel, mueva el puntero del mouse sobre el icono y arrastre el icono a una nueva ubicación o cambie su tamaño.

### <a name="add-a-device-form"></a>Adición de un formulario de dispositivo

Un formulario de dispositivo permite que un operador edite las propiedades del dispositivo y de la nube grabables. Como desarrollador, puede definir varios formularios y elegir qué propiedades de la nube y del dispositivo se van a mostrar en cada uno. También puede mostrar las propiedades de un dispositivo de solo lectura en un formulario.

Para crear un formulario y ver y editar las propiedades del sensor ambiental, siga estos pasos:

1. Seleccione el icono **Editing Device and Cloud data** (Editar datos del dispositivo y la nube).

1. Escriba el nombre del formulario **Propiedades del sensor ambiental**.

1. Arrastre las propiedades de la nube **Customer name** (Nombre del cliente) y **Last service date** (Última fecha del servicio) a la sección existente del formulario.

1. Seleccione las propiedades del dispositivo **Brightness Level** (Nivel de brillo) y **Device State** (Estado del dispositivo). Después, seleccione **Add Section** (Agregar sección). Edite el título de la sección para que sea **Propiedades del sensor**. Seleccione **Aplicar**.

1. Seleccione las propiedades del dispositivo **Battery remaining** (Batería restante), **Device model** (Modelo de dispositivo), **Firmware version** (Versión de firmware), **Manufacturer**(Fabricante) y **Serial number** (Número de serie). Después, seleccione **Add Section** (Agregar sección). Edite el título de la sección para que sea **Propiedades del dispositivo**. Seleccione **Aplicar**.

1. Seleccione **Save** (Guardar) para guardar la vista.


## <a name="publish-device-template"></a>Publicación de la plantilla de dispositivo

Antes de poder crear un sensor ambiental simulado o de conectar un sensor ambiental real, debe publicar la plantilla de dispositivo.

Para publicar una plantilla de dispositivo, siga estos pasos:

1. Vaya a la plantilla de dispositivo en la página **Device Templates** (Plantillas de dispositivo).

1. Seleccione **Publicar**.

1. En el cuadro de diálogo **Publish a Device Template** (Publicar una plantilla de dispositivo), elija **Publish** (Publicar):

    ![Modelo publicado](media/tutorial-define-device-type-pnp/publishedmodel.png)

Una vez publicada una plantilla de dispositivo, es visible en la página **Devices** (Dispositivos) y para el operador. En una plantilla de dispositivo publicada, no se puede editar un modelo de funcionalidad del dispositivo sin crear una versión. Sin embargo, puede hacer actualizaciones en las propiedades de la nube, las personalizaciones y las vistas en una plantilla de dispositivo publicada sin control de versiones. Después de realizar los cambios, seleccione **Publish** (Publicar) para enviar esos cambios al operador.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

* Creación de una nueva plantilla de dispositivo
* Importar un modelo de funcionalidad del dispositivo
* Crear propiedades de la nube
* Crear personalizaciones
* Definir una visualización para la telemetría del dispositivo
* Publicar la plantilla de dispositivo

Ahora que ha creado una plantilla de dispositivo en la aplicación de Azure IoT Central, este es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Incorporación de un dispositivo](tutorial-add-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
