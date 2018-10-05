---
title: Ajuste de los hiperparámetros de un modelo mediante Azure Machine Learning
description: Aprenda a ajustar los hiperparámetros de su modelo de aprendizaje profundo o aprendizaje automático mediante el servicio Azure Machine Learning. Verá cómo definir el espacio de búsqueda de parámetros, especificar una métrica principal para optimizar y terminar anticipadamente las configuraciones de bajo rendimiento.
ms.author: swatig
author: swatig007
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: b6370fd9125c5b14df781b27e028c139175b7589
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405808"
---
# <a name="tune-hyperparameters-for-your-model"></a>Ajuste de los hiperparámetros de un modelo

En este artículo, aprenderá a ajustar eficazmente los hiperparámetros de su modelo. Verá cómo definir el espacio de búsqueda de parámetros, especificar una métrica principal para optimizar y terminar anticipadamente las configuraciones de bajo rendimiento. También puede visualizar las diversas series de entrenamientos y seleccionar la mejor configuración de rendimiento para el modelo.

## <a name="what-are-hyperparameters"></a>¿Qué son los hiperparámetros?
Los hiperparámetros son parámetros ajustables que se eligen antes de entrenar un modelo y que rigen el propio proceso de entrenamiento. Por ejemplo, antes de entrenar una red neuronal profunda, deberá decidir el número de capas ocultas en la red y la cantidad de nodos de cada capa. Estos valores suelen permanecer constantes durante el proceso de entrenamiento.

En escenarios de aprendizaje profundo o aprendizaje automático, el rendimiento del modelo depende en gran medida de los valores de hiperparámetro seleccionados. El objetivo de la exploración de los hiperparámetros es buscar entre diversas configuraciones de hiperparámetros hasta dar con la que tenga como resultado el rendimiento deseado. Normalmente, el proceso de exploración de hiperparámetros es un trabajo manual muy laborioso, dado que el espacio de búsqueda es muy extenso y la evaluación de cada configuración puede ser costosa.

Azure Machine Learning le permite automatizar esta exploración de hiperparámetros de manera eficiente, de forma que le ahorra una cantidad considerable de tiempo y recursos. Puede especificar el intervalo de valores de hiperparámetro para explorar y un número máximo de series de entrenamientos para esta exploración. El sistema inicia entonces automáticamente varias series de entrenamientos simultáneas con configuraciones de parámetros diferentes y busca la configuración que da como resultado un rendimiento óptimo, según se mide mediante una métrica elegida por el usuario. Las series de entrenamientos con bajo rendimiento se terminan automáticamente de forma anticipada, lo que reduce el desperdicio de recursos de proceso. En su lugar, estos recursos se usan para explorar otras configuraciones de hiperparámetros.

Con el fin de ajustar los hiperparámetros del modelo mediante el servicio Azure Machine Learning, deberá hacer lo siguiente:
* Definir el espacio de búsqueda de hiperparámetros
* Especificar una métrica principal para optimizar
* Especificar una directiva de terminación anticipada
* Asignar recursos para el ajuste de hiperparámetros
* Iniciar un experimento con la configuración anterior

## <a name="define-the-hyperparameter-search-space"></a>Definir el espacio de búsqueda de hiperparámetros
Para ajustar automáticamente los hiperparámetros, el servicio Azure Machine Learning explora el intervalo de valores definidos para cada hiperparámetro.

### <a name="types-of-hyperparameters"></a>Tipos de hiperparámetros
Cada hiperparámetro puede ser discreto o continuo.

#### <a name="discrete-hyperparameters"></a>Hiperparámetros discretos 
Los hiperparámetros discretos se pueden especificar con un objeto `choice` entre valores discretos. `choice` puede tomar uno o varios valores separados por comas, un objeto `range` o cualquier objeto `list` arbitrario. Por ejemplo:  

```Python
    {    
        "batch_size": choice(16, 32, 64, 128)
        "number_of_hidden_layers": choice(range(1,5))
    }
```

En este caso, batch_size puede tomar uno de los valores [16, 32, 64, 128] y number_of_hidden_layers puede tomar uno de los valores [1, 2, 3, 4].

