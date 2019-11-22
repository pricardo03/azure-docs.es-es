---
title: Definición de un nuevo tipo de dispositivo de Azure IoT Edge en Azure IoT Central | Microsoft Docs
description: Este tutorial le muestra, como desarrollador, cómo crear un dispositivo de Azure IoT Edge en la aplicación de Azure IoT Central. Se definen la telemetría, el estado, las propiedades y los comandos del tipo.
author: rangv
ms.author: rangv
ms.date: 10/22/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: c205b4dd4871ed53e32dce72f12cc2dcfb3baf41
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73892031"
---
# <a name="tutorial-define-a-new-azure-iot-edge-device-type-in-your-azure-iot-central-application-preview-features"></a>Tutorial: Definición de un nuevo tipo de dispositivo de Azure IoT Edge en la aplicación de Azure IoT Central (característica en versión preliminar)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Este tutorial le muestra, como desarrollador, cómo usar una plantilla de dispositivo para definir un nuevo tipo de dispositivo de Azure IoT Edge en la aplicación de Azure IoT Central. 

Para información general de Azure IoT Edge, [consulte este artículo](overview-iot-central.md). 

Azure IoT Edge está formado por tres componentes:
* Los **módulos de IoT Edge** son contenedores que ejecutan servicios de Azure, de terceros o código propio del usuario. Se implementan en los dispositivos de IoT Edge y se ejecutan en ellos.
* El **entorno en tiempo de ejecución de IoT Edge** se ejecuta en todos los dispositivos de IoT Edge y administra los módulos que se implementan en cada dispositivo.
* Una **interfaz basada en la nube** permite supervisar y administrar los dispositivos de IoT Edge de forma remota. IoT Central será la interfaz de nube.

Un dispositivo **Azure IoT Edge** puede ser un dispositivo de puerta de enlace con dispositivos de nivel inferior que se conectan al dispositivo Azure IoT Edge. En este tutorial se tratarán los patrones de conectividad de dispositivos de nivel inferior.

Una **plantilla de dispositivo** define las funcionalidades del dispositivo y de los módulos de IoT Edge. Entre estas se incluyen la telemetría que envía el módulo, las propiedades del módulo y los comandos a los que responde un módulo.

En este tutorial, se crea una plantilla de dispositivo **Environment Sensor**. Un dispositivo sensor de entorno:

* Envía datos de telemetría, como la temperatura.
* Responde a las propiedades que se escriben cuando se actualiza en la nube, como el intervalo de envío de telemetría.
* Responde a comandos como el restablecimiento de la temperatura.

También en este tutorial, se va a crear una plantilla de dispositivo **Environment Gateway**. Un dispositivo de puerta de enlace de entorno:

* Envía datos de telemetría, como la temperatura.
* Responde a las propiedades que se escriben cuando se actualiza en la nube, como el intervalo de envío de telemetría.
* Responde a comandos como el restablecimiento de la temperatura.
* Permite relaciones con otros modelos de funcionalidad del dispositivo


En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una plantilla de dispositivo de Azure IoT Edge
> * Cargar un manifiesto de implementación
> * Crear funcionalidades, como telemetría, propiedades y comandos para cada módulo
> * Definir una visualización para la telemetría del módulo
> * Agregar relaciones a las plantillas de dispositivo de nivel inferior
> * Publicar la plantilla de dispositivo

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará una aplicación de Azure IoT Central. Siga este inicio rápido para [crear una aplicación de Azure IoT Central](quick-deploy-iot-central.md).


## <a name="downstream-device-relationships-with-gateway--modules"></a>Relaciones de dispositivo de nivel inferior con módulos y puertas de enlace

Los dispositivos de nivel inferior pueden conectarse a un dispositivo de puerta de enlace de Azure IoT Edge mediante el módulo $edgeHub. Este dispositivo de Azure IoT Edge se convierte en una puerta de enlace transparente en este escenario.

![Página Central Application (Aplicación central)](./media/tutorial-define-edge-device-type/gateway-transparent.png)

