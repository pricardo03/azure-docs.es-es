---
title: Implementación y supervisión de módulos para Azure IoT Edge | Microsoft Docs
description: Administración de los módulos que se ejecutan en dispositivos perimetrales
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 07/25/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 6ebd2a4e24a5f0bd9a9adad97bf26ae61219c8e0
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "51566252"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-portal"></a>Implementación y supervisión de módulos de IoT Edge a escala mediante Azure Portal

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-edge-how-to-deploy-monitor-selector.md)]

Azure IoT Edge permite mover los análisis al perímetro y proporciona una interfaz en la nube para que pueda administrar y supervisar los dispositivos de IoT Edge sin tener que acceder físicamente a cada uno de ellos. La capacidad de administrar dispositivos de forma remota es cada vez más importante, ya que las soluciones de Internet de las cosas son cada vez más grandes y complejas. Azure IoT Edge está pensado para apoyar sus objetivos empresariales, sin importar cuántos dispositivos agregue.

Puede administrar dispositivos individuales e implementarles módulos de uno en uno. Sin embargo, si desea realizar cambios en los dispositivos en a gran escala, puede crear una **implementación automática de IoT Edge**, que forma parte de la administración automática de dispositivos de IoT Hub. Las implementaciones son procesos dinámicos que permiten poner en marcha varios módulos en múltiples dispositivos a la vez, realizar un seguimiento del estado y del mantenimiento de los módulos y realizar cambios cuando sea necesario. 

## <a name="identify-devices-using-tags"></a>Identificación de dispositivos mediante etiquetas

Antes de crear una implementación, tendrá que especificar los dispositivos que desea afectar. Azure IoT Edge identifica los dispositivos mediante **etiquetas** en el dispositivo gemelo. Cada dispositivo puede tener varias etiquetas y puede definirlas de cualquier manera que tenga sentido para su solución. Por ejemplo, si administra un campus de edificios inteligentes, puede agregar las siguientes etiquetas a un dispositivo:

```json
"tags":{
    "location":{
        "building": "20",
        "floor": "2"
    },
    "roomtype": "conference",
    "environment": "prod"
}
```

Para más información sobre dispositivos gemelos y etiquetas, consulte [Información y uso de dispositivos gemelos en IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md).

## <a name="create-a-deployment"></a>de una implementación