También se pueden especificar hiperparámetros discretos avanzados mediante una distribución. Se admiten las siguientes distribuciones:
* `quniform(low, high, q)`: devuelve un valor como round(uniform(low, high) / q) * q
* `qloguniform(low, high, q)`: devuelve un valor como round(exp(uniform(low, high)) / q) * q
* `qnormal(mu, sigma, q)`: devuelve un valor como round(normal(mu, sigma) / q) * q
* `qlognormal(mu, sigma, q)`: devuelve un valor como round(exp(normal(mu, sigma)) / q) * q

#### <a name="continuous-hyperparameters"></a>Hiperparámetros continuos 
Los hiperparámetros continuos se pueden especificar como una distribución a través de un intervalo continuo de valores. Las distribuciones admitidas son:
* `uniform(low, high)`: devuelve un valor distribuido uniformemente entre bajo y alto.
* `loguniform(low, high)`: devuelve un valor que se extrae según exp(uniform(low, high)) de forma que el logaritmo del valor devuelto se distribuye uniformemente.
* `normal(mu, sigma)`: devuelve un valor real que se distribuye normalmente con media mu y desviación estándar sigma.
* `lognormal(mu, sigma)`: devuelve un valor extraído según exp(normal(mu, sigma)) de forma que el logaritmo del valor devuelto se distribuye normalmente.

Este es un ejemplo de una definición de espacio de parámetros:

```Python
    {    
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1)
    }
```

En este ejemplo se define un espacio de búsqueda con dos parámetros: learning_rate y keep_probability. learning_rate tendrá una distribución normal con un valor medio de 10 y una desviación estándar de 3. keep_probability tendrá una distribución uniforme con un valor mínimo de 0,05 y un valor máximo de 0,1.

### <a name="sampling-the-hyperparameter-space"></a>Muestreo del espacio de hiperparámetros
El usuario también especifica el método de muestreo de parámetros que se usará durante la definición del espacio de hiperparámetros especificado. El servicio Azure Machine Learning admite muestreo aleatorio, muestreo de cuadrícula y muestreo bayesiano.

#### <a name="random-sampling"></a>Muestreo aleatorio
En el muestreo aleatorio, los valores de hiperparámetro se seleccionan aleatoriamente del espacio de búsqueda definido. El muestreo aleatorio permite que el espacio de búsqueda incluya hiperparámetros discretos y continuos. Por ejemplo:

