---
title: 'Información general: Azure Machine Learning Studio | Microsoft Docs'
description: Información general de Machine Learning Studio, una herramienta de arrastrar y colocar para crear rápidamente modelos desde una biblioteca lista para usar de algoritmos y módulos.
keywords: azure machine learning, machine learning studio
services: machine-learning
documentationcenter: ''
author: garyericson
ms.custom: seodec18
ms.author: garye
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/28/2018
ms.openlocfilehash: b1c0d3da6dba4bf56baaf583d89c58fdc13ef83f
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53250525"
---
# <a name="what-is-azure-machine-learning-studio"></a>¿Qué es Azure Machine Learning Studio?
Microsoft Azure Machine Learning Studio es una herramienta de arrastrar y colocar que le permite crear, probar e implementar soluciones de análisis predictivo en sus datos. Machine Learning Studio publica modelos como servicios web que pueden utilizarse fácilmente en aplicaciones personalizadas o herramientas de BI como Excel.

Machine Learning Studio es el lugar en el que confluyen la ciencia de datos, el análisis predictivo, los recursos en la nube y sus datos.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>El área de trabajo interactivo de Machine Learning Studio
Para desarrollar un modelo de análisis predictivo, normalmente se utilizan datos de una o varias fuentes, se transforman y analizan los datos a través de diversas funciones estadísticas y de manipulación de datos y se genera un conjunto de resultados. Desarrollar un modelo como este es un proceso iterativo: a medida que se modifican las diversas funciones y sus parámetros, sus resultados convergen hasta que esté satisfecho con un modelo entrenado y efectivo.

**Azure Machine Learning Studio** le proporciona un área de trabajo visual e interactiva para generar, probar e iterar con toda facilidad sobre un modelo de análisis predictivo. Se arrastran y colocan ***conjuntos de datos*** y ***módulos*** de análisis en un lienzo interactivo, conectándolos todos para formar un ***experimento*** que se ejecuta en Machine Learning Studio. Para iterar su diseño de modelo, se puede editar el experimento, guardar una copia si así se desea y ejecutarlo de nuevo. Cuando esté listo, puede convertir el ***experimento de entrenamiento*** en un ***experimento predictivo***, y luego publicarlo como ***servicio web*** para que otros usuarios puedan acceder al modelo.

No se requiere ningún tipo de programación, basta con conectar visualmente conjuntos de datos y módulos para construir el modelo de análisis predictivo.

> [!TIP]
> Para descargar e imprimir un diagrama con información general de las funcionalidades de Machine Learning Studio, vea [Diagrama de información general de las funcionalidades de Azure Machine Learning Studio](studio-overview-diagram.md).
> 
> 

