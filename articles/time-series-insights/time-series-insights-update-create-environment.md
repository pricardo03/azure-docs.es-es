---
title: 'Tutorial: Configuración de un entorno de versión preliminar de Azure Time Series Insights | Microsoft Docs'
description: Obtenga información sobre cómo configurar el entorno en la versión preliminar de Azure Time Series Insights.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 04/25/2019
ms.custom: seodec18
ms.openlocfilehash: f83063a88207f51f9d481447923fd8a8498692a2
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64713916"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Tutorial: Configuración de un entorno en la versión preliminar de Azure Time Series Insights

Este tutorial le guía en el proceso de crear un entorno de pago por uso (PAYG) de la versión preliminar de Azure Time Series Insights. En este tutorial, aprenderá a:

* Crear un entorno en la versión preliminar de Azure Time Series Insights.
* Conectar el entorno de versión preliminar de Azure Time Series Insights a un centro de eventos en Azure Event Hubs.
* Ejecutar un ejemplo del acelerador de soluciones para transmitir datos en el entorno de versión preliminar de Azure Time Series Insights.
* Realizar un análisis básico de los datos.
* Definir el tipo y la jerarquía de un modelo de Time Series y asociarlos con sus instancias.

# <a name="create-a-device-simulation"></a>Creación de una simulación de dispositivo

En esta sección, creará tres dispositivos simulados que enviarán datos a un centro de IoT.