```Python
from azureml.train.hyperdrive import RandomParameterSampling
param_sampling = RandomParameterSampling( {
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

#### <a name="grid-sampling"></a>Muestreo de cuadrícula
El muestreo de cuadrícula realiza una búsqueda de cuadrícula sencilla por todos los valores posibles del espacio de búsqueda definido. Solo se puede usar con hiperparámetros especificados con `choice`. Por ejemplo, el siguiente espacio tiene seis muestras en total:

```Python
from azureml.train.hyperdrive import GridParameterSampling
param_sampling = GridParameterSampling( {
        "num_hidden_layers": choice(1, 2, 3),
        "batch_size": choice(16, 32)
    }
)
```

#### <a name="bayesian-sampling"></a>Muestreo bayesiano
El muestreo bayesiano se basa en el algoritmo de optimización bayesiano y toma decisiones inteligentes sobre los valores de hiperparámetro que se van a muestrear a continuación. Esta muestra se toma en función de cómo se hayan ejecutado las muestras anteriores, de tal forma que la nueva muestra mejora la métrica principal notificada.

Cuando se usa el muestreo bayesiano, el número de ejecuciones simultáneas tiene un efecto sobre la eficacia del proceso de ajuste. Normalmente, un número menor de ejecuciones simultáneas puede conducir a una mejor convergencia de muestreo. El motivo es que el grado menor de paralelismo aumenta el número de ejecuciones que se benefician de las ejecuciones completadas anteriormente.

El muestreo bayesiano solo admite distribuciones `choice` y `uniform` en el espacio de búsqueda. Por ejemplo: 

```Python
from azureml.train.hyperdrive import BayesianParameterSampling
param_sampling = BayesianParameterSampling( {
        "learning_rate": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

> [!NOTE]
> El muestreo bayesiano no admite actualmente ninguna directiva de terminación anticipada (consulte [Especificar una directiva de terminación anticipada](#specify-an-early-termination-policy)). Si usa el muestreo de parámetros bayesiano, debe establecer la directiva en `None`. La no especificación de una directiva de terminación con el muestreo bayesiano tendrá el mismo efecto.
>
> ```Python
> early_termination_policy = None
> ```

## <a name="specify-a-primary-metric-to-optimize"></a>Especificar una métrica principal para optimizar
Al ajustar los hiperparámetros, debe especificar la métrica principal que quiere que se optimice con el experimento de ajuste de hiperparámetros. En cada serie de entrenamientos se evalúa esta métrica principal y las ejecuciones con un rendimiento deficiente (donde la métrica principal no satisface los criterios establecidos por la directiva de terminación anticipada) se terminan. Además de especificar el nombre de la métrica principal, también deberá especificar el objetivo de la optimización: si maximizar o minimizar la métrica principal.
* `primary_metric_name`: el nombre de la métrica principal para optimizar. El nombre de la métrica principal debe coincidir exactamente con el nombre de la métrica registrado por el script de entrenamiento. Consulte [Registrar métricas para el ajuste de hiperparámetros](#log-metrics-for-hyperparameter-tuning).
* `primary_metric_goal`: puede ser `PrimaryMetricGoal.MAXIMIZE` o `PrimaryMetricGoal.MINIMIZE` y determina si la métrica principal se maximizará o minimizará al evaluar las ejecuciones. 

Por ejemplo:
```Python
primary_metric_name="accuracy",
primary_metric_goal=PrimaryMetricGoal.MAXIMIZE
```
En este caso se optimizan las ejecuciones para maximizar la "precisión".

### <a name="log-metrics-for-hyperparameter-tuning"></a>Registrar métricas para el ajuste de hiperparámetros
Para poder usar el servicio Azure Machine Learning para el ajuste de hiperparámetros, el script de entrenamiento del modelo debe notificar las métricas pertinentes mientras se ejecuta el modelo. El usuario especifica la métrica principal que quiere que use el servicio para evaluar el rendimiento de ejecución, y el script de entrenamiento debe registrar esta métrica. Consulte [Especificar una métrica principal para optimizar](#specify-a-primary-metric-to-optimize).

Puede actualizar el script de entrenamiento para registrar esta métrica mediante el siguiente fragmento de código de ejemplo:

```Python
from azureml.core.run import Run
run_logger = Run.get_submitted_run()
run_logger.log("accuracy", float(val_accuracy))
```

En este ejemplo, el script de entrenamiento calcula el valor `val_accuracy` y registra esta "precisión", que se usa como métrica principal. Es el desarrollador del modelo quien debe determinar la frecuencia con que se notifica esta métrica.

## <a name="specify-an-early-termination-policy"></a>Especificar una directiva de terminación anticipada
Cuando se usa el servicio Azure Machine Learning para ajustar los hiperparámetros, las ejecuciones con un bajo rendimiento se terminan automáticamente de forma anticipada. De esta forma se reduce el desperdicio de recursos, los cuales se usan para explorar otras configuraciones de parámetros.

Cuando se usa una directiva de terminación anticipada, los usuarios pueden configurar los siguientes parámetros que controlan cuándo se aplica una directiva:
* `evaluation_interval`: la frecuencia con que se aplica la directiva. Cada vez que el script de entrenamiento registra la métrica principal se considera un intervalo. Por lo tanto, un parámetro `evaluation_interval` de 1 aplicará la directiva cada vez que el script de entrenamiento notifique la métrica principal. Un parámetro `evaluation_interval` de 2 aplicará la directiva las demás veces que el script de entrenamiento notifique la métrica principal. Este parámetro es opcional y, si no se especifica, `evaluation_interval` se establece en 1 de forma predeterminada.
* `delay_evaluation`: retrasa la primera evaluación de la directiva un número especificado de intervalos. Se trata de un parámetro opcional que permite que todas las configuraciones se ejecuten durante un número mínimo inicial de intervalos, lo que evita la terminación prematura de series de entrenamientos. Si se especifica, la directiva aplica cada múltiplo de evaluation_interval que sea mayor o igual que delay_evaluation.

El servicio Azure Machine Learning admite las siguientes directivas de terminación anticipada:

### <a name="bandit-policy"></a>Directiva de bandidos
La directiva de bandidos es una directiva de terminación basada en el factor de demora o la cantidad de demora y el intervalo de evaluación. Esta directiva termina anticipadamente aquellas ejecuciones en las que la métrica principal no se encuentre dentro del factor de demora o cantidad de demora especificadas con respecto a la serie de entrenamientos con el mejor rendimiento. Esta directiva toma los siguientes parámetros de configuración:
* `slack_factor` o `slack_amount`: la demora permitida con respecto a la serie de entrenamientos con el mejor rendimiento. `slack_factor` especifica la demora permitida como una relación. `slack_amount` especifica la demora permitida como una cantidad absoluta, en lugar de una relación.

    Por ejemplo, imagine que se aplica una directiva de bandidos en el intervalo 10. Suponga que la ejecución con el mejor rendimiento en el intervalo 10 notifica una métrica principal de 0,8 con el objetivo de maximizar esta. Si la directiva se especificó con un parámetro `slack_factor` de 0,2, se terminarán aquellas series de entrenamientos cuya mejor métrica en el intervalo 10 sea inferior a 0,66 (0,8/(1 +`slack_factor`)). Si, por el contrario, la directiva se especificó con un parámetro `slack_amount` de 0,2, se terminarán aquellas series de entrenamiento cuya mejor métrica en el intervalo 10 sea inferior a 0,6 (0,8 - `slack_amount`).
* `evaluation_interval`: la frecuencia con que se aplica la directiva (parámetro opcional).
* `delay_evaluation`: retrasa la primera evaluación de directiva un número especificado de intervalos (parámetro opcional).

Considere este ejemplo:

```Python
from azureml.train.hyperdrive import BanditPolicy
early_termination_policy = BanditPolicy(slack_factor = 0.1, evaluation_interval=1, delay_evaluation=5)
```

En este ejemplo, se aplica la directiva de terminación anticipada en cada intervalo cuando se notifican las métricas, comenzando en el intervalo de evaluación 5. Cualquier ejecución cuya mejor métrica sea inferior a (1/(1+0,1), o al 91 % de la ejecución con el mejor rendimiento, se terminará.

### <a name="median-stopping-policy"></a>Directiva de mediana de detención
La directiva de mediana de detención es una directiva de terminación anticipada basada en la ejecución de valores medios de las métricas principales notificadas por las ejecuciones. Esta directiva calcula los valores medios de ejecución en todas las series de entrenamientos y termina las ejecuciones cuyo rendimiento es peor que la mediana de los valores medios de ejecución. Esta directiva toma los parámetros de configuración siguientes:
* `evaluation_interval`: la frecuencia con que se aplica la directiva (parámetro opcional).
* `delay_evaluation`: retrasa la primera evaluación de directiva un número especificado de intervalos (parámetro opcional).

Considere este ejemplo:

```Python
from azureml.train.hyperdrive import MedianStoppingPolicy
early_termination_policy = MedianStoppingPolicy(evaluation_interval=1, delay_evaluation=5)
```

En este ejemplo, se aplica la directiva de terminación anticipada en cada intervalo, comenzando en el intervalo de evaluación 5. Una ejecución se termina en el intervalo 5 si su mejor métrica principal es peor que la mediana de los valores medios de ejecución durante los intervalos en una relación de 1 a 5 en todas las series de entrenamientos.

### <a name="truncation-selection-policy"></a>Directiva de selección de truncamiento
La directiva de selección de truncamiento cancela un porcentaje dado de ejecuciones con el rendimiento más bajo en cada intervalo de evaluación. Las ejecuciones se comparan en función de su rendimiento en la métrica principal y las X % con el rendimiento más bajo se terminan. Esta directiva toma los siguientes parámetros de configuración:
* `truncation_percentage`: el porcentaje de ejecuciones con el rendimiento más bajo que se terminarán en cada intervalo de evaluación. Debe ser un valor entero entre 1 y 99.
* `evaluation_interval`: la frecuencia con que se aplica la directiva (parámetro opcional).
* `delay_evaluation`: retrasa la primera evaluación de directiva un número especificado de intervalos (parámetro opcional).

Considere este ejemplo:

```Python
from azureml.train.hyperdrive import TruncationSelectionPolicy
early_termination_policy = TruncationSelectionPolicy(evaluation_interval=1, truncation_percentage=20, delay_evaluation=5)
```

En este ejemplo, se aplica la directiva de terminación anticipada en cada intervalo, comenzando en el intervalo de evaluación 5. Una ejecución se terminará en el intervalo 5 si su rendimiento en este intervalo se encuentra en el 20% del rendimiento más bajo de todas las ejecuciones en el intervalo 5.

### <a name="no-termination-policy"></a>Sin directiva de terminación
Si quiere que todas las series de entrenamientos se ejecuten hasta su finalización, use NoTerminationPolicy. El efecto será el de no aplicar ninguna directiva de terminación anticipada. Por ejemplo: 

```Python
from azureml.train.hyperdrive import NoTerminationPolicy
early_termination_policy = NoTerminationPolicy()
```

### <a name="default-policy"></a>Directiva predeterminada
Si no se especifica ninguna directiva, el servicio de ajuste de hiperparámetros usará una directiva de mediana de detención con `evaluation_interval` 1 y `delay_evaluation` 5 de forma predeterminada. Se trata de una configuración conservadora que puede proporcionar unos ahorros de entre un 25 % y un 35 % sin pérdidas de la métrica principal (según nuestros datos de evaluación).

## <a name="allocate-resources-for-hyperparameter-tuning"></a>Asignar recursos para el ajuste de hiperparámetros
Puede controlar el presupuesto de recursos para el experimento de ajuste de hiperparámetros mediante la especificación del número total máximo de series de entrenamientos y, opcionalmente, la duración máxima de este experimento (en minutos). 
* `max_total_runs`: número total máximo de series de entrenamientos que se crearán. Este es un límite superior; por ejemplo, se pueden tener menos ejecuciones si el espacio de hiperparámetros es finito y tiene menos muestras. Debe ser un número entre 1 y 1000.
* `max_duration_minutes`: duración máxima del experimento de ajuste de hiperparámetros en minutos. Este es un parámetro opcional y, si existe, automáticamente se cancelan todas las ejecuciones que podrían estar ejecutándose después de esta duración.

>[!NOTE] 
>Si se especifica `max_total_runs` y `max_duration_minutes`, el experimento de ajuste de hiperparámetros se termina cuando se alcanza el primero de estos dos umbrales.

Además, puede especificar el número máximo de series de entrenamientos que se ejecutarán al mismo tiempo durante la búsqueda de ajuste de hiperparámetros.
* `max_concurrent_runs`: es el número máximo de ejecuciones que se ejecutarán al mismo tiempo en un momento dado. Si no especifica ninguno, el número de ejecuciones de `max_total_runs` se iniciarán en paralelo. Si se especifica, el valor debe ser un número entre 1 y 100.

>[!NOTE] 
>El número de ejecuciones simultáneas viene determinado por los recursos disponibles en el destino de proceso especificado. Por lo tanto, deberá asegurarse de que el destino de proceso tenga los recursos disponibles para la simultaneidad deseada.

Puede asignar recursos para el ajuste de hiperparámetros, como se muestra en este ejemplo:
```Python
max_total_runs=20,
max_concurrent_runs=4
```
En este caso se configura el experimento de ajuste de hiperparámetros para usar un máximo de 20 ejecuciones totales, de forma tal que se ejecutan 4 configuraciones al mismo tiempo.

## <a name="configure-your-hyperparameter-tuning-experiment"></a>Configurar el experimento de ajuste de hiperparámetros
Puede configurar el experimento de ajuste de hiperparámetros mediante el espacio de búsqueda de hiperparámetros definido, las directivas de terminación anticipada, las métricas principales y la asignación de recursos que ya se han visto en las secciones anteriores. Además, deberá proporcionar un parámetro `estimator` que se invocará con los hiperparámetros muestreados. El parámetro `estimator` describe el script de entrenamiento que se ejecutará, los recursos por trabajo (una o varias GPU) y el destino de proceso que se usará. Dado que la simultaneidad en el experimento de ajuste de hiperparámetros viene determinada por los recursos disponibles, deberá asegurarse de que el destino de proceso especificado en `estimator` tenga recursos suficientes para la simultaneidad deseada. (Para más información sobre los estimadores, consulte [Cómo entrenar modelos](how-to-train-ml-models.md)).

Este es un ejemplo de cómo puede configurar el experimento de ajuste de hiperparámetros:

```Python
from azureml.train.hyperdrive import HyperDriveRunConfig
hyperdrive_run_config = HyperDriveRunConfig(estimator=estimator,
                          hyperparameter_sampling=param_sampling, 
                          policy=early_termination_policy,
                          primary_metric_name="accuracy", 
                          primary_metric_goal=PrimaryMetricGoal.MAXIMIZE,
                          max_total_runs=100,
                          max_concurrent_runs=4)
```

## <a name="submit-your-hyperparameter-tuning-experiment"></a>Enviar el experimento de ajuste de hiperparámetros
Una vez que ha definido la configuración de ajuste de hiperparámetros, puede enviar un experimento mediante esta configuración:

```Python
from azureml.core.experiment import Experiment
experiment = Experiment(workspace, experiment_name)
hyperdrive_run = experiment.submit(hyperdrive_run_config)
```

donde `experiment_name` es el nombre que quiere asignar a su experimento de ajuste de hiperparámetros y `workspace` es el área de trabajo en el que quiere crear el experimento (consulte el [vínculo](/concept-azure-machine-learning-architecture.md) para más información sobre los experimentos).

## <a name="visualize-your-hyperparameter-tuning-experiment"></a>Visualizar el experimento de ajuste de hiperparámetros
El SDK de Azure Machine Learning proporciona un widget de cuaderno que puede usarse para visualizar el progreso de las series de entrenamientos. El siguiente fragmento de código se puede usar para visualizar todas las ejecuciones de ajuste de hiperparámetros en un solo lugar:

```Python
from azureml.train.widgets import RunDetails
RunDetails(hyperdrive_run).show()
```

En este caso, se muestra una tabla con detalles sobre las series de entrenamientos de cada una de las configuraciones de hiperparámetros. Por ejemplo:

![tabla de ajuste de hiperparámetros](media/how-to-tune-hyperparameters/HyperparameterTuningTable.png)

También puede visualizar el rendimiento de cada una de las ejecuciones a medida que progresa el entrenamiento. Por ejemplo:

![trazado de ajuste de hiperparámetros](media/how-to-tune-hyperparameters/HyperparameterTuningPlot.png)

Por último, puede identificar visualmente la correlación entre el rendimiento y los valores de hiperparámetros individuales mediante un trazado de coordenadas paralelas. Por ejemplo: 

![coordenadas paralelas de ajuste de hiperparámetros](media/how-to-tune-hyperparameters/HyperparameterTuningParallelCoordinates.png)

Como alternativa, también puede visualizar todas las ejecuciones de ajuste de hiperparámetros en el portal web de Azure. Consulte el [vínculo](/how-to-track-experiments.md/#view-the-experiment-in-the-web-portal) para más información sobre cómo ver un experimento en el portal web. Por ejemplo:

![portal de ajuste de hiperparámetros](media/how-to-tune-hyperparameters/HyperparameterTuningPortal.png)

## <a name="find-the-configuration-that-resulted-in-the-best-performance"></a>Buscar la configuración que tuvo como resultado el mejor rendimiento
Una vez que se han completado todas las ejecuciones de ajuste de hiperparámetros, puede identificar la configuración con el mejor rendimiento y los valores de hiperparámetro correspondientes con el siguiente fragmento de código:

```Python
best_run = hyperdrive_run.get_best_run_by_primary_metric()
best_run_metrics = best_run.get_metrics()
parameter_values = best_run.get_details()['runDefinition']['Arguments']

print('Best Run Id: ', best_run.id)
print('\n Accuracy:', best_run_metrics['accuracy'])
print('\n learning rate:',parameter_values[3])
print('\n keep probability:',parameter_values[5])
print('\n batch size:',parameter_values[7])
```

## <a name="sample-notebook"></a>Cuaderno de ejemplo
Consulte 
* `training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb` para ver un tutorial sobre el ajuste de hiperparámetros para un modelo de Tensorflow. 

Obtenga este cuaderno:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes
* [Seguimiento de experimentos](how-to-track-experiments.md)
* [Implementar un modelo entrenado](how-to-deploy-and-where.md)
