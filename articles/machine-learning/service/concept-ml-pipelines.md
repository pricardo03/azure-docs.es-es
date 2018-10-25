---
title: 'Compilación de canalizaciones de Machine Learning: servicio Azure Machine Learning'
description: En este artículo, obtenga información sobre las canalizaciones de Machine Learning que puede compilar con el SDK de Azure Machine Learning para Python y las ventajas de utilizar canalizaciones. Los científicos de datos usan canalizaciones de Machine Learning (ML) para crear, optimizar y administrar sus flujos de trabajo de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: sanpil
author: sanpil
ms.date: 09/24/2018
ms.openlocfilehash: b450d2ca2af0c0f7598d74e2f07c0acc81b811f3
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116460"
---
# <a name="pipelines-and-azure-machine-learning"></a>Canalizaciones y Azure Machine Learning

En este artículo, obtenga información sobre las canalizaciones de Machine Learning que puede compilar con el SDK de Azure Machine Learning para Python y las ventajas de utilizar canalizaciones.

## <a name="what-are-machine-learning-pipelines"></a>¿Qué son las canalizaciones de Machine Learning?

Los científicos de datos usan canalizaciones de Machine Learning (ML) para crear, optimizar y administrar sus flujos de trabajo de aprendizaje automático. Una canalización típica implica una secuencia de pasos que abarcan las siguientes áreas:

+ Preparación de datos, como normalizaciones y transformaciones
+ Entrenamiento del modelo, como la optimización y la validación de hiperparámetros
+ Implementación y evaluación del modelo  

El diagrama siguiente muestra un ejemplo de canalización:

[ ![Canalizaciones de Machine Learning en el servicio de Azure Machine Learning](./media/concept-ml-pipelines/pipelines.png) ] (./media/concept-ml-pipelines/machine-learning-pipelines-big.png#lightbox)

## <a name="why-build-pipelines-with-azure-machine-learning"></a>Motivos para compilar canalizaciones con Azure Machine Learning

El [SDK de Azure Machine Learning para Python](#the-python-sdk-for-pipelines) puede usarse para crear canalizaciones de Machine Learning, así como para el envío y seguimiento de ejecuciones de canalizaciones individuales.

Con las canalizaciones, puede optimizar su flujo de trabajo con sencillez, velocidad, portabilidad y reutilización. Al compilar canalizaciones mediante Azure Machine Learning, puede centrarse en lo que conoce mejor &mdash;el aprendizaje automático&mdash; en lugar de en la infraestructura.

El uso de distintos pasos permite volver a ejecutar solo los pasos que necesita al ajustar y probar el flujo de trabajo. Un paso es una unidad de proceso en la canalización. Como se muestra en el diagrama anterior, la tarea de preparación de datos puede implicar muchos pasos, incluidos, sin limitación, los de normalización, transformación, validación y caracterización.

Una vez diseñada la canalización, a menudo se aplican más ajustes en torno al bucle de aprendizaje de la canalización. Cuando se vuelve a ejecutar una canalización, la ejecución salta a los pasos necesarios para volver a ejecutarse, como un script de aprendizaje actualizado, y omite lo que no ha cambiado. El mismo paradigma se aplica a los scripts sin cambios utilizados para la ejecución del paso. 

Con Azure Machine Learning, puede utilizar diversos kits de herramientas y marcos, como Microsoft Cognitive Toolkit o TensorFlow, en cada paso de la canalización. Azure se coordina entre los distintos [destinos de proceso](concept-azure-machine-learning-architecture.md) que usa para que los datos intermedios se pueden compartir con los destinos de proceso de bajada fácilmente. 

## <a name="key-advantages"></a>Ventajas clave

Las principales ventajas de la compilación de canalizaciones para los flujos de trabajo de aprendizaje automático es:

|Ventaja clave|DESCRIPCIÓN|
|:-------:|-----------|
|**Ejecuciones&nbsp;desatendidas**|Programe unos pocos pasos para la ejecución en paralelo o en secuencia de manera confiable y desatendida. Dado que la preparación de datos y el modelado pueden tardar días o semanas, ahora puede centrarse en otras tareas mientras se ejecuta la canalización. |
|**Proceso mixto y diverso**|Utilice varias canalizaciones coordinadas de forma confiable en procesos y almacenamientos heterogéneos y escalables. Se pueden volver a ejecutar pasos individuales de la canalización en diferentes destinos de proceso, como HDInsight, Data Science VM de la GPU y Databricks, para usar de manera eficaz las opciones de proceso disponibles.|
|**Reusabilidad**|Se pueden crear plantillas de las canalizaciones para escenarios específicos, como repetir el aprendizaje y puntuar lotes.  Pueden desencadenarse desde sistemas externos mediante llamadas REST sencillas.|
|**Seguimiento y control de versiones**|En lugar de realizar manualmente el seguimiento de datos y rutas de acceso de resultados durante la iteración, utilice el SDK de canalizaciones para asignar explícitamente el nombre y la versión a sus orígenes de datos, entradas y salidas, así como para administrar scripts y datos por separado para aumentar la productividad.|

## <a name="the-python-sdk-for-pipelines"></a>SDK de Python para las canalizaciones

Use Python para crear sus canalizaciones de ML. El SDK de Azure Machine Learning ofrece construcciones imperativas para definir la secuencia y el paralelismo de los pasos descritos de las canalizaciones cuando no existen ninguna dependencia de datos. Puede interactuar con él en Jupyter Notebook o en otro IDE que prefiera. 

Con las dependencias de datos declarativas, puede optimizar sus tareas. El SDK incluye un marco de módulos compilados previamente para tareas comunes, como la transferencia de datos, la creación de destinos de proceso y la publicación del modelo. El marco puede ampliarse para definir sus propias convenciones mediante la implementación de pasos personalizados que se puedan reutilizar en las canalizaciones.

Las canalizaciones se pueden guardar como plantillas e implementar en un punto de conexión de REST, con la finalidad de que pueda programar trabajos de puntuación por lotes o reentrenamiento.

Consulte los [documentos de referencia del SDK de Python para canalizaciones](http://aka.ms/aml-sdk) y el cuaderno de la sección siguiente para ver cómo crear las suyas propias.

## <a name="example-notebooks"></a>Cuadernos de ejemplo
 
El cuaderno siguiente muestra las canalizaciones con Azure Machine Learning: [pipeline/pipeline-batch-scoring.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/pipeline/pipeline-batch-scoring.ipynb).
 
Obtenga este cuaderno:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