1. En [Azure Portal](https://portal.azure.com), vaya hasta su instancia de IoT Hub. 
1. Seleccione **IoT Edge**.
1. Seleccione **Agregar implementación de IoT Edge**.

Hay cinco pasos para crear una implementación. En las siguientes secciones se abordan cada uno de ellos. 

### <a name="step-1-name-and-label"></a>Paso 1: Nombre y etiqueta

1. Asigne a su implementación un nombre exclusivo de hasta 128 letras en minúscula. Evite los espacios y los siguientes caracteres no válidos: `& ^ [ ] { } \ | " < > /`.
1. Agregue etiquetas para realizar un mejor seguimiento de las implementaciones. Las etiquetas son pares de **nombre** y **valor** que describen la implementación. Por ejemplo, `HostPlatform, Linux` o `Version, 3.0.1`.
1. Seleccione **Siguiente** para ir al segundo paso. 

### <a name="step-2-add-modules-optional"></a>Paso 2: Adición de módulos (opcional)

Hay dos tipos de módulos que se pueden agregar a una implementación. El primero es un módulo basado en un servicio de Azure, como una cuenta de Storage o Stream Analytics. El segundo es un módulo basado en su propio código. Puede agregar varios módulos de cualquier tipo a una implementación. 

Si se crea una implementación sin módulos, quita cualquier módulo actual de los dispositivos. 

>[!NOTE]
>Azure Machine Learning y Azure Functions todavía no admiten la implementación del servicio de Azure automatizada. Use la implementación de módulo personalizada para agregar manualmente esos servicios a la implementación. 

Para agregar un módulo desde Azure Stream Analytics, siga estos pasos:
1. En la sección **Deployment Modules** (Módulos de implementación) de la página, haga clic en **Agregar**.
1. Seleccione **Azure Stream Analytics module** (Módulo de Azure Stream Analytics).
1. Seleccione su **Suscripción** en el menú desplegable.
1. Seleccione su **trabajo de Microsoft Edge** en el menú desplegable.
1. Seleccione **Guardar** para agregar el módulo a la implementación. 

Para agregar código personalizado como un módulo, o para agregar manualmente un módulo de servicio de Azure, siga estos pasos:
1. En la sección **Configuración del registro** de la página, proporcione los nombres y las credenciales para cualquier registro del contenedor privado que contenga las imágenes del módulo para esta implementación. El agente de Edge notificará el error 500 si no puede encontrar las credenciales del registro del contenedor para una imagen de Docker.
1. En la sección **Deployment Modules** (Módulos de implementación) de la página, haga clic en **Agregar**.
1. Seleccione **Módulo IoT Edge**.
1. Asigne un **nombre** al módulo.
1. En el campo **URI de imagen**, introduzca la imagen de contenedor para el módulo. 
1. Especifique cualquier **opción de creación de contenedor** que deba pasarse al contenedor. Para más información, consulte [docker create](https://docs.docker.com/engine/reference/commandline/create/).
1. Use el menú desplegable para seleccionar una **directiva de reinicio**. Elija entre las siguientes opciones: 
   * **Always** (Siempre): el módulo siempre se reinicia si se cierra por cualquier razón.
   * **Never** (Nunca): el módulo nunca se reinicia si se cierra por cualquier razón.
   * **On-failed** (En error): el módulo se reinicia si se bloquea, pero no si se cierra sin problemas. 
   * **On-unhealthy** (En estado incorrecto): el módulo se reinicia si se bloquea o devuelve un estado incorrecto. Depende de cada módulo la implementación de la función de estado de mantenimiento. 
1. Use el menú desplegable para seleccionar el **estado deseado** para el módulo. Elija entre las siguientes opciones:
   * **En ejecución**: esta es la opción predeterminada. El módulo volverá a ejecutarse inmediatamente después de la implementación.
   * **Detenido**: tras la implementación, el módulo permanecerá inactivo hasta que se llame después del inicio por el usuario u otro módulo.
1. Seleccione **Habilitar** si quiere agregar etiquetas o propiedades al módulo gemelo. 
1. Escriba las **Variables de entorno** para este módulo. Las variables de entorno brindan información adicional a un módulo, lo cual facilita el proceso de configuración.
1. Seleccione **Guardar** para agregar el módulo a la implementación. 

Cuando tenga todos los módulos configurados para una implementación, seleccione **Siguiente** para ir al tercer paso.

### <a name="step-3-specify-routes-optional"></a>Paso 3: Especificación de rutas (opcional)

Las rutas definen cómo los módulos se comunican entre sí dentro de una implementación. De forma predeterminada, el asistente proporciona una ruta llamada **route** y definida como **FROM /\* INTO $upstream**, que significa que cualquier mensaje de salida de cualquier módulo se envía al IoT Hub.  

Agregue o actualice las rutas con información de [Declaración de rutas](module-composition.md#declare-routes) y, luego, seleccione **Siguiente** para continuar con la sección de revisión.


### <a name="step-4-target-devices"></a>Paso 4: Dispositivos de destino

Use la propiedad de etiquetas en los dispositivos para dirigirse a los dispositivos específicos que deberían recibir esta implementación. 

Como varias implementaciones pueden tener como destino el mismo dispositivo, debe dar a cada implementación un número de prioridad. En caso de conflicto, gana la implementación con la prioridad más alta (los valores más altos indican prioridad más alta). Si dos implementaciones tienen el mismo número de prioridad, gana la que se creó más recientemente. 

1. Especifique un número entero positivo en el valor de **Prioridad** de la implementación. En el caso de que dos o más implementaciones se destinen al mismo dispositivo, se aplicará la implementación con el valor numérico más alto para la prioridad.
1. Escriba una **condición de destino** para determinar qué dispositivos se dirigirán a esta implementación. La condición se basa en las etiquetas del dispositivo gemelo o en las propiedades notificadas del dispositivo gemelo y debe coincidir con el formato de expresión. Por ejemplo, `tags.environment='test'` o `properties.reported.devicemodel='4000x'`. 
1. Seleccione **Siguiente** para pasar al último paso.

### <a name="step-5-review-template"></a>Paso 5: Revisión de la plantilla

Revise la información de implementación y seleccione **Enviar**.

## <a name="monitor-a-deployment"></a>Supervisión de una implementación

Para ver los detalles de una implementación y supervisar los dispositivos que la ejecutan, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub. 
1. Seleccione **IoT Edge**.
1. Seleccione **IoT Edge deployments** (Implementaciones de IoT Edge). 

   ![Visualización de las implementaciones de IoT Edge](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Examine la lista de implementaciones. Para cada implementación, puede ver los detalles siguientes:
   * **ID**: nombre de la implementación.
   * **Target condition** (Condición de destino): la etiqueta que se utiliza para definir los dispositivos dirigidos.
   * **Priority** (Prioridad): el número de prioridad asignado a la implementación.
   * **Métricas del sistema** - **Dirigidas** especifica el número de dispositivos gemelos en IoT Hub que coinciden con la condición de destino, y **Aplicadas** especifica el número de dispositivos que ha tenido el contenido de implementación aplicado a sus módulos gemelos en IoT Hub. 
   * **Métricas del dispositivo**: el número de dispositivos de Edge en el informe de implementación correcto o con errores del entorno de ejecución del cliente de IoT Edge.
   * **Creation time** (Hora de creación): la marca de tiempo de cuando se creó la implementación. Esta marca de tiempo se utiliza para dirimir cuando dos implementaciones tienen la misma prioridad. 
2. Seleccione la implementación que desea supervisar.  
3. Examine los detalles de la implementación. Puede usar las pestañas para revisar los detalles de la implementación.

## <a name="modify-a-deployment"></a>Modificación de una implementación

Cuando se modifica una implementación, los cambios se replican inmediatamente a todos los dispositivos seleccionados. 

Si actualiza la condición de destino, se producen las siguientes actualizaciones:
* Si un dispositivo no cumplía la antigua condición de destino, pero cumple la nueva condición de destino y esta implementación es la prioridad más alta para ese dispositivo, esta implementación se aplica al dispositivo. 
* Si un dispositivo que actualmente ejecuta esta implementación ya no cumple la condición de destino, desinstala esta implementación y asume la siguiente implementación de mayor prioridad. 
* Si un dispositivo que actualmente ejecuta esta implementación ya no cumple la condición de destino y no cumple la condición de destino de cualquier otra implementación, no se produce ningún cambio en el dispositivo. El dispositivo sigue ejecutando los módulos actuales en su estado actual, pero ya no se administra como parte de esta implementación. Cuando se cumple la condición de destino de cualquier otra implementación, desinstala esta implementación y adopta una nueva. 

Para modificar una implementación, siga estos pasos: 

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub. 
1. Seleccione **IoT Edge**.
1. Seleccione **IoT Edge deployments** (Implementaciones de IoT Edge). 

   ![Visualización de las implementaciones de IoT Edge](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Seleccione la implementación que desea modificar. 
1. Realice las actualizaciones en los campos siguientes: 
   * Condición de destino 
   * Etiquetas 
   * Prioridad 
1. Seleccione **Guardar**.
1. Siga los pasos de [Supervisión de una implementación](#monitor-a-deployment) para observar la implementación de los cambios. 

## <a name="delete-a-deployment"></a>Eliminación de una implementación

Cuando se elimina una implementación, los dispositivos adoptan la siguiente implementación de prioridad más alta. Si los dispositivos no cumplen la condición de destino de alguna implementación, los módulos no se quitan cuando se elimina la implementación. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub. 
1. Seleccione **IoT Edge**.
1. Seleccione **IoT Edge deployments** (Implementaciones de IoT Edge). 

   ![Visualización de las implementaciones de IoT Edge](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Utilice la casilla de verificación para seleccionar la implementación que desea eliminar. 
1. Seleccione **Eliminar**.
1. Un mensaje le informará de que esta acción eliminará esta implementación y volverá al estado anterior para todos los dispositivos.  Esto significa que se aplicará una implementación con una prioridad más baja.  Si ninguna otra implementación está dirigida, no se quitará ningún módulo. Si quiere quitar todos los módulos del dispositivo, cree una implementación con cero módulos e impleméntela a los mismos dispositivos. Seleccione **Yes** (Sí) para continuar. 

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo [implementar módulos en dispositivos Edge](module-deployment-monitoring.md).
