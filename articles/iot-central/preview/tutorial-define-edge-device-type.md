---
title: 'Tutorial: Definición de un nuevo tipo de dispositivo de Azure IoT Edge en Azure IoT Central'
description: Este tutorial le muestra, como desarrollador, cómo crear un dispositivo de Azure IoT Edge en la aplicación de Azure IoT Central. Se definen la telemetría, el estado, las propiedades y los comandos del tipo.
author: rangv
ms.author: rangv
ms.date: 10/22/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 97bfd2b1e8b571f44c0b782459567f5677dd36a7
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74702786"
---
# <a name="tutorial-define-a-new-azure-iot-edge-device-type-in-your-azure-iot-central-application-preview-features"></a>Tutorial: Definición de un nuevo tipo de dispositivo de Azure IoT Edge en la aplicación de Azure IoT Central (característica en versión preliminar)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Este tutorial le muestra, como desarrollador, cómo usar una plantilla de dispositivo para definir un nuevo tipo de dispositivo de Azure IoT Edge en la aplicación de Azure IoT Central. 

Para ver una introducción, consulte [¿Qué es Azure IoT Central (características en versión preliminar)?](overview-iot-central.md). 

IoT Edge está formado por tres componentes:
* Los **módulos de IoT Edge** son contenedores que ejecutan servicios de Azure, de asociados o código propio del usuario. Los módulos se implementan en los dispositivos de IoT Edge y se ejecutan de forma local en ellos.
* El **entorno en tiempo de ejecución de IoT Edge** se ejecuta en todos los dispositivos de IoT Edge y administra los módulos que se implementan en cada dispositivo.
* Una **interfaz basada en la nube** permite supervisar y administrar los dispositivos de IoT Edge de forma remota. IoT Central es la interfaz de la nube.

Un dispositivo **Azure IoT Edge** puede ser un dispositivo de puerta de enlace con dispositivos de nivel inferior que se conectan al dispositivo de IoT Edge. En este tutorial se comparte más información sobre los patrones de conectividad de los dispositivos de nivel inferior.

Una **plantilla de dispositivo** define las funcionalidades del dispositivo y de los módulos de IoT Edge. Entre estas se incluyen la telemetría que envía el módulo, las propiedades del módulo y los comandos a los que un módulo responde.

En este tutorial, se crea una plantilla de dispositivo Environment Sensor. Un dispositivo sensor de entorno:

* Envía datos de telemetría, como la temperatura.
* Responde a las propiedades que se escriben cuando se actualiza en la nube, como el intervalo de envío de telemetría.
* Responde a comandos como el restablecimiento de la temperatura.

También en este tutorial, se va a crear una plantilla de dispositivo Environment Gateway. Un dispositivo de puerta de enlace de entorno:

* Envía datos de telemetría, como la temperatura.
* Responde a las propiedades que se escriben cuando se actualiza en la nube, como el intervalo de envío de telemetría.
* Responde a comandos como el restablecimiento de la temperatura.
* Permite relaciones con otros modelos de funcionalidad del dispositivo.


En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una plantilla de dispositivo de Azure IoT Edge.
> * Cargar un manifiesto de implementación.
> * Crear funcionalidades, como la telemetría, las propiedades y los comandos de cada módulo.
> * Definir una visualización para la telemetría del módulo.
> * Agregar relaciones a las plantillas de dispositivo de nivel inferior.
> * Publicar la plantilla de dispositivo

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará [crear una aplicación de Azure IoT Central](quick-deploy-iot-central.md).


## <a name="downstream-device-relationships-with-a-gateway-and-modules"></a>Relaciones de dispositivo de nivel inferior con una puerta de enlace y módulos

Los dispositivos de nivel inferior pueden conectarse a un dispositivo de puerta de enlace IoT Edge mediante el módulo `$edgeHub`. Este dispositivo de IoT Edge se convierte en una puerta de enlace transparente en este escenario.

![Diagrama de una puerta de enlace transparente](./media/tutorial-define-edge-device-type/gateway-transparent.png)

Los dispositivos de nivel inferior también pueden conectarse a un dispositivo de puerta de enlace IoT Edge mediante un módulo personalizado. En el escenario siguiente, los dispositivos de nivel inferior se conectan mediante un módulo personalizado de Modbus.

![Diagrama de conexión de módulo personalizado](./media/tutorial-define-edge-device-type/gateway-module.png)

