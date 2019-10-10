---
title: 'Tutorial: Configuración de un entorno de versión preliminar de Azure Time Series Insights | Microsoft Docs'
description: Obtenga información sobre cómo configurar el entorno en la versión preliminar de Azure Time Series Insights.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 10/02/2019
ms.custom: seodec18
ms.openlocfilehash: e680652d490d75701780e2e3618a43451bfe2819
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71845095"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Tutorial: Configuración de un entorno en la versión preliminar de Azure Time Series Insights

Este tutorial le guía en el proceso de crear un entorno de pago por uso (PAYG) de la versión preliminar de Azure Time Series Insights. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un entorno en la versión preliminar de Azure Time Series Insights.
> * Conectar el entorno de versión preliminar de Azure Time Series Insights a un centro de eventos en Azure Event Hubs.
> * Ejecutar un ejemplo del acelerador de soluciones para transmitir datos en el entorno de versión preliminar de Azure Time Series Insights.
> * Realizar un análisis básico de los datos.
> * Definir el tipo y la jerarquía de un modelo de Time Series y asociarlos con sus instancias.

>[!TIP]
> Los [aceleradores de soluciones de IoT](https://www.azureiotsolutions.com/Accelerators) proporcionan soluciones preconfiguradas de nivel empresarial que se pueden usar puede usar para acelerar el desarrollo de soluciones de IoT personalizadas.

Regístrese para obtener [suscripción gratuita a Azure](https://azure.microsoft.com/free/), en caso de que no tenga ninguna.

## <a name="prerequisites"></a>Requisitos previos

* La cuenta de inicio de sesión de Azure también debe ser miembro del rol **Propietario** de la suscripción. Para más información, consulte [Administración del acceso a los recursos de Azure mediante RBAC y Azure Portal](../role-based-access-control/role-assignments-portal.md).

## <a name="create-a-device-simulation"></a>Creación de una simulación de dispositivo

En esta sección, creará tres dispositivos simulados que envían datos a una instancia de Azure IoT Hub.

1. Vaya a la [página de aceleradores de soluciones de Azure IoT](https://www.azureiotsolutions.com/Accelerators). En la página se muestran varios ejemplos creados previamente. Inicie sesión con la cuenta de Azure. A continuación, seleccione **Simulación de dispositivo**.

   [![Página de aceleradores de soluciones de Azure IoT](media/v2-update-provision/device-one-accelerator.png)](media/v2-update-provision/device-one-accelerator.png#lightbox)

   Seleccione **Try Now** (Intentar ahora).

1. Escriba los parámetros necesarios en la página **Crear la solución Simulación de dispositivos**:

    | Parámetro | . |
    | --- | --- |
    | **Nombre de la implementación** | Escriba un valor único para un nuevo grupo de recursos. Los recursos de Azure enumerados se crean y se asigna al grupo de recursos. |
    | **Suscripción de Azure** | Seleccione la suscripción que usó para crear el entorno de Time Series Insights. |
    | **Ubicación de Azure** | Seleccione la región que usó para crear el entorno de Time Series Insights. |
    | **Opciones de implementación** | Seleccione **Provision new IoT Hub** (Aprovisionar nuevo centro de IoT). |
 
    Seleccione **Create solution** (Crear solución). La solución puede tardar hasta 20 minutos en terminar de implementarse.

    [![Página de creación de la solución de simulación de dispositivos](media/v2-update-provision/device-two-create.png)](media/v2-update-provision/device-two-create.png#lightbox)

## <a name="create-a-preview-payg-environment"></a>Creación de un entorno de pago por uso (versión preliminar)

En esta sección se describe cómo crear un entorno dee Azure Time Series Insights en versión preliminar y conectarlo al centro de IoT que el Acelerador de soluciones de IOT mediante [Azure Portal](https://portal.azure.com/).

1. Inicie sesión en Azure Portal con la cuenta de su suscripción.

1. Seleccione **Crear un recurso** > **Internet de las cosas** > **Time Series Insights**.

   [![Seleccionar Internet de las cosas y, luego, Time Series Insights](media/v2-update-provision/payg-one-azure.png)](media/v2-update-provision/payg-one-azure.png#lightbox)

1. En el panel **Creación del entorno de Time Series Insights**, en la pestaña **Aspectos básicos**, establezca los parámetros siguientes:

    | Parámetro | . |
    | --- | ---|
    | **Nombre del entorno** | Especifique un nombre único para el entorno de la versión preliminar de Azure Time Series Insights. |
    | **Suscripción** | Especifique la suscripción donde desea crear el entorno de versión preliminar de Azure Time Series Insights. Es un procedimiento recomendado usar la misma suscripción que el resto de los recursos de IoT creados por el simulador de dispositivos. |
    | **Grupos de recursos** | Seleccione un grupo de recursos existente o cree un nuevo grupo de recursos para el recurso de entorno de versión preliminar de Azure Time Series Insights. Un grupo de recursos es un contenedor para recursos de Azure. Un procedimiento recomendado es usar el mismo grupo de recursos que el resto de los recursos de IoT creados por el simulador de dispositivos. |
    | **Ubicación** | Seleccione una región del centro de datos para su entorno de versión preliminar de Azure Time Series Insights. Para evitar una latencia adicional, es mejor crear el entorno de Azure Time Series Insights en versión preliminar en la misma región que los demás recursos de IoT. |
    | **Nivel** |  Seleccione **Pago por uso** *.* Esta es la SKU del producto de versión preliminar de Azure Time Series Insights. |
    | **Id. de propiedad** | Especifique un valor que identifique de forma única su instancia de serie temporal. El valor que especifique en el cuadro **Id. de propiedad** es inmutable. No se puede cambiar después. En este tutorial, escriba **iothub-connection-device-id**. Para obtener más información acerca del identificador de Time Series, consulte [Procedimientos recomendados al elegir un id. de serie temporal](./time-series-insights-update-how-to-id.md). |
    | **Nombre de cuenta de almacenamiento** | Escriba un nombre único global para la nueva cuenta de almacenamiento que se va a crear. |
   
   Seleccione **Siguiente: Origen de eventos**.

   [![Página de creación de un entorno de Time Series Insights](media/v2-update-provision/payg-two-create.png)](media/v2-update-provision/payg-two-create.png#lightbox)

1. En la pestaña **Origen del evento**, puede establecer los siguientes parámetros:

   | Parámetro | . |
   | --- | --- |
   | **Create an event source?** ¿Crear un origen del evento? | Seleccione **Sí**.|
   | **Nombre** | Escriba un valor único para el nombre del origen de eventos. |
   | **Tipo de origen** | Seleccione **IoT Hub**. |
   | **Selección de un centro** | Elija **Seleccionar existente**. |
   | **Suscripción** | Seleccione la suscripción que usó para el simulador de dispositivos. |
   | **Nombre de la instancia de IoT Hub** | Seleccione el nombre del centro de IoT que creó para el simulador de dispositivos. |
   | **Directiva de acceso de IoT Hub** | seleccione **iothubowner**. |
   | **Grupo de consumidores de IoT Hub** | Seleccione **New** (Nuevo), escriba un nombre único y, a continuación, seleccione **Add** (Agregar). El grupo de consumidores debe ser un valor único en la versión preliminar de Azure Time Series Insights. |
   | **Propiedad de marca de tiempo** | Este campo se utiliza para identificar la propiedad de **Marca de tiempo** en los datos de telemetría entrantes. En este tutorial, deje este cuadro vacío. Este simulador usa la marca de tiempo entrante de IoT Hub, que Time Series Insights tiene como valor predeterminado. |

   Seleccione **Revisar + crear**.

   [![Configuración de un origen del evento](media/v2-update-provision/payg-five-event-source.png)](media/v2-update-provision/payg-five-event-source.png#lightbox)

1. En la pestaña **Revisar y crear**, revise sus selecciones y, a continuación, seleccione **Crear**.

    [![Página Review + Create (Revisar + crear), con el botón Create (Crear)](media/v2-update-provision/payg-six-review.png)](media/v2-update-provision/payg-six-review.png#lightbox)

    Puede ver el estado de la implementación:

    [![Notificación de que la implementación ha finalizado](media/v2-update-provision/payg-seven-deploy.png)](media/v2-update-provision/payg-seven-deploy.png#lightbox)

1. Tiene acceso a su entorno de versión preliminar de Azure Time Series Insights si es propietario del inquilino. Para asegurarse de que tiene acceso:

   1. Busque el grupo de recursos y seleccione el entorno de versión preliminar de Azure Time Series Insights:

      [![Entorno seleccionado](media/v2-update-provision/payg-eight-environment.png)](media/v2-update-provision/payg-eight-environment.png#lightbox)

   1. En la página de la versión preliminar de Azure Time Series Insights, vaya a **Directivas de acceso a datos**:

      [![Directivas de acceso a datos](media/v2-update-provision/payg-nine-data-access.png)](media/v2-update-provision/payg-nine-data-access.png#lightbox)

   1. Compruebe que aparecen las credenciales:

      [![Credenciales mostradas](media/v2-update-provision/payg-ten-verify.png)](media/v2-update-provision/payg-ten-verify.png#lightbox)

   Si no se muestran sus credenciales, deberá darse permiso para acceder al entorno. Para más información sobre cómo establecer permisos, lea [Concesión de acceso a datos](./time-series-insights-data-access.md).

## <a name="stream-data"></a>Datos de streaming

Ahora que ha implementado el entorno de Time Series Insights, transmita datos a él para su análisis.

1. Vaya a la [página de aceleradores de soluciones de Azure IoT](https://www.azureiotsolutions.com/Accelerators). Busque la solución en el panel del acelerador de soluciones. Luego, seleccione **Iniciar**:

    [![Inicio de la solución de simulación de dispositivo](media/v2-update-provision/device-three-launch.png)](media/v2-update-provision/device-three-launch.png#lightbox)

1. Se le redirigirá a la página de **simulación de dispositivos IoT de Microsoft Azure**. Seleccione **+ New simulation** (+Nueva simulación) en la esquina superior derecha.

    [![Página de simulación de Azure IoT](media/v2-update-provision/device-four-iot-sim-page.png)](media/v2-update-provision/device-four-iot-sim-page.png#lightbox)

1. En el panel del **programa de instalación de la simulación**, establezca los parámetros siguientes:

    | Parámetro | . |
    | --- | --- |
    | **Nombre** | Escriba un nombre único para un simulador. |
    | **Descripción** | Escriba una definición. |
    | **Simulation duration** (Duración de la simulación) | Establézcala en **Run indefinitely** (Ejecutar indefinidamente). |
    | **Device model** (Modelo de dispositivo) | **Nombre**: Escriba **Refrigerador**. <br />**Amount** (Cantidad): Escriba **3**. |
    | **Target IoT Hub** (IoT Hub de destino) | Establézcalo en **Use pre-provisioned IoT Hub** (Usar IoT Hub aprovisionado previamente). |

    [![Parámetros para especificar](media/v2-update-provision/device-five-params.png)](media/v2-update-provision/device-five-params.png#lightbox)

    Seleccione **Start simulation** (Iniciar simulación).

    En el panel de simulación de dispositivos, consulte la información relativa a los **dispositivos activos** y los **mensajes por segundo**.

    [![Panel de simulación de Azure IoT](media/v2-update-provision/device-seven-dashboard.png)](media/v2-update-provision/device-seven-dashboard.png#lightbox)

## <a name="analyze-data"></a>Análisis de datos

En esta sección, se va realizar un análisis básico de los datos de la serie temporal mediante el [explorador de la versión preliminar de Azure Time Series Insights](./time-series-insights-update-explorer.md).

1. Vaya al explorador de la versión preliminar de Azure Time Series Insights; para ello, seleccione la dirección URL de la página de recursos de [Azure Portal](https://portal.azure.com/).

    [![La dirección URL del explorador de la versión preliminar de Azure Time Series Insights](media/v2-update-provision/analyze-one-portal.png)](media/v2-update-provision/analyze-one-portal.png#lightbox)

1. En el explorador, seleccione el nodo **Time Series Instances** (Instancias de serie temporal) para ver todas las instancias de la versión preliminar de Azure Time Series Insights en el entorno.

    [![Lista de instancias no primarias](media/v2-update-provision/analyze-two-unparented.png)](media/v2-update-provision/analyze-two-unparented.png#lightbox)

1. Seleccione la primera instancia de la serie temporal. A continuación, seleccione **Show pressure** (Mostrar presión).

    [![Instancia de Time Series seleccionada con el comando de menú para mostrar la presión media](media/v2-update-provision/analyze-three-show-pressure.png)](media/v2-update-provision/analyze-three-show-pressure.png#lightbox)

    Aparece un gráfico con las series temporales. Actualice el **Intervalo** a **15s**.

    [![Gráfico de serie temporal](media/v2-update-provision/analyze-four-chart.png)](media/v2-update-provision/analyze-four-chart.png#lightbox)

1. Repita el paso 3 con las otras dos instancias de series temporales. Puede ver todas las series temporales, como se muestra en este gráfico:

    [![Gráfico de todas las series temporales](media/v2-update-provision/analyze-five-chart.png)](media/v2-update-provision/analyze-five-chart.png#lightbox)

1. En el cuadro de opciones **Período de tiempo**, modifique el intervalo de tiempo para ver las tendencias de serie temporal durante la última hora:

    [![Establezca el intervalo de tiempo en una hora](media/v2-update-provision/analyze-six-time.png)](media/v2-update-provision/analyze-six-time.png#lightbox)

## <a name="define-and-apply-a-model"></a>Definir y aplicar un modelo

En esta sección, aplicará un modelo para estructurar los datos. Para completar el modelo, definirá los tipos, las jerarquías y las instancias. Para más información sobre el modelado de datos, vea [Modelo de Time Series](./time-series-insights-update-tsm.md).

1. En el explorador, seleccione la pestaña **Modelo**:

   [![Pestaña Model (Modelo) en el explorador](media/v2-update-provision/define-one-model.png)](media/v2-update-provision/define-one-model.png#lightbox)

1. Seleccione **Agregar** para agregar un tipo:

   [![El botón Add (Agregar) para tipos](media/v2-update-provision/define-two-add.png)](media/v2-update-provision/define-two-add.png#lightbox)

1. A continuación, defina tres variables para el tipo: *presión*, *temperatura* y *humedad*. En el panel **Add a Type** (Agregar un tipo), establezca los parámetros siguientes:

    | Parámetro | . |
    | --- | ---|
    | **Nombre** | Escriba **Refrigerador**. |
    | **Descripción** | Escriba **Esta es una definición de tipo de refrigerador**. |

   * Para definir la *presión*, en **Variables**, establezca los parámetros siguientes:

     | Parámetro | . |
     | --- | ---|
     | **Nombre** | Escriba **Avg Pressure** (Presión media). |
     | **Valor** | Seleccione **presión (doble)** . Es posible que el campo de **valor** tarde unos minutos en rellenarse automáticamente después de que la versión preliminar de Azure Time Series Insights comience a recibir eventos. |
     | **Operación de agregación** | Seleccione **AVG** (MEDIA). |

      [![Selecciones para definir la presión](media/v2-update-provision/define-three-variable.png)](media/v2-update-provision/define-three-variable.png#lightbox)

      Para agregar la siguiente variable, seleccione **Agregar variable**.

   * Defina la *temperatura*:

     | Parámetro | . |
     | --- | ---|
     | **Nombre** | Escriba **Avg Temperature** (Temperatura media). |
     | **Valor** | Seleccione **temperatura (doble)** . Es posible que el campo de **valor** tarde unos minutos en rellenarse automáticamente después de que la versión preliminar de Azure Time Series Insights comience a recibir eventos. |
     | **Operación de agregación** | Seleccione **AVG** (MEDIA).|

      [![Selecciones para definir la temperatura](media/v2-update-provision/define-four-avg.png)](media/v2-update-provision/define-four-avg.png#lightbox)

      Para agregar la siguiente variable, seleccione **Agregar variable**.

   * Defina la *humedad*:

      | | |
      | --- | ---|
      | **Nombre** | Escriba **Max Humidity** (Humedad máxima). |
      | **Valor** | Seleccione **humedad (doble)** . Es posible que el campo de **valor** tarde unos minutos en rellenarse automáticamente después de que la versión preliminar de Azure Time Series Insights comience a recibir eventos. |
      | **Operación de agregación** | Seleccione **MAX** (MÁX).|

      [![Selecciones para definir la temperatura](media/v2-update-provision/define-five-humidity.png)](media/v2-update-provision/define-five-humidity.png#lightbox)

    Seleccione **Crear**.

    Puede ver el tipo que agregó:

    [![Información sobre el tipo agregado](media/v2-update-provision/define-six-type.png)](media/v2-update-provision/define-six-type.png#lightbox)

1. El siguiente paso es agregar una jerarquía. En **Jerarquías**, seleccione **Agregar**:

    [![Pestaña Hierarchies (Jerarquías) con el botón Add (Agregar)](media/v2-update-provision/define-seven-hierarchy.png)](media/v2-update-provision/define-seven-hierarchy.png#lightbox)

1. En el panel **Edit Hierarchy** (Editar jerarquía), establezca los parámetros siguientes:

   | Parámetro | . |
   | --- | ---|
   | **Nombre** | Especifique el valor de **Location Hierarchy** (Jerarquía de ubicación). |
   | **Nivel 1** | Especifique el valor de **Country** (País). |
   | **Nivel 2** | Especifique el valor de **City** (Ciudad). |
   | **Nivel 3** | Especifique el valor de **Building** (Edificio). |

   Seleccione **Guardar**.

    [![Campos de jerarquía con el botón Create (Crear)](media/v2-update-provision/define-eight-add-hierarchy.png)](media/v2-update-provision/define-eight-add-hierarchy.png#lightbox)

   Puede ver la jerarquía que ha creado:

    [![Información sobre la jerarquía](media/v2-update-provision/define-nine-created.png)](media/v2-update-provision/define-nine-created.png#lightbox)

1. Seleccione **Instancias**. Seleccione la primera instancia y haga clic en **Editar**:

    [![Selección del botón Edit (Editar) para una instancia](media/v2-update-provision/define-ten-edit.png)](media/v2-update-provision/define-ten-edit.png#lightbox)

1. En el panel **Edit instances** (Editar instancias), establezca los parámetros siguientes:

    | Parámetro | . |
    | --- | --- |
    | **Tipo** | Seleccione **Refrigerador**. |
    | **Descripción** | Escriba **Instance for Chiller-01.1** (Instancia de refrigerador-01.1). |
    | **Jerarquías** | Seleccione **Location Hierarchy** (Jerarquía de ubicación). |
    | **País** | Escriba **USA** (Estados Unidos). |
    | **Ciudad** | Escriba **Seattle**. |
    | **Edificio** | Escriba **Space Needle**. |

    [![Campos de instancia con el botón Save (Guardar)](media/v2-update-provision/define-eleven-chiller.png)](media/v2-update-provision/define-eleven-chiller.png#lightbox)

   Seleccione **Guardar**.

1. Repita el paso anterior para los otros sensores. Actualice los valores siguientes:

   * Para el refrigerador 01.2:

     | Parámetro | . |
     | --- | --- |
     | **Tipo** | Seleccione **Refrigerador**. |
     | **Descripción** | Escriba **Instancia de refrigerador-01.2**. |
     | **Jerarquías** | Seleccione **Location Hierarchy** (Jerarquía de ubicación). |
     | **País** | Escriba **USA** (Estados Unidos). |
     | **Ciudad** | Escriba **Seattle**. |
     | **Edificio** | Escriba **Pacific Science Center**. |

   * Para el refrigerador 01.3:

     | Parámetro | . |
     | --- | --- |
     | **Tipo** | Seleccione **Refrigerador**. |
     | **Descripción** | Escriba **Instancia de refrigerador-01.3**. |
     | **Jerarquías** | Seleccione **Location Hierarchy** (Jerarquía de ubicación). |
     | **País** | Escriba **USA** (Estados Unidos). |
     | **Ciudad** | Escriba **New York** (Nueva York). |
     | **Edificio** | Escriba **Empire State Building**. |

1. Seleccione la pestaña **Analizar** y actualice la página. En **Location Hierarchy** (Jerarquía de ubicación), expanda todos los niveles de jerarquía para mostrar las instancias de la serie temporal:

   [![La pestaña Analyze (Analizar)](media/v2-update-provision/define-twelve.png)](media/v2-update-provision/define-twelve.png#lightbox)

1. Para explorar las instancias de la serie temporal durante la última hora, cambie **Quick Times** (Tiempos rápidos) a **Last Hour** (Última hora):

    [![El cuadro Quick Times (Tiempos rápidos), con Last Hour (Última hora) seleccionada](media/v2-update-provision/define-thirteen-explore.png)](media/v2-update-provision/define-thirteen-explore.png#lightbox)

1. En **Pacific Science Center**, seleccione la instancia de serie temporal y seleccione **Show Max Humidity** (Mostrar humedad máxima).

    [![Instancia de serie temporal seleccionada con la selección del menú Show Max Humidity (Mostrar humedad máxima)](media/v2-update-provision/define-fourteen-show-max.png)](media/v2-update-provision/define-fourteen-show-max.png#lightbox)

1. Se abre la serie temporal para **Max Humidity** (Humedad máxima) con un tamaño de intervalo de **1 minuto**. Seleccione una región para filtrar un intervalo. Para analizar los eventos durante el período de tiempo, haga clic derecho en el gráfico y seleccione **Zoom**:

   [![Intervalo seleccionado con el comando Zoom en un menú contextual](media/v2-update-provision/define-fifteen-filter.png)](media/v2-update-provision/define-fifteen-filter.png#lightbox)

1. Para ver los detalles del evento, seleccione una región y después haga clic derecho en el gráfico:

   [![Lista detallada de eventos](media/v2-update-provision/define-eighteen.png)](media/v2-update-provision/define-eighteen.png#lightbox)


## <a name="clean-up-resources"></a>Limpieza de recursos

Ahora que ha completado el tutorial, limpie los recursos que ha creado:

1. En el menú de la izquierda de [Azure Portal](https://portal.azure.com), seleccione **Todos los recursos** y busque el grupo de recursos de Azure Time Series Insights.
1. Seleccione **Eliminar** para eliminar todo el grupo de recursos (y todos los recursos que contiene), o quite los recursos de uno en uno.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:  

> [!div class="checklist"]
> * Crear y usar un acelerador de simulación de dispositivos.
> * Crear un entorno de pago por uso de la versión preliminar de Azure Time Series Insights.
> * Conectar el entorno de versión preliminar de Azure Time Series Insights a un centro de eventos.
> * Ejecutar un ejemplo del acelerador de soluciones para transmitir datos en el entorno de versión preliminar de Azure Time Series Insights.
> * Realizar un análisis básico de los datos.
> * Definir el tipo y la jerarquía de un modelo de Time Series y asociarlos con sus instancias.

Ahora que sabe cómo crear su propio entorno de versión preliminar de Azure Time Series Insights, obtenga más información sobre los conceptos clave de Azure Time Series Insights.

Lea sobre la configuración de almacenamiento de Azure Time Series Insights:

> [!div class="nextstepaction"]
> [Almacenamiento y entrada de datos en la versión preliminar de Azure Time Series Insights](./time-series-insights-update-storage-ingress.md)

Obtenga más información sobre los modelos de serie temporal:

> [!div class="nextstepaction"]
> [Modelado de datos de la versión preliminar de Azure Time Series Insights](./time-series-insights-update-tsm.md)
