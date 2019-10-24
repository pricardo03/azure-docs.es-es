---
title: Flujos de datos de asignación en Azure Data Factory | Microsoft Docs
description: Introducción a los flujos de datos de asignación en Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/7/2019
ms.openlocfilehash: 37fec388acda78f3d13c8e85ddddf780ad099d69
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387993"
---
# <a name="what-are-mapping-data-flows"></a>¿Qué son los flujos de datos de asignación?

La asignación de flujos de datos es una transformación de datos diseñada visualmente en Azure Data Factory. Los flujos de datos permiten a los ingenieros de datos desarrollar una lógica de transformación de datos gráfica sin necesidad de escribir código. Los flujos de datos resultantes se ejecutan como actividades en las canalizaciones de Azure Data Factory mediante clústeres de Spark de escalabilidad horizontal. Las actividades de flujo de datos pueden ponerse en marcha mediante las funcionalidades de programación, control, flujo y supervisión existentes en Data Factory.

La asignación de flujos de datos proporciona una experiencia completamente visual que no requiere programación. Los flujos de datos se ejecutarán en su propio clúster de ejecución durante el procesamiento de datos de escalabilidad horizontal. Asimismo, Azure Data Factory controla toda la traducción de código, la optimización de rutas de acceso y la ejecución de trabajos de flujo de datos.

## <a name="getting-started"></a>Introducción

Para crear un flujo de datos, haga clic en el signo más de Factory Resources (Recursos de fábrica). 

![nuevo flujo de datos](media/data-flow/newdataflow2.png "nuevo flujo de datos")

De este modo, accederá al lienzo de flujo de datos, donde podrá crear la lógica de transformación. Haga clic en el cuadro "Agregar origen" para comenzar a configurar la transformación de origen. Para más información, consulte [Transformación de origen](data-flow-source.md).

## <a name="data-flow-canvas"></a>Lienzo de flujo de datos

El lienzo de flujo de datos está dividido en tres partes: la barra superior, el gráfico y el panel de configuración. 

![Lienzo](media/data-flow/canvas1.png "Lienzo")

### <a name="graph"></a>Grafo

En el gráfico se muestra el flujo de transformación. Muestra el linaje de los datos de origen a medida que fluyen hacia uno o varios receptores. Para agregar un nuevo origen, haga clic en el cuadro "Agregar origen". Para agregar una nueva transformación, haga clic en el signo más situado en la parte inferior derecha de una transformación existente.

![Lienzo](media/data-flow/canvas2.png "Lienzo")

### <a name="configuration-panel"></a>Panel de configuración

En el panel configuración, se muestra la configuración específica de la transformación seleccionada actualmente o, si no se ha seleccionado ninguna transformación, el flujo de datos. En la configuración general del flujo de datos, puede modificar el nombre y la descripción en la pestaña **General** o agregar parámetros en la pestaña **Parámetros**. Para más información, consulte [Parámetros de asignación de Data Flow](parameters-data-flow.md).

Cada transformación tiene al menos cuatro pestañas de configuración:

#### <a name="transformation-settings"></a>Configuración de la transformación

La primera pestaña del panel de configuración de cada transformación contiene los valores específicos de esa transformación. Para más información, consulte la página de documentación de la transformación.

![Pestaña de configuración de origen](media/data-flow/source1.png "Pestaña de configuración de origen")

#### <a name="optimize"></a>Optimizar

La pestaña _Optimizar_ contiene valores opcionales para configurar los esquemas de partición.

![Optimize](media/data-flow/optimize1.png "Optimizar") (Optimizar)

El ajuste predeterminado es "current partitioning" (Usar particiones actuales), que le indica a Azure Data Factory que use el esquema de partición nativo en los flujos de datos que se ejecutan en Spark. En la mayoría de los escenarios, este valor es el recomendado.

Hay casos en los que es posible que desee ajustar la creación de particiones. Por ejemplo, si desea que el resultado de las transformaciones llegue a un único archivo del lago, elija "Partición única" en la transformación del receptor.

Otro caso en el que es probable que desee controlar los esquemas de partición es para optimizar el rendimiento. Ajustar las particiones permite controlar la distribución de los datos entre los nodos de proceso y las optimizaciones de la localidad de los datos, lo que puede tener efectos tanto positivos como negativos en el rendimiento general de los flujos de datos. Para más información, consulte la [guía del rendimiento de Data Flow](concepts-data-flow-performance.md).

