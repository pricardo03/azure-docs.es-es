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
ms.date: 05/03/2019
ms.custom: seodec18
ms.openlocfilehash: 862465a7611f1a2bc65dbb0c49c4de512bd239de
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65442089"
---
# <a name="visualize-data-in-the-explorer-preview"></a>Visualización de datos en la versión preliminar del explorador

Este documento describe las características de interfaz de usuario/experiencia de usuario y la interfaz de la versión preliminar de Azure tiempo Series Insights [aplicación web de demostración](https://insights.timeseries.azure.com/preview/demo). En concreto, describe el diseño del ejemplo hospedada, opciones de personalización de la interfaz y la navegación a través de la demostración proporcionada.

## <a name="prerequisites"></a>Requisitos previos

Para empezar a trabajar con el Explorador de vista previa de Azure tiempo Series Insights, debe:

* Configurar el entorno de Time Series Insights. Para obtener más información sobre cómo aprovisionar una instancia pruebe nuestra [versión preliminar de Azure tiempo Series Insights](./time-series-insights-update-create-environment.md) tutorial.
* [Proporcionar acceso a datos](./time-series-insights-data-access.md) en el entorno de Time Series Insights que ha creado para la cuenta. Puede proporcionar acceso al resto de usuarios y a usted mismo.
* Agregar un origen de eventos para el entorno de Time Series Insights para insertar datos en el entorno:
  * Obtenga información sobre [cómo conectarse a un centro de eventos](./time-series-insights-how-to-add-an-event-source-eventhub.md)
  * Obtenga información sobre [cómo conectarse a un centro de IoT](./time-series-insights-how-to-add-an-event-source-iothub.md)

## <a name="learn-about-the-preview-explorer"></a>Obtenga información sobre el Explorador de vista previa

El explorador de la versión preliminar de Azure Time Series Insights consta de los siguientes elementos:

[![La vista del explorador](media/v2-update-explorer/explorer-one.png)](media/v2-update-explorer/explorer-one.png#lightbox)

1. <a href="#environment-dropdown">**Panel entorno**</a>: Muestra los entornos de Azure TSI.
1. <a href="#navigation-menu">**Menú de navegación**</a>: Permite cambiar entre el **analizar** y **modelo** páginas.
1. <a href="#hierarchy-tree">**Árbol de jerarquía**</a>: Permite seleccionar el modelo específico y elementos de datos que se va a mostrarse.
1. <a href="#preview-well">**Series temporales también**</a>: Muestra los elementos de datos actualmente seleccionado en formato de tabla con codificación en colores.
1. <a href="#preview-chart">**Chart panel**</a>:  muestra el gráfico de trabajo actual.
1. <a href="#time-editor-panel">**Escala de tiempo**</a>:  le permite modificar el intervalo de tiempo de trabajo.
1. <a href="#navigation-panel">**Barra de la aplicación**</a>:  contiene las opciones de administración de usuario, como el inquilino actual; además le permite cambiar la configuración del tema y el idioma.

## <a name="environment-dropdown"></a>Menú desplegable de entornos

La lista desplegable muestra todos los entornos de Time Series Insights a que tienen acceso. La lista incluye los entornos de pago por uso (versión preliminar) y S1/S2 (disponibilidad general o GA). 

1. Simplemente haga clic en la flecha desplegable situada junto a su entorno mostrado:

   [![El panel de control](media/v2-update-explorer/explorer-two.png)](media/v2-update-explorer/explorer-two.png#lightbox)

1. A continuación, seleccione el entorno deseado.

## <a name="navigation-menu"></a>Menú de navegación

  [![El menú de navegación](media/v2-update-explorer/explorer-three.png)](media/v2-update-explorer/explorer-three.png#lightbox)

El menú de navegación permite seleccionar entre dos vistas:

* **Analizar**: le permite trazar y realizar análisis exhaustivos en sus datos de serie temporal modelados o sin modelar.
* **Modelo**: le permite incluir nuevos tipos, jerarquías e instancias de la versión preliminar de Time Series Insights en su modelo de Time Series Insights.

## <a name="hierarchy-tree"></a>árbol de jerarquía

El árbol de jerarquía muestra los elementos de datos seleccionados, incluidos los modelos de dispositivos específicos y sensores en los dispositivos.

### <a name="model-search-panel"></a>Panel de búsqueda del modelo

El panel de búsqueda de modelos le permite buscar y navegar fácilmente en su jerarquía de modelos de serie temporal para buscar las instancias de series temporales específicas que quiera mostrar en su gráfico. Cuando selecciona las instancias, estas se agregan tanto al gráfico actual como a los datos.

  [![El panel de búsqueda del modelo](media/v2-update-explorer/explorer-four.png)](media/v2-update-explorer/explorer-four.png#lightbox)

### <a name="model-authoring"></a>Creación de modelos

La versión preliminar de Azure tiempo Series Insights admite operaciones de creación, lectura, actualización y eliminación (CRUD) completas en el modelo de serie temporal.  

* **Tipo de modelo de serie temporal**: Los tipos de Time Series Insights le permiten definir variables o fórmulas para realizar cálculos. Además están asociados a una instancia determinada de Time Series Insights. Un tipo puede tener una o más variables.
* **Jerarquía del modelo de serie temporal**: Las jerarquías son modos de organizar sus datos de forma sistemática. Asimismo, las jerarquías representan las relaciones entre diferentes entidades en sus datos de Time Series Insights.
* **Instancia del modelo de serie temporal**: Las instancias son la serie temporal en sí mismas. En la mayoría de los casos, son el **DeviceID** o **AssetID**, que es el identificador único del recurso en el entorno.

Para obtener más información sobre el modelo de serie temporal, consulte los [Modelos de serie temporal](./time-series-insights-update-tsm.md).

## <a name="preview-well"></a>Obtener una vista previa también

El área muestra los campos de instancia y otros metadatos asociados con las instancias seleccionadas de TSI. Las casillas de verificación a la derecha permiten ocultar o mostrar instancias específicas del gráfico actual. También puede quitar elementos de datos específicos de los datos actuales bien haciendo clic en el color rojo **eliminar** (Papelera) control en el lado izquierdo del elemento.

  [![La versión preliminar bien](media/v2-update-explorer/explorer-five.png)](media/v2-update-explorer/explorer-five.png#lightbox)

También puede volver a configurar el diseño de su **analizar** página seleccionando el icono de botón de puntos suspensivos en la parte superior derecha del gráfico:

  [![Opciones de diseño de datos de telemetría](media/v2-update-explorer/explorer-six.png)](media/v2-update-explorer/explorer-six.png#lightbox)

> [!NOTE]
> Si ve el siguiente mensaje, quiere decir que la instancia no tiene ningún dato durante el intervalo de tiempo seleccionado. Para resolver el problema, puede aumentar el intervalo de tiempo o confirmar que la instancia está enviando datos.
>
> ![Ninguna notificación de datos](media/v2-update-explorer/explorer-seven.png)

## <a name="preview-chart"></a>Vista previa del gráfico

Con el gráfico, puede mostrar las instancias TSI como líneas. Asimismo, puede contraer el panel de entorno, el modelo de datos y el panel de control de intervalos de tiempo haciendo clic en los controles web para agrandar el gráfico.

  [![Información general sobre el gráfico de vista previa](media/v2-update-explorer/explorer-eight.png)](media/v2-update-explorer/explorer-eight.png#lightbox)

1. **Rango de fecha seleccionado**: controla qué elementos de datos están disponibles para su visualización.

1. **Herramienta de control deslizante del intervalo de fechas internas**: use los dos controles de punto de conexión; para ello, arrástrelos sobre el intervalo de tiempo que quiera usar.

1. **Control para contraer los intervalos de tiempo**: contrae y expande el editor del panel de intervalos de tiempo.

1. **Control de formato del eje Y**: recorre las opciones de vista del eje Y disponibles:

    * `Default`: cada línea tiene un eje Y individual.
    * `Stacked`: Le permite apilar varias líneas en el mismo eje Y para que los datos del eje Y cambien en función de la línea seleccionada.
    * `Shared`: Todos los datos del eje Y se muestran juntos.

1. **Elemento de datos actual**: es el elemento de datos seleccionado actualmente y sus detalles asociados.

Puede profundizar aún más en un segmento de datos específico si hace clic con el botón izquierdo en un punto de datos del gráfico actual y arrastrando a continuación el área seleccionada al punto final que usted elija. Haga clic en el área sombreado, seleccionado y haga clic en **Zoom** tal como se muestra en esta imagen siguiente:

  [![Zoom del gráfico de vista previa](media/v2-update-explorer/explorer-nine.png)](media/v2-update-explorer/explorer-nine.png#lightbox)

Después de realizar la **Zoom** acción, verá el conjunto de datos seleccionado. Haga clic en el control de formato del eje Y para desplazarse por las tres representaciones del eje Y de sus datos de Time Series Insights.

  [![Vista previa del eje y del gráfico](media/v2-update-explorer/explorer-ten.png)](media/v2-update-explorer/explorer-ten.png#lightbox)

Aquí puede ver un ejemplo de ejes Y compartidos:

  [![Versión preliminar compartido del eje y](media/v2-update-explorer/explorer-eleven.png)](media/v2-update-explorer/explorer-eleven.png#lightbox)

## <a name="time-editor-panel"></a>Panel del editor de tiempo

Cuando trabaje con la versión preliminar de Time Series Insights, primero debe seleccionar un intervalo de tiempo. El intervalo de tiempo seleccionado controla el conjunto de datos que está disponible para la manipulación con los widgets de la versión preliminar de Time Series Insights. Los siguientes controles web están disponibles en la versión preliminar de Time Series Insights para que pueda seleccionar el intervalo de tiempo de trabajo.

  [![Panel de selección de tiempo](media/v2-update-explorer/explorer-twelve.png)](media/v2-update-explorer/explorer-twelve.png#lightbox)

1. **Herramienta de control deslizante del intervalo de fechas internas**: use los dos controles de punto de conexión; para ello, arrástrelos sobre el intervalo de tiempo que quiera usar. El control deslizante del intervalo de fechas externas se encarga de restringir el intervalo de fechas internas.

1. **Aumentar y disminuir los botones de intervalo de fechas**: puede aumentar o disminuir el intervalo de tiempo si selecciona cualquiera de los botones del intervalo que quiera.

1. **Control para contraer los intervalos de tiempo**: este control web le permite ocultar todos los controles, excepto la herramienta de control deslizante del intervalo de fechas de internas.

1. **Control deslizante del intervalo de fechas externas**: use los controles del punto de conexión para seleccionar el intervalo de fechas externas que estará disponible para el control de intervalo de fechas internas.

1. **Intervalo de fechas de tiempos rápidos desplegable**: Le permite alternar rápidamente entre las selecciones de intervalo de tiempo preestablecido, como la última **30 minutos**, **últimas 12 horas**, o un **intervalo personalizado**. Si cambia este valor también cambiará los intervalos disponibles que se describen en la herramienta de control deslizante para el tamaño de intervalos.

1. **Herramienta de control deslizante para el tamaño de intervalos**: le permite acercar y alejar los intervalos durante el mismo intervalo de tiempo. Esta acción le proporciona un control más preciso del movimiento entre grandes segmentos de tiempo. Asimismo, muestra tendencias uniformes hasta en intervalos tan pequeños como un milisegundo, lo que le permite ver partes detalladas y de alta resolución de los datos. El punto inicial predeterminado del control deslizante está establecido como la vista más óptima de los datos de la selección, lo que permite equilibrar la resolución, la velocidad de la consulta y la granularidad.

1. **Intervalo de fechas desde y hacia el control web**: Con este control web, fácilmente puede haga clic en y seleccionar intervalos de tiempo y en la fecha deseada. También puede usar el control para cambiar entre zonas horarias diferentes. Después de realizar los cambios, puede aplicarlos al área de trabajo actual si selecciona **Guardar**.

   [![Hacia y desde el panel de selección](media/v2-update-explorer/explorer-thirteen.png)](media/v2-update-explorer/explorer-thirteen.png#lightbox)

## <a name="navigation-panel"></a>Panel de navegación

El panel de navegación de la vista previa en tiempo Series Insights aparece en la parte superior de la aplicación TSI. Proporciona las siguientes funcionalidades.

### <a name="current-session-share-link-control"></a>Control de vínculo del recurso compartido de la sesión actual

  [![Icono de uso compartido](media/v2-update-explorer/explorer-fifteen.png)](media/v2-update-explorer/explorer-fifteen.png#lightbox)

Seleccione la nueva **compartir** icono para compartir un vínculo de dirección URL con el equipo.

  [![Compartir la dirección URL de la instancia](media/v2-update-explorer/url-share.png)](media/v2-update-explorer/url-share.png#lightbox)

### <a name="tenant-section"></a>Sección del inquilino

  [![Selección del inquilino](media/v2-update-explorer/explorer-sixteen.png)](media/v2-update-explorer/explorer-sixteen.png#lightbox)

* Muestra la información de cuenta de inicio de sesión actual de Time Series Insights.
* Le permite alternar entre los temas disponibles de Time Series Insights.
* Le permite ver la versión preliminar [aplicación web de demostración](https://insights.timeseries.azure.com/preview/demo).

### <a name="theme-selection"></a>Selección de temas

Para seleccionar un nuevo tema, haga clic en el icono de perfil que se encuentra en la esquina superior derecha. A continuación, seleccione **cambiar tema**.

  [![Selección de tema](media/v2-update-explorer/theme-selection.png)](media/v2-update-explorer/theme-selection.png#lightbox)

> [!TIP]
> Selección de idioma también está disponible al hacer clic en el icono del perfil.

La versión preliminar de Azure Time Series Insights admite dos temas:

* **Tema claro**: es el tema predeterminado que se muestra en este documento.
* **Tema oscuro**:  representa el explorador tal como se muestra aquí:

  [![Selecciona el tema oscuro](media/v2-update-explorer/explorer-seventeen.png)](media/v2-update-explorer/explorer-seventeen.png#lightbox)

## <a name="s1s2-environment-controls"></a>Controles de entorno S1/S2

### <a name="preview-terms-panel"></a>Panel de términos de vista previa

Esta sección se aplica solo a entornos S1/S2 existentes que intentan usar el explorador en la interfaz de usuario actualizada. Puede usar el producto de disponibilidad general y la versión preliminar juntos. Hemos agregado algunas funcionalidades desde la interfaz de usuario existente en el explorador actualizado, pero puede obtener la experiencia completa de la interfaz de usuario para el entorno de S1/S2 en el explorador de Time Series Insights existente.  

En lugar de la jerarquía, verá el panel de términos de Time Series Insights, donde podrá definir consultas en el entorno. Permite filtrar los datos según un predicado.

  [![Donde consultar el panel](media/v2-update-explorer/explorer-eighteen.png)](media/v2-update-explorer/explorer-eighteen.png#lightbox)

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

  [![En los ejemplos de cláusula](media/v2-update-explorer/explorer-nineteen.png)](media/v2-update-explorer/explorer-nineteen.png#lightbox)

**Medida**: Una lista desplegable que muestra todas las columnas numéricas (**dobles**) puede usar como elementos para el gráfico actual.

**Split by**: este menú desplegable muestra todas las columnas categóricas disponibles (Cadenas) en el modelo mediante las cuales puede agrupar sus datos. Puede agregar hasta cinco términos para verlos en el mismo eje X. Escriba los parámetros que quiera usar y, a continuación, seleccione **Agregar** para agregar un término nuevo.

  [![Vista filtrada y consultada uno](media/v2-update-explorer/explorer-twenty.png)](media/v2-update-explorer/explorer-twenty.png#lightbox)

Puede mostrar y ocultar elementos en el panel del gráfico si selecciona el icono visible tal como se muestra en la siguiente imagen. Puede quitar completamente las consultas, haga clic en el color rojo **X**.

  [![Vista filtrada y consultada dos](media/v2-update-explorer/explorer-twenty-one.png)](media/v2-update-explorer/explorer-twenty-one.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre [almacenamiento y la entrada](./time-series-insights-update-storage-ingress.md) en la versión preliminar de Azure tiempo Series Insights.

- Lea el documento de vista previa en tiempo Series Insights [modelado de datos](./time-series-insights-update-tsm.md).

- Obtenga información sobre [cómo diagnosticar y solucionar problemas](./time-series-insights-update-how-to-troubleshoot.md) su instancia de Time Series Insights.