Los dispositivos de nivel inferior pueden conectarse a un dispositivo de puerta de enlace de Azure IoT Edge mediante un módulo personalizado. En el escenario siguiente, los dispositivos de nivel inferior se conectan a través de un módulo personalizado de Modbus; no se pueden conectar al dispositivo de puerta de enlace de Azure IoT Edge mediante el módulo $edgeHub.

![Página Central Application (Aplicación central)](./media/tutorial-define-edge-device-type/gateway-module.png)

Los dispositivos de nivel inferior pueden conectarse a un dispositivo de puerta de enlace de Azure IoT Edge mediante un módulo personalizado. En el escenario siguiente, los dispositivos de nivel inferior se conectan mediante un módulo personalizado de Modbus. 

![Página Central Application (Aplicación central)](./media/tutorial-define-edge-device-type/gateway-module-transparent.png)

Los dispositivos de nivel inferior pueden conectarse a un dispositivo de puerta de enlace de Azure IoT Edge mediante varios módulos personalizados. En el escenario siguiente, los dispositivos de nivel inferior se conectan mediante un módulo personalizado de Modbus y un módulo personalizado de BLE, y se pueden conectar al dispositivo de puerta de enlace de Azure IoT Edge mediante el módulo $edgeHub. 

![Página Central Application (Aplicación central)](./media/tutorial-define-edge-device-type/gateway-module2-transparent.png)


## <a name="create-a-template"></a>Creación de una plantilla

Como desarrollador, puede crear y editar plantillas de dispositivo de Azure IoT Edge en la aplicación. Después de publicar una plantilla de dispositivo, puede conectar dispositivos reales que implementan la plantilla de dispositivo.

### <a name="select-device-template-type"></a>Selección del tipo de plantilla de dispositivo 

Para agregar una nueva plantilla de dispositivo a la aplicación, debe ir a la página **Device Templates** (Plantillas de dispositivo). Para ello, seleccione la pestaña **Device Templates** (Plantillas de dispositivo) en el panel izquierdo.

![Página Central Application (Aplicación central)](./media/tutorial-define-edge-device-type/edgedevicetemplate.png)

Haga clic en **+ New** (+ Nueva) para empezar a crear una plantilla de dispositivo.

![Plantillas de dispositivo: nueva](./media/tutorial-define-edge-device-type/edgedevicetemplatenew.png)

Llegará a la página de selección del tipo de plantilla de dispositivo. Seleccione el icono **Azure IoT Edge** y haga clic en **Next: Customize** (Siguiente: Personalizar) en la parte inferior.

![Selección de plantillas de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/selectiotedge.png)

### <a name="customize-device-template"></a>Personalización de la plantilla de dispositivo

Azure IoT Edge le permite implementar y administrar la lógica de negocios en forma de módulos. Los **módulos de Azure IoT Edge** son la unidad más pequeña de cálculo que administra IoT Edge y pueden contener servicios de Azure (por ejemplo, Azure Stream Analytics) o su propio código específico de la solución. Para entender cómo se desarrollan, implementan y mantienen los módulos, lea [Módulos de IoT Edge](../../iot-edge/iot-edge-modules.md).

A nivel general, un manifiesto de implementación es una lista de módulos gemelos que se configuran con sus propiedades deseadas. Un manifiesto de implementación indica a un dispositivo IoT Edge (o a un grupo de dispositivos) qué módulos debe instalar y cómo configurarlos. Los manifiestos de implementación incluyen las propiedades deseadas de cada módulo gemelo. Los dispositivos IoT Edge informan sobre las propiedades notificadas de cada módulo.