Para cambiar las particiones de cualquier transformación, solo tiene que hacer clic en la pestaña Optimizar y seleccionar el botón de radio "Set Partitioning" (Establecer particiones). Aparecerán una serie de opciones relacionadas con la creación de particiones. El mejor método para crear particiones dependerá de los volúmenes de datos, las claves candidatas, los valores nulos y la cardinalidad. El procedimiento recomendado consiste en comenzar con el proceso de creación de particiones predeterminado y probar después diferentes opciones. Puede probar las ejecuciones de depuración de canalizaciones y ver el tiempo que tarda y el uso de las particiones de cada agrupación en la vista Supervisión. Para más información, consulte [Supervisión de flujos de datos](concepts-data-flow-monitoring.md).

A continuación, se muestran las opciones disponibles relacionadas con la creación de particiones.

##### <a name="round-robin"></a>Round Robin 

Round Robin es una partición simple que distribuye automáticamente los datos equitativamente entre las distintas particiones. Use Round Robin cuando los candidatos principales no sean lo suficientemente buenos como para implementar una estrategia de creación de particiones sólida e inteligente. Puede establecer el número de particiones físicas.

##### <a name="hash"></a>Hash

Azure Data Factory generará un valor hash de las columnas para generar particiones uniformes, con el fin de que las filas con valores similares estarán en la misma partición. Si usa la opción Hash, pruebe si hay una posible distorsión de las particiones. Puede establecer el número de particiones físicas.

##### <a name="dynamic-range"></a>Intervalo dinámico

Intervalo dinámico usará los intervalos dinámicos de Spark en función de las columnas o expresiones que proporcione. Puede establecer el número de particiones físicas. 

##### <a name="fixed-range"></a>Intervalo fijo

Cree una expresión que proporcione un intervalo de valores fijo en las columnas de datos con particiones. Para evitar la distorsión de la partición, es preciso que conozca perfectamente los datos antes de usar esta opción. El valor que especifique para la expresión se usará como parte de una función de partición. Puede establecer el número de particiones físicas.

##### <a name="key"></a>Clave

Si conoce bien la cardinalidad de los datos, la creación de particiones clave puede ser una buena estrategia de partición. La creación de particiones clave creará particiones para cada valor único de la columna. No puede establecer el número de particiones porque dicho número dependerá de valores únicos de los datos.

#### <a name="inspect"></a>Inspeccionar

La pestaña _Inspeccionar_ proporciona una vista de los metadatos del flujo de datos que se está transformando. Puede ver el número de columnas, las columnas que han cambiado, las columnas que se han agregado, los tipos de datos, el orden de las columnas y las referencias de las columnas. "Inspeccionar" es una vista de solo lectura de los metadatos. Para ver los metadatos en el panel Inspeccionar, no es preciso que el modo de depuración esté habilitado.

![Inspeccionar](media/data-flow/inspect1.png "Inspeccionar")

Al cambiar la forma de los datos mediante transformaciones, verá que los cambios de los metadatos fluyen por el panel Inspeccionar. Si no hay un esquema definido en la transformación de origen, los metadatos no estarán visibles en el panel Inspeccionar. La falta de metadatos es habitual en escenarios de desviación en el esquema.

#### <a name="data-preview"></a>Vista previa de datos

Si el modo de depuración está activado, en la pestaña _Vista previa de los datos_, podrá ver una instantánea interactiva de los datos en cada transformación. Para más información, consulte [Vista previa de los datos en modo de depuración](concepts-data-flow-debug-mode.md#data-preview).

### <a name="top-bar"></a>Barra superior

La barra superior contiene acciones que afectan a todo el flujo de datos, como la operación de guardar y la validación. Puede alternar entre el modo de gráfico y el modo de configuración con los botones **Mostrar gráfico** y **Ocultar gráfico**.

![Ocultar gráfico](media/data-flow/hideg.png "Ocultar gráfico")

Si oculta el gráfico, podrá navegar lateralmente por los nodos de transformación con los botones **anterior** y **siguiente**.

![Navegar](media/data-flow/showhide.png "navegar")

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a crear una [transformación de origen](data-flow-source.md).
* Aprenda a crear flujos de datos en [modo de depuración](concepts-data-flow-debug-mode.md).
