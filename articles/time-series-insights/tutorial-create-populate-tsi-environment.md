---
title: 'Tutorial: Creación de un entorno de Azure Time Series Insights | Microsoft Docs'
description: Aprenda a crear un entorno de Time Series Insights que se rellena con datos de dispositivos simulados.
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 04/26/2019
ms.author: dpalled
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: b8b46db043113f29f559ad44855d19f0d6ca73c3
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244157"
---
# <a name="tutorial-create-an-azure-time-series-insights-environment"></a>Tutorial: Creación de un entorno de Azure Time Series Insights

Este tutorial le guía a través del proceso de creación de un entorno de Azure Time Series Insights que se rellena con datos de dispositivos simulados. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Cree el entorno de Time Series Insights.
> * Crear una solución de simulación de dispositivos que contenga un centro de IoT.
> * Conectar el entorno de Time Series Insights al centro de IoT.
> * Ejecutar de una simulación de dispositivo para transmitir en secuencias datos al entorno de Time Series Insights.
> * Comprobar los datos de telemetría simulados.

## <a name="video"></a>Vídeo

### <a name="learn-how-to-use-an-azure-iot-solution-accelerator-to-generate-data-and-get-started-with-time-series-insights-br"></a>Aprenda a usar un acelerador de soluciones de Azure IoT para generar datos y empiece a trabajar con Time Series Insights. </br>

> [!VIDEO https://www.youtube.com/embed/6ehNf6AJkFo]

## <a name="prerequisites"></a>Requisitos previos

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/).
* La cuenta de inicio de sesión de Azure también debe ser miembro del rol **Propietario** de la suscripción. Para más información, consulte [Administración del acceso a los recursos de Azure mediante RBAC y Azure Portal](/azure/role-based-access-control/role-assignments-portal).

## <a name="overview"></a>Información general

El entorno de Time Series Insights es donde se recopilan y almacenan los datos del dispositivo. Una vez que los datos se han almacenados, el [explorador de Azure Time Series Insights](time-series-quickstart.md) y la [Query API de Time Series Insights](/rest/api/time-series-insights/ga-query-api) se pueden usar para consultar y analizar los datos. Azure IoT Hub es el punto de conexión que utilizan todos los dispositivos (simulados o físicos) para conectarse a la nube de Azure y transmitir datos de forma segura a ella. En [Introducción a Time Series Insights](time-series-insights-overview.md), se explica que Azure IoT Hub también sirve como origen del evento para la transmisión en secuencias de datos a un entorno de Time Series Insights. En este tutorial se utiliza un [acelerador de soluciones de IoT](/azure/iot-accelerators/) para generar y transmitir datos de telemetría de ejemplo a IoT Hub.

>[!TIP]
> Los aceleradores de soluciones de IoT proporcionan soluciones preconfiguradas de nivel empresarial que se pueden usar puede usar para acelerar el desarrollo de soluciones de IoT personalizadas.

## <a name="create-an-environment"></a>Creación de un entorno

