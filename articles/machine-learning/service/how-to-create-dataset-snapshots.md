---
title: Comparar & reproducir los datos con el tiempo con las instantáneas
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo comparar los datos con el tiempo y garantizar la reproducibilidad con instantáneas de conjunto de datos
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.date: 05/02/2019
ms.openlocfilehash: ed10cb259802321769605bc0399a610131ddb174
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029151"
---
# <a name="compare-data-and-ensure-reproducibility-with-snapshots-preview"></a>Comparar los datos y garantizar la reproducibilidad con instantáneas (versión preliminar)

En este artículo, obtendrá información sobre cómo crear y administrar las instantáneas de su [conjuntos de datos de Azure Machine Learning](how-to-create-register-datasets.md) (conjuntos de datos) para que pueda capturar o comparar los datos con el tiempo. Los conjuntos de datos facilitan el acceso y trabajar con los datos en la nube en distintos escenarios. 

**Las instantáneas del conjunto de datos** almacenar un perfil (estadísticas de resumen) de los datos en el momento de crearlo. Puede también almacenar una copia de los datos en la instantánea para la reproducibilidad. 

>[!Important]
> Las instantáneas de incurrir en costos de almacenamiento. Almacenar una copia de datos de la instantánea requiere incluso más almacenamiento. Use [ `dataset.delete_snapshot()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#delete-snapshot-snapshot-name-) cuando ya no son necesarios.

## <a name="when-to-use-snapshots"></a>Cuándo usar instantáneas

Hay tres usos principales de las instantáneas:

+ **Validación de modelos**: Compare el perfil de datos de distintas instantáneas entre ejecuciones de entrenamiento o con datos de producción. 

+ **Modelar la reproducibilidad**: Reproducir los resultados mediante una llamada a una instantánea que incluye datos durante el entrenamiento. 

+ **Seguimiento de los datos con el tiempo**: Vea cómo ha evolucionado el conjunto de datos por [comparar perfiles](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_snapshot.datasetsnapshot?view=azure-ml-py#compare-profiles-rhs-dataset-snapshot--include-columns-none--exclude-columns-none--histogram-compare-method--histogramcomparemethod-wasserstein--0--)
  
## <a name="prerequisites"></a>Requisitos previos

Para crear instantáneas del conjunto de datos, necesita un conjunto de datos registrado Azure Machine Learning. Si no tiene una, consulte [conjuntos de datos crear y registrar](how-to-create-register-datasets.md).

## <a name="create-dataset-snapshots"></a>Crear instantáneas del conjunto de datos

Para crear una instantánea de un conjunto de datos, utilice [ `dataset.create_snapshot()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?#create-snapshot-snapshot-name--compute-target-none--create-data-snapshot-false--target-datastore-none-) desde el SDK de Azure Machine Learning. 

De forma predeterminada, la instantánea almacena el perfil (estadísticas de resumen) de los datos con la versión más reciente [definición del conjunto de datos](how-to-manage-dataset-definitions.md) aplicado. Una definición de conjunto de datos contiene un registro de los pasos de transformación definido para los datos. Es una excelente manera de que la preparación de datos funcione reproducible.

Si lo desea, también puede incluir una copia de los datos de la instantánea mediante la adición de `create_data_snapshot = True`.  Estos datos pueden ser útiles para la reproducibilidad. 

Este ejemplo se utiliza [los datos de ejemplo](https://dprepdata.blob.core.windows.net/dataset-sample-files/crime.csv) y un conjunto de datos denominado `dataset_crime` creado mediante el artículo, ["crear y registrar los conjuntos de datos"](how-to-create-register-datasets.md).

```Python
from azureml.core.dataset import Workspace, Dataset
from azureml.data.dataset_snapshot import DatasetSnapshot
import datetime

# get existing workspace
workspace = Workspace.from_config()

# get existing, named dataset:
dataset = workspace.Dataset['dataset_crime']

# assign name to snapshot
snapshot_name = 'snapshot_' + datetime.datetime.today().strftime('%Y%m%d%H%M%S')

# create snapshot to store profile from a remote environment
# and include copy of data
snapshot = dataset.create_snapshot(snapshot_name = snapshot_name,
                                   compute_target = remote_compute_target,
                                   create_data_snapshot = True)
```
 

Dado que las instantáneas se crean de forma asincrónica, use la [ `wait_for_completion()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_snapshot.datasetsnapshot?view=azure-ml-py#wait-for-completion-show-output-true--status-update-frequency-10-) método para supervisar el proceso.

```python
# monitor process every 10 seconds
snapshot.wait_for_completion(show_output=True, status_update_frequency=10)

# return snapshot just created
dataset.get_snapshot(snapshot_name)
```

Salida:

```
Action status: Running
Action status: Running
Action status: Completed


DatasetSnapshot(Name: snapshot_20190411165420,
Dataset Id: 66cd0830-2f72-41e5-b677-d2d953f54821,
Workspace: "your_workspace_name",
Dataset definition version: 1,
Snapshot created date: 2019-05-11 17:24:00+00:00)
```

Use [ `dataset.delete_snapshot()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#delete-snapshot-snapshot-name-) cuando ya no son necesarios.

## <a name="find-snapshots"></a>Buscar las instantáneas

Para recuperar una instantánea existente, use [ `get_snapshot()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#get-snapshot-snapshot-name-).

Para obtener una lista de las instantáneas guardadas de un conjunto de datos determinado, use [ `get_all_snapshots()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#get-all-snapshots--). 

```Python
# Get named snapshot for this dataset
dataset.get_snapshot(snapshot_name)

# Get all snapshots for this dataset
dataset.get_all_snapshots()
```

## <a name="get-data-and-profiles-from-snapshots"></a>Obtener perfiles y datos de instantáneas

Cada instantánea genera un perfil del conjunto de datos, que incluye las estadísticas de resumen y le ofrece la opción para guardar una copia de los datos.

### <a name="get-the-data-profile"></a>Obtener el perfil de datos

Use [ `get_profile()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#get-profile-arguments-none--generate-if-not-exist-true--workspace-none--compute-target-none-) tener acceso al perfil de los datos.  Puede usar este perfil para comparar los datos de entrenamiento y de producción, por ejemplo.

```Python
snapshot.get_profile()
```

||Type|Min|max|Recuento|Falta el recuento|No falta el recuento|Falta el porcentaje|Recuento de errores|Recuento vacío|Cuantil 0,1 %|Cuantil 1 %|Cuantil 5 %|Cuantil 25 %|Cuantil 50 %|Cuantil 75 %|Cuantil 95 %|Cuantil 99 %|Cuantil 99,9 %|Media|Desviación estándar|Variance|Asimetría|Curtosis
-|----|---|---|-----|-------------|-----------------|---------------|-----------|-----------|-------------|-----------|-----------|------------|------------|------------|------------|------------|--------------|----|------------------|--------|--------|--------
ID|FieldType.INTEGER|1.04986e+07|1.05351e+07|10.0|0.0|10.0|0.0|0.0|0.0|1.04986e+07|1.04992e+07|1.04986e+07|1.05166e+07|1.05209e+07|1.05259e+07|1.05351e+07|1.05351e+07|1.05351e+07|1.05195e+07|12302.7|1.51358e+08|-0.495701|-1.02814
Case Number|FieldType.STRING|HZ239907|HZ278872|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Date|FieldType.DATE|2016-04-04 23:56:00+00:00|2016-04-15 17:00:00+00:00|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Block|FieldType.STRING|004XX UARDAR KILBOURN|113XX UARDAR ASUSTADOS|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
IUCR|FieldType.INTEGER|810|1154|10.0|0.0|10.0|0.0|0.0|0.0|810|850|810|890|1136|1153|1154|1154|1154|1058.5|137.285|18847.2|-0.785501|-1.3543
Primary Type|FieldType.STRING|PRÁCTICA ENGAÑOSA|THEFT|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
DESCRIPCIÓN|FieldType.STRING|COMPROBACIÓN DE FANTASMA|A TRAVÉS DE 500 USD|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Location Description|FieldType.STRING||SCHOOL, PÚBLICO, CREAR|10.0|0.0|10.0|0.0|0.0|1.0||||||||||||||
Arrest|FieldType.BOOLEAN|False|False|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Domestic|FieldType.BOOLEAN|False|False|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
latido|FieldType.INTEGER|531|2433|10.0|0.0|10.0|0.0|0.0|0.0|531|531|531|614|1318.5|1911|2433|2433|2433|1371.1|692.094|478994|0.105418|-1.60684
Distrito|FieldType.INTEGER|5|24|10.0|0.0|10.0|0.0|0.0|0.0|5|5|5|6|13|19|24|24|24|13.5|6.94822|48.2778|0.0930109|-1.62325
Ward|FieldType.INTEGER|1|48|10.0|0.0|10.0|0.0|0.0|0.0|1|5|1|9|22.5|40|48|48|48|24,5|16.2635|264.5|0.173723|-1.51271
Community Area|FieldType.INTEGER|4|77|10.0|0.0|10.0|0.0|0.0|0.0|4|8.5|4|24|37.5|71|77|77|77|41.2|26.6366|709.511|0.112157|-1.73379


### <a name="get-the-data-from-the-snapshot"></a>Obtener los datos de la instantánea

Para obtener una copia de los datos guardados en una instantánea del conjunto de datos, generar un DataFrame de pandas con los [ `to_pandas_dataframe()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#to-pandas-dataframe--) método.

Este método produce un error si no se ha solicitado una copia de los datos durante la creación de instantáneas. 

```Python
snapshot.to_pandas_dataframe().head(3)
```

||ID|Case Number|Date|Block|IUCR|Primary Type|DESCRIPCIÓN|Location Description|Arrest|Domestic|...|Ward|Community Area|FBI Code|X Coordinate|Y Coordinate|Year|Updated On|Latitud|Longitud|Location
-|--|-----------|----|-----|----|------------|-----------|--------------------|------|--------|---|----|--------------|--------|------------|------------|----|----------|--------|---------|--------
0|10498554|HZ239907|2016-04-04 23:56:00|007XX E 111TH ST|1153|PRÁCTICA ENGAÑOSA|ROBO DE IDENTIDAD FINANCIERA A TRAVÉS DE 300 USD|OTHER|False|False|...|9|50|11|1183356.0|1831503.0|2016|2016-05-11 15:48:00|41.692834|-87.604319|(41.692833841, -87.60431945)
1|10516598|HZ258664|2016-04-15 17:00:00|082XX S MARSHFIELD AVE|890|THEFT|DESDE LA CREACIÓN|RESIDENCIA|False|False|...|21|71|6|1166776.0|1850053.0|2016|2016-05-12 15:48:00|41.744107|-87.664494|(41.744106973, -87.664494285)
2|10519196|HZ261252|2016-04-15 10:00:00|104XX S SACRAMENTO AVE|1154|PRÁCTICA ENGAÑOSA|ROBO DE IDENTIDAD FINANCIERA 300 DÓLARES Y EN|RESIDENCIA|False|False|...|19|74|11|NaN|NaN|2016|2016-05-12 15:50:00|NaN|NaN|


## <a name="next-steps"></a>Pasos siguientes

* Consulte el SDK de [Introducción](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) para obtener ejemplos de uso y patrones de diseño.

* Para obtener un ejemplo del uso de instantáneas del conjunto de datos, vea el [cuadernos de ejemplo](https://aka.ms/dataset-tutorial).
