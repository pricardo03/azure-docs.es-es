---
title: Iniciar, supervisar y cancelar las ejecuciones de entrenamiento en Python
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo iniciar, establecer el estado de etiqueta y organizar sus experimentos de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 04/05/2019
ms.openlocfilehash: c0c1c1353b12944fa913dfb0789192917b99f234
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59543600"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Iniciar, supervisar y cancelar las ejecuciones de entrenamiento en Python

El [SDK de Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) proporciona varios métodos para supervisar, organizar y administrar las ejecuciones para el aprendizaje y experimentación.

En este artículo se muestra ejemplos de las tareas siguientes:

* Monitor de rendimiento de ejecución.
* Cancelar o no se ejecuta.
* Crear a secundario se ejecuta.
* Etiquetar y búsqueda de ejecuciones.

## <a name="prerequisites"></a>Requisitos previos

Necesitará los siguientes elementos:

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Un [área de trabajo de servicio de Azure Machine Learning](setup-create-workspace.md).

* El SDK de Azure Machine Learning para Python (versión 1.0.21 o posterior). Para instalar o actualizar a la versión más reciente del SDK, consulte [instalar o actualizar el SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

    Para comprobar la versión del SDK de Azure Machine Learning, use el código siguiente:

    ```Python
    print(azureml.core.VERSION)
    ```

## <a name="start-a-run-and-its-logging-process"></a>Iniciar una ejecución y su proceso de registro

Configure el experimento importando el [área de trabajo](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py), [experimentar](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py), [ejecutar](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py), y [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) clases desde el [azureml.core](https://docs.microsoft.com/python/api/azureml-core/azureml.core?view=azure-ml-py) paquete.

```Python
import azureml.core
from azureml.core import Workspace, Experiment, Run
from azureml.core import ScriptRunConfig

ws = Workspace.from_config()
exp = Experiment(workspace=ws, name="explore-runs")
```

Iniciar una ejecución y su proceso de registro con el [ `start_logging()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#start-logging--args----kwargs-) método.

```Python
notebook_run = exp.start_logging()

notebook_run.log(name="message", value="Hello from run!")
```

## <a name="monitor-the-status-of-a-run"></a>Supervisar el estado de una ejecución

Obtener el estado de una ejecución con el [ `get_status()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--) método.

```Python
print(notebook_run.get_status())
```

Para obtener detalles adicionales sobre la ejecución, use el [ `get_details()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--) método.

```Python
notebook_run.get_details()
```

Cuando la ejecución finaliza correctamente, utilice el [ `complete()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-) método para marcarlo como completado.

```Python
notebook_run.complete()
print(notebook_run.get_status())
```

Si usas de Python `with...as` patrón, la ejecución marcará automáticamente propio como completada cuando la ejecución fuera del ámbito. No es necesario marcar manualmente la ejecución como completado.

```Python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print("Is it still running?",notebook_run.get_status())

print("Has it completed?",notebook_run.get_status())
```

## <a name="cancel-or-fail-runs"></a>Cancela o no se ejecuta

 Si observe un error o si su ejecución está tardando demasiada en completarse, use el [ `cancel()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--) método para detener la ejecución antes de que finalice y marcarla como canceladas.

```Python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')

local_script_run = exp.submit(run_config)
print("Did the run start?",local_script_run.get_status())

local_script_run.cancel()
print("Did the run cancel?",local_script_run.get_status())
```

Si termina la ejecución, pero contiene un error (por ejemplo, se usó el script de entrenamiento incorrecto), puede usar el [ `fail()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#fail-error-details-none---set-status-true-) método que va a marcar como no superado.

```Python
local_script_run = exp.submit(run_config)
local_script_run.fail()

print(local_script_run.get_status())
```

## <a name="create-child-runs"></a>Crear ejecuciones de secundarios

Crear a secundario se ejecuta para agrupar series relacionadas, como para las iteraciones de ajuste de hiperparámetros diferentes.

Este ejemplo de código utiliza el `hello_with_children.py` secuencia de comandos para crear un lote de cinco ejecuciones secundarios desde dentro de una ejecución enviada mediante el [ `child_run()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#child-run-name-none--run-id-none--outputs-none-) método:

```Python
!more hello_with_children.py
run_config = ScriptRunConfig(source_directory='.', script='hello_with_children.py')

local_script_run = exp.submit(run_config)
local_script_run.wait_for_completion(show_output=True)
print(local_script_run.get_status())

with exp.start_logging() as parent_run:
    for c,count in enumerate(range(5)):
        with parent_run.child_run() as child:
            child.log(name="Hello from child run", value=c)
```

> [!NOTE]
> A medida que se desplaza fuera del ámbito, ejecuciones secundarias se marcan automáticamente como completada.

También puede iniciar ejecuciones secundarios uno por uno, pero dado que cada creación da como resultado una llamada de red, es menos eficaz que envía un lote de ejecuciones.

Para consultar las ejecuciones de secundarios de un nodo principal específico, use el [ `get_children()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-) método.

```Python
list(parent_run.get_children())
```

## <a name="tag-and-find-runs"></a>Etiquetar y búsqueda de ejecuciones

En el servicio de Azure Machine Learning, puede usar etiquetas y propiedades para ayudar a organizar y consultar las ejecuciones para obtener información importante.

### <a name="add-properties-and-tags"></a>Agregar etiquetas y propiedades

Para agregar metadatos de búsqueda a las ejecuciones, use el [ `add_properties()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#add-properties-properties-) método. Por ejemplo, el código siguiente agrega el `"author"` propiedad a la ejecución:

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

Las propiedades son inmutables, por lo que crean un registro permanente con fines de auditoría. El siguiente código de ejemplo resulta en un error, porque ya se han agregado `"azureml-user"` como el `"author"` valor de propiedad en el código anterior:

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

A diferencia de las propiedades, las etiquetas son modificables. Para agregar información significativa y permite realizar búsqueda para los consumidores del experimento, use el [ `tag()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#tag-key--value-none-) método.

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

También puede agregar etiquetas de cadena simple. Cuando estas etiquetas aparecen en el diccionario de etiqueta, tienen un valor de `None`.

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

### <a name="query-properties-and-tags"></a>Etiquetas y propiedades de la consulta

Puede consultar se ejecuta dentro de un experimento para devolver una lista de ejecuciones que coinciden con las etiquetas y propiedades específicas.

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

## <a name="example-notebooks"></a>Cuadernos de ejemplo

Los notebooks siguientes muestran los conceptos de este artículo:

* Para obtener más información acerca de las API de registro, vea el [registro API portátil](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api/logging-api.ipynb).

* Para obtener más información acerca de cómo administrar se ejecuta con el SDK de Azure Machine Learning, consulte el [administrar ejecuciones de notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/manage-runs).

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre cómo registrar las métricas para los experimentos, consulte [registrar las métricas durante las ejecuciones de entrenamiento](how-to-track-experiments.md).