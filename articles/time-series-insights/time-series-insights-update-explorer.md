---
title: Visualización de datos con el explorador de la versión preliminar de Azure Time Series Insights | Microsoft Docs
description: En este artículo se analizan las distintas características y opciones disponibles en la aplicación web del explorador de la versión preliminar de Azure Time Series Insights.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 10/07/2019
ms.custom: seodec18
ms.openlocfilehash: a0d8f7cdace8d3e810dfcda6e54df5823840307c
ms.sourcegitcommit: f9e81b39693206b824e40d7657d0466246aadd6e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72034334"
---
# <a name="visualize-data-in-the-explorer-preview"></a>Visualización de datos en la versión preliminar del explorador

En este documento se describen las características de la interfaz y la experiencia del usuario de la [aplicación web de demostración](https://insights.timeseries.azure.com/preview/demo) de la versión preliminar de Azure Time Series Insights. En concreto, se describen el diseño del ejemplo hospedado, las opciones de personalización de la interfaz y la navegación a través de la demostración proporcionada.

## <a name="prerequisites"></a>Requisitos previos

Para empezar a usar el explorador de la versión preliminar de Azure Time Series Insights, debe hacer lo siguiente:

* Configurar el entorno de Time Series Insights. Para obtener más información sobre cómo aprovisionar una instancia, pruebe el tutorial de la [versión preliminar de Azure Time Series Insights](./time-series-insights-update-create-environment.md).
* [Proporcionar acceso a datos](./time-series-insights-data-access.md) al entorno de Time Series Insights que creó para la cuenta. Puede proporcionar acceso al resto de usuarios y a usted mismo.
* Agregar un origen de eventos al entorno de Time Series Insights para insertarle datos:
  * Aprenda a [conectarse a un centro de eventos](./time-series-insights-how-to-add-an-event-source-eventhub.md).
  * Aprenda a [conectarse a un centro de IoT](./time-series-insights-how-to-add-an-event-source-iothub.md).

## <a name="learn-about-the-preview-explorer"></a>Obtenga información sobre el explorador de la versión preliminar

El explorador de la versión preliminar de Azure Time Series Insights consta de los siguientes elementos:

[![Vista del explorador](media/v2-update-explorer/explorer-one.png)](media/v2-update-explorer/explorer-one.png#lightbox)

- <a href="#environment-drop-down-list">Panel de entornos</a>: muestra los entornos de Azure Time Series Insights.
- <a href="#navigation-menu">Menú de navegación</a>: se usa para cambiar entre las páginas **Analizar** y **Modelo**.
- <a href="#hierarchy-tree">Árbol de jerarquía</a>: use esta opción para seleccionar los elementos de datos y modelo específicos que se van a agregar al gráfico.
- <a href="#preview-well">Fuente de serie temporal</a>: muestra los elementos de datos seleccionados actualmente en formato de tabla con codificación en colores.
- <a href="#preview-chart">Panel del gráfico</a>: muestra el gráfico de trabajo actual.
- <a href="#time-editor-panel">Escala de tiempo</a>: use esta opción para modificar el intervalo de tiempo de trabajo.
- <a href="#navigation-panel">Barra de la aplicación</a>: contiene las opciones de administración de usuarios, como el inquilino actual. Puede usarlo para cambiar la configuración del tema y del idioma.

## <a name="environment-drop-down-list"></a>Lista desplegable de entornos

La lista desplegable de entornos muestra todos los entornos de Time Series Insights a los que tiene acceso. La lista incluye los entornos de pago por uso, como la versión preliminar de Time Series Insights. La lista también incluye entornos S1/S2, que tienen disponibilidad general.

1. Seleccione la flecha desplegable situada junto a su entorno mostrado.

   [![Panel de control](media/v2-update-explorer/explorer-two.png)](media/v2-update-explorer/explorer-two.png#lightbox)

1. A continuación, seleccione el entorno que desee.

## <a name="navigation-menu"></a>Menú de navegación

  [![Menú de navegación](media/v2-update-explorer/explorer-three.png)](media/v2-update-explorer/explorer-three.png#lightbox)

Use el menú de navegación para seleccionar entre dos vistas:

* **Analizar**: se usa para trazar y realizar análisis exhaustivos en sus datos de serie temporal modelados o sin modelar.
* **Modelo**: se usa para insertar nuevos tipos, jerarquías e instancias de la versión preliminar de Time Series Insights en su modelo de Time Series Insights.

## <a name="hierarchy-tree"></a>Árbol de jerarquía

El árbol de jerarquía muestra los elementos de datos seleccionados que incluyen modelos, dispositivos específicos y sensores en sus dispositivos.

### <a name="model-search-panel"></a>Panel de búsqueda de modelos

Puede usar el panel de búsqueda de modelos para buscar y navegar fácilmente en su jerarquía de modelos de Time Series a fin de buscar las instancias de Time Series específicas que quiera mostrar en su gráfico. Una vez que haya seleccionado las instancias, estas se agregan tanto al gráfico actual como a los datos.

  [![Panel de búsqueda de modelos](media/v2-update-explorer/explorer-four.png)](media/v2-update-explorer/explorer-four.png#lightbox)

### <a name="model-authoring"></a>Creación de modelos

La vista preliminar de Azure Time Series Insights admite operaciones completas de creación, lectura, actualización y eliminación (CRUD) en su modelo de Time Series.

* **Tipo de modelo de serie temporal**: Puede usar tipos de Time Series Insights para definir variables o fórmulas para realizar cálculos. Dichos tipos están asociados a una instancia determinada de Time Series Insights. Un tipo puede tener una o más variables.
* **Jerarquía del modelo de serie temporal**: Las jerarquías son modos de organizar sus datos de forma sistemática. Asimismo, las jerarquías representan las relaciones entre diferentes entidades en sus datos de Time Series Insights.
* **Instancia del modelo de serie temporal**: Las instancias son la serie temporal en sí mismas. En la mayoría de los casos, son **DeviceID** o **AssetID**, que funcionan como el identificador único del recurso en el entorno.

Para obtener más información sobre el modelo de serie temporal, consulte los [Modelos de serie temporal](./time-series-insights-update-tsm.md).

## <a name="preview-well"></a>Cuadro de versión preliminar

El cuadro muestra los campos de instancia y otros metadatos asociados con las instancias de Time Series Insights seleccionadas. Mediante la selección de las casillas del lado derecho, puede ocultar o mostrar instancias específicas del gráfico actual. También puede eliminar elementos de datos específicos de su cuadro de datos actual seleccionando el control **Eliminar** (papelera de reciclaje) de color rojo a la izquierda del elemento.

  [![Cuadro de versión preliminar](media/v2-update-explorer/explorer-five.png)](media/v2-update-explorer/explorer-five.png#lightbox)

Para volver a configurar el diseño de la página del gráfico **Analizar**, seleccione el icono de botón de puntos suspensivos situado en la esquina superior derecha:

  [![Opciones de diseño de telemetría](media/v2-update-explorer/explorer-six.png)](media/v2-update-explorer/explorer-six.png#lightbox)

> [!NOTE]
> Si ve el siguiente mensaje, quiere decir que la instancia no tiene ningún dato durante el intervalo de tiempo seleccionado. Para resolver el problema, aumente el intervalo de tiempo o confirme que la instancia está enviando datos.
>
> ![Sin notificación de datos](media/v2-update-explorer/explorer-seven.png)

## <a name="preview-chart"></a>Gráfico de vista previa

Con este gráfico, puede mostrar las instancias de Time Series Insights como líneas. Asimismo, puede contraer el panel de entornos, el modelo de datos y el panel de control de intervalos de tiempo seleccionando los controles web para ampliar el gráfico.

  [![Información general sobre el gráfico de vista previa](media/v2-update-explorer/explorer-eight.png)](media/v2-update-explorer/explorer-eight.png#lightbox)

- **Intervalo de fechas seleccionado**: controla qué elementos de datos están disponibles para su visualización.

- **Herramienta de control deslizante de intervalos de fechas internas**: use los dos controles de punto de conexión arrastrándolos sobre el intervalo de tiempo que quiera usar.

- **Control para contraer los intervalos de tiempo**: contrae y expande el editor del panel de intervalos de tiempo.

- **Control de formato del eje Y**: recorre las opciones de vista del eje Y disponibles:

    * `Default`: cada línea tiene un eje Y individual.
    * `Stacked`: úselo para apilar varias líneas en el mismo eje Y para que los datos del eje Y cambien en función de la línea seleccionada.
    * `Shared`: todos los datos del eje Y se muestran juntos.

- **Elemento de datos actual**: es el elemento de datos seleccionado actualmente y sus detalles asociados.

Para profundizar aún más en un segmento de datos específico, haga clic con el botón izquierdo en un punto de datos del gráfico actual y, a continuación, arrastre el área seleccionada al punto de conexión que elija. Haga clic con el botón derecho en el área seleccionada de color gris y seleccione **Zoom**, tal como se muestra en la siguiente imagen:

  [![Zoom del gráfico de vista previa](media/v2-update-explorer/explorer-nine.png)](media/v2-update-explorer/explorer-nine.png#lightbox)

Después de realizar la acción **Zoom**, verá el conjunto de datos seleccionado. Seleccione el control de formato del eje Y para desplazarse por las tres representaciones del eje Y de sus datos de Time Series Insights.

  [![Eje Y del gráfico de vista previa](media/v2-update-explorer/explorer-ten.png)](media/v2-update-explorer/explorer-ten.png#lightbox)

Aquí puede ver un ejemplo de ejes Y compartidos:

  [![Ejes Y compartidos de vista previa](media/v2-update-explorer/explorer-eleven.png)](media/v2-update-explorer/explorer-eleven.png#lightbox)

## <a name="time-editor-panel"></a>Panel del editor de tiempo

Cuando trabaje con la versión preliminar de Time Series Insights, primero debe seleccionar un intervalo de tiempo. El intervalo de tiempo seleccionado controla el conjunto de datos que está disponible para la manipulación con los widgets de la versión preliminar de Time Series Insights. Los siguientes controles web están disponibles en la versión preliminar de Time Series Insights para que pueda seleccionar el intervalo de tiempo de trabajo:

  [![Panel de selección de hora](media/v2-update-explorer/explorer-twelve.png)](media/v2-update-explorer/explorer-twelve.png#lightbox)

1. **Herramienta de control deslizante de intervalos de fechas internas**: use los dos controles de punto de conexión arrastrándolos sobre el intervalo de tiempo que quiera usar. El control deslizante del intervalo de fechas externas se encarga de restringir el intervalo de fechas internas.

1. **Aumentar y disminuir los botones de intervalo de fechas**: puede aumentar o disminuir el intervalo de tiempo si selecciona cualquiera de los botones del intervalo que quiera.

1. **Control para contraer los intervalos de tiempo**: este control web le permite ocultar todos los controles, excepto la herramienta de control deslizante de intervalos de fechas internas.

1. **Control deslizante de intervalos de fechas externas**: use los controles del punto de conexión para seleccionar el intervalo de fechas externas que estará disponible para el control de intervalos de fechas internas.

1. **Lista desplegable del intervalo de fechas de Tiempos rápidos**: use esta opción para cambiar rápidamente entre las selecciones de intervalos de tiempo preestablecidos, como los últimos **30 minutos**, las **últimas 12 horas** o un **intervalo personalizado**. Si cambia este valor también cambiará los intervalos disponibles que se describen en la herramienta de control deslizante para el tamaño de intervalos.

1. **Herramienta de control deslizante para el tamaño de intervalos**: use esta opción para acercar y alejar los intervalos durante el mismo intervalo de tiempo. Esta acción le proporciona un control más preciso del movimiento entre grandes segmentos de tiempo. Muestra tendencias uniformes hasta segmentos tan pequeños como un milisegundo. Puede usar esta opción para ver cortes granulares de alta resolución de los datos. El punto inicial predeterminado del control deslizante está establecido como la vista más óptima de los datos de la selección, lo que permite equilibrar la resolución, la velocidad de la consulta y la granularidad.

1. **Intervalo de fechas desde y hacia el control web**: con este control web, puede seleccionar fácilmente los intervalos de fecha y hora que quiera. También puede usar el control para cambiar entre zonas horarias diferentes. Después de realizar los cambios para aplicarlos al área de trabajo actual, seleccione **Guardar**.

   [![Panel de selección de fecha inicial y de finalización](media/v2-update-explorer/explorer-thirteen.png)](media/v2-update-explorer/explorer-thirteen.png#lightbox)

## <a name="navigation-panel"></a>Panel de navegación

El panel de navegación de la versión preliminar de Time Series Insights aparece en la parte superior de la aplicación de Time Series Insights. Ofrece la siguiente funcionalidad.

### <a name="current-session-share-link-control"></a>Control de vínculo del recurso compartido de la sesión actual

  [![Icono Compartir](media/v2-update-explorer/explorer-fifteen.png)](media/v2-update-explorer/explorer-fifteen.png#lightbox)

Seleccione el nuevo icono **Compartir** para compartir un vínculo URL con su equipo.

  [![Uso compartido de la dirección URL de su instancia](media/v2-update-explorer/url-share.png)](media/v2-update-explorer/url-share.png#lightbox)

### <a name="tenant-section"></a>Sección del inquilino

  [![Selección de inquilino](media/v2-update-explorer/explorer-sixteen.png)](media/v2-update-explorer/explorer-sixteen.png#lightbox)

* Muestra la información de cuenta de inicio de sesión actual de Time Series Insights.
* Use esta opción para cambiar entre los temas disponibles de Time Series Insights.
* Use esta opción para ver la [aplicación web de demostración](https://insights.timeseries.azure.com/preview/demo) de la versión preliminar.

### <a name="theme-selection"></a>Selección de temas

Para seleccionar un nuevo tema, seleccione el icono del perfil ubicado en la esquina superior derecha. A continuación, seleccione **Cambiar tema**.

  [![Selección de tema](media/v2-update-explorer/theme-selection.png)](media/v2-update-explorer/theme-selection.png#lightbox)

> [!TIP]
> La selección del idioma también está disponible si selecciona el icono del perfil.

La versión preliminar de Azure Time Series Insights admite dos temas:

* **Tema claro**: es el tema predeterminado que se muestra en este documento.
* **Tema oscuro**: representa el explorador tal como se muestra aquí:

  [![Tema oscuro seleccionado](media/v2-update-explorer/explorer-seventeen.png)](media/v2-update-explorer/explorer-seventeen.png#lightbox)

## <a name="s1s2-environment-controls"></a>Controles de entorno S1/S2

### <a name="preview-terms-panel"></a>Panel de términos de la versión preliminar

Esta sección se aplica solo a entornos S1/S2 existentes que intentan usar el explorador en la interfaz de usuario actualizada. Puede usar el producto de disponibilidad general y la versión preliminar juntos. Hemos agregado algunas funcionalidades desde la interfaz de usuario existente en el explorador actualizado, pero puede obtener la experiencia completa de la interfaz de usuario para el entorno de S1/S2 en el explorador de Time Series Insights existente. 

En lugar de la jerarquía, se ve el panel de términos de Time Series Insights, donde puede definir consultas en el entorno. Úselo para filtrar los datos según un predicado.

  [![Panel de consultas con la cláusula Where](media/v2-update-explorer/explorer-eighteen.png)](media/v2-update-explorer/explorer-eighteen.png#lightbox)

El panel del editor de términos de la versión preliminar de Time Series Insights toma los parámetros siguientes:

**Where**: use la cláusula Where para filtrar rápidamente sus eventos mediante el conjunto de operandos que se enumeran en la siguiente tabla. Si realiza una búsqueda seleccionando un operando, el predicado se actualizará automáticamente según esa búsqueda. Los tipos de operando compatibles incluyen:

| Operación | Tipos admitidos   | Notas |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` | Double, DateTime, TimeSpan | |
| `=`, `!=`, `<>` | String, Bool, Double, DateTime, TimeSpan, NULL |
| `IN` | String, Bool, Double, DateTime, TimeSpan, NULL | Todos los operandos deben ser del mismo tipo o deben ser una constante NULL. |
| `HAS` | Cadena | En el lado derecho solo se permiten literales de cadenas constantes. No se permiten cadenas vacías ni NULL. |

Para obtener más información sobre las operaciones de consulta y los tipos de datos admitidos, consulte [Time Series Expression (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx) (Expresión de serie temporal [TSX]).

### <a name="examples-of-where-clauses"></a>Ejemplos de cláusulas Where

  [![Ejemplos de cláusulas Where](media/v2-update-explorer/explorer-nineteen.png)](media/v2-update-explorer/explorer-nineteen.png#lightbox)

**Medida**: lista desplegable que muestra todas las columnas numéricas (**Dobles**) que puede usar como elementos para su gráfico actual.

**Split by**: esta lista desplegable muestra todas las columnas categóricas disponibles (Cadenas) en el modelo por las que puede agrupar sus datos. Puede agregar hasta cinco términos para verlos en el mismo eje X. Escriba los parámetros que quiera usar y, a continuación, seleccione **Agregar** para agregar un término nuevo.

  [![Vista filtrada y consultada uno](media/v2-update-explorer/explorer-twenty.png)](media/v2-update-explorer/explorer-twenty.png#lightbox)

Puede mostrar y ocultar elementos en el panel del gráfico si selecciona el icono visible, tal como se muestra en la siguiente imagen. Para quitar las consultas completamente, seleccione la **X** de color rojo.

  [![Vista filtrada y consultada uno](media/v2-update-explorer/explorer-twenty-one.png)](media/v2-update-explorer/explorer-twenty-one.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre [el almacenamiento y la entrada](./time-series-insights-update-storage-ingress.md) en la versión preliminar de Time Series Insights.
- Lea la documentación de la versión preliminar de Time Series Insights sobre el [modelado de datos](./time-series-insights-update-tsm.md).
- Obtenga información sobre [cómo diagnosticar y solucionar problemas](./time-series-insights-update-how-to-troubleshoot.md) con la instancia de Time Series Insights.
