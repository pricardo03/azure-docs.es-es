---
title: ¿Qué es?
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio es una herramienta de arrastrar y colocar que permite crear rápidamente modelos desde una biblioteca con elementos que están listos para usarse, como algoritmos y módulos.
services: machine-learning
documentationcenter: ''
author: garyericson
ms.custom: seodec18
ms.author: garye
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.subservice: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/28/2018
ms.openlocfilehash: 570f0a825f679ec5f0cc4c62b0608ec517148161
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57894009"
---
# <a name="what-is-azure-machine-learning-studio"></a>¿Qué es Azure Machine Learning Studio?
Microsoft Azure Machine Learning Studio es una herramienta de arrastrar y colocar que le permite crear, probar e implementar soluciones de análisis predictivo en sus datos. Machine Learning Studio publica modelos como servicios web que pueden utilizarse fácilmente en aplicaciones personalizadas o herramientas de BI como Excel.

Machine Learning Studio es el lugar en el que confluyen la ciencia de datos, el análisis predictivo, los recursos en la nube y sus datos.


## <a name="the-machine-learning-studio-interactive-workspace"></a>El área de trabajo interactivo de Machine Learning Studio
Para desarrollar un modelo de análisis predictivo, normalmente se utilizan datos de una o varias fuentes, se transforman y analizan los datos a través de diversas funciones estadísticas y de manipulación de datos y se genera un conjunto de resultados. Desarrollar un modelo como este es un proceso iterativo: a medida que se modifican las diversas funciones y sus parámetros, sus resultados convergen hasta que esté satisfecho con un modelo entrenado y efectivo.

**Azure Machine Learning Studio** le proporciona un área de trabajo visual e interactiva para generar, probar e iterar con toda facilidad sobre un modelo de análisis predictivo. Se arrastran y colocan ***conjuntos de datos*** y ***módulos*** de análisis en un lienzo interactivo, conectándolos todos para formar un ***experimento*** que se ejecuta en Machine Learning Studio. Para iterar su diseño de modelo, se puede editar el experimento, guardar una copia si así se desea y ejecutarlo de nuevo. Cuando esté listo, puede convertir el ***experimento de entrenamiento*** en un ***experimento predictivo***, y luego publicarlo como ***servicio web*** para que otros usuarios puedan acceder al modelo.

No se requiere ningún tipo de programación, basta con conectar visualmente conjuntos de datos y módulos para construir el modelo de análisis predictivo.

![Diagrama de Azure Machine Learning Studio: crear experimentos, leer datos de muchos orígenes, escribir datos puntuados, modelos de escritura.](./media/what-is-ml-studio/azure-ml-studio-diagram.jpg)

## <a name="download-the-machine-learning-studio-overview-diagram"></a>Descargar el diagrama de información general de Machine Learning Studio
Descargue el diagrama **Información general de las funcionalidades de Microsoft Azure Machine Learning Studio** y obtenga una visión de alto nivel de las funcionalidades de Machine Learning Studio. Para tenerlo a mano, puede imprimir el diagrama en tamaño tabloide (279 x 432 mm).

**Descargue aquí el diagrama: [Información general de las funcionalidades de Microsoft Azure Machine Learning Studio](https://download.microsoft.com/download/C/4/6/C4606116-522F-428A-BE04-B6D3213E9E52/ml_studio_overview_v1.1.pdf)**
![Microsoft Azure Machine Learning Studio Capabilities Overview](./media/what-is-ml-studio/ml_studio_overview_v1.1.png)

## <a name="get-started-with-machine-learning-studio"></a>Introducción a Machine Learning Studio
La primera vez que entre en [Machine Learning Studio](https://studio.azureml.net), verá la página **principal** . Desde aquí puede ver documentación, vídeos y seminarios web, y encontrar otros recursos valiosos.

Haga clic en el menú de la parte superior izquierda ![Menú](./media/what-is-ml-studio/menu.png) y verá varias opciones.
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
Haga clic en **Galería** y se le dirigirá a la **[Galería de Azure AI](https://gallery.azure.ai/)**. La Galería es un lugar donde una comunidad de científicos de datos y desarrolladores puede compartir soluciones creadas con componentes de Cortana Intelligence Suite.

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

Para obtener un tutorial más completo de la creación de una solución de análisis predictivos, consulte [Desarrollo de una solución predictiva con Azure Machine Learning Studio](tutorial-part1-credit-risk.md).

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

Para que le resulte más fácil navegar por la gran biblioteca de algoritmos de aprendizaje automático, consulte [Selección de algoritmos para Microsoft Azure Machine Learning Studio](algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Implementación del servicio web de análisis predictivo
Cuando el modelo de análisis predictivo esté listo, puede implementarlo como servicio web directamente desde Machine Learning Studio. Para obtener más información vea [Implementación de un servicio web de Azure Machine Learning](publish-a-machine-learning-web-service.md).


## <a name="how-is-machine-learning-studio-different-from-azure-machine-learning-service"></a>¿En qué se diferencia Machine Learning Studio de Azure Machine Learning Service?

[Azure Machine Learning Service](../service/overview-what-is-azure-ml.md) proporciona SDK y servicios para preparar datos, entrenar e implementar los modelos de Machine Learning con rapidez. Mejore la productividad y los costos con el proceso y las canalizaciones de escalado automático. Use estas funcionalidades con plataformas de Python de código abierto, como PyTorch, TensorFlow y scikit-learn. 

Utilice Machine Learning Studio cuando desee experimentar con modelos de aprendizaje automático de forma rápida y sencilla, y los algoritmos de aprendizaje automático integrados son suficientes para sus soluciones.

Utilice el servicio Machine Learning si trabaja en un entorno Python, si desea tener más control sobre los algoritmos de aprendizaje automático o si desea utilizar bibliotecas de aprendizaje automático de código abierto.

> [!NOTE]
> Azure Machine Learning Service no puede implementar ni administrar los modelos creados en Azure Machine Learning Studio.

## <a name="free-trial"></a>Evaluación gratuita

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]


## <a name="next-steps"></a>Pasos siguientes
Puede aprender los datos básicos del análisis predictivo y de Machine Learning mediante un [inicio rápido paso a paso](create-experiment.md) y el [uso de ejemplos](sample-experiments.md).

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