En primer lugar, cree un entorno de Time Series Insights en la suscripción de Azure.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con su cuenta de suscripción de Azure. 
1. Seleccione **+ Crear un recurso** en la esquina superior izquierda. 
1. Seleccione la categoría **Internet de las cosas** y, después, elija **Time Series Insights**. 

   [![Selección del recurso de entorno de Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png#lightbox)

1. En la página **Entorno de Time Series Insights**, rellene los parámetros necesarios.

   Parámetro|DESCRIPCIÓN
   ---|---
   **Nombre del entorno** | Elija un nombre único para el entorno de Time Series Insights. Los nombres los utilizan el explorador de Time Series Insights y las Query API.
   **Suscripción** | Las suscripciones son contenedores para recursos de Azure. Elija una suscripción para crear el entorno de Time Series Insights.
   **Grupos de recursos** | Un grupo de recursos es un contenedor para recursos de Azure. Elija un grupo de recursos existente o cree uno para el recurso de entorno de Azure Time Series Insights.
   **Ubicación** | Elija una región del centro de datos para su entorno de Time Series Insights. Para no aumentar los costos de ancho de banda y la latencia, mantenga el entorno de Azure Time Series Insights en la misma región que los restantes recursos de IoT.
   **SKU de precios** | Elija el rendimiento requerido. Para que los costos y capacidad de inicio sean menores, seleccione `S1`.
   **Capacity** | La capacidad es el multiplicador que se aplica a la tasa de entrada, la capacidad de almacenamiento y el costo asociado con la SKU seleccionada. Puede cambiar la capacidad después de la creación. Para que los costos sean menores, seleccione una capacidad de 1.

   Cuando finalice, seleccione **Crear** para comenzar el proceso de aprovisionamiento.

   [![Creación del recurso de entorno de Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png#lightbox)

1. Compruebe el panel **Notificaciones** para supervisar la finalización de la implementación. 

   [![Implementación correcta del entorno de Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png#lightbox)

## <a name="create-a-device-simulation"></a>Creación de una simulación de dispositivo

A continuación, cree la solución de simulación de dispositivo, que genera datos de prueba para rellenar el entorno de Time Series Insights.

1. En una ventana o pestaña independientes, vaya a [azureiotsolutions.com](https://www.azureiotsolutions.com). Inicie sesión con la misma cuenta de suscripción de Azure y seleccione el acelerador **Device Simulation** (Simulación de dispositivos).

   [![Ejecución del acelerador Simulación de dispositivos](media/tutorial-create-populate-tsi-environment/sa-main.png)](media/tutorial-create-populate-tsi-environment/sa-main.png#lightbox)

1. Escriba los parámetros necesarios en la página **Crear la solución Simulación de dispositivos**.

   Parámetro|DESCRIPCIÓN
   ---|---
   **Nombre de la solución** | Este valor único se usa para crear un grupo de recursos. Los recursos de Azure enumerados se crean y se asigna al grupo de recursos.
   **Suscripción** | Especifique la misma suscripción que se usó para crear el entorno de Time Series Insights en la sección anterior.
   **Región** | Especifique la misma región que se usó para crear el entorno de Time Series Insights en la sección anterior.
   **Implementación de recursos opcionales de Azure** | Deje la opción **IoT Hub** activada. Los dispositivos simulados la usan para conectarse y transmitir datos en secuencias.

   Cuando termine, haga clic en **Crear solución** para aprovisionar los recursos de Azure de la solución. Este proceso puede tardar entre 6 y 7 minutos en completarse.

   [![Aprovisionamiento de la solución de simulación de dispositivos](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png#lightbox)

1. Tras finalizar el aprovisionamiento, el texto que hay encima de la nueva solución cambia de **Aprovisionando** a **Listo**.

   >[!IMPORTANT]
   > No seleccione **Iniciar** aún. Mantenga abierta esta página web, ya que volverá a ella más adelante.

   [![Aprovisionamiento de la solución Simulación de dispositivos completada](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png#lightbox)

1. Ahora, vuelva a Azure Portal e inspeccione los recursos recién creados en la suscripción. En la página **Grupos de recursos** del portal, se ve que se ha creado un nuevo grupo de recursos, para lo que se ha usado el **nombre de solución** proporcionado en el último paso. Fíjese también en todos los recursos creados para dar soporte a la solución de simulación de dispositivos.

   [![Recursos de la solución de simulación de dispositivos](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png)](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png#lightbox)

## <a name="connect-the-environment-to-the-iot-hub"></a>Conexión del entorno de TSI al centro de IoT

En este momento, ha aprendido cómo crear dos conjuntos de recursos, cada uno en su propio grupo de recursos:

- Un entorno de Time Series Insights vacío.
- Recursos de la solución de simulación de dispositivos, entre los que se incluye una instancia de IoT Hub, generados por un acelerador de soluciones.

Recuerde que los dispositivos simulados necesitan conectarse a una instancia de IoT Hub para transmitir datos de dispositivo. Para que los datos fluyan en el entorno de Time Series Insights, es preciso realizar cambios de configuración tanto en IoT Hub como en el entorno de Time Series Insights.

### <a name="iot-hub-configuration-define-a-consumer-group"></a>Configuración de IoT Hub: definición de un grupo de consumidores

IoT Hub proporciona varios puntos de conexión que permiten compartir la funcionalidad con otros actores. El punto de conexión "Eventos" proporciona una forma para que otras aplicaciones consuman datos, ya que se transmite en secuencias a una instancia de IoT Hub. En concreto, los "grupos de consumidores" proporcionan un mecanismo para que las aplicaciones escuchen y extraigan datos de IoT Hub.

A continuación, defina una nueva propiedad **grupo de consumidores** en el **Punto de conexión de eventos** de IoT Hub de la solución de simulación de dispositivos.

1. En Azure Portal, vaya a la página **Información general** del grupo de recursos que ha creado para la solución de simulación de dispositivos. Seleccione el recurso de IoT Hub.

   [![Grupo de recursos de la solución Simulación de dispositivo](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png#lightbox)

   Tome nota del **nombre** del recurso de IoT Hub generado para la solución, ya que hará referencia a él más adelante.

1. Desplácese hacia abajo, seleccione la página **Puntos de conexión**; después y seleccione el punto de conexión **Eventos**. En la página **Propiedades** del punto de conexión, escriba un nombre único para el punto de conexión en el grupo de consumidores "$Default". Seleccione **Guardar**.

   [![Puntos de conexión de IoT Hub de la solución de simulación de dispositivos](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png#lightbox)

### <a name="environment-configuration-define-an-event-source"></a>Configuración del entorno: definición de un origen del evento

Ahora, conecte el nuevo punto de conexión de eventos **grupo de consumidores** de IoT Hub al entorno de Time Series Insights, como un **origen del evento**.

1. Vaya a la página **Información general** del grupo de recursos que ha creado para el entorno de Time Series Insights. Seleccione el entorno de Time Series Insights.

   [![Entorno y grupo de recursos del entorno de Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png#lightbox)

1. En la página del entorno de Time Series Insights, seleccione **Orígenes de eventos**. Seguidamente, seleccione **+ Agregar**.

   [![Información general del entorno de Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png#lightbox)

1. Escriba los parámetros necesarios en la página **Nuevo origen de eventos**.

   Parámetro|DESCRIPCIÓN
   ---|---
   **Nombre de origen de evento** | Requiere un valor único, que se utiliza para denominar el origen del evento.
   **Origen** | Seleccione **IoT Hub**.
   **Opción de importación** | Seleccione la opción predeterminada `Use IoT hub from available subscriptions`. Esta opción hace que la siguiente lista desplegable se llene con las suscripciones disponibles.
   **Suscripción** | Seleccione la misma suscripción en la que creó los recursos de Simulación de dispositivo y de entorno de Time Series Insights.
   **Nombre de la instancia de IoT Hub** | De forma predeterminada, debe ser el nombre de la instancia de IoT Hub que ha anotado anteriormente. Si no es así, seleccione la instancia de IoT Hub correcta.
   **Nombre de la directiva del IoT Hub** | seleccione **iothubowner**.
   **Grupo de consumidores de IoT Hub** | De forma predeterminada, debe ser el nombre del grupo de consumidores de IoT Hub que ha creado anteriormente. En caso contrario, seleccione el nombre del grupo de consumidor correcto.
   **Formato de serialización de eventos** | Deje el valor predeterminado de `JSON`.
   **Nombre de propiedad de marca de tiempo** | Especifique `timestamp`.

   Cuando termine, seleccione **Crear** para agregar el origen del evento. Cuando regrese a la página **Información general** del grupo de recursos, junto con su recurso de entorno de Time Series Insights, verá un nuevo recurso "Origen del evento de Time Series Insights".

   [![Nuevo origen de evento del entorno de Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png#lightbox)

## <a name="run-device-simulation-to-stream-data"></a>Ejecución de una simulación de dispositivo para transmitir datos

Ahora que ha finalizado todo el trabajo de configuración, es el momento de rellenar el entorno de Time Series Insights con datos de ejemplo de los dispositivos simulados.

Puede que recuerde que en la [sección Creación de una simulación de dispositivo](#create-a-device-simulation) el acelerador creó varios recursos de Azure para dar soporte a la solución. Junto con la instancia de IoT Hub tratada anteriormente, se ha generado una aplicación web de Azure App Service para crear y transmitir telemetría de los dispositivos simulados.

1. Vuelva al [panel de aceleradores de soluciones](https://www.azureiotsolutions.com/Accelerators#dashboard). Vuelva a iniciar sesión si fuera necesario, y use la misma cuenta de Azure que ha usado en este tutorial. Ahora puede seleccionar **Iniciar** en la solución "Simulación de dispositivos".

     [![Panel de aceleradores de soluciones](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png#lightbox)

1. La aplicación web de simulación de dispositivo se inicia en este punto, y puede tardar varios segundos con la carga inicial. También se le pide su consentimiento para conceder a la aplicación web el permiso "Iniciar sesión y leer su perfil". Este permiso permite a la aplicación recuperar la información de perfil de usuario necesaria para admitir el funcionamiento de la aplicación.

     [![Consentimiento de la aplicación web de simulación de dispositivos](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png)](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png#lightbox)

1. Después de que se cargue la página **Configuración de simulación**, escriba los parámetros necesarios.

   Parámetro|DESCRIPCIÓN
   ---|---
   **Target IoT Hub** (IoT Hub de destino) | Seleccione **Use pre-provisioned IoT Hub** (Usar IoT Hub aprovisionado previamente).
   **Device model** (Modelo de dispositivo) | Seleccione **Refrigerador**.
   **Number of devices** (Número de dispositivos)  | Escriba `1000` en **Amount** (Cantidad).
   **Telemetry frequency** (Frecuencia de telemetría) | Escriba `10` segundos.
   **Simulation duration** (Duración de la simulación) | Seleccione **End in:** (Termina en) y escriba `5` minutos.

   Cuando haya terminado, seleccione **Iniciar simulación**. La simulación se ejecuta durante 5 minutos. Genera datos de 1000 dispositivos simulados cada 10 segundos. 

   [![Configuración de la simulación de dispositivos](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png#lightbox)

1. Mientras se ejecuta la simulación, se ve que los campos **Total messages** (Total de mensajes) y **Messages per second** (Mensajes por segundo) se actualizan, aproximadamente, cada 10 segundos. La simulación finaliza aproximadamente a los 5 minutos y vuelve a **Configuración de simulación**.

   [![Ejecución de la simulación de dispositivos](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png#lightbox)

## <a name="verify-the-telemetry-data"></a>Comprobación de los datos de telemetría

En esta sección final, va a comprobar que los datos de telemetría se han generado y se han almacenado en el entorno de Time Series Insights. Para comprobar los datos, se utiliza el explorador de Time Series Insights, que se utiliza para consultar y analizar datos de telemetría.

1. Vuelva a la página **Información general** del grupo de recursos del entorno de Time Series Insights. Seleccione el entorno de Time Series Insights.

   [![Entorno y grupo de recursos del entorno de Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png#lightbox)

1. En la página **Información general** del entorno de Time Series Insights, seleccione la **dirección URL del explorador de Time Series Insights** para abrir el explorador de Time Series Insights.

   [![Explorador de Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png#lightbox)

1. El explorador de Time Series Insights se carga y se autentica mediante su cuenta de Azure Portal. En la vista inicial, se puede ver en el área del gráfico que el entorno de Time Series Insights estaba relleno con datos de telemetría simulados. Para filtrar por un rango de tiempo más reducido, seleccione el menú desplegable en la esquina superior izquierda. Escriba un rango de tiempo lo suficientemente amplio como para abarcar la duración de la simulación del dispositivo. A continuación, seleccione la lupa de buscar.

   [![Filtro de intervalo de tiempo del explorador de Time Series Insights](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png#lightbox)

1. La reducción del rango de tiempo permite al gráfico acercarse a las distintivas ráfagas de transferencia de datos, a la instancia de IoT Hub y al entorno de Time Series Insights. Observe también el texto **Se ha completado la transmisión** en la esquina superior derecha, que muestra el número total de eventos encontrados. También puede arrastrar el control deslizante **Tamaño de intervalo** para controlar la granularidad del gráfico.

   [![Vista filtrada del intervalo de tiempo del explorador de Time Series Insights](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png#lightbox)

1. Por último, también puede hacer clic en una región para filtrar un rango. Después, haga clic con el botón derecho y utilice **Explorar eventos** para mostrar los detalles del evento en la vista tabular **Eventos**.

   [![Vista filtrada y eventos del intervalo de tiempo del explorador de Time Series Insights](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png#lightbox)

## <a name="clean-up-resources"></a>Limpieza de recursos

En este tutorial se crean varios servicios de Azure activos para dar soporte tanto al entorno de Time Series Insights como a la solución de simulación de dispositivos. Si desea abandonar o posponer esta serie de tutoriales, elimine todos los recursos para no incurrir en costos innecesarios.

En el menú de la izquierda de Azure Portal:

1. Seleccione el icono **Grupos de recursos**. Luego, seleccione el grupo de recursos que creó para el entorno de Time Series Insights. En la parte superior de la página, seleccione **Eliminar grupo de recursos**, escriba el nombre del grupo de recursos y seleccione **Eliminar**.

1. Seleccione el icono **Grupos de recursos**. Luego, seleccione el grupo de recursos que creó el acelerador de soluciones de simulación de dispositivos. En la parte superior de la página, seleccione **Eliminar grupo de recursos**, escriba el nombre del grupo de recursos y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Cree el entorno de Time Series Insights.
> * Crear una solución de simulación de dispositivos que contenga un centro de IoT.
> * Conectar el entorno de Time Series Insights al centro de IoT.
> * Ejecutar de una simulación de dispositivo para transmitir en secuencias datos al entorno de Time Series Insights.
> * Comprobar los datos de telemetría simulados.

Ahora que sabe cómo crear su propio entorno de Time Series Insights puede aprender a crear una aplicación web que consuma datos de un entorno de Time Series Insights:

> [!div class="nextstepaction"]
> [Creación de una aplicación web de página única de Azure Time Series Insights](tutorial-create-tsi-sample-spa.md)