![Diagrama de Azure ML Studio: crear experimentos, leer datos de muchos orígenes, escribir datos puntuados, modelos de escritura.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Introducción a Machine Learning Studio
La primera vez que entre en [Machine Learning Studio](https://studio.azureml.net), verá la página **principal** . Desde aquí puede ver documentación, vídeos y seminarios web, y encontrar otros recursos valiosos.

Haga clic en el menú de la parte superior izquierda ![Menú](./media/what-is-ml-studio/menu.png) y verá varias opciones.

### <a name="cortana-intelligence"></a>Cortana Intelligence
Haga clic en **Cortana Intelligence**, se le dirigirá a la página principal de [Cortana Intelligence Suite](https://www.microsoft.com/cloud-platform/cortana-intelligence-suite). Cortana Intelligence Suite es un conjunto de análisis avanzados y macrodatos totalmente administrado que le permite transformar los datos en acciones inteligentes. Consulte la página de inicio del conjunto de aplicaciones para ver la documentación completa, incluidas las historias de los clientes.

### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
Aquí hay dos opciones: **Inicio**, la página donde ha comenzado, y **Studio**.

Haga clic en **Studio**, se le dirigirá a la **Azure Machine Learning Studio**. Primero se le pedirá que inicie sesión con su cuenta de Microsoft o su cuenta profesional o educativa. Después de iniciar sesión, verá las pestañas siguientes a la izquierda:

* **PROYECTOS** : colecciones de experimentos, conjuntos de datos, cuadernos y otros recursos que representan un proyecto individual
* **EXPERIMENTOS**: experimentos que ha creado y ejecutado, o que ha guardado como borrador.
* **SERVICIOS WEB** : servicios web que implementó a partir de los experimentos
* **CUADERNOS** : cuadernos de Jupyter que creó
* **CONJUNTOS DE DATOS** : conjuntos de datos que cargó en Estudio
* **MODELOS ENTRENADOS** : modelos que entrenó en experimentos y guardó en Estudio
* **CONFIGURACIÓN** : una colección de ajustes que puede utilizar para configurar la cuenta y los recursos.

### <a name="gallery"></a>Gallery
Haga clic en **Galería** y se le dirigirá a la **[Galería de Azure AI](http://gallery.cortanaintelligence.com/)**. La Galería es un lugar donde una comunidad de científicos de datos y desarrolladores puede compartir soluciones creadas con componentes de Cortana Intelligence Suite.

Para más información acerca de la galería, consulte [Uso compartido y descubrimiento de soluciones en la Galería de Azure AI](gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Componentes de un experimento
Un experimento consta de conjuntos de datos que proporcionan datos a módulos analíticos, que se conectan en conjunto para construir un modelo de análisis predictivo. En concreto, un experimento válido tiene estas características:

* Tiene al menos un conjunto de datos y un módulo.
* Los conjuntos de datos pueden estar solo conectados a módulos.
* Los módulos pueden conectarse a conjuntos de datos o a otros módulos.
* Todos los puertos de entrada de los módulos deben tener alguna conexión al flujo de datos.
* Deben establecerse todos los parámetros necesarios para cada módulo.

Puede crear un experimento desde cero, o puede usar un experimento de ejemplo existente como plantilla. Para más información, consulte [Copia de experimentos de ejemplo para crear nuevos experimentos de aprendizaje automático](sample-experiments.md).

Para obtener un ejemplo de creación de un experimento simple, consulte [Creación de un experimento sencillo en Azure Machine Learning Studio](create-experiment.md).

Para obtener un tutorial más completo de la creación de una solución de análisis predictivos, consulte [Desarrollo de una solución predictiva con Azure Machine Learning](walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Conjuntos de datos
Un conjunto de datos son datos que se han cargado en Machine Learning Studio para utilizarse en el proceso de modelado. Machine Learning Studio incluye varios conjuntos de datos de ejemplo con los que puede experimentar, y puede cargar más a medida que los necesite. A continuación se muestran algunos ejemplos de los conjuntos de datos incluidos:

* **Datos sobre consumo de combustible por distancia recorrida para varios automóviles** : valores sobre consumo de combustible por distancia recorrida para automóviles identificados por el número de cilindros, los caballos de potencia, etc.
* **Datos sobre cáncer de mama** : datos de diagnóstico de cáncer de mama.
* **Datos de incendios forestales** : magnitud de los incendios forestales en el noreste de Portugal.

Cuando crea un experimento, puede elegir un conjunto de datos en la lista de conjuntos de datos disponibles a la izquierda del lienzo.

Para obtener una lista de conjuntos de datos de ejemplo incluidos en Machine Learning Studio, vea [Uso de los conjuntos de datos de muestra en Azure Machine Learning Studio](use-sample-datasets.md).

### <a name="modules"></a>Módulos
Un módulo es un algoritmo que puede aplicar sobre sus datos. Machine Learning Studio cuenta con diversos módulos que van desde las funciones de incorporación de datos hasta procesos de entrenamiento, puntuación y validación. A continuación se muestran algunos ejemplos de los módulos incluidos:

* [Convertir a ARFF][convert-to-arff]: convierte un conjunto de datos serializados de .NET a formato ARFF.
* [Estadísticas elementales de procesos][elementary-statistics]: calcula estadísticas elementales como la media, la desviación estándar, etc.
* [Regresión lineal][linear-regression]: crea un modelo de regresión lineal basado en un descenso de gradiente en línea.
* [Puntuar modelo][score-model]: puntúa un modelo entrenado de clasificación o regresión.

Cuando crea un experimento, puede elegir un módulo en la lista de módulos disponibles a la izquierda del lienzo.  

Un módulo puede tener un conjunto de parámetros que puede utilizar para configurar los algoritmos internos del módulo. Al seleccionar un módulo en el lienzo, los parámetros del módulo se muestran en el panel **Propiedades** a la derecha del lienzo. Puede modificar los parámetros en ese panel para ajustar su modelo.

Para que le resulte más fácil navegar por la gran biblioteca de algoritmos de aprendizaje automático, vea [Selección de algoritmos para Microsoft Azure Machine Learning](algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Implementación del servicio web de análisis predictivo
Cuando el modelo de análisis predictivo esté listo, puede implementarlo como servicio web directamente desde Machine Learning Studio. Para obtener más información vea [Implementación de un servicio web de Azure Machine Learning](publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/what-is-ml-studio/azure-ml-studio-diagram.jpg



## <a name="key-machine-learning-terms-and-concepts"></a>Términos clave del aprendizaje automático
La terminología del aprendizaje automático puede resultar confusa. Aquí se muestran definiciones de términos clave que le ayudarán. Use los comentarios que aparecen a continuación para indicarnos cualquier otro término del que le gustaría ver una definición.

### <a name="data-exploration-descriptive-analytics-and-predictive-analytics"></a>Exploración de datos, análisis descriptivo y análisis predictivos

**Exploración de datos** es el proceso de recopilación de información sobre un conjunto de datos grande y a menudo no estructurado para encontrar características para análisis más detallados.

**Minería de datos** hace referencia a la exploración de datos automatizada.

**Análisis descriptivo** es el proceso de análisis de un conjunto de datos con el fin de resumir lo que sucedió. La inmensa mayoría de análisis de negocios, como los informes de ventas, las métricas de web y el análisis de redes sociales, son descriptivos.

**Análisis predictivos** es el proceso de creación de modelos de datos históricos o actuales con el fin de pronosticar resultados futuros.

### <a name="supervised-and-unsupervised-learning"></a>Aprendizaje supervisado y no supervisado
 **aprendizaje supervisado** se ha entrenado con datos etiquetados; es decir, con datos formados por ejemplos de las respuestas deseadas. Por ejemplo, se podría preparar un modelo que identifique el uso de tarjetas de crédito fraudulentas desde un conjunto de datos en el cual se etiqueten los puntos de datos que indiquen cargos fraudulentos y cargos válidos conocidos. Se supervisa la mayoría del aprendizaje automático.

 **aprendizaje no supervisado** se utiliza en los datos sin etiquetas y el objetivo es buscar relaciones en los datos. Por ejemplo, puede querer buscar agrupaciones de datos demográficos del cliente con hábitos de compra similares.

### <a name="model-training-and-evaluation"></a>Entrenamiento y evaluación de modelos
Un modelo de aprendizaje automático es una abstracción de la pregunta que intenta responder o el resultado que desea predecir. Modelos se entrenan y se evalúan a partir de datos existentes.

#### <a name="training-data"></a>Datos de aprendizaje.
Cuando entrena un modelo a partir de datos, utiliza un conjunto de datos conocido y realiza los ajustes necesarios en el modelo en función de las características de datos para obtener la respuesta más precisa. En Azure Machine Learning, se crea un modelo a partir de un módulo de algoritmo que procesa los datos de entrenamiento y módulos funcionales, como un módulo de puntuación.

En el aprendizaje supervisado, si está entrenando un modelo de detección de fraudes, usará un conjunto de transacciones que se etiquetan como fraudulentas o válidas. Podrá dividir el conjunto de datos de forma aleatoria y usar una parte para entrenar el modelo y otra parte para probar o evaluar el modelo.

#### <a name="evaluation-data"></a>Datos de evaluación
Una vez entrenado un modelo, se evalúa con los datos restantes de la prueba. Puede usar los datos de los que ya conoce los resultados, por lo que puede saber si su modelo predice con precisión.

## <a name="other-common-machine-learning-terms"></a>Otros términos comunes relativos al aprendizaje automático
* **algoritmo**: un conjunto independiente de reglas usado para resolver problemas a través de procesamiento de datos, cálculos matemáticos o razonamiento automatizado.
* **detección de anomalías**: un modelo que marca valores o eventos no habituales y le ayuda a detectar problemas. Por ejemplo, para la detección de fraudes en tarjetas de crédito se buscan compras no usuales.
* **datos categóricos**: datos que se organizan por categorías y que se pueden dividir en grupos. Por ejemplo, un conjunto de datos categóricos de coches podría especificar el año, la marca, el modelo y el precio.
* **clasificación**: un modelo para organizar los puntos de datos en categorías según un conjunto de datos para el que ya se conocen las agrupaciones de categorías.
* **diseño de características**: el proceso de extracción o de selección de características relacionadas con un conjunto de datos con el fin de mejorar el conjunto de datos y de mejorar los resultados. Por ejemplo, los datos de vuelo podrían mejorarse por días de la semana y días festivos. Consulte [Selección de características e ingeniería en Azure Machine Learning](../team-data-science-process/create-features.md).
* **módulo**: una parte funcional en un modelo de Machine Learning Studio, como el módulo Especificar datos que permite especificar y editar conjuntos de datos pequeños. Un algoritmo también es un tipo de módulo de Machine Learning Studio.
* **modelo**: un modelo de aprendizaje supervisado es el producto de un experimento de aprendizaje automático que consta de un conjunto de datos de entrenamiento, un módulo de algoritmo y módulos funcionales, como un módulo de puntuación de modelos.
* **datos numéricos**: datos que significan medidas (datos continuos) o cuentas (datos discretos). También se denomina *datos cuantitativos*.
* **partición**: el método por el que se dividen los datos en los ejemplos. Consulte [Partición y ejemplo](https://msdn.microsoft.com/library/azure/dn905960.aspx) para obtener más información.
* **predicción**: una predicción es una previsión de un valor o valores en un modelo de aprendizaje automático. También puede ver el término "puntuación prevista". Sin embargo, los resultados previstos no son el resultado final de un modelo. Una evaluación del modelo sigue la puntuación.
* **regresión**: un modelo para predecir un valor en función de variables independientes; por ejemplo, predecir el precio de un coche en función de su año y marca.
* **puntuación**: un valor predictivo que se genera a partir de una clasificación entrenada o un modelo de regresión, mediante el [módulo Puntuar modelo](https://msdn.microsoft.com/library/azure/dn905995.aspx) en Machine Learning Studio. Los modelos de clasificación también devuelven un resultado de la probabilidad del valor de predicción. Cuando se hayan generado las puntuaciones de un modelo, puede evaluar la exactitud del modelo con el [módulo Evaluar modelo](https://msdn.microsoft.com/library/azure/dn905915.aspx).
* **ejemplo**: una parte de un conjunto de datos diseñado para ser representante del conjunto. Los ejemplos pueden elegirse aleatoriamente o según características específicas del conjunto de datos.

## <a name="next-steps"></a>Pasos siguientes
Puede aprender los datos básicos del análisis predictivo y de Machine Learning mediante un [tutorial detallado](create-experiment.md) y el [uso de ejemplos](sample-experiments.md).  


<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
