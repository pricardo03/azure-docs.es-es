---
title: Definición de conjunto de datos y control de versiones con los conjuntos de datos de aprendizaje automático de Azure
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo actualizar las definiciones de conjunto de datos y administrar el ciclo de vida de las definiciones
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.reviewer: larryfr
ms.date: 05/02/2019
ms.openlocfilehash: e58ce156deaaad259ea7b74521bcf9b79afbd183
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66146215"
---
# <a name="update-and-manage-the-lifecycle-of-dataset-definitions"></a>Actualizar y administrar el ciclo de vida de las definiciones de conjunto de datos

Obtenga información sobre cómo actualizar y administrar las definiciones de conjunto de datos con conjuntos de datos de Azure Machine Learning (versión preliminar).

Las definiciones de conjunto de datos son las recetas que se utiliza para preparar los conjuntos de datos, que incluyen tanto la referencia a los datos de origen y realizar los pasos de transformación. Un conjunto de datos puede tener muchas definiciones, y cada definición tiene su propio ciclo de vida.

El siguiente escenario es un ejemplo del uso de las definiciones de conjunto de datos:

1. Crear una canalización que consume la definición actual del conjunto de datos de aprendizaje automático.
1. Los cambios de datos subyacente; se han agregado nuevos atributos.
1. Cree una nueva versión de la definición que agrega las transformaciones adicionales para controlar los nuevos atributos.

En este ejemplo, la canalización existente continúa consumir la versión de la definición original. La nueva versión de definición se puede usar para escenarios nuevos, tales como entrenar un modelo o crear una canalización.

## <a name="prerequisites"></a>Requisitos previos

Debe tener una suscripción de Azure y un área de trabajo para registrar el conjunto de datos con el fin de administrar el ciclo de vida de las definiciones de conjunto de datos.

