---
title: Iniciar, supervisar y cancelar las ejecuciones de entrenamiento en Python
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo iniciar, establecer el estado de etiqueta y organizar los experimentos de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 04/05/2019
ms.openlocfilehash: 82df2258116ce55fa440b67ec0a66b106d0d72c7
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2019
ms.locfileid: "59494054"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Iniciar, supervisar y cancelar las ejecuciones de entrenamiento en Python

El [SDK de Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) proporciona varios métodos para supervisar, organizar y administrar las ejecuciones para el aprendizaje y experimentación.

Este tema de procedimientos muestra ejemplos de las tareas siguientes:

* [Monitor de rendimiento de ejecución](#monitor)
* [Cancela o no se ejecuta](#cancel)
* [Crear ejecuciones de secundarios](#children)
* [Etiquetar y búsqueda de ejecuciones](#tag)

## <a name="prerequisites"></a>Requisitos previos

Necesitará lo siguiente:

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Un área de trabajo de Azure Machine Learning. Consulte [crear un área de trabajo del servicio de Azure Machine Learning](setup-create-workspace.md).

* El SDK de Azure Machine Learning de Python instalada (versión 1.0.21 o posterior). Para instalar o actualizar a la versión más reciente del SDK, vaya a la [instalar o actualizar el SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) página.

    Para comprobar la versión del SDK de Azure Machine Learning, use el código siguiente.

    ```Python
    print(azureml.core.VERSION)
    ```

<a name="monitor"></a>

## <a name="start-a-run-and-set-its-status"></a>Iniciar una ejecución y establecer su estado

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

Obtener el estado de la ejecución con [ `get_status()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--).

```Python
print(notebook_run.get_status())
```

Para obtener detalles adicionales sobre el uso de ejecución [ `get_details()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--).

```Python
notebook_run.get_details()
```

Cuando la ejecución finaliza correctamente, utilice el [ `complete()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-) método para marcarlo como completado.

```Python
notebook_run.complete()
print(notebook_run.get_status())
```

También puede usar de Python `with...as` patrón. En este contexto, la ejecución marcará automáticamente propio como completada cuando la ejecución fuera del ámbito. De este modo no es necesario marcar manualmente la ejecución como completado.

```Python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print("Is it still running?",notebook_run.get_status())

print("Has it completed?",notebook_run.get_status())
```

<a name="cancel"></a>

## <a name="cancel-or-fail-runs"></a>Cancela o no se ejecuta

 Si observa un error o la ejecución parece tardar un rato en Finalizar, use el [ `cancel()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--) método para detener la ejecución antes de que finalice y marcarla como canceladas.

```Python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')

local_script_run = exp.submit(run_config)
print("Did the run start?",local_script_run.get_status())

local_script_run.cancel()
print("Did the run cancel?",local_script_run.get_status())
```

Si la ejecución finaliza, pero contiene un error como, se usó el script de entrenamiento incorrecto, puede usar el [ `fail()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#fail-error-details-none---set-status-true-) método que va a marcar como no superado.

```Python
local_script_run = exp.submit(run_config)
local_script_run.fail()

print(local_script_run.get_status())
```

<a name="children"></a>

## <a name="create-child-runs"></a>Crear ejecuciones de secundarios

Crear a secundario se ejecuta para agrupar series relacionadas, como para las iteraciones de ajuste de hiperparámetros diferentes.

Este ejemplo de código usa la secuencia de comandos hello_with_children.py para crear un lote de cinco ejecuciones secundarios desde dentro de un envío mediante ejecución el [ `child_run()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#child-run-name-none--run-id-none--outputs-none-) método.

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
> Elemento secundario se ejecuta completa automáticamente cuando se mueven fuera del ámbito.

También puede iniciar ejecuciones secundarios uno por uno, pero dado que cada creación da como resultado una llamada de red, es menos eficaz que envía un lote de ejecuciones.

Use la [ `get_children()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-) se ejecuta el método para consultar el elemento secundario de un nodo principal específico.

```Python
list(parent_run.get_children())
```

<a name="tag"></a>

## <a name="tag-and-find-runs"></a>Etiquetar y búsqueda de ejecuciones

En el servicio de Azure Machine Learning, puede usar etiquetas y propiedades para ayudar a organizar y consultar las ejecuciones para obtener información importante.

### <a name="add-properties-and-tags"></a>Agregar etiquetas y propiedades

Use la [ `add_properties()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#add-properties-properties-) para agregar metadatos de búsqueda a las ejecuciones. Por ejemplo, el código siguiente agrega la propiedad "author" a la ejecución.

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

Las propiedades son inmutables, lo que resulta útil como un registro permanente con fines de auditoría. El ejemplo de código siguiente provocará un error, puesto que ya se han agregado "azureml-user" como la propiedad "author" en el código anterior.

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

Sin embargo, las etiquetas son modificables. Use [ `tag()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#tag-key--value-none-) para agregar información significativa y permite realizar búsqueda para los consumidores del experimento.

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

También puede agregar una etiqueta de cadena simple. Aparece en el diccionario de etiqueta con el valor de `None`.

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

### <a name="query-properties-and-tags"></a>Etiquetas y propiedades de la consulta

Puede consultar se ejecuta dentro de un experimento que coinciden con las etiquetas y propiedades específicas.

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

## <a name="example-notebooks"></a>Cuadernos de ejemplo

Los cuadernos siguientes muestran los conceptos de este artículo:

* Para más información sobre el registro de API, consulte el [registro API portátil](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api/logging-api.ipynb).

* Para obtener información adicional acerca de cómo administrar se ejecuta con el SDK de Azure Machine Learning, consulte el [administrar ejecuciones de notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/manage-runs).

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre cómo registrar las métricas para los experimentos, consulte el [registrar las métricas durante las ejecuciones de entrenamiento](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiment) artículo.