1. Vaya a la [página de aceleradores de soluciones de Azure IoT](https://www.azureiotsolutions.com/Accelerators). En la página se muestran varios ejemplos creados previamente. Inicie sesión con la cuenta de Azure. A continuación, seleccione **Simulación de dispositivo**.

   [![Página de aceleradores de soluciones de Azure IoT](media/v2-update-provision/device-one-accelerator.png)](media/v2-update-provision/device-one-accelerator.png#lightbox)

   Seleccione **Try Now** (Intentar ahora).

1. Escriba los parámetros necesarios en la página **Crear la solución Simulación de dispositivo**:

   | Parámetro | DESCRIPCIÓN |
   | --- | --- |
   | **Nombre de la solución** |    Escriba un valor único para la creación de un grupo de recursos. Los recursos de Azure enumerados se crean y se asigna al grupo de recursos. |
   | **Suscripción** | Especifique la misma suscripción usada para la creación del entorno de Time Series Insights. |
   | **Región** |   Especifique la misma región usada para la creación del entorno de Time Series Insights. |
   | **Implementación de recursos opcionales de Azure**    | Deje seleccionado IoT Hub, dado que los dispositivos simulados lo usarán para conectarse y transmitir datos. |

   A continuación, seleccione **Create solution** (Crear solución). Espere de 10 a 15 minutos a que se implemente su solución.

   [![Página de creación de la solución de simulación de dispositivos](media/v2-update-provision/device-two-create.png)](media/v2-update-provision/device-two-create.png#lightbox)

1. En el panel del acelerador de soluciones, seleccione el botón **Launch** (Iniciar):

   [![Inicio de la solución de simulación de dispositivo](media/v2-update-provision/device-three-launch.png)](media/v2-update-provision/device-three-launch.png#lightbox)

1. Se le redirigirá a la página de **simulación de dispositivos IoT de Microsoft Azure**. Seleccione **+ New simulation** (+Nueva simulación) en la esquina superior derecha.

   [![Página de simulación de Azure IoT](media/v2-update-provision/device-four-iot-sim-page.png)](media/v2-update-provision/device-four-iot-sim-page.png#lightbox)

1. Rellene los parámetros necesarios como sigue:

    [![Parámetros para rellenar](media/v2-update-provision/device-five-params.png)](media/v2-update-provision/device-five-params.png#lightbox)

    |||
    | --- | --- |
    | **Nombre** | Escriba un nombre único para un simulador. |
    | **Descripción** | Escriba una definición. |
    | **Simulation duration** (Duración de la simulación) | Establézcala en **Run indefinitely** (Ejecutar indefinidamente). |
    | **Device model** (Modelo de dispositivo) | **Nombre**: Escriba `Chiller`. </br>**Amount** (Cantidad): Escriba `3`. |
    | **Target IoT Hub** (IoT Hub de destino) | Establézcalo en **Use pre-provisioned IoT Hub** (Usar IoT Hub aprovisionado previamente). |

    A continuación, seleccione **Start simulation** (Iniciar simulación).

1. En el panel de simulación de dispositivos, vea **Active devices** (Dispositivos activos) y **Messages per second** (Mensajes por segundo).

    [![Panel de simulación de Azure IoT](media/v2-update-provision/device-seven-dashboard.png)](media/v2-update-provision/device-seven-dashboard.png#lightbox)

## <a name="list-device-simulation-properties"></a>Enumeración de propiedades de simulación de dispositivos

Antes de crear un entorno de Azure Time Series Insights, necesitará los nombres del centro de IoT Hub, la suscripción y el grupo de recursos.

1. Vaya al panel del acelerador de soluciones e inicie sesión con la misma cuenta de la suscripción de Azure. Encuentre la simulación del dispositivo que creó en los pasos anteriores.

1. Seleccione el simulador de dispositivos y elija **Launch** (Iniciar). Seleccione el vínculo **Azure Management Portal** (Portal de administración de Azure) en el lado derecho.

    [![Anuncios del simulador](media/v2-update-provision/device-six-listings.png)](media/v2-update-provision/device-six-listings.png#lightbox)

1. Anote los nombres del centro de IoT Hub, la suscripción y el grupo de recursos.

    [![Azure Portal](media/v2-update-provision/device-eight-portal.png)](media/v2-update-provision/device-eight-portal.png#lightbox)

## <a name="create-a-time-series-insights-preview-payg-environment"></a>Creación de un entorno de pago por uso en la versión preliminar de Time Series Insights

En esta sección se describe cómo crear un entorno en la versión preliminar de Azure Time Series Insights con [Azure Portal](https://portal.azure.com/).

1. Inicie sesión en Azure Portal con la cuenta de su suscripción.

1. Seleccione **Crear un recurso**.

1. Seleccione la categoría **Internet de las cosas** y, después, elija **Time Series Insights**.

   [![Seleccionar Internet de las cosas y, luego, Time Series Insights](media/v2-update-provision/payg-one-azure.png)](media/v2-update-provision/payg-one-azure.png#lightbox)

1. Rellene los campos de la página de la manera siguiente:

   | | |
   | --- | ---|
   | **Nombre del entorno** | Elija un nombre único para el entorno de la versión preliminar de Azure Time Series Insights. |
   | **Suscripción** | Especifique la suscripción donde desea crear el entorno de versión preliminar de Azure Time Series Insights. Un procedimiento recomendado es usar la misma suscripción que el resto de los recursos de IoT creados por el simulador de dispositivos. |
   | **Grupos de recursos** | Un grupo de recursos es un contenedor para recursos de Azure. Elija un grupo de recursos existente o cree uno, para el recurso de entorno de versión preliminar de Azure Time Series Insights. Un procedimiento recomendado es usar el mismo grupo de recursos que el resto de los recursos de IoT creados por el simulador de dispositivos. |
   | **Ubicación** | Elija una región del centro de datos para su entorno de versión preliminar de Azure Time Series Insights. Para evitar costos de ancho de banda y una latencia agregados, es mejor mantener el entorno de la versión preliminar de Azure Time Series Insights en la misma región que otros recursos de IoT. |
   | **Nivel** |  Seleccione **PAYG**, que significa pago por uso. Esta es la SKU del producto de versión preliminar de Azure Time Series Insights. |
   | **Id. de propiedad** | Especifique algo que identifique de forma única su serie temporal. Tenga en cuenta que este campo es inmutable y no se puede cambiar más adelante. Para este tutorial usaremos `iothub-connection-device-id`. Para más información sobre el identificador de Time Series, lea [Elección de un identificador de Time Series](./time-series-insights-update-how-to-id.md). |
   | **Nombre de cuenta de almacenamiento** | Escriba un nombre único global para la nueva cuenta de almacenamiento que se va a crear. |

   Después, seleccione **Next (Siguiente): Origen de eventos**.

   [![Página de creación de un entorno de Time Series Insights](media/v2-update-provision/payg-two-create.png)](media/v2-update-provision/payg-two-create.png#lightbox)

1. En la página del origen del evento, rellene los campos de la manera siguiente:

   | | |
   | --- | --- |
   | **Create an event source?** ¿Crear un origen del evento? | Escriba `Yes`.|
   | **Nombre** | Escriba un valor único que se usa para denominar el origen del evento.|
   | **Tipo de origen** | Seleccione **IoT Hub**. |
   | **Select a hub?** (Selección de un centro) | Elija **Seleccionar existente**. |
   | **Suscripción** | Seleccione la suscripción que usó para el simulador de dispositivos. |
   | **Nombre de la instancia de IoT Hub** | Seleccione el nombre del centro de IoT que creó para el simulador de dispositivos. |
   | **Directiva de acceso de IoT Hub** | seleccione **iothubowner**. |
   | **Grupo de consumidores de IoT Hub** | Necesita un grupo de consumidores exclusivo para la versión preliminar de Azure Time Series Insights. Seleccione **New** (Nuevo), escriba un nombre único y, a continuación, seleccione **Add** (Agregar). |
   | **Propiedad de marca de tiempo** | Este campo se utiliza para identificar la propiedad de marca de tiempo en los datos de telemetría entrantes. En este tutorial, no rellene el campo. Este simulador usa la marca de tiempo entrante de IoT Hub, que Time Series Insights tiene como valor predeterminado.|

   Seleccione **Review + create** (Revisar + crear).

   [![Configuración de un origen del evento](media/v2-update-provision/payg-five-event-source.png)](media/v2-update-provision/payg-five-event-source.png#lightbox)

1. Revise todos los campos de la página de revisión y seleccione **Create** (Crear).

   [![Página Review + Create (Revisar + crear), con el botón Create (Crear)](media/v2-update-provision/payg-six-review.png)](media/v2-update-provision/payg-six-review.png#lightbox)

1. Puede ver el estado de la implementación.

   [![Notificación de que la implementación ha finalizado](media/v2-update-provision/payg-seven-deploy.png)](media/v2-update-provision/payg-seven-deploy.png#lightbox)

1. Recibirá acceso a su entorno de versión preliminar de Azure Time Series Insights si es propietario del inquilino. Para asegurarse de que tiene acceso:

    a. Busque el grupo de recursos y seleccione el entorno de versión preliminar de Azure Time Series Insights:

      [![Entorno seleccionado](media/v2-update-provision/payg-eight-environment.png)](media/v2-update-provision/payg-eight-environment.png#lightbox)

   b. En la página de la versión preliminar de Azure Time Series Insights, vaya a **Data Access Policies** (Directivas de acceso de datos).

     [![Directivas de acceso a datos](media/v2-update-provision/payg-nine-data-access.png)](media/v2-update-provision/payg-nine-data-access.png#lightbox)

   c. Compruebe que aparecen las credenciales.

     [![Credenciales mostradas](media/v2-update-provision/payg-ten-verify.png)](media/v2-update-provision/payg-ten-verify.png#lightbox)

   Si no se muestran sus credenciales, deberá darse permiso para acceder al entorno. Para más información sobre cómo establecer permisos, lea [Concesión de acceso a datos](./time-series-insights-data-access.md).

## <a name="analyze-data-in-your-environment"></a>Análisis de datos en el entorno

En esta sección, se va realizar un análisis básico de los datos de la serie temporal mediante el [explorador de la versión preliminar de Azure Time Series Insights](./time-series-insights-update-explorer.md).

1. Vaya al explorador de la versión preliminar de Azure Time Series Insights; para ello, seleccione la dirección URL de la página de recursos de [Azure Portal](https://portal.azure.com/).

   [![La dirección URL del explorador de la versión preliminar de Azure Time Series Insights](media/v2-update-provision/analyze-one-portal.png)](media/v2-update-provision/analyze-one-portal.png#lightbox)

1. En el explorador, seleccione el nodo **Time Series Instances** (Instancias de serie temporal) para ver todas las instancias de la versión preliminar de Azure Time Series Insights en el entorno.

   [![Lista de instancias no primarias](media/v2-update-provision/analyze-two-unparented.png)](media/v2-update-provision/analyze-two-unparented.png#lightbox)

1. En la serie temporal que se muestra, seleccione la primera instancia. A continuación, seleccione **Show Avg pressure** (Mostrar presión media).

   [![Instancia seleccionada con el comando de menú para mostrar la presión media](media/v2-update-provision/analyze-three-show-pressure.png)](media/v2-update-provision/analyze-three-show-pressure.png#lightbox)

   Debe aparecer un gráfico de serie temporal a la derecha. Ajuste el **Intervalo** en `15s`.

   [![Gráfico de serie temporal](media/v2-update-provision/analyze-four-chart.png)](media/v2-update-provision/analyze-four-chart.png#lightbox)

1. Repita el **paso 3** con las otras dos series temporales. Luego, podrá ver todas las series temporales, como se muestra en este gráfico:

   [![Gráfico de todas las series temporales](media/v2-update-provision/analyze-five-chart.png)](media/v2-update-provision/analyze-five-chart.png#lightbox)

1. Modifique el intervalo de tiempo para ver las tendencias de serie temporal durante la última hora.

    a. Seleccione el cuadro de opción **Período de tiempo**:

      [![Establezca el intervalo de tiempo en una hora](media/v2-update-provision/analyze-six-time.png)](media/v2-update-provision/analyze-six-time.png#lightbox)

## <a name="define-and-apply-a-model"></a>Definir y aplicar un modelo

En esta sección, aplicará un modelo para estructurar los datos. Para completar el modelo, definirá los tipos, las jerarquías y las instancias. Para más información sobre el modelado de datos, vaya a [Modelo de Time Series](./time-series-insights-update-tsm.md).

1. En el explorador, seleccione la pestaña **Modelo**:

   [![Pestaña Model (Modelo) en el explorador](media/v2-update-provision/define-one-model.png)](media/v2-update-provision/define-one-model.png#lightbox)

1. Seleccione **+ Add** (+Agregar) para agregar un tipo. En el lado derecho, se abre un editor de tipos.

   [![El botón Add (Agregar) para tipos](media/v2-update-provision/define-two-add.png)](media/v2-update-provision/define-two-add.png#lightbox)

1. Defina tres variables para el tipo: presión, temperatura y humedad. Escriba la siguiente información:

   | | |
   | --- | ---|
   | **Nombre** | Escriba `Chiller`. |
   | **Descripción** | Escriba `This is a type definition of Chiller`. |

   * Defina la presión con tres variables:

      | | |
      | --- | ---|
      | **Nombre** | Escriba `Avg Pressure`. |
      | **Valor** | Seleccione **presión (doble)** . Tenga en cuenta que este campo puede tardar unos minutos en rellenarse después de que la versión preliminar de Azure Time Series Insights comienza a recibir eventos. |
      | **Operación de agregación** | Seleccione **AVG** (MEDIA). |

      [![Selecciones para definir la presión](media/v2-update-provision/define-three-variable.png)](media/v2-update-provision/define-three-variable.png#lightbox)

      Seleccione **+ Add Variable** (+Agregar variable) para agregar la siguiente variable.

   * Defina la temperatura:

      | | |
      | --- | ---|
      | **Nombre** | Escriba `Avg Temperature`. |
      | **Valor** | Seleccione **temperatura (doble)** . Tenga en cuenta que este campo puede tardar unos minutos en rellenarse después de que la versión preliminar de Azure Time Series Insights comienza a recibir eventos. |
      | **Operación de agregación** | Seleccione **AVG** (MEDIA).|

      [![Selecciones para definir la temperatura](media/v2-update-provision/define-four-avg.png)](media/v2-update-provision/define-four-avg.png#lightbox)

   * Defina la humedad:

      | | |
      | --- | ---|
      | **Nombre** | Escriba `Max Humidity`. |
      | **Valor** | Seleccione **humedad (doble)** . Tenga en cuenta que este campo puede tardar unos minutos en rellenarse después de que la versión preliminar de Azure Time Series Insights comienza a recibir eventos. |
      | **Operación de agregación** | Seleccione **MAX** (MÁX).|

      [![Selecciones para definir la temperatura](media/v2-update-provision/define-five-humidity.png)](media/v2-update-provision/define-five-humidity.png#lightbox)

   Seleccione **Crear**.

1. Puede ver el tipo agregado:

   [![Información sobre el tipo agregado](media/v2-update-provision/define-six-type.png)](media/v2-update-provision/define-six-type.png#lightbox)

1. El siguiente paso es agregar una jerarquía. En la sección **Hierarchies** (Jerarquías), seleccione **+ Add** (+Agregar):

   [![Pestaña Hierarchies (Jerarquías) con el botón Add (Agregar)](media/v2-update-provision/define-seven-hierarchy.png)](media/v2-update-provision/define-seven-hierarchy.png#lightbox)

1. Defina la jerarquía. Rellene los campos de la manera siguiente:

   | | |
   | --- | ---|
   | **Nombre** | Escriba `Location Hierarchy`. |
   | **Nivel 1** | Escriba `Country`. |
   | **Nivel 2** | Escriba `City`. |
   | **Nivel 3** | Escriba `Building`. |

   Seleccione **Crear**.

   [![Campos de jerarquía con el botón Create (Crear)](media/v2-update-provision/define-eight-add-hierarchy.png)](media/v2-update-provision/define-eight-add-hierarchy.png#lightbox)

1. Puede ver la jerarquía que ha creado:

   [![Información sobre la jerarquía](media/v2-update-provision/define-nine-created.png)](media/v2-update-provision/define-nine-created.png#lightbox)

1. Seleccione **Instances** (Instancias) a la izquierda. Cuando aparezcan las instancias, seleccione la primera de ellas y haga clic en **Edit** (Editar):

   [![Selección del botón Edit (Editar) para una instancia](media/v2-update-provision/define-ten-edit.png)](media/v2-update-provision/define-ten-edit.png#lightbox)

1. En el lado derecho se abre un editor de texto. Agregue la siguiente información:

   | | |
   | --- | --- |
   | **Tipo** | Seleccione **Refrigerador**. |
   | **Descripción** | Escriba `Instance for Chiller-01.1`. |
   | **Jerarquías** | Seleccione **Location Hierarchy** (Jerarquía de ubicación). |
   | **País** | Escriba `USA`. |
   | **Ciudad** | Escriba `Seattle`. |
   | **Edificio** | Escriba `Space Needle`. |

    Después, seleccione **Guardar**.

   [![Campos de instancia con el botón Save (Guardar)](media/v2-update-provision/define-eleven-chiller.png)](media/v2-update-provision/define-eleven-chiller.png#lightbox)

1. Repita el paso anterior para los otros sensores. Use los siguientes campos:

   * Para el refrigerador 01.2:

     | | |
     | --- | --- |
     | **Tipo** | Seleccione **Refrigerador**. |
     | **Descripción** | Escriba `Instance for Chiller-01.2`. |
     | **Jerarquías** | Seleccione **Location Hierarchy** (Jerarquía de ubicación). |
     | **País** | Escriba `USA`. |
     | **Ciudad** | Escriba `Seattle`. |
     | **Edificio** | Escriba `Pacific Science Center`. |

   * Para el refrigerador 01.3:

     | | |
     | --- | --- |
     | **Tipo** | Seleccione **Refrigerador**. |
     | **Descripción** | Escriba `Instance for Chiller-01.3`. |
     | **Jerarquías** | Seleccione **Location Hierarchy** (Jerarquía de ubicación). |
     | **País** | Escriba `USA`. |
     | **Ciudad** | Escriba `New York`. |
     | **Edificio** | Escriba `Empire State Building`. |

1. Vaya a la pestaña **Analyze** (Analizar) y actualice la página. Expanda todos los niveles de jerarquía para buscar la serie temporal.

   [![La pestaña Analyze (Analizar)](media/v2-update-provision/define-twelve.png)](media/v2-update-provision/define-twelve.png#lightbox)

1. Para explorar la serie temporal durante la última hora, cambie **Quick Times** (Tiempos rápidos) a **Last Hour** (Última hora):

    [![El cuadro Quick Times (Tiempos rápidos), con Last Hour (Última hora) seleccionada](media/v2-update-provision/define-thirteen-explore.png)](media/v2-update-provision/define-thirteen-explore.png#lightbox)

1. Seleccione la serie temporal que aparece en **Pacific Science Center** y seleccione **Show Max Humidity** (Mostrar humedad máxima).

    [![Serie temporal seleccionada con la selección del menú Show Max Humidity (Mostrar humedad máxima)](media/v2-update-provision/define-fourteen-show-max.png)](media/v2-update-provision/define-fourteen-show-max.png#lightbox)

1. Se abre la serie temporal para **Max Humidity** (Humedad máxima) con un tamaño de intervalo de **1 minuto**. Seleccione una región para filtrar un intervalo. A continuación, haga clic con el botón derecho y seleccione **Zoom** para analizar los eventos durante el plazo de tiempo:

   [![Intervalo seleccionado con el comando Zoom en un menú contextual](media/v2-update-provision/define-fifteen-filter.png)](media/v2-update-provision/define-fifteen-filter.png#lightbox)

1. También puede seleccionar una región y después hacer clic con el botón derecho para ver los detalles del evento:

   [![Lista detallada de eventos](media/v2-update-provision/define-eighteen.png)](media/v2-update-provision/define-eighteen.png#lightbox)

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