El archivo de ejemplo usado en los ejemplos de este documento está disponible en [ https://dprepdata.blob.core.windows.net/dataset-sample-files/crime.csv ](https://dprepdata.blob.core.windows.net/dataset-sample-files/crime.csv).

## <a name="update-dataset-definitions"></a>Actualizar las definiciones de conjunto de datos

Se creará en primer lugar y registrar un conjunto de datos con el área de trabajo.

```python
from azureml.core import Workspace, Datastore, Dataset

# change the configuration for workspace and Datastore
subscription_id = 'your subscription id'
resource_group = 'your resource group name'
workspace_name = 'your workspace name'
datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace(subscription_id, resource_group, workspace_name)

# get a Datastore that has already been created in the workspace
dstore = Datastore.get(workspace, datastore_name)

# create an in-memory Dataset from Datastore
datapath = dstore.path('data/src/crime.csv')
dataset = Dataset.from_delimited_files(datapath)

# register the Dataset with the workspace. if a Dataset with the same name already exists, retrieve it by turning `exist_ok = True`
dataset_name = 'crime dataset'
dataset = dataset.register(workspace = workspace, 
                 name = dataset_name, 
                 description = 'crime dataset for demo', 
                 tags = {'year':'2019', 'month':'Apr'},
                 exist_ok = True)
```

La primera definición de conjunto de datos (`version_id` = 1) se crea cuando se crea el conjunto de datos. A continuación, cada vez que actualice la definición de conjunto de datos, un nuevo version_id se asignará a la definición más reciente.

```python
# get the Dataset from the workspace by name
dataset = workspace.datasets['dataset_name']

# get the latest Dataset definition
ds_def = dataset.get_definition()

# update the Dataset definition to select only the columns I'm interested in
ds_def = ds_def.keep_columns(['ID', 'Arrest', 'Latitude', 'Longitude'])

# with this update, the latest definition for dataset now has `version_id = 2`
dataset = dataset.update_definition(ds_def, 'select useful column')

dataset = workspace.datasets['dataset_name']
dataset.head(5)
```
<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>Arrest</th>
      <th>Latitud</th>
      <th>Longitud</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10498554</td>
      <td>FALSE</td>
      <td>41.69283384</td>
      <td>-87.60431945</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10516598</td>
      <td>FALSE</td>
      <td>41.74410697</td>
      <td>-87.66449429</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10519196</td>
      <td>FALSE</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>10519591</td>
      <td>FALSE</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>10534446</td>
      <td>FALSE</td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>

Esta actualización de definición no borrará las definiciones de conjunto de datos anteriores. Todavía puede acceder y consumir las definiciones anteriores.

```python
# specify `version_id` to get a previous definition 
ds_def_old = dataset.get_definition(version_id = 1)
ds_def_old.head(5)
```
<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>Case Number</th>
      <th>Date</th>
      <th>Bloquear</th>
      <th>IUCR</th>
      <th>Primary Type</th>
      <th>DESCRIPCIÓN</th>
      <th>Location Description</th>
      <th>Arrest</th>
      <th>Domestic</th>
      <th>...</th>
      <th>Ward</th>
      <th>Community Area</th>
      <th>FBI Code</th>
      <th>X Coordinate</th>
      <th>Y Coordinate</th>
      <th>Year</th>
      <th>Updated On</th>
      <th>Latitud</th>
      <th>Longitud</th>
      <th>Ubicación</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10498554</td>
      <td>HZ239907</td>
      <td>4/4/2016 23:56</td>
      <td>007XX E 111TH ST</td>
      <td>1153</td>
      <td>PRÁCTICA ENGAÑOSA</td>
      <td>ROBO DE IDENTIDAD FINANCIERA A TRAVÉS DE 300 USD</td>
      <td>OTHER</td>
      <td>FALSE</td>
      <td>FALSE</td>
      <td>...</td>
      <td>9</td>
      <td>50</td>
      <td>11</td>
      <td>1183356</td>
      <td>1831503</td>
      <td>2016</td>
      <td>5/11/2016 15:48</td>
      <td>41.69283384</td>
      <td>-87.60431945</td>
      <td>(41.692833841, -87.60431945)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10516598</td>
      <td>HZ258664</td>
      <td>4/15/2016 17:00</td>
      <td>082XX S MARSHFIELD AVE</td>
      <td>890</td>
      <td>THEFT</td>
      <td>DESDE LA CREACIÓN</td>
      <td>RESIDENCIA</td>
      <td>FALSE</td>
      <td>FALSE</td>
      <td>...</td>
      <td>21</td>
      <td>71</td>
      <td>6</td>
      <td>1166776</td>
      <td>1850053</td>
      <td>2016</td>
      <td>5/12/2016 15:48</td>
      <td>41.74410697</td>
      <td>-87.66449429</td>
      <td>(41.744106973, -87.664494285)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10519196</td>
      <td>HZ261252</td>
      <td>4/15/2016 10:00</td>
      <td>104XX S SACRAMENTO AVE</td>
      <td>1154</td>
      <td>PRÁCTICA ENGAÑOSA</td>
      <td>ROBO DE IDENTIDAD FINANCIERA 300 DÓLARES Y EN</td>
      <td>RESIDENCIA</td>
      <td>FALSE</td>
      <td>FALSE</td>
      <td>...</td>
      <td>19</td>
      <td>74</td>
      <td>11</td>
      <td></td>
      <td></td>
      <td>2016</td>
      <td>5/12/2016 15:50</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>10519591</td>
      <td>HZ261534</td>
      <td>4/15/2016 9:00</td>
      <td>113XX UARDAR ASUSTADOS</td>
      <td>1120</td>
      <td>PRÁCTICA ENGAÑOSA</td>
      <td>FALSIFICACIÓN</td>
      <td>RESIDENCIA</td>
      <td>FALSE</td>
      <td>FALSE</td>
      <td>...</td>
      <td>9</td>
      <td>49</td>
      <td>10</td>
      <td></td>
      <td></td>
      <td>2016</td>
      <td>5/13/2016 15:51</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>10534446</td>
      <td>HZ277630</td>
      <td>4/15/2016 10:00</td>
      <td>055XX N KEDZIE AVE</td>
      <td>890</td>
      <td>THEFT</td>
      <td>DESDE LA CREACIÓN</td>
      <td>SCHOOL, PÚBLICO, CREAR</td>
      <td>FALSE</td>
      <td>FALSE</td>
      <td>...</td>
      <td>40</td>
      <td>13</td>
      <td>6</td>
      <td></td>
      <td></td>
      <td>2016</td>
      <td>5/25/2016 15:59</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>

Como podemos ver en el resultado, la primera versión de definición de conjunto de datos mantiene todas las columnas de los datos de delitos, independientemente de la actualización posterior. Por ejemplo, si tiene un aprendizaje automático modelar actualmente consumiendo la primera versión del conjunto de datos y el uso `Date` columna del conjunto de datos como una de las características, no se encuentra algún error con la actualización de definición más reciente, lo que mantiene solo `ID`, `Arrest`, `Latitude`, `Longitude` columnas de los datos de crimen.

## <a name="how-to-access-dataset-definitions"></a>Cómo obtener acceso a las definiciones de conjunto de datos

Para obtener una lista de todas las definiciones, use `get_definitions()`. Para obtener una versión específica de una definición, use `get_definition()`. El ejemplo siguiente se muestra cómo recuperar una lista de todas las definiciones y, a continuación, recuperar la versión 1:

```python
# list all definitions for the dataset
dataset.get_definitions()
# get version ID 1
dataset.get_definition(version_id=1)
```

La salida de `get_definitions()` es similar al ejemplo siguiente:

```text
{'2': VersionID: 2, State: active, Created: 2019-04-19 16:43:52.439890+00:00, Modified: 2019-04-19 16:43:52.439890+00:00, Notes: select useful column,
 '1': VersionID: 1, State: active, Created: 2019-04-19 16:39:14.112046+00:00, Modified: 2019-04-19 16:44:12.912663+00:00, Notes: None}
```

Una vez que haya recuperado una definición, puede usar con los modelos de aprendizaje automático o en una canalización de aprendizaje automático.

## <a name="manage-lifecycle-of-dataset-definitions"></a>Administrar el ciclo de vida de las definiciones de conjunto de datos

Puede administrar el ciclo de vida de cada definición de conjunto de datos de forma independiente. Hay tres fases del ciclo de vida: activo, en desuso o archivado.

### <a name="active"></a>Activas

Cuando se crea una nueva definición de conjunto de datos, está activo de forma predeterminada. 

### <a name="deprecate"></a>Dejar de usar

Las definiciones de conjunto de datos pueden en desuso cuando ya no se recomienda el uso y un reemplazo está disponible. Cuando se usa una definición de conjunto de datos en desuso en canalizaciones de machine learning, se devuelve un mensaje de advertencia, pero no se bloqueará la ejecución.

Al dejar de usar una definición de conjunto de datos, especifique el identificador de conjunto de datos y el identificador de versión de definición de conjunto de datos para la definición de reemplazo. Esta información se almacena y se usa en el mensaje de advertencia cuando se intenta consumir una definición de conjunto de datos en desuso.

```python
# get the definition that you want to deprecate
ds_def = dataset.get_definition(version_id = 1)

# deprecate it by providing the information for the replacement definition, which is recommended to be used in machine learning scenarios
ds_def.deprecate(deprecate_by_dataset_id=dataset.id, deprecated_by_definition_version=2)
```

### <a name="archive"></a>Archivar

Las definiciones de conjunto de datos se pueden archivar cuando las definiciones no deberían usarse por cualquier motivo (por ejemplo, los datos subyacentes ya no está disponibles). Cuando se usa una definición de conjunto de datos archivada en canalizaciones de machine learning, se bloqueará la ejecución con error.

```python
# archive the definition with `version_id = 1`
ds_def = dataset.get_definition(version_id = 1)
ds_def.archive()
```

### <a name="reactivate"></a>Reactivar

Puede reactivar fácilmente cualquier definición de conjunto de datos en desuso o archivado.

```python
# reactivate Dataset definition with `version_id =1` which was archived in the previous step
ds_def = dataset.get_definition(version_id = 1)
ds_def.reactivate()
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo trabajar con conjuntos de datos, vea [crear y registrar los conjuntos de datos de Azure Machine Learning](how-to-create-register-datasets.md).

Para obtener un ejemplo del uso de conjuntos de datos, vea el [cuadernos de ejemplo](https://aka.ms/dataset-tutorial).