En el diagrama siguiente se muestra la conexión a un dispositivo de puerta de enlace IoT Edge mediante ambos tipos de módulos (personalizado y `$edgeHub`).  

![Diagrama de conexión mediante ambos módulos de conexión](./media/tutorial-define-edge-device-type/gateway-module-transparent.png)

Finalmente, los dispositivos de nivel inferior se pueden conectar a un dispositivo de puerta de enlace IoT Edge mediante varios módulos personalizados. En el diagrama siguiente se muestran los dispositivos de nivel inferior que se conectan mediante un módulo personalizado de Modbus, un módulo personalizado de BLE y el módulo `$edgeHub`. 

![Diagrama de conexión mediante varios módulos personalizados](./media/tutorial-define-edge-device-type/gateway-module2-transparent.png)


## <a name="create-a-template"></a>Creación de una plantilla

Como desarrollador, puede crear y editar plantillas de dispositivo de IoT Edge en la aplicación. Después de publicar una plantilla de dispositivo, puede conectar dispositivos reales que implementan la plantilla de dispositivo.

### <a name="select-device-template-type"></a>Selección del tipo de plantilla de dispositivo 

Para agregar una nueva plantilla de dispositivo a la aplicación, seleccione **Plantillas de dispositivo** en el panel izquierdo.

![Captura de pantalla de la aplicación de vista previa, con las plantillas de dispositivo resaltadas](./media/tutorial-define-edge-device-type/edgedevicetemplate.png)

Seleccione **+ Nuevo** para empezar a crear una plantilla de dispositivo.

![Captura de pantalla de la página Plantillas de dispositivo con la opción Nuevo resaltada](./media/tutorial-define-edge-device-type/edgedevicetemplatenew.png)

En la página **Seleccionar tipo de plantilla**, seleccione **Azure IoT Edge** y, después, seleccione **Siguiente: Customize** (Personalizar)

![Captura de pantalla de la página Seleccionar tipo de plantilla](./media/tutorial-define-edge-device-type/selectiotedge.png)

### <a name="customize-device-template"></a>Personalización de la plantilla de dispositivo

En IoT Edge, puede implementar y administrar la lógica de negocios en forma de módulos. Los módulos de IoT Edge son la unidad más pequeña de cálculo que administra IoT Edge y pueden contener servicios de Azure (por ejemplo, Azure Stream Analytics) o su propio código específico de la solución. Para entender cómo se desarrollan, implementan y mantienen los módulos, consulte [Módulos de IoT Edge](../../iot-edge/iot-edge-modules.md).

A nivel general, un manifiesto de implementación es una lista de módulos gemelos que se configuran con sus propiedades deseadas. Un manifiesto de implementación indica a un dispositivo IoT Edge (o a un grupo de dispositivos) qué módulos debe instalar y cómo configurarlos. Los manifiestos de implementación incluyen las propiedades deseadas de cada módulo gemelo. Los dispositivos IoT Edge informan sobre las propiedades notificadas de cada módulo.

