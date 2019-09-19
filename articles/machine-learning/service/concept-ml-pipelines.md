---
title: ¿Qué son las canalizaciones de Machine Learning?
titleSuffix: Azure Machine Learning service
description: En este artículo, obtenga información sobre las canalizaciones de Machine Learning que puede compilar con el SDK de Azure Machine Learning para Python y las ventajas de utilizar canalizaciones. Los científicos de datos usan canalizaciones de Machine Learning (ML) para crear, optimizar y administrar sus flujos de trabajo de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: sanpil
author: sanpil
ms.date: 08/08/2019
ms.custom: seodec18
ms.openlocfilehash: c40fd4f776e0372d7368d182d30cfb2e01a4b0e2
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70858824"
---
# <a name="what-are-ml-pipelines-in-azure-machine-learning-service"></a>¿Qué son las canalizaciones de Machine Learning en Azure Machine Learning Service?

Obtenga información sobre las canalizaciones de aprendizaje automático que puede crear y administrar con Azure Machine Learning Service. 

Mediante las canalizaciones de Machine Learning (ML), los científicos de datos, los ingenieros de datos y los profesionales de TI pueden colaborar en los pasos relacionados con las acciones siguientes:
+ Preparación de datos, como normalizaciones y transformaciones
+ Entrenamiento del modelo
+ Evaluación del modelo
+ Implementación

Aprenda a [crear su primera canalización](how-to-create-your-first-pipeline.md).

![Canalizaciones de aprendizaje automático en el servicio Azure Machine Learning](./media/concept-ml-pipelines/pipeline-flow.png)

<a name="compare"></a>
### <a name="which-azure-pipeline-technology-should-i-use"></a>¿Qué tecnología de canalización de Azure se debe usar?

La nube de Azure proporciona otras canalizaciones, cada una con una finalidad diferente. En la tabla siguiente se enumeran las diferentes canalizaciones y para qué se usan:

| Canalización | Qué hace | Canalización canónica |
| ---- | ---- | ---- |
| Canalizaciones de Azure Machine Learning | Define flujos de trabajo de aprendizaje automático reutilizables que pueden emplearse como plantilla para los escenarios de aprendizaje automático. | Datos -> modelo |
| [Canalizaciones de Azure Data Factory](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) | Agrupa las actividades de movimiento, transformación y control de datos necesarias para realizar una tarea.  | Datos -> datos |
| [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) | Integración y entrega continuas de la aplicación en cualquier plataforma y en cualquier nube.  | Código -> aplicación/servicio |

## <a name="why-build-pipelines-with-azure-machine-learning"></a>Motivos para compilar canalizaciones con Azure Machine Learning

Las canalizaciones de aprendizaje automático optimizan el flujo de trabajo con velocidad, portabilidad y reutilización para que pueda centrarse en sus conocimientos, el aprendizaje automático, en lugar de hacerlo en la infraestructura y la automatización.

Las canalizaciones se construyen a partir de varios **pasos**, que son unidades de cálculo diferentes en la canalización. Cada paso se puede ejecutar de forma independiente y usar recursos de proceso aislados.
Los pasos independientes permiten que varios científicos de datos trabajen en la misma canalización al mismo tiempo sin sobrecargar los recursos de proceso y facilita el uso de diferentes tamaños o tipos de proceso para cada paso.

Una vez diseñada la canalización, a menudo se aplican más ajustes en torno al bucle de aprendizaje de la canalización. Cuando se vuelve a ejecutar una canalización, la ejecución salta a los distintos pasos necesarios para volver a ejecutarse, como un script de entrenamiento actualizado, y omite lo que no ha cambiado. El mismo paradigma se aplica a los scripts sin cambios utilizados para la ejecución del paso. Esta funcionalidad de reutilización ayuda a evitar la ejecución de pasos costosos y lentos, como la ingesta de datos y su transformación si los datos subyacentes no han cambiado.

Con Azure Machine Learning, puede usar diversos kits de herramientas y marcos, como PyTorch o TensorFlow, en cada paso de la canalización. Azure se coordina entre los distintos [destinos de proceso](concept-azure-machine-learning-architecture.md) que usa para que los datos intermedios se puedan compartir con los destinos de proceso de bajada fácilmente.

