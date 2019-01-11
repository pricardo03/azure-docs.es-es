---
title: Visualización de datos con el explorador de la versión preliminar de Azure Time Series Insights | Microsoft Docs
description: En este artículo se analizan las distintas características y opciones disponibles en la aplicación web del explorador de la versión preliminar de Azure Time Series Insights.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: d5c18521f39b1b57b5f94a54bb3131e05abba745
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2018
ms.locfileid: "53556652"
---
# <a name="visualize-data-in-the-explorer-preview"></a>Visualización de datos en la versión preliminar del explorador

En este artículo se analizan las distintas características y opciones disponibles en la [aplicación web del explorador](https://insights.timeseries.azure.com/preview/samples) de la versión preliminar de Azure Time Series Insights.

## <a name="prerequisites"></a>Requisitos previos

Antes de usar el explorador de la versión preliminar de Azure Time Series Insights, debe hacer lo siguiente:

* Configurar el entorno de Time Series Insights. Para obtener más información, consulte el [Tutorial: Versión preliminar de Azure Time Series Insights](./time-series-insights-update-create-environment.md).
* Proporcionar acceso a los datos al entorno de Time Series Insights que creó para la cuenta. Puede proporcionar acceso al resto de usuarios y a usted mismo.
* Agregar un origen de eventos al entorno de Time Series Insights para enviar datos al entorno.

## <a name="learn-about-the-azure-time-series-insights-preview-explorer"></a>Obtenga más información sobre el explorador de la versión preliminar de Azure Time Series Insights.

  ![explorer-one][1]

El explorador de la versión preliminar de Azure Time Series Insights consta de los siguientes elementos:

* **Barra de navegación**: le permite cambiar entre las páginas de análisis y de modelo.
* **Árbol de jerarquía**: le permite seleccionar los elementos de datos específicos que se van a agregar al gráfico.
* **Fuente de serie temporal**: muestra los elementos de datos seleccionados actualmente.
* **Panel del gráfico**: muestra el gráfico de trabajo actual.
* **Escala de tiempo**: le permite modificar el intervalo de tiempo de trabajo.
* **Barra de la aplicación**: contiene las opciones de administración de usuario, como el inquilino actual; además le permite cambiar la configuración del tema y el idioma.

## <a name="time-series-insights-preview-environment-panel"></a>Panel del entorno de la versión preliminar de Time Series Insights

El panel del entorno muestra todos los entornos de Time Series Insights a los que tiene acceso. La lista incluye entornos de la versión preliminar y entornos de S1/S2 (GA). Solo tiene que hacer clic en el entorno de Time Series Insights que quiera usar.

  ![explorer-two][2]

## <a name="time-series-insights-preview-navigation-menu"></a>Menú de navegación de la versión preliminar de Azure Time Series Insights

  ![explorer-three][3]

Con el menú de navegación puede ir alternando las aplicaciones de Time Series Insights:

* **Analizar**: le permite trazar y realizar análisis exhaustivos en sus datos de serie temporal modelados o sin modelar.

* **Modelo**: le permite incluir nuevos tipos, jerarquías e instancias de la versión preliminar de Time Series Insights en su modelo de Time Series Insights.

## <a name="time-series-insights-preview-model-authoring"></a>Creación de modelos de la versión preliminar de Time Series Insights

Gracias a esta aplicación puede realizar las operaciones Crear, Leer, Actualizar y Eliminar (CRUD) en su modelo de serie temporal.  

* **Tipo de modelo de serie temporal**: Los tipos de Time Series Insights le permiten definir variables o fórmulas para realizar cálculos. Además están asociados a una instancia determinada de Time Series Insights. Un tipo puede tener una o más variables.
* **Jerarquía del modelo de serie temporal**: Las jerarquías son modos de organizar sus datos de forma sistemática. Asimismo, las jerarquías representan las relaciones entre diferentes entidades en sus datos de Time Series Insights.
* **Instancia del modelo de serie temporal**: Las instancias son las series temporales en sí mismas. En la mayoría de los casos, son DeviceID o AssetID, que funcionan como el identificador único del recurso en el entorno.

Para obtener más información sobre el modelo de serie temporal, consulte los [Modelos de serie temporal](./time-series-insights-update-tsm.md).

## <a name="time-series-insights-preview-model-search-panel"></a>Panel de búsqueda del modelo de la versión preliminar de Time Series Insights

El panel de búsqueda de modelos le permite buscar y navegar fácilmente en su jerarquía de modelos de serie temporal para buscar las instancias de series temporales específicas que quiera mostrar en su gráfico. Cuando selecciona las instancias, estas se agregan tanto al gráfico actual como a los datos.

  ![explorer-four][4]

## <a name="time-series-insights-preview-well"></a>Fuente de la versión preliminar de Time Series Insights

La fuente muestra los campos de instancia y otros metadatos asociados con las instancias de serie temporal seleccionadas. Las casillas del lado derecho le permiten ocultar o mostrar instancias específicas del gráfico actual. También puede eliminar elementos de datos específicos de su fuente de datos actual si hace clic en el control x de color rojo a la derecha del elemento.

  ![explorer-five][5]

También puede abrir el panel de datos de telemetría para obtener una vista vertical mejorada de los elementos de los datos.

  ![explorer-six][6]

> [!NOTE]
> Si ve el siguiente mensaje, quiere decir que la instancia no tiene ningún dato durante el intervalo de tiempo seleccionado. Para resolver el problema, puede aumentar el intervalo de tiempo o confirmar que la instancia está enviando datos.
>
> ![explorer-seven][7]

## <a name="time-series-insights-preview-chart"></a>Gráfico de la versión preliminar de Time Series Insights

Gracias a este gráfico puede mostrar las instancias de serie temporal como líneas. Asimismo, puede contraer el panel de entorno, el modelo de datos y el panel de control de intervalos de tiempo haciendo clic en los controles web para agrandar el gráfico.

  ![explorer-eight][8]

1. **Rango de fecha seleccionado**: controla qué elementos de datos están disponibles para su visualización.

1. **Herramienta de control deslizante del intervalo de fechas internas**: use los dos controles de punto de conexión; para ello, arrástrelos sobre el intervalo de tiempo que quiera usar.

1. **Control para contraer los intervalos de tiempo**: contrae y expande el editor del panel de intervalos de tiempo.

1. **Control de formato del eje Y**: recorre las opciones de vista del eje Y disponibles:

    * `Default`: cada línea tiene un eje Y individual.
    * `Stacked`: Le permite apilar varias líneas en el mismo eje Y para que los datos del eje Y cambien en función de la línea seleccionada.
    * `Shared`: Todos los datos del eje Y se muestran juntos.

1. **Elemento de datos actual**: es el elemento de datos seleccionado actualmente y sus detalles asociados.

Puede profundizar aún más en un segmento de datos específico si hace clic con el botón izquierdo en un punto de datos del gráfico actual y arrastrando a continuación el área seleccionada al punto final que usted elija. Haga clic con el botón derecho en el área seleccionada de color gris y, a continuación, haga clic en el zoom tal como se muestra en la siguiente imagen:

  ![explorer-nine][9]

Después de realizar la acción de zoom, verá el conjunto de datos seleccionado. Haga clic en el control de formato del eje Y para desplazarse por las tres representaciones del eje Y de sus datos de Time Series Insights.

  ![explorer-ten][10]

Aquí puede ver un ejemplo de ejes Y compartidos:

  ![explorer-eleven][11]

## <a name="time-series-insights-preview-time-editor-panel"></a>Panel del editor temporal de la versión preliminar de Time Series Insights

Cuando trabaje con la versión preliminar de Time Series Insights, primero debe seleccionar un intervalo de tiempo. El intervalo de tiempo seleccionado controla el conjunto de datos que está disponible para la manipulación con los widgets de la versión preliminar de Time Series Insights. Los siguientes controles web están disponibles en la versión preliminar de Time Series Insights para que pueda seleccionar el intervalo de tiempo de trabajo.

  ![explorer-twelve][12]

1. **Herramienta de control deslizante del intervalo de fechas internas**: use los dos controles de punto de conexión; para ello, arrástrelos sobre el intervalo de tiempo que quiera usar. El control deslizante del intervalo de fechas externas se encarga de restringir el intervalo de fechas internas.

1. **Aumentar y disminuir los botones de intervalo de fechas**: puede aumentar o disminuir el intervalo de tiempo si selecciona cualquiera de los botones del intervalo que quiera.

1. **Control para contraer los intervalos de tiempo**: este control web le permite ocultar todos los controles, excepto la herramienta de control deslizante del intervalo de fechas de internas.

1. **Control deslizante del intervalo de fechas externas**: use los controles del punto de conexión para seleccionar el intervalo de fechas externas que estará disponible para el control de intervalo de fechas internas.

1. **Lista desplegable del intervalo de fechas de tiempos rápidos**: esta opción le permite cambiar rápidamente entre las selecciones de intervalos de tiempo preestablecidos, como los últimos 30 minutos, las últimas 12 horas o un intervalo personalizado. Si cambia este valor también cambiará los intervalos disponibles que se describen en la herramienta de control deslizante para el tamaño de intervalos.

1. **Herramienta de control deslizante para el tamaño de intervalos**: le permite acercar y alejar los intervalos durante el mismo intervalo de tiempo. Esta acción le proporciona un control más preciso del movimiento entre grandes segmentos de tiempo. Asimismo, muestra tendencias uniformes hasta en intervalos tan pequeños como un milisegundo, lo que le permite ver partes detalladas y de alta resolución de los datos. El punto inicial predeterminado del control deslizante está establecido como la vista más óptima de los datos de la selección, lo que permite equilibrar la resolución, la velocidad de la consulta y la granularidad.

1. **Intervalo de fechas desde y hacia el control web**: gracias a este control web puede hacer clic y seleccionar fácilmente los intervalos de fecha y hora que quiera. También puede usar el control para cambiar entre zonas horarias diferentes. Después de realizar los cambios, puede aplicarlos al área de trabajo actual si selecciona **Guardar**.

  ![explorer-thirteen][13]

## <a name="time-series-insights-preview-navigation-panel"></a>Panel de navegación de la versión preliminar de Time Series Insights

El panel de navegación de la versión preliminar de Time Series Insights proporciona la siguiente funcionalidad:

  ![explorer-fourteen][14]

### <a name="current-session-share-link-control"></a>Control de vínculo del recurso compartido de la sesión actual

  ![explorer-fifteen][15]

Seleccione el control web del vínculo (resaltado) para generar una dirección URL y así poder guardar o compartir su sesión de trabajo actual de Azure Time Series Insights, que incluye lo siguiente:

* Intervalo de tiempo seleccionado actualmente
* Tamaño del intervalo seleccionado actualmente
* Fuente de datos seleccionada actualmente

### <a name="tenant-section"></a>Sección del inquilino

  ![explorer-sixteen][16]

* Muestra la información de cuenta de inicio de sesión actual de Time Series Insights.
* Le permite alternar entre los temas disponibles de Time Series Insights.

### <a name="theme-selection"></a>Selección de temas

La versión preliminar de Azure Time Series Insights admite dos temas:

* **Tema claro**: es el tema predeterminado que se muestra en este documento.
* **Tema oscuro**:  representa el explorador tal como se muestra aquí:

  ![explorer-seventeen][17]

Aquí también puede cambiar entre los idiomas admitidos.

## <a name="s1s2-environment-controls"></a>Controles de entorno S1/S2

### <a name="time-series-insights-preview-terms-panel"></a>Panel de términos de la versión preliminar de Time Series Insights

Esta sección se aplica solo a entornos S1/S2 existentes que intentan usar el explorador en la interfaz de usuario actualizada. Puede usar el producto de disponibilidad general y la versión preliminar juntos. Hemos agregado algunas funcionalidades desde la interfaz de usuario existente en el explorador actualizado, pero puede obtener la experiencia completa de la interfaz de usuario para el entorno de S1/S2 en el explorador de Time Series Insights existente.  

En lugar de la jerarquía, verá el panel de términos de Time Series Insights, donde podrá definir consultas en el entorno. Permite filtrar los datos según un predicado.

  ![explorer-eighteen][18]

El panel del editor de términos de la versión preliminar de Time Series Insights toma los parámetros siguientes:

**Where**: la cláusula where le permite filtrar rápidamente sus eventos utilizando el conjunto de operandos que se enumeran en la siguiente tabla. Si realiza una búsqueda seleccionando un operando, el predicado se actualizará automáticamente según esa búsqueda. Los tipos de operando compatibles incluyen:

| Operación | Tipos admitidos   | Notas |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` | Double, DateTime, TimeSpan | |
| `=`, `!=`, `<>` | String, Bool, Double, DateTime, TimeSpan, NULL |
| `IN` | String, Bool, Double, DateTime, TimeSpan, NULL | Todos los operandos deben ser del mismo tipo o deben ser una constante NULL. |
| `HAS` | string | Solo los literales de cadenas constantes se permiten en el lado derecho. No se permiten cadenas vacías ni NULL. |

Obtenga más información acerca de los tipos de datos y las operaciones de consulta admitidas; para ello, consulte [Time Series Expression (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx) (Expresión de serie temporal [TSX]).

### <a name="examples-of-where-clauses"></a>Ejemplos de cláusulas Where

  ![explorer-nineteen][19]

**Measure**: este menú desplegable muestra todas las columnas numéricas (**Dobles**) que puede usar como elementos para el gráfico actual.

**Split by**: este menú desplegable muestra todas las columnas categóricas disponibles (Cadenas) en el modelo mediante las cuales puede agrupar sus datos. Puede agregar hasta cinco términos para verlos en el mismo eje X. Escriba los parámetros que quiera usar y, a continuación, seleccione **Agregar** para agregar un término nuevo.

  ![explorer-twenty][20]

Puede mostrar y ocultar elementos en el panel del gráfico si selecciona el icono visible tal como se muestra en la siguiente imagen. También puede eliminar completamente las consultas haciendo clic en la **x** roja.

  ![explorer-twenty-one][21]

## <a name="next-steps"></a>Pasos siguientes

Consulte los artículos siguientes:
* [Azure Time Series Insights Preview storage and ingress](./time-series-insights-update-storage-ingress.md) (Almacenamiento y entrada de datos en la versión preliminar de Azure Time Series Insights)
* [Data modeling](./time-series-insights-update-tsm.md) (Modelado de datos)
* [Diagnóstico y solución de problemas](./time-series-insights-update-how-to-troubleshoot.md)

<!-- Images -->
[1]: media/v2-update-explorer/explorer-one.png
[2]: media/v2-update-explorer/explorer-two.png
[3]: media/v2-update-explorer/explorer-three.png
[4]: media/v2-update-explorer/explorer-four.png
[5]: media/v2-update-explorer/explorer-five.png
[6]: media/v2-update-explorer/explorer-six.png
[7]: media/v2-update-explorer/explorer-seven.png
[8]: media/v2-update-explorer/explorer-eight.png
[9]: media/v2-update-explorer/explorer-nine.png
[10]: media/v2-update-explorer/explorer-ten.png
[11]: media/v2-update-explorer/explorer-eleven.png
[12]: media/v2-update-explorer/explorer-twelve.png
[13]: media/v2-update-explorer/explorer-thirteen.png
[14]: media/v2-update-explorer/explorer-fourteen.png
[15]: media/v2-update-explorer/explorer-fifteen.png
[16]: media/v2-update-explorer/explorer-sixteen.png
[17]: media/v2-update-explorer/explorer-seventeen.png
[18]: media/v2-update-explorer/explorer-eighteen.png
[19]: media/v2-update-explorer/explorer-nineteen.png
[20]: media/v2-update-explorer/explorer-twenty.png
[21]: media/v2-update-explorer/explorer-twenty-one.png
