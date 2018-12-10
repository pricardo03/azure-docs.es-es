---
title: Tutorial sobre Azure Time Series Insights (versión preliminar) | Microsoft Docs
description: Más información sobre Azure Time Series Insights (versión preliminar)
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 11/26/2018
ms.openlocfilehash: ed25d03f7c592476b9284790ac12f9954661a42b
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52872312"
---
# <a name="azure-time-series-insights-preview-tutorial"></a>Tutorial sobre Azure Time Series Insights (versión preliminar)

Este tutorial le guiará a través del proceso de creación de un entorno en la versión preliminar de Azure Time Series Insights (TSI), rellenado con datos de dispositivos simulados. En este tutorial, aprenderá a:

* Crear un entorno de TSI (versión preliminar).
* Conectar el entorno de TSI (versión preliminar) a un centro de eventos.
* Ejecutar una simulación de un parque eólico para transmitir datos en el entorno de TSI (versión preliminar).
* Realizar un análisis básico de los datos.
* Defina el tipo y la jerarquía de un modelo de serie temporal y asócielo con sus instancias.

## <a name="create-a-time-series-insights-preview-environment"></a>Creación de un entorno de Time Series Insights (versión preliminar)

En esta sección se describe cómo crear un entorno de Azure TSI (versión preliminar) mediante [Azure Portal](https://portal.azure.com/).

1. Inicie sesión en Azure Portal mediante la cuenta de suscripción.
1. Seleccione **+ Crear un recurso** en la esquina superior izquierda.
1. Seleccione la categoría **Internet de las cosas** y elija **Time Series Insights**.

  ![tutorial-uno][1]

1. En la página de entorno de Time Series Insights, rellene los parámetros necesarios y haga clic en **Siguiente: Origen del evento**.

  ![tutorial-dos][2]

1. En la página  **Origen del evento**, rellene los parámetros necesarios y haga clic en **Revisar y crear**.

  ![tutorial-tres][3]

1. Revise todos los detalles y haga clic en **Crear** para empezar a aprovisionar su entorno.

  ![tutorial-cuatro][4]

1. Recibirá una notificación cuando la implementación se haya completado correctamente.

  ![tutorial-cinco][5]

## <a name="send-events-to-your-tsi-environment"></a>Envío de eventos al entorno de Time Series Insights

En esta sección usará un simulador de dispositivo de aerogenerador para enviar eventos al entorno de Time Series Insights mediante un centro de eventos.

  1. En Azure Portal, vaya al recurso del centro de eventos y conéctelo al entorno de Time Series Insights. Aprenda [cómo conectar el recurso a un centro de eventos ya existente](./time-series-insights-how-to-add-an-event-source-eventhub.md).

  1. En la página del recurso del centro de eventos, vaya a **Directivas de acceso compartido** y, después, a **RootManageSharedAccessKey**. Copie la **Cadena de conexión: clave principal**  que aparece aquí.

      ![tutorial-seis][6]

  1. Vaya a [https://tsiclientsample.azurewebsites.net/windFarmGen.html]( https://tsiclientsample.azurewebsites.net/windFarmGen.html). Esta aplicación web simula dispositivos aerogeneradores.
  1. Pegue la cadena de conexión que ha copiado en el paso 3 en **Event Hub Connection String** (Cadena de conexión del centro de eventos).

      ![tutorial-siete][7]

  1. Haga clic en **Click to Start** (Haga clic para empezar) para insertar eventos en el centro de eventos. En esta fase, se descargará un archivo llamado `instances.json` en la máquina. Guarde este archivo ya que lo necesitará más adelante.

  1. Vuelva al centro de eventos. Ahora debería ver los nuevos eventos que el centro ha recibido.

     ![tutorial-ocho][8]

## <a name="analyze-data-in-your-environment"></a>Análisis de datos en el entorno

En esta sección realizará un análisis básico de los datos de la serie temporal mediante el explorador de actualizaciones de Time Series Insights.

  1. Para ir al explorador de actualizaciones de Time Series Insights, haga clic en la dirección URL de la página de recursos de Azure Portal.

      ![tutorial-nueve][9]

  1. En el explorador, haga clic en los nodos **Unparented Instances** (Instancias no primarias) para ver todas las instancias de la serie temporal en el entorno.

     ![tutorial-diez][10]

  1. En este tutorial vamos a analizar los datos que se han enviado el último día. Para ello, haga clic en **Tiempos rápidos** y seleccione la opción **Últimas 24 horas**.

     ![tutorial-once][11]

  1. Seleccione **Sensor_0** y elija **Mostrar valor promedio** para visualizar los datos que se envían desde esta instancia de serie temporal.

     ![tutorial-doce][12]

  1. De forma parecida, se pueden trazar datos procedentes de otras instancias de serie temporal para realizar un análisis básico.

     ![tutorial-trece][13]

## <a name="define-a-type--hierarchy"></a>Definición de un tipo y una jerarquía 

En esta sección va a crear un tipo y una jerarquía y los va a asociar con las instancias de la serie temporal. Obtenga más información sobre [Time Series Models](./time-series-insights-update-tsm.md) (Modelos de serie temporal).

  1. En el explorador, haga clic en la pestaña **Modelo** en la barra de la aplicación.

     ![tutorial-catorce][14]

  1. En la sección de tipos, haga clic en **+ Agregar**. Esto le permitirá crear un nuevo tipo de modelo de serie temporal.

     ![tutorial-quince][15]

  1. En el editor de tipos, escriba un **Nombre**, una **Descripción** y cree variables para los valores **Promedio**, **Mínimo** y **Máximo** como se indica a continuación. Haga clic en **Crear** para guardar el tipo.

     ![tutorial-dieciséis][16]

     ![tutorial-diecisiete][17]

  1. En la sección **Jerarquías**, haga clic en **+ Agregar**. Esto le permitirá crear una nueva jerarquía de modelo de serie temporal.

     ![tutorial-dieciocho][18]

  1. En el editor de jerarquías, escriba un **nombre** y agregue los niveles de la jerarquía como se muestra a continuación. Haga clic en **Crear** para guardar la jerarquía.

     ![tutorial-diecinueve][19]

     ![tutorial-veinte][20]

  1. En la sección **Instancias**, seleccione una instancia y haga clic en **Editar**. Esto le permitirá asociar un tipo y una jerarquía a esta instancia.

     ![tutorial-veintiuno][21]

  1. En el editor de instancias, elija el tipo y la jerarquía que se ha definido en los pasos 3 y 5 anteriores como se indica a continuación.

     ![tutorial-veintidós][22]

  1. Como alternativa, para hacer esto para todas las instancias a la vez, puede editar el archivo `instances.json` que descargó anteriormente. En este archivo, reemplace todos los campos **typeId** y **hierarchyId** con el identificador que se obtuvo en los pasos 3 y 5 anteriores.

  1. En la sección **Instancias**, haga clic en **Cargar JSON** y cargue el archivo `instances.json` editado como se indica a continuación.

     ![tutorial-veintitrés][23]

  1. Vaya a la pestaña **Análisis** y actualice el explorador. Ahora debería ver todas las instancias asociadas con el tipo y la jerarquía definidos anteriormente.

     ![tutorial-veinticuatro][24]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:  

* Crear un entorno de TSI (versión preliminar).
* Conectar el entorno de TSI (versión preliminar) a un centro de eventos.
* Ejecutar una simulación de un parque eólico para transmitir datos en el entorno de TSI (versión preliminar).
* Realizar un análisis básico de los datos.
* Definir el tipo y la jerarquía de un modelo de serie temporal y asociarlo con sus instancias.

Ahora que sabe cómo crear su propio entorno de actualización de Time Series Insights, obtenga más información sobre los conceptos clave de este.

Lea acerca de la configuración de almacenamiento de Time Series Insights:

> [!div class="nextstepaction"]
> [Entrada y almacenamiento de Azure Time Series Insights (versión preliminar)](./time-series-insights-update-storage-ingress.md)

Obtenga más información sobre los modelos de serie temporal:

> [!div class="nextstepaction"]
> [Modelado de datos de Azure Time Series Insights (versión preliminar)](./time-series-insights-update-tsm.md)

<!-- Images -->
[1]: media/v2-update-provision/tutorial-one.png
[2]: media/v2-update-provision/tutorial-two.png
[3]: media/v2-update-provision/tutorial-three.png
[4]: media/v2-update-provision/tutorial-four.png
[5]: media/v2-update-provision/tutorial-five.png
[6]: media/v2-update-provision/tutorial-six.png
[7]: media/v2-update-provision/tutorial-seven.png
[8]: media/v2-update-provision/tutorial-eight.png
[9]: media/v2-update-provision/tutorial-nine.png
[10]: media/v2-update-provision/tutorial-ten.png
[11]: media/v2-update-provision/tutorial-eleven.png
[12]: media/v2-update-provision/tutorial-twelve.png
[13]: media/v2-update-provision/tutorial-thirteen.png
[14]: media/v2-update-provision/tutorial-fourteen.png
[15]: media/v2-update-provision/tutorial-fifteen.png
[16]: media/v2-update-provision/tutorial-sixteen.png
[17]: media/v2-update-provision/tutorial-seventeen.png
[18]: media/v2-update-provision/tutorial-eighteen.png
[19]: media/v2-update-provision/tutorial-nineteen.png
[20]: media/v2-update-provision/tutorial-twenty.png
[21]: media/v2-update-provision/tutorial-twenty-one.png
[22]: media/v2-update-provision/tutorial-twenty-two.png
[23]: media/v2-update-provision/tutorial-twenty-three.png
[24]: media/v2-update-provision/tutorial-twenty-four.png