Puede [realizar un seguimiento de las métricas de los experimentos de canalización](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiments) directamente en Azure Portal o en la [página de aterrizaje del área de trabajo (versión preliminar)](https://ml.azure.com). Una vez que se publica una canalización, puede configurar un punto de conexión REST que le permite volver a ejecutar la canalización desde cualquier plataforma o pila.

## <a name="key-advantages"></a>Ventajas clave

Las ventajas clave de usar canalizaciones para los flujos de trabajo de aprendizaje automático son:

|Ventaja clave|DESCRIPCIÓN|
|:-------:|-----------|
|**Ejecuciones&nbsp;desatendidas**|Programe los pasos para la ejecución en paralelo o en secuencia de manera confiable y desatendida. La preparación de datos y su modelado pueden durar días o semanas y las canalizaciones le permiten centrarse en otras tareas mientras se ejecuta el proceso. |
|**Proceso heterogéneo**|Utilice varias canalizaciones coordinadas de forma confiable en ubicaciones de almacenamiento y recursos de procesos heterogéneos y escalables. Use de manera eficaz los recursos de proceso disponibles mediante la ejecución de pasos individuales de la canalización en diferentes destinos de proceso, como HDInsight, instancias de Data Science VM de la GPU y Databricks.|
|**Reusabilidad**|Cree plantillas de canalizaciones para escenarios específicos, como reentrenamiento y puntuación por lotes. Desencadene las canalizaciones publicadas desde sistemas externos a través de llamadas REST sencillas.|
|**Seguimiento y control de versiones**|En lugar de hacer un seguimiento manual de los datos y los resultados conforme itera, use el SDK de canalizaciones para asignar de forma explícita un nombre y una versión a los orígenes de datos, las entradas y las salidas. También puede administrar scripts y datos por separado para aumentar la productividad.|
|**Colaboración**|Las canalizaciones permiten que los científicos de datos colaboren en todas las áreas del proceso de diseño de aprendizaje automático, a la vez que pueden trabajar de manera simultánea en los pasos de la canalización.|

## <a name="the-python-sdk-for-pipelines"></a>SDK de Python para las canalizaciones

[Use el SDK de Python](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) para crear canalizaciones de Machine Learning en el entorno de desarrollo integrado (IDE) que prefiera o los cuadernos de Jupyter Notebook. El SDK de Azure Machine Learning ofrece construcciones imperativas para definir la secuencia y el paralelismo de los pasos descritos de las canalizaciones cuando no existen ninguna dependencia de datos. 

Con las dependencias de datos declarativas, puede optimizar sus tareas. El SDK incluye un marco de módulos compilados previamente para tareas comunes, como la transferencia de datos y la publicación de modelos. Puede ampliar el marco para modelar sus propias convenciones, mediante la implementación de pasos personalizados reutilizables en las canalizaciones. También puede administrar los destinos de proceso y los recursos de almacenamiento directamente desde el SDK.

Guarde las canalizaciones como plantillas e impleméntelas en un punto de conexión REST para trabajos de puntuación por lote o de reentrenamiento.

Hay dos paquetes de Python para canalizaciones con Azure Machine Learning: [azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) y [azureml-pipeline-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py). Para empezar rápidamente, use uno de los módulos predefinidos, como:

* Ejecución del script de Python en un paso con [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep)
* Transferencia de datos entre opciones de almacenamiento con [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep)
* Creación de un paso de canalización de AutoML con [AutoMLStep](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlstep)

## <a name="next-steps"></a>Pasos siguientes

+ Aprenda a [crear su primera canalización](how-to-create-your-first-pipeline.md).

+ Aprenda a [ejecutar predicciones por lotes en grandes cantidades de datos](how-to-run-batch-predictions.md).

+ Consulte la [documentación de referencia del SDK para canalizaciones](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py).

+ Pruebe los cuadernos de Jupyter Notebook que muestran [canalizaciones de Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines). Aprenda a [ejecutar cuadernos para explorar este servicio](samples-notebooks.md).