Use Visual Studio Code para crear un manifiesto de implementación. Siga la documentación sobre cómo crear un [manifiesto de implementación](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

Este es un manifiesto de implementación básico con un módulo que se usará como ejemplo en este tutorial. Copie el siguiente código JSON y guárdelo como un archivo JSON. 

   ```JSON
   {
     "modulesContent": {
       "$edgeAgent": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "runtime": {
             "type": "docker",
             "settings": {
               "minDockerVersion": "v1.25",
               "loggingOptions": "",
               "registryCredentials": {}
             }
           },
           "systemModules": {
             "edgeAgent": {
               "type": "docker",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                 "createOptions": "{}"
               }
             }
           },
           "modules": {
             "SimulatedTemperatureSensor": {
               "version": "1.0",
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                 "createOptions": "{}"
               }
             }
           }
         }
       },
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
               "route": "FROM /* INTO $upstream"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       },
       "SimulatedTemperatureSensor": {
         "properties.desired": {
              "SendData": true,
              "SendInterval": 10
         }
       }
     }
   }
   ```

**Carga de un manifiesto de implementación de Azure IoT Edge**

Haga clic en el botón **Browse** (Examinar). 

![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgedevicetemplateuploadmanifest.png)

Si tiene previsto crear una plantilla de dispositivo de puerta de enlace de Azure IoT Edge, asegúrese de activar la casilla **Gateway device with downstream devices** (Dispositivo de puerta de enlace con dispositivos de bajada).

![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/gateway-upload.png)

Aparecerá un cuadro de diálogo de selección de archivos. Seleccione el archivo de manifiesto de implementación y haga clic en el botón **Open** (Abrir).

El archivo de manifiesto de implementación se validará con un esquema. Tras una validación correcta, haga clic en el botón **Review** (Revisar).

![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/deploymentmanifestvalidate.png)

A continuación se muestra el flujo del ciclo de vida de un manifiesto de implementación en IoT Central.

![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/dmflow.png)

La página de revisión se muestra con detalles del manifiesto de implementación. La lista de módulos del manifiesto de implementación se mostrará en la página de revisión. En este tutorial, verá que aparece el módulo SimulatedTemperatureSensor. Haga clic en el botón **Crear**.

![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgedevicetemplatereview.png)

Si ha seleccionado un dispositivo de puerta de enlace, verá esta página de revisión.

![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/gateway-review.png)


Se mostrará el indicador giratorio de que se está creando una plantilla de dispositivo en IoT Central.

La plantilla de dispositivo se crea con modelos de funcionalidad de módulo. En este tutorial, verá la creación con el modelo de funcionalidad del módulo SimulatedTemperatureSensor. 

Cambie el título de la plantilla de dispositivo por Environment Sensor Device.

![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgedevicetemplatelanding.png)

El modelado Plug and Play de dispositivos Azure IoT Edge se realiza de la siguiente manera:
* Cada plantilla de dispositivo de Azure IoT Edge tendrá un **modelo de funcionalidad del dispositivo**.
* Para cada módulo personalizado incluido en el manifiesto de implementación, se generará un **modelo de funcionalidad del módulo**.
* Se establecerá una **relación** entre cada modelo de funcionalidad del módulo y un modelo de funcionalidad del dispositivo.
* El modelo de funcionalidad del módulo implementa **interfaces de módulo**.
* Cada interfaz de módulo contiene:
   - Telemetría
   - properties (Propiedades)
   - Comandos:

![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgemodelling.png)

**Adición de funcionalidades al modelo de funcionalidad del módulo**

Esta es una salida de ejemplo del módulo SimulatedTemperatureSensor.
```json
{

    "machine": {

        "temperature": 75.0,
        "pressure": 40.2
    },
    "ambient": {
        "temperature": 23.0,
        "humidity": 30.0
    },
    "timeCreated": ""
}
```

Agregue funcionalidades al módulo SimulatedTemperatureSensor, que reflejará el código JSON anterior. 

* Haga clic en **Manage** (Administrar) para administrar una interfaz del modelo de funcionalidad del módulo SimulatedTemperatureSensor. Haga clic en **Add Capability** (Agregar funcionalidad). 

    ![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgetemplateaddcapability.png)
  
* Agregue "machine" como tipo de objeto, dado que es un tipo complejo.
  
    ![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgetemplatemachineobject.png)

    Haga clic en **Define** (Definir). En el modelo emergente, cambie el nombre del objeto por "machine" y cree propiedades de temperatura y presión. Luego, haga clic en **Apply** (Aplicar).
  
    ![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgetemplatemachineattributes.png)
  
* Agregue "ambient" como tipo de objeto, dado que es un tipo complejo

    Haga clic en **Define** (Definir). En el modelo emergente, cambie el nombre del objeto por "ambient" y cree propiedades de temperatura y humedad. Luego, haga clic en **Apply** (Aplicar).
  
    ![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgetemplateambientattributes.png)

  
* Agregue timeCreated como tipo de DateTime y haga clic en **Save** (Guardar).
  
    ![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgetemplateallattributes.png)


### <a name="add-relationships"></a>Agregar relaciones

Si seleccionó el dispositivo Azure IoT Edge como dispositivo de puerta de enlace, puede agregar relaciones de nivel inferior a los modelos de funcionalidad de los dispositivos que se conectarán a él.
  
  ![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/gateway-add-relationship.png)

La relación se puede agregar en un dispositivo o en un módulo.
  
  ![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/gateway-relationship-types.png)


Puede seleccionar un modelo de funcionalidad del dispositivo de nivel inferior o seleccionar un asterisco. 
  
  ![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/gateway-downstream-rel.png)

  En este tutorial, se seleccionará el asterisco, lo que significa que se permitirán relaciones de nivel inferior. Haga clic en **Guardar**

  ![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-define-edge-device-type/gateway-add-relationship-asterix.png)


### <a name="add-cloud-properties"></a>Adición de propiedades de nube

Una plantilla de dispositivo puede incluir propiedades de la nube. Las propiedades de la nube solo existen en la aplicación IoT Central y nunca se envían a un dispositivo ni se reciben de él.

1. Seleccione **Cloud Properties** (Propiedades de nube) y, luego, **+ Add Cloud Property** (+ Agregar propiedad de la nube). Use la información de la tabla siguiente para agregar una propiedad de la nube a la plantilla de dispositivo.

    | Display Name (Nombre para mostrar)      | Tipo semántico | Schema |
    | ----------------- | ------------- | ------ |
    | Fecha de la última revisión | None          | Date   |
    | Nombre del cliente     | None          | Cadena |

2. Seleccione **Save** (Guardar) para guardar los cambios.

  
    ![Propiedades de la nube: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgetemplatecloudproperties.png)

### <a name="add-customizations"></a>Adición de personalizaciones

Use personalizaciones cuando necesite modificar una interfaz o agregar características específicas de IoT Central a una funcionalidad que no requiera controlar la versión del modelo de funcionalidad del dispositivo. Se pueden personalizar los campos cuando el modelo de funcionalidad esté en estado borrador o publicado. Y solo se pueden personalizar aquellos que no interrumpan la compatibilidad de la interfaz. Por ejemplo, puede:

- Personalizar el nombre para mostrar y las unidades de una funcionalidad.
- Agregar un color predeterminado para usarlo cuando el valor aparezca en un gráfico.
- Especificar los valores inicial, mínimo y máximo de una propiedad.

No se puede personalizar el tipo o el nombre de la funcionalidad. Haga clic en **Guardar**
  
![Personalizaciones: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgetemplatecustomize.png)


### <a name="create-views"></a>Creación de vistas

Como desarrollador, puede personalizar la aplicación para mostrar información pertinente sobre el dispositivo sensor ambiental a un operador. Las personalizaciones permiten al operador administrar estos dispositivo conectados a la aplicación. Puede crear dos tipos de vistas para que un operador interactúe con los dispositivos:

* Formularios para ver y editar las propiedades del dispositivo y la nube
* Paneles para visualizar los dispositivos

### <a name="configure-a-view-to-visualize-devices"></a>Configuración de una vista para visualizar los dispositivos

Un panel de dispositivos permite a un operador visualizar un dispositivo mediante gráficos y métricas. Como generador, puede definir qué información se muestra en el panel del dispositivo. Puede definir varios paneles para los dispositivos. Para crear un panel y visualizar la telemetría del sensor ambiental, seleccione **Views** (Vistas) y, luego, **Visualizing the Device** (Visualización del dispositivo):

  
![Vistas: Azure IoT Edge](./media/tutorial-define-edge-device-type/visualizingthedevice.png)


Ambient Telemetry y Machine Telemetry son objetos complejos. Para crear gráficos, haga lo siguiente:

Arrastre Ambient Telemetry y seleccione el gráfico de líneas. 
  
![Vistas: Azure IoT Edge](./media/tutorial-define-edge-device-type/sensorambientchart.png)

Haga clic en el icono de configuración, seleccione la temperatura y la humedad para visualizar los datos y haga clic en el botón **Update configuration** (Actualizar configuración). 
  
![Vistas: Azure IoT Edge](./media/tutorial-define-edge-device-type/sensorambienttelemetrychart.png)

Seleccione **Save** (Guardar) para guardar la vista:

Puede agregar más iconos que muestren otras propiedades o valores de telemetría. También puede agregar texto estático, vínculos e imágenes. Para mover o cambiar el tamaño de un icono en el panel, mueva el puntero del mouse sobre el icono y arrastre el icono a una nueva ubicación o cambie su tamaño.
  
![Vistas: Azure IoT Edge](./media/tutorial-define-edge-device-type/viewsdashboard.png)

### <a name="add-a-device-form"></a>Adición de un formulario de dispositivo

Un formulario de dispositivo permite que un operador edite las propiedades del dispositivo y de la nube grabables. Como desarrollador, puede definir varios formularios y elegir qué propiedades de la nube y del dispositivo se van a mostrar en cada uno. También puede mostrar las propiedades de un dispositivo de solo lectura en un formulario.

Para crear un formulario y ver y editar las propiedades del sensor ambiental, siga estos pasos:

Vaya a **Views** (Vistas) en la plantilla **Environmental Sensor** (Sensor del entorno). Seleccione el icono **Editing Device and Cloud data** (Editar datos del dispositivo y de la nube) para agregar una vista nueva.
  
![Vistas: Azure IoT Edge](./media/tutorial-define-edge-device-type/editingdeviceandclouddata.png)

Escriba el nombre del formulario **Propiedades del sensor ambiental**.

Arrastre las propiedades de la nube **Customer name** (Nombre del cliente) y **Last service date** (Última fecha del servicio) a la sección existente del formulario.
  
![Vistas: Azure IoT Edge](./media/tutorial-define-edge-device-type/views-properties.png)

Seleccione **Save** (Guardar) para guardar la vista.

### <a name="generate-default-views"></a>Generación de vistas predeterminadas

No se admite la funcionalidad de generación de vistas predeterminadas para plantillas de Azure IoT Edge. 

## <a name="publish-device-template"></a>Publicación de la plantilla de dispositivo

Antes de poder crear un sensor ambiental simulado o de conectar un sensor ambiental real, debe publicar la plantilla de dispositivo.

Para publicar una plantilla de dispositivo, siga estos pasos:

1. Vaya a la plantilla de dispositivo en la página **Device Templates** (Plantillas de dispositivo).

2. Seleccione **Publicar**.
  
    ![Vistas: Publicar](./media/tutorial-define-edge-device-type/edgetemplatepublish.png)

1. En el cuadro de diálogo **Publish a Device Template** (Publicar una plantilla de dispositivo), elija **Publish** (Publicar):
  
    ![Vistas: Publicar](./media/tutorial-define-edge-device-type/edgepublishtemplate.png)

Una vez publicada una plantilla de dispositivo, es visible en la página **Devices** (Dispositivos) y para el operador. En una plantilla de dispositivo publicada, no se puede editar un modelo de funcionalidad del dispositivo sin crear una versión. Sin embargo, puede hacer actualizaciones en las propiedades de la nube, las personalizaciones y las vistas en una plantilla de dispositivo publicada sin control de versiones. Después de realizar los cambios, seleccione **Publish** (Publicar) para enviar esos cambios al operador.
  
![Vistas: Publicar](./media/tutorial-define-edge-device-type/publishedtemplate.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

* Creación de un dispositivo perimetral como plantilla de dispositivo hoja
* Generación de módulos a partir de un manifiesto de implementación cargado
* Adición de telemetría de tipo complejo y propiedades
* Crear propiedades de la nube
* Crear personalizaciones
* Definir una visualización para la telemetría del dispositivo
* Publicación de la plantilla del dispositivo perimetral

Ahora que ha creado una plantilla de dispositivo en la aplicación de Azure IoT Central, este es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Conexión de dispositivos](./tutorial-connect-pnp-device.md)