Use Visual Studio Code para crear un manifiesto de implementación. Para más información, consulte [Azure IoT Edge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

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

#### <a name="upload-an-iot-edge-deployment-manifest"></a>Carga de un manifiesto de implementación de IoT Edge

En la página **Personalizar dispositivo**, en **Upload an Azure IoT Edge deployment manifest** (Cargar un manifiesto de implementación de Azure IoT Edge), seleccione **Examinar**. 

![Captura de pantalla de la página Personalizar dispositivo, con la opción Examinar resaltada](./media/tutorial-define-edge-device-type/edgedevicetemplateuploadmanifest.png)

Si tiene previsto crear una plantilla de dispositivo de puerta de enlace de IoT Edge, asegúrese de seleccionar la casilla **Gateway device with downstream devices** (Dispositivo de puerta de enlace con dispositivos de bajada).

![Captura de pantalla de la página Personalizar dispositivo, con un dispositivo de puerta de enlace con dispositivos de nivel inferior resaltados](./media/tutorial-define-edge-device-type/gateway-upload.png)

En el cuadro de diálogo de selección de archivos, seleccione el archivo del manifiesto de implementación y seleccione **Abrir**.

IoT Edge valida el archivo de manifiesto de implementación con un esquema. Si la validación es correcta, seleccione **Revisar**.

![Captura de pantalla de la página Personalizar dispositivo con las opciones Manifiesto de implementación y Revisar resaltadas](./media/tutorial-define-edge-device-type/deploymentmanifestvalidate.png)

El siguiente diagrama de flujo muestra el ciclo de vida de un manifiesto de implementación en IoT Central.

![Diagrama de flujo del ciclo de vida de un manifiesto de implementación](./media/tutorial-define-edge-device-type/dmflow.png)

A continuación, se muestra la página de revisión con detalles del manifiesto de implementación. Esta página muestra una lista de módulos del manifiesto de implementación. En este tutorial, observe que se muestra el módulo `SimulatedTemperatureSensor`. Seleccione **Crear**.

![Captura de pantalla de la página Revisar, con las opciones Módulo y Crear resaltadas](./media/tutorial-define-edge-device-type/edgedevicetemplatereview.png)

Si había seleccionado un dispositivo de puerta de enlace, verá la siguiente página de revisión.

![Captura de pantalla de la página Revisar, con la puerta de enlace de Azure IoT Edge resaltada](./media/tutorial-define-edge-device-type/gateway-review.png)


Puede crear una plantilla de dispositivo con modelos de funcionalidad de módulo. En este tutorial, puede crear una plantilla de dispositivo con el modelo de funcionalidad del módulo `SimulatedTemperatureSensor`. 

Cambie el título de la plantilla de dispositivo por **Environment Sensor Device Template**.

![Captura de pantalla de la plantilla de dispositivo, con el título actualizado resaltado](./media/tutorial-define-edge-device-type/edgedevicetemplatelanding.png)

En el dispositivo IoT Edge, modele IoT Plug and Play de la siguiente manera:
* Cada plantilla de dispositivo de IoT Edge tiene un modelo de funcionalidad del dispositivo.
* Para cada módulo personalizado incluido en el manifiesto de implementación, se genera un modelo de funcionalidad del módulo.
* Se establece una relación entre cada modelo de funcionalidad del módulo y un modelo de funcionalidad del dispositivo.
* Un modelo de funcionalidad del módulo implementa interfaces de módulo.
* Cada interfaz del módulo contiene datos de telemetría, propiedades y comandos.

![Diagrama de modelado de IoT Edge](./media/tutorial-define-edge-device-type/edgemodelling.png)

#### <a name="add-capabilities-to-a-module-capability-model"></a>Adición de funcionalidades a un modelo de funcionalidad del módulo

Esta es una salida de ejemplo del módulo `SimulatedTemperatureSensor`:
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

Puede agregar funcionalidades al módulo `SimulatedTemperatureSensor` que reflejarán la salida anterior. 

1. Para administrar una interfaz del modelo de funcionalidad del módulo `SimulatedTemperatureSensor`, seleccione **Administrar** > **Agregar funcionalidad**. 

    ![Captura de pantalla de Environment Sensor Template, con la opción Agregar funcionalidad resaltada](./media/tutorial-define-edge-device-type/edgetemplateaddcapability.png)
  
1. Agregue una máquina como un tipo de objeto.
  
    ![Captura de pantalla de la página de funcionalidades de Environment Sensor Template con el esquema resaltado](./media/tutorial-define-edge-device-type/edgetemplatemachineobject.png)

1. Seleccione **Definir**. En el cuadro de diálogo que aparece, cambie el nombre del objeto a **machine** (máquina). Cree las propiedades de temperatura y presión, y seleccione **Aplicar**.
  
    ![Captura de pantalla del cuadro de diálogo de atributos con varias opciones resaltadas](./media/tutorial-define-edge-device-type/edgetemplatemachineattributes.png)
  
1. Agregue **ambient** (ambiente) como tipo de objeto.

1. Seleccione **Definir**. En el cuadro de diálogo que aparece, cambie el nombre del objeto a **ambient** (ambiente). Cree las propiedades de temperatura y humedad, y seleccione **Aplicar**.
  
    ![Captura de pantalla del cuadro de diálogo de atributos con varias opciones resaltadas](./media/tutorial-define-edge-device-type/edgetemplateambientattributes.png)

  
1. Agregue `timeCreated` como un tipo `DateTime` y seleccione **Guardar**.
  
    ![Captura de pantalla de Environment Sensor Template, con la opción Guardar resaltada](./media/tutorial-define-edge-device-type/edgetemplateallattributes.png)


### <a name="add-relationships"></a>Adición de relaciones

Si seleccionó un dispositivo de IoT Edge como dispositivo de puerta de enlace, puede agregar relaciones de nivel inferior a los modelos de funcionalidad de los dispositivos que desea conectar a él.
  
  ![Captura de pantalla de Environment Gateway Template con la opción Agregar relación resaltada](./media/tutorial-define-edge-device-type/gateway-add-relationship.png)

Puede agregar una relación en un dispositivo o en un módulo.
  
  ![Captura de pantalla de Environment Gateway Template con las relaciones en el nivel de dispositivo y de módulo resaltadas](./media/tutorial-define-edge-device-type/gateway-relationship-types.png)


Puede seleccionar un modelo de funcionalidad del dispositivo de nivel inferior o seleccionar el símbolo del asterisco. 
  
  ![Captura de pantalla de Environment Gateway Template con la opción Destino resaltada](./media/tutorial-define-edge-device-type/gateway-downstream-rel.png)

  En este tutorial, seleccione el asterisco. Esta opción permite todas las relaciones de nivel inferior. Después, seleccione **Guardar**.

  ![Captura de pantalla de Environment Gateway Template con la opción Destino resaltada](./media/tutorial-define-edge-device-type/gateway-add-relationship-asterix.png)


### <a name="add-cloud-properties"></a>Adición de propiedades de nube

Una plantilla de dispositivo puede incluir propiedades de la nube. Las propiedades de la nube solo existen en la aplicación IoT Central y nunca se envían a un dispositivo ni se reciben de él.

1. Seleccione **Propiedades de nube** >  **+ Agregar propiedad de la nube**. Use la información de la tabla siguiente para agregar una propiedad de la nube a la plantilla de dispositivo.

    | Nombre para mostrar      | Tipo semántico | Schema |
    | ----------------- | ------------- | ------ |
    | Fecha de la última revisión | None          | Date   |
    | Nombre del cliente     | None          | Cadena |

2. Seleccione **Guardar**.

  
    ![Captura de pantalla de Environment Sensor Template, con la opción Guardar resaltada](./media/tutorial-define-edge-device-type/edgetemplatecloudproperties.png)

### <a name="add-customizations"></a>Adición de personalizaciones

Use personalizaciones para modificar una interfaz o agregar características específicas de IoT Central a una funcionalidad que no requiera controlar la versión del modelo de funcionalidad del dispositivo. Se pueden personalizar los campos cuando el modelo de funcionalidad esté en estado borrador o publicado. Y solo se pueden personalizar aquellos que no interrumpan la compatibilidad de la interfaz. Por ejemplo, puede:

- Personalizar el nombre para mostrar y las unidades de una funcionalidad.
- Agregar un color predeterminado para usarlo cuando el valor aparezca en un gráfico.
- Especificar los valores inicial, mínimo y máximo de una propiedad.

No se puede personalizar el tipo o el nombre de la funcionalidad.

Cuando haya terminado la personalización, seleccione **Guardar**.
  
![Captura de pantalla de la página de personalización de Environment Sensor Template](./media/tutorial-define-edge-device-type/edgetemplatecustomize.png)


### <a name="create-views"></a>Creación de vistas

Como desarrollador, puede personalizar la aplicación para mostrar información pertinente sobre el dispositivo sensor ambiental a un operador. Las personalizaciones permiten al operador administrar estos dispositivo conectados a la aplicación. Puede crear dos tipos de vistas para que un operador interactúe con los dispositivos:

* Formularios para ver y editar las propiedades del dispositivo y la nube
* Paneles para visualizar los dispositivos

### <a name="configure-a-view-to-visualize-devices"></a>Configuración de una vista para visualizar los dispositivos

Un panel de dispositivos permite a un operador visualizar un dispositivo mediante gráficos y métricas. Como desarrollador, puede definir qué información aparece en el panel del dispositivo. Puede definir varios paneles para los dispositivos. Para crear un panel y visualizar la telemetría del sensor ambiental, seleccione **Vistas** > **Visualización del dispositivo**:

  
![Captura de pantalla de la página de vistas de Environment Sensor Template con la opción Visualización del dispositivo resaltada](./media/tutorial-define-edge-device-type/visualizingthedevice.png)


Ambient Telemetry (Telemetría ambiental) y Machine Telemetry (Telemetría de máquina) son objetos complejos. Para crear gráficos:

1. Arrastre **Ambient Telemetry** (Telemetría ambiental) y seleccione **Gráfico de líneas**. 
  
   ![Captura de pantalla de Environment Sensor Template con Ambient Telemetry (Telemetría ambiental) y Gráfico de líneas resaltados](./media/tutorial-define-edge-device-type/sensorambientchart.png)

1. Seleccione el icono de configuración. Seleccione **Temperature** (Temperatura) y **Humidity** (Humedad) para visualizar los datos y seleccione **Actualizar configuración**. 
  
   ![Captura de pantalla de Environment Sensor Template con varias opciones resaltadas](./media/tutorial-define-edge-device-type/sensorambienttelemetrychart.png)

1. Seleccione **Guardar**.

Puede agregar más iconos que muestren otras propiedades o valores de telemetría. También puede agregar texto estático, vínculos e imágenes. Para mover o cambiar el tamaño de un icono en el panel, mueva el puntero del mouse sobre el icono y arrastre el icono a una nueva ubicación o cambie su tamaño.
  
![Captura de pantalla de la vista Panel de Environment Sensor Template](./media/tutorial-define-edge-device-type/viewsdashboard.png)

### <a name="add-a-device-form"></a>Adición de un formulario de dispositivo

Un formulario de dispositivo permite que un operador edite las propiedades del dispositivo y de la nube grabables. Como desarrollador, puede definir varios formularios y elegir qué propiedades de la nube y del dispositivo se van a mostrar en cada uno. También puede mostrar las propiedades de un dispositivo de solo lectura en un formulario.

Para crear un formulario y ver y editar las propiedades del sensor ambiental, siga estos pasos:

1. En **Environmental Sensor Template**, vaya a **Vistas**. Seleccione el icono **Editing Device and Cloud data** (Editar datos del dispositivo y de la nube) para agregar una vista nueva.
  
   ![Captura de pantalla de la página de vistas de Environmental Sensor Template con la opción Editar datos del dispositivo y la nube resaltada](./media/tutorial-define-edge-device-type/editingdeviceandclouddata.png)

1. Escriba el nombre del formulario **Propiedades del sensor ambiental**.

1. Arrastre las propiedades de la nube **Customer name** (Nombre del cliente) y **Last service date** (Última fecha del servicio) a la sección existente del formulario.
  
   ![Captura de pantalla de la página de vistas de Environmental Sensor Template con varias opciones resaltadas](./media/tutorial-define-edge-device-type/views-properties.png)

1. Seleccione **Guardar**.

## <a name="publish-a-device-template"></a>Publicación de una plantilla de dispositivo

Antes de poder crear un sensor ambiental simulado o de conectar un sensor ambiental real, debe publicar la plantilla de dispositivo.

Para publicar una plantilla de dispositivo, siga estos pasos:

1. Vaya a la plantilla de dispositivo en la página **Device Templates** (Plantillas de dispositivo).

2. Seleccione **Publicar**.
  
    ![Captura de pantalla de Environmental Sensor Template con la opción Publicar resaltada](./media/tutorial-define-edge-device-type/edgetemplatepublish.png)

1. En el cuadro de diálogo **Publish a Device Template** (Publicar una plantilla de dispositivo), elija **Publicar**.
  
    ![Captura de pantalla del cuadro de diálogo Publish a Device Template (Publicar una plantilla de dispositivo) con la opción Publicar resaltada](./media/tutorial-define-edge-device-type/edgepublishtemplate.png)

Una vez publicada una plantilla de dispositivo, es visible en la página **Devices** (Dispositivos) y para el operador. En una plantilla de dispositivo publicada, no se puede editar un modelo de funcionalidad del dispositivo sin crear una versión. Sin embargo, puede hacer actualizaciones en las propiedades de la nube, las personalizaciones y las vistas en una plantilla de dispositivo publicada. Estas actualizaciones no hacen que se cree una nueva versión. Después de realizar los cambios, seleccione **Publicar** para enviar esos cambios al operador.
  
![Captura de pantalla de la lista Plantillas de dispositivo de plantillas publicadas](./media/tutorial-define-edge-device-type/publishedtemplate.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

* Crear un dispositivo perimetral como plantilla de dispositivo hoja.
* Generar módulos a partir de un manifiesto de implementación cargado.
* Agregar telemetría de tipo complejo y propiedades
* Crear propiedades de la nube
* Crear personalizaciones
* Definir una visualización para la telemetría del dispositivo
* Publique la plantilla del dispositivo perimetral.

Ahora que ha creado una plantilla de dispositivo en una aplicación de Azure IoT Central puede hacer esto a continuación:

> [!div class="nextstepaction"]
> [Conexión de dispositivos](./tutorial-connect-pnp-device.md)
