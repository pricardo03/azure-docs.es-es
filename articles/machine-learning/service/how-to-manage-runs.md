---
title: Inicio, supervisión y cancelación de las ejecuciones de entrenamiento en Python
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo iniciar, etiquetar y organizar sus experimentos de aprendizaje automático, además de establecer su estado.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 04/05/2019
ms.openlocfilehash: a93492b8ea97500fe3c761f3ac0c49f8c1342d09
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67074958"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Inicio, supervisión y cancelación de las ejecuciones de entrenamiento en Python

El [SDK de Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) y la [CLI de Machine Learning](reference-azure-machine-learning-cli.md) proporcionan varios métodos para supervisar, organizar y administrar las ejecuciones de entrenamiento y experimentación.

En este artículo se muestran ejemplos de las tareas siguientes:

* Supervisión del rendimiento de la ejecución.
* Cancelación o ejecuciones erróneas.
* Creación de ejecuciones secundarias.
* Etiquetado y búsqueda de ejecuciones.

## <a name="prerequisites"></a>Requisitos previos

Necesitará los siguientes elementos:

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Un [área de trabajo de Azure Machine Learning Service](setup-create-workspace.md).

* El SDK de Azure Machine Learning para Python (versión 1.0.21 o posterior). Para instalar o actualizar a la versión más reciente del SDK, consulte [Instalación o actualización del SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

    Para comprobar su versión del SDK de Azure Machine Learning, use el siguiente código:

    ```Python
    print(azureml.core.VERSION)
    ```

* La [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) y la [extensión de la CLI para Azure Machine Learning Service](reference-azure-machine-learning-cli.md).

## <a name="start-a-run-and-its-logging-process"></a>Inicio de una ejecución y su proceso de registro

### <a name="using-the-sdk"></a>Uso del SDK

Para configurar el experimento, importe las clases [Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py), [Experiment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py), [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) y [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) del paquete [azureml.core](https://docs.microsoft.com/python/api/azureml-core/azureml.core?view=azure-ml-py).

```Python
import azureml.core
from azureml.core import Workspace, Experiment, Run
from azureml.core import ScriptRunConfig

ws = Workspace.from_config()
exp = Experiment(workspace=ws, name="explore-runs")
```

Inicie una ejecución y su proceso de registro con el método [`start_logging()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#start-logging--args----kwargs-).

```Python
notebook_run = exp.start_logging()

notebook_run.log(name="message", value="Hello from run!")
```

### <a name="using-the-cli"></a>Uso de la CLI

Para iniciar una ejecución del experimento, use los pasos siguientes:

1. En el shell o el símbolo del sistema, use la CLI de Azure para autenticarse en la suscripción de Azure:

    ```azurecli-interactive
    az login
    ```

1. Adjunte una configuración de área de trabajo a la carpeta que contiene el script de entrenamiento. Reemplace `myworkspace` por el área de trabajo de Azure Machine Learning Service. Reemplace `myresourcegroup` por el grupo de recursos de Azure que contiene el área de trabajo:

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Este comando crea un subdirectorio `.azureml` que contiene archivos de entorno de conda y runconfig de ejemplo. También contiene un archivo `config.json` que se usa para comunicarse con el área de trabajo de Azure Machine Learning.

    Para obtener más información, consulte [az ml folder attach](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).

2. Para iniciar la ejecución, use el comando siguiente. Cuando use este comando, especifique el nombre del archivo runconfig (el texto que va antes de \*.runconfig, si mira el sistema de archivo) con respecto al parámetro -c.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > El comando `az ml folder attach` ha creado un subdirectorio `.azureml`, que contiene dos archivos runconfig de ejemplo. 
    >
    > Si tiene un script de Python que crea un objeto de configuración de ejecución mediante programación, puede usar [RunConfig.save()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) para guardarlo como un archivo runconfig.
    >
    > Para ver más archivos runconfig de ejemplo, consulte [https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml](https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml).

    Para obtener más información, consulte [az ml run submit-script](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

## <a name="monitor-the-status-of-a-run"></a>Supervisión del estado de una ejecución

### <a name="using-the-sdk"></a>Uso del SDK

Obtenga el estado de una ejecución con el método [`get_status()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--).

```Python
print(notebook_run.get_status())
```

Para obtener detalles adicionales sobre la ejecución, use el método [`get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--).

```Python
notebook_run.get_details()
```

Cuando la ejecución finalice correctamente, use el método [`complete()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-) para marcarla como completada.

```Python
notebook_run.complete()
print(notebook_run.get_status())
```

Si usa el patrón `with...as` de Python, la ejecución se marcará automáticamente como completada cuando esta quede fuera del ámbito. No tiene que marcar la ejecución como completada de forma manual.

```Python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print("Is it still running?",notebook_run.get_status())

print("Has it completed?",notebook_run.get_status())
```

### <a name="using-the-cli"></a>Uso de la CLI

1. Para ver una lista de ejecuciones del experimento, use el siguiente comando. Reemplace `experiment` por el nombre del experimento:

    ```azurecli-interactive
    az ml run list --experiment-name experiment
    ```

    Este comando devuelve un documento JSON que muestra información sobre las ejecuciones de este experimento.

    Para obtener más información, consulte [az ml experiment list](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

2. Para ver información sobre una ejecución concreta, use el siguiente comando. Reemplace `runid` por el identificador de la ejecución:

    ```azurecli-interactive
    az ml run show -r runid
    ```

    Este comando devuelve un documento JSON que muestra información sobre la ejecución.

    Para obtener más información, consulte [az ml run show](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-show).

## <a name="cancel-or-fail-runs"></a>Cancelación o ejecuciones erróneas

Si detecta un error o si la ejecución tarda demasiado en finalizar, puede cancelarla.

### <a name="using-the-sdk"></a>Uso del SDK

Para cancelar una ejecución mediante el SDK, use el método [`cancel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--):

```Python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')

local_script_run = exp.submit(run_config)
print("Did the run start?",local_script_run.get_status())

local_script_run.cancel()
print("Did the run cancel?",local_script_run.get_status())
```

Si la ejecución finaliza, pero contiene un error (por ejemplo, se ha usado el script de entrenamiento incorrecto), puede usar el método [`fail()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#fail-error-details-none---set-status-true-) para marcarla como errónea.

```Python
local_script_run = exp.submit(run_config)
local_script_run.fail()

print(local_script_run.get_status())
```

### <a name="using-the-cli"></a>Uso de la CLI

Para cancelar una ejecución con la CLI, use el siguiente comando. Reemplace `runid` por el identificador de la ejecución.

```azurecli-interactive
az ml run cancel -r runid
```

Para obtener más información, consulte [az ml run cancel](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-cancel).

## <a name="create-child-runs"></a>Creación de ejecuciones secundarias

Cree ejecuciones secundarias para agrupar ejecuciones relacionadas (por ejemplo, para diferentes iteraciones de ajuste de hiperparámetros).

> [!NOTE]
> Las ejecuciones secundarias solo se pueden crear mediante el SDK.

En este ejemplo de código se usa el script `hello_with_children.py` para crear un lote de cinco ejecuciones secundarias desde una ejecución enviada mediante el método [`child_run()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#child-run-name-none--run-id-none--outputs-none-):

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
> A medida que quedan fuera del ámbito, las ejecuciones secundarias se marcan automáticamente como completadas.

También puede iniciar las ejecuciones secundarias de una en una, pero, dado que cada creación implica una llamada de red, es menos eficaz que enviar un lote de ejecuciones.

Para consultar las ejecuciones secundarias de un elemento primario específico, use el método [`get_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-).

```Python
list(parent_run.get_children())
```

## <a name="tag-and-find-runs"></a>Etiquetado y búsqueda de ejecuciones

En Azure Machine Learning Service, puede usar etiquetas y propiedades para ayudar a organizar y consultar las ejecuciones a fin de obtener información importante.

### <a name="add-properties-and-tags"></a>Adición de etiquetas y propiedades

#### <a name="using-the-sdk"></a>Uso del SDK

Para agregar metadatos de búsqueda a las ejecuciones, use el método [`add_properties()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#add-properties-properties-). Por ejemplo, el código siguiente agrega la propiedad `"author"` a la ejecución:

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

Las propiedades son inmutables, por lo que crean un registro permanente con fines de auditoría. El siguiente ejemplo de código da como resultado un error porque ya hemos agregado `"azureml-user"` como el valor de propiedad `"author"` en el código anterior:

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

A diferencia de las propiedades, las etiquetas se pueden cambiar. Para agregar información que permite realizar búsquedas y que es significativa para los consumidores del experimento, use el método [`tag()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#tag-key--value-none-).

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

También puede agregar etiquetas de cadena simples. Cuando estas etiquetas aparecen en el diccionario de etiquetas, tienen un valor de `None`.

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

#### <a name="using-the-cli"></a>Uso de la CLI

> [!NOTE]
> Con la CLI, solo puede agregar o actualizar las etiquetas.

Para agregar o actualizar una etiqueta, use el siguiente comando:

```azurecli-interactive
az ml run update -r runid --add-tag quality='fantastic run'
```

Para obtener más información, consulte [az ml run update](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-update).

### <a name="query-properties-and-tags"></a>Consulta de etiquetas y propiedades

Puede consultar las ejecuciones de un experimento para devolver una lista de ejecuciones que coinciden con etiquetas y propiedades específicas.

#### <a name="using-the-sdk"></a>Uso del SDK

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

#### <a name="using-the-cli"></a>Uso de la CLI

La CLI de Azure admite las consultas [JMESPath](http://jmespath.org), que se pueden usar para filtrar las ejecuciones en función de etiquetas y propiedades. Para usar una consulta JMESPath con la CLI de Azure, especifíquela con el parámetro `--query`. En los ejemplos siguientes se muestran consultas básicas que usan etiquetas y propiedades:

```azurecli-interactive
# list runs where the author property = 'azureml-user'
az ml run list --experiment-name experiment [?properties.author=='azureml-user']
# list runs where the tag contains a key that starts with 'worth another look'
az ml run list --experiment-name experiment [?tags.keys(@)[?starts_with(@, 'worth another look')]]
# list runs where the author property = 'azureml-user' and the 'quality' tag starts with 'fantastic run'
az ml run list --experiment-name experiment [?properties.author=='azureml-user' && tags.quality=='fantastic run']
```

Para obtener más información sobre la consulta de resultados de la CLI de Azure, vea [Resultados de los comandos de consulta de la CLI de Azure](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest).

## <a name="example-notebooks"></a>Cuadernos de ejemplo

Los siguientes cuadernos muestran los conceptos de este artículo:

* Para obtener más información sobre las API de registro, vea el [cuaderno de API de registro](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api/logging-api.ipynb).

* Para obtener más información sobre cómo administrar ejecuciones con el SDK de Azure Machine Learning, consulte el [cuaderno de administración de ejecuciones](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/manage-runs).

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre cómo registrar las métricas de sus experimentos, consulte [Registrar métricas durante las ejecuciones de entrenamiento](how-to-track-experiments.md).
