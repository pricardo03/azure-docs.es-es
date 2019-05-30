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
ms.date: 05/03/2019
ms.custom: seodec18
ms.openlocfilehash: c4f3053063ce33d2777387da2c53effd61b05f1a
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399875"
---
# <a name="visualize-data-in-the-explorer-preview"></a>Visualización de datos en la versión preliminar del explorador

Este documento describe la interfaz de usuario y las características de experiencia de usuario y la interfaz de la versión preliminar de Azure tiempo Series Insights [aplicación web de demostración](https://insights.timeseries.azure.com/preview/demo). En concreto, describe el diseño del ejemplo hospedada, opciones de personalización de la interfaz y la navegación a través de la demostración proporcionada.

## <a name="prerequisites"></a>Requisitos previos

Para empezar a trabajar con el Explorador de vista previa de Azure tiempo Series Insights, debe:

* Configurar el entorno de Time Series Insights. Para obtener más información sobre cómo aprovisionar una instancia, pruebe el [versión preliminar de Azure tiempo Series Insights](./time-series-insights-update-create-environment.md) tutorial.
* [Proporcionar acceso a datos](./time-series-insights-data-access.md) en el entorno de Time Series Insights que ha creado para la cuenta. Puede proporcionar acceso al resto de usuarios y a usted mismo.
* Agregar un origen de eventos para el entorno de Time Series Insights para insertar datos en el entorno:
  * Obtenga información sobre [cómo conectarse a un centro de eventos](./time-series-insights-how-to-add-an-event-source-eventhub.md).
  * Obtenga información sobre [cómo conectarse a un centro de IoT](./time-series-insights-how-to-add-an-event-source-iothub.md).

## <a name="learn-about-the-preview-explorer"></a>Obtenga información sobre el Explorador de vista previa

El explorador de la versión preliminar de Azure Time Series Insights consta de los siguientes elementos:

[![La vista del explorador](media/v2-update-explorer/explorer-one.png)](media/v2-update-explorer/explorer-one.png#lightbox)

- <a href="#environment-drop-down-list">Panel entorno</a>: Muestra los entornos de Azure Time Series Insights.
- <a href="#navigation-menu">Menú de navegación</a>: Usar para cambiar entre el **analizar** y **modelo** páginas.
- <a href="#hierarchy-tree">Árbol de jerarquía</a>: Usar para seleccionar elementos de modelo y datos específicos que se muestran en el gráfico.
- <a href="#preview-well">Fuente de serie temporal</a>: Muestra los elementos de datos actualmente seleccionado en formato de tabla con codificación en colores.
- <a href="#preview-chart">Panel del gráfico</a>: muestra el gráfico de trabajo actual.
- <a href="#time-editor-panel">Escala de tiempo</a>: Usar para modificar el intervalo de tiempo de trabajo.
- <a href="#navigation-panel">Barra de la aplicación</a>: Contiene las opciones de administración de usuario, como el inquilino actual. Puede usarlo para cambiar la configuración de tema y de idioma.

## <a name="environment-drop-down-list"></a>Lista desplegable de entorno

La lista desplegable de entorno muestra que tiene acceso a todos los entornos de Time Series Insights. La lista incluye los entornos de pago por uso, como la versión preliminar de tiempo Series Insights. La lista también incluye entornos S1/S2, que están disponibles con carácter general.

1. Seleccione la flecha desplegable situada junto a su entorno mostrada.

   [![El panel de control](media/v2-update-explorer/explorer-two.png)](media/v2-update-explorer/explorer-two.png#lightbox)

1. A continuación, seleccione el entorno que desea.

## <a name="navigation-menu"></a>Menú de navegación

  [![El menú de navegación](media/v2-update-explorer/explorer-three.png)](media/v2-update-explorer/explorer-three.png#lightbox)

Use el menú de navegación para seleccionar entre dos vistas:

* **Analizar**: Usar para el gráfico y realizar análisis exhaustivos en datos de series temporales modelado o no modelados.
* **Modelo**: Usar para insertar nuevos tipos, jerarquías e instancias de vista previa en tiempo Series Insights en el modelo de serie temporal.

## <a name="hierarchy-tree"></a>árbol de jerarquía

El árbol de jerarquía muestra los elementos de datos seleccionado que incluyen modelos de dispositivos específicos y sensores en los dispositivos.

### <a name="model-search-panel"></a>Panel de búsqueda del modelo

Puede usar el panel de búsqueda del modelo para buscar fácilmente y navegar por la jerarquía de modelo de serie temporal para encontrar las instancias de la serie de tiempo específico que desea mostrar en el gráfico. Después de seleccionar las instancias, se agregan también en el gráfico actual y los datos.

  [![El panel de búsqueda del modelo](media/v2-update-explorer/explorer-four.png)](media/v2-update-explorer/explorer-four.png#lightbox)

### <a name="model-authoring"></a>Creación de modelos

El admite vista previa de Azure tiempo Series Insights completa crea, lee, actualizar y elimina operaciones (CRUD) en el modelo de serie temporal.

* **Tipo de modelo de serie temporal**: Puede usar tipos de Time Series Insights para definir las variables o fórmulas para realizar cálculos. Que están asociados con una determinada instancia de Time Series Insights. Un tipo puede tener una o más variables.
* **Jerarquía del modelo de serie temporal**: Las jerarquías son modos de organizar sus datos de forma sistemática. Asimismo, las jerarquías representan las relaciones entre diferentes entidades en sus datos de Time Series Insights.
* **Instancia del modelo de serie temporal**: Las instancias son la serie temporal en sí mismas. En la mayoría de los casos, son el **DeviceID** o **AssetID**, que es el identificador único del recurso en el entorno.

Para obtener más información sobre el modelo de serie temporal, consulte los [Modelos de serie temporal](./time-series-insights-update-tsm.md).

## <a name="preview-well"></a>Obtener una vista previa también

El área muestra los campos de instancia y otros metadatos asociados con las instancias seleccionadas de Time Series Insights. Seleccionando las casillas de verificación en el lado derecho, puede ocultar o mostrar instancias específicas del gráfico actual. También puede quitar elementos de datos específicos de los datos actuales bien seleccionando el rojo **eliminar** (Papelera) control en el lado izquierdo del elemento.

  [![La versión preliminar bien](media/v2-update-explorer/explorer-five.png)](media/v2-update-explorer/explorer-five.png#lightbox)

Para volver a configurar el diseño de su **analizar** página del gráfico, seleccione el icono de botón de puntos suspensivos en la esquina superior derecha:

  [![Opciones de diseño de datos de telemetría](media/v2-update-explorer/explorer-six.png)](media/v2-update-explorer/explorer-six.png#lightbox)

> [!NOTE]
> Si ve el mensaje siguiente, la instancia no tiene ningún dato durante el intervalo de tiempo seleccionado. Para resolver el problema, aumente el intervalo de tiempo o confirmar que la instancia está insertando datos.
>
> ![Ninguna notificación de datos](media/v2-update-explorer/explorer-seven.png)

## <a name="preview-chart"></a>Vista previa del gráfico

Con el gráfico, puede mostrar las instancias de Time Series Insights como líneas. Puede contraer el panel de entorno, el modelo de datos y el panel de control de intervalo de tiempo seleccionando los controles web para aumentar el tamaño del gráfico.

  [![Información general sobre el gráfico de vista previa](media/v2-update-explorer/explorer-eight.png)](media/v2-update-explorer/explorer-eight.png#lightbox)

- **Intervalo de fecha seleccionado**: controla qué elementos de datos están disponibles para su visualización.

- **Herramienta de control deslizante de intervalo de fecha interna**: Use los dos controles de punto de conexión arrastrándolos durante el intervalo de tiempo que desee.

- **Control para contraer los intervalos de tiempo**: contrae y expande el editor del panel de intervalos de tiempo.

- **Control de formato del eje Y**: Recorre el eje disponible y ver las opciones:

    * `Default`: Cada línea tiene un eje y individual.
    * `Stacked`: Puede usarlo para apilar varias líneas en el mismo eje y con los datos del eje y que cambian en función de la línea seleccionada.
    * `Shared`: Todos los datos del eje y muestra juntos.

- **Elemento de datos actual**: es el elemento de datos seleccionado actualmente y sus detalles asociados.

Para profundizar aún más en un segmento de datos específico, haga clic en un punto de datos en el gráfico actual y, a continuación, arrastre el área seleccionada al punto de conexión de su elección. Haga clic en el área gris seleccionada y seleccione **Zoom**, tal y como se muestra en esta imagen siguiente:

  [![Zoom del gráfico de vista previa](media/v2-update-explorer/explorer-nine.png)](media/v2-update-explorer/explorer-nine.png#lightbox)

Después de realizar la **Zoom** acción, verá los datos seleccionados establecido. Seleccione el control de formato del eje y para desplazarse por las representaciones de eje y tres de los datos de Time Series Insights.

  [![Eje y del gráfico de vista previa](media/v2-update-explorer/explorer-ten.png)](media/v2-update-explorer/explorer-ten.png#lightbox)

En este caso, puede ver un ejemplo de los ejes compartidos:

  [![Obtener una vista previa de los ejes compartidos](media/v2-update-explorer/explorer-eleven.png)](media/v2-update-explorer/explorer-eleven.png#lightbox)

## <a name="time-editor-panel"></a>Panel del editor de tiempo

Cuando trabaje con la versión preliminar de Time Series Insights, primero debe seleccionar un intervalo de tiempo. El intervalo de tiempo seleccionado controla el conjunto de datos que está disponible para la manipulación de los widgets de vista previa en tiempo Series Insights. Los controles web siguientes están disponibles en versión preliminar de información de serie de tiempo para seleccionar el intervalo de tiempo de trabajo:

  [![Panel de selección de hora](media/v2-update-explorer/explorer-twelve.png)](media/v2-update-explorer/explorer-twelve.png#lightbox)

1. **Herramienta de control deslizante de intervalo de fecha interna**: Use los dos controles de punto de conexión arrastrándolos durante el intervalo de tiempo que desee. Este intervalo de fechas interna está restringido por el control deslizante de intervalo de fecha externa.

1. **Aumentar y disminuir los botones de intervalo de fechas**: puede aumentar o disminuir el intervalo de tiempo si selecciona cualquiera de los botones del intervalo que quiera.

1. **Control para contraer los intervalos de tiempo**: Este control web le permite ocultar todos los controles, excepto la herramienta de control deslizante de intervalo de fecha interna.

1. **Control deslizante de intervalo de fecha externa**: Utilice los controles de punto de conexión para seleccionar el intervalo de fechas externa, que estará disponible para el control de intervalo de fecha interna.

1. **Lista desplegable de intervalo de fechas de tiempos rápidos**: Usar para cambiar rápidamente entre las selecciones de intervalo de tiempo preestablecido, como la última **30 minutos**, **últimas 12 horas**, o un **intervalo personalizado**. Si cambia este valor también cambiará los intervalos disponibles que se describen en la herramienta de control deslizante para el tamaño de intervalos.

1. **Herramienta de control deslizante para el tamaño de intervalos**: Usarlo para acercar dentro y fuera de intervalos durante el mismo intervalo de tiempo especificado. Esta acción le proporciona un control más preciso del movimiento entre grandes segmentos de tiempo. Muestra tendencias uniformes hasta en intervalos tan pequeños como un milisegundo. Puede usarlo para ver cortes granulares y de alta resolución de los datos. El punto inicial predeterminado del control deslizante está establecido como la vista más óptima de los datos de la selección, lo que permite equilibrar la resolución, la velocidad de la consulta y la granularidad.

1. **Control de a y desde la web de intervalo de fecha**: Con este control web, puede seleccionar fácilmente los intervalos de fecha y hora en que desee. También puede usar el control para cambiar entre zonas horarias diferentes. Después de realizar los cambios que se aplican al área de trabajo actual, seleccione **guardar**.

   [![Hacia y desde el panel de selección](media/v2-update-explorer/explorer-thirteen.png)](media/v2-update-explorer/explorer-thirteen.png#lightbox)

## <a name="navigation-panel"></a>Panel de navegación

El panel de navegación de la vista previa en tiempo Series Insights aparece en la parte superior de la aplicación de Time Series Insights. Proporciona las siguientes funcionalidades.

### <a name="current-session-share-link-control"></a>Control de vínculo del recurso compartido de la sesión actual

  [![Icono de uso compartido](media/v2-update-explorer/explorer-fifteen.png)](media/v2-update-explorer/explorer-fifteen.png#lightbox)

Seleccione la nueva **compartir** icono para compartir un vínculo de dirección URL con el equipo.

  [![Compartir la dirección URL de la instancia](media/v2-update-explorer/url-share.png)](media/v2-update-explorer/url-share.png#lightbox)

### <a name="tenant-section"></a>Sección del inquilino

  [![Selección del inquilino](media/v2-update-explorer/explorer-sixteen.png)](media/v2-update-explorer/explorer-sixteen.png#lightbox)

* Muestra la información de inicio de sesión de cuenta actual de Time Series Insights.
* Usar para cambiar entre los temas disponibles de Time Series Insights.
* Para ver la versión preliminar [aplicación web de demostración](https://insights.timeseries.azure.com/preview/demo).

### <a name="theme-selection"></a>Selección de temas

Para seleccionar un nuevo tema, seleccione el icono situado en la esquina superior derecha del perfil. A continuación, seleccione **cambiar tema**.

  [![Selección de tema](media/v2-update-explorer/theme-selection.png)](media/v2-update-explorer/theme-selection.png#lightbox)

> [!TIP]
> Selección de idioma también está disponible si selecciona el icono del perfil.

La versión preliminar de Azure Time Series Insights admite dos temas:

* **El tema claro**: es el tema predeterminado que se muestra en este documento.
* **Tema oscuro**: representa el explorador tal como se muestra aquí:

  [![Selecciona el tema oscuro](media/v2-update-explorer/explorer-seventeen.png)](media/v2-update-explorer/explorer-seventeen.png#lightbox)

## <a name="s1s2-environment-controls"></a>Controles de entorno S1/S2

### <a name="preview-terms-panel"></a>Panel de términos de vista previa

Esta sección se aplica solo a entornos S1/S2 existentes que intentan usar el explorador en la interfaz de usuario actualizada. Puede usar el producto disponible con carácter general y vista previa en combinación. Hemos agregado algunas funcionalidades desde la interfaz de usuario existente en el explorador actualizado, pero puede obtener la experiencia completa de la interfaz de usuario para el entorno de S1/S2 en el explorador de Time Series Insights existente. 

En lugar de la jerarquía, consulte el panel de términos de Time Series Insights, donde definir consultas en su entorno. Usar para filtrar los datos según un predicado.

  [![Donde consultar el panel](media/v2-update-explorer/explorer-eighteen.png)](media/v2-update-explorer/explorer-eighteen.png#lightbox)

El panel del editor de términos de la versión preliminar de Time Series Insights toma los parámetros siguientes:

**Where**: Usar where cláusula para filtrar rápidamente los eventos con el conjunto de operandos que se muestra en la tabla siguiente. Si realiza una búsqueda seleccionando un operando, el predicado se actualizará automáticamente según esa búsqueda. Los tipos de operando compatibles incluyen:

| Operación | Tipos admitidos   | Notas |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` | Double, DateTime, TimeSpan | |
| `=`, `!=`, `<>` | String, Bool, Double, DateTime, TimeSpan, NULL |
| `IN` | String, Bool, Double, DateTime, TimeSpan, NULL | Todos los operandos deben ser del mismo tipo o deben ser una constante NULL. |
| `HAS` | string | Literales de cadena constante solo se permiten en el lado derecho. No se permiten cadenas vacías ni NULL. |

Para obtener más información acerca de las operaciones de consulta admitidos y tipos de datos, vea [expresión de la serie de tiempo (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx).

### <a name="examples-of-where-clauses"></a>Ejemplos de dónde cláusulas

  [![En los ejemplos de cláusula](media/v2-update-explorer/explorer-nineteen.png)](media/v2-update-explorer/explorer-nineteen.png#lightbox)

**Medida**: Una lista desplegable que muestra todas las columnas numéricas (**dobles**) puede usar como elementos para el gráfico actual.

**Split by**: Esta lista desplegable muestra todas las categorías columnas disponibles (cadenas) en el modelo que puede agrupar los datos. Puede agregar hasta cinco términos para verlos en el mismo eje x. Escriba los parámetros que desee y, a continuación, seleccione **agregar** para agregar un término nuevo.

  [![Vista filtrada y consultada uno](media/v2-update-explorer/explorer-twenty.png)](media/v2-update-explorer/explorer-twenty.png#lightbox)

Puede mostrar y ocultar elementos en el panel de gráfico, seleccione el icono visible, como se muestra en la siguiente imagen. Para quitar completamente las consultas, seleccione el color rojo **X**.

  [![Vista filtrada y consultada dos](media/v2-update-explorer/explorer-twenty-one.png)](media/v2-update-explorer/explorer-twenty-one.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre [almacenamiento y la entrada](./time-series-insights-update-storage-ingress.md) en la versión preliminar de Azure tiempo Series Insights.
- Lea el documento de vista previa en tiempo Series Insights [modelado de datos](./time-series-insights-update-tsm.md).
- Obtenga información sobre [cómo diagnosticar y solucionar problemas](./time-series-insights-update-how-to-troubleshoot.md) su instancia de Time Series Insights.
