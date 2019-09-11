---
title: Creación de conjuntos de datos para acceder a los datos con conjuntos de datos de Azure ML
titleSuffix: Azure Machine Learning service
description: Aprenda a crear conjuntos de datos desde diversos orígenes y registre los conjuntos de datos con el área de trabajo.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 08/22/2019
ms.openlocfilehash: 2ce64df5eeb8aa44ef714d6b465b7f2e1819635d
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70259288"
---
# <a name="create-and-access-datasets-preview-in-azure-machine-learning"></a>Creación y acceso a conjuntos de datos (versión preliminar) en Azure Machine Learning

En este artículo aprenderá a crear conjuntos de datos de Azure Machine Learning (versión preliminar) y a acceder a los datos de los experimentos locales o remotos.

Con los conjuntos de datos de Azure Machine Learning, puede: 

* **Mantener una copia de datos única en el almacenamiento** que los conjuntos de datos usan como referencia. 

* **Acceder fácilmente a los datos durante el entrenamiento de un modelo** sin preocuparse por la ruta de acceso a los datos ni la cadena de conexión.

* **Compartir datos y colaborar** con otros usuarios.

## <a name="prerequisites"></a>Requisitos previos

Para crear y trabajar con conjuntos de datos, necesita:

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Un [área de trabajo de Azure Machine Learning Service](how-to-manage-workspace.md)

* El [SDK de Azure Machine Learning para Python instalado](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), que incluye el paquete azureml-datasets.

> [!Note]
> Algunas clases Dataset (versión preliminar) tienen dependencias en el paquete [azureml dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py). Para los usuarios de Linux, estas clases solo se admiten en las siguientes distribuciones:  Red Hat Enterprise Linux, Ubuntu, Fedora y CentOS.

## <a name="dataset-types"></a>Tipos de conjuntos de datos

Los conjuntos de valores se clasifican en varios tipos en función de cómo los usuarios los consumen en el entrenamiento. Lista de tipos de conjuntos de datos:
* [TabularDatasets](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) representa los datos en formato tabular mediante el análisis del archivo o la lista de archivos proporcionados. Esto le proporciona la capacidad de materializar los datos en un DataFrame de Pandas. Se puede crear un objeto `TabularDataset` a partir de archivos csv, tsv o parquet, resultados de consultas SQL, etc. Para obtener una lista completa, consulte nuestra [documentación](https://aka.ms/tabulardataset-api-reference).
* [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) hace referencia a uno o varios archivos de sus almacenes de archivos o direcciones URL públicas. Este proporciona la capacidad de descargar o montar los archivos en el proceso. Los archivos pueden tener cualquier formato, lo que le ofrece una amplia gama de escenarios de aprendizaje automático, incluido el aprendizaje profundo.

Para obtener más información sobre los próximos cambios en la API, consulte [esto](https://aka.ms/tabular-dataset).

## <a name="create-datasets"></a>Creación de conjuntos de datos 

Mediante la creación de un conjunto de datos, puede crear una referencia a la ubicación del origen de datos, junto con una copia de sus metadatos. Los datos se mantienen en la ubicación existente, por lo que no se genera ningún costo de almacenamiento adicional.

Para que Azure Machine Learning Service pueda acceder a los datos, hay que crear los conjuntos de datos a partir de las rutas de acceso de los [almacenes de datos de Azure](how-to-access-data.md) o las direcciones URL web públicas.

Para crear conjuntos de datos desde un [almacén de datos de Azure](how-to-access-data.md):

* Compruebe que dispone de acceso `contributor` o `owner` para el almacén de datos de Azure registrado.

* Cree el conjunto de datos mediante una referencia a una ruta de acceso en el almacén de datos.

```Python
from azureml.core.workspace import Workspace
from azureml.core.datastore import Datastore
from azureml.core.dataset import Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()

# retrieve an existing datastore in the workspace by name
datastore = Datastore.get(workspace, datastore_name)
```
### <a name="create-tabulardatasets"></a>Creación de TabularDatasets

Use el método `from_delimited_files()` en la clase `TabularDatasetFactory` para leer archivos en formato csv o tsv y cree una clase TabularDataset sin registrar. Si está leyendo de varios archivos, los resultados se agregarán en una representación tabular.

```Python
# create a TabularDataset from multiple paths in datastore
datastore_paths = [
                  (datastore, 'weather/2018/11.csv'),
                  (datastore, 'weather/2018/12.csv'),
                  (datastore, 'weather/2019/*.csv')
                 ]
weather_ds = Dataset.Tabular.from_delimited_files(path=datastore_paths)

# create a TabularDataset from a delimited file behind a public web url
web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path)

# preview the first 3 rows of titanic_ds
titanic_ds.take(3).to_pandas_dataframe()
```

| |PassengerId|Survived|Pclass|NOMBRE|Sex|Edad|SibSp|Parch|Vale|Fare|Cabin|Embarked
-|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
0|1|0|3|Braund, Mr. Owen Harris|hombre|22,0|1|0|A/5 21171|7,2500||S
1|2|1|1|Cumings, Mrs. John Bradley (Florence Briggs Th...|mujer|38,0|1|0|PC 17599|71,2833|C85|C
2|3|1|3|Heikkinen, Miss. Laina|mujer|26,0|0|0|STON/O2. 3101282|7,9250||S

### <a name="create-filedatasets"></a>Crear FileDatasets
Use el método `from_files()` en la clase `FileDatasetFactory` para cargar archivos en cualquier formato y crear un elemento FileDataset sin registrar.

```Python
# create a FileDataset from multiple paths in datastore
datastore_paths = [
                  (datastore, 'animals/dog/1.jpg'),
                  (datastore, 'animals/dog/2.jpg'),
                  (datastore, 'animals/dog/*.jpg')
                 ]
animal_ds = Dataset.File.from_files(path=datastore_paths)

# create a FileDataset from image and label files behind public web urls
web_paths = [
            'https://azureopendatastorage.blob.core.windows.net/mnist/train-images-idx3-ubyte.gz',
            'https://azureopendatastorage.blob.core.windows.net/mnist/train-labels-idx1-ubyte.gz'
           ]          
mnist_ds = Dataset.File.from_files(path=web_paths)
```
## <a name="register-datasets"></a>Registro de conjuntos de datos

Para completar el proceso de creación, registre los conjuntos de datos con el área de trabajo:

Use el método `register()` para registrar los conjuntos de datos en el área de trabajo de modo que puedan compartirse con otros usuarios y reutilizarse en varios experimentos.

```Python
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'titanic training data')
```

## <a name="version-datasets"></a>Conjuntos de datos de versión

Puede registrar un nuevo conjunto de datos con el mismo nombre mediante la creación de una nueva versión. La versión del conjunto de datos es una manera de marcar el estado de los datos, por lo que puede aplicar una versión específica del conjunto de datos para experimentación o reproducción futura. Escenarios típicos para considerar el control de versiones: 
* Cuando hay nuevos datos disponibles para el reentrenamiento.
* Cuando se aplican diferentes enfoques de preparación de datos o de ingeniería de características.

```Python
# create a TabularDataset from new Titanic training data
web_paths = [
            'https://dprepdata.blob.core.windows.net/demo/Titanic.csv',
            'https://dprepdata.blob.core.windows.net/demo/Titanic2.csv'
           ]          
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_paths)

# create a new version of titanic_ds
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'new titanic training data',
                                 create_new_version = True)
```


## <a name="access-your-data-during-training"></a>Acceso a los datos durante el aprendizaje

Es posible acceder local y remotamente a los conjuntos de datos registrados en clústeres de proceso como el proceso de Azure Machine Learning. Para acceder al conjunto de datos registrado en experimentos, use el siguiente código para obtener el área de trabajo y el conjunto de datos registrado por nombre. De forma predeterminada, el método [`get_by_name()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-by-name-workspace--name--version--latest--) en la clase `Dataset` devuelve la versión más reciente del conjunto de datos registrados con el área de trabajo.

```Python
%%writefile $script_folder/train.py

from azureml.core import Dataset, Run

run = Run.get_context()
workspace = run.experiment.workspace

dataset_name = 'titanic_ds'

# Get a dataset by name
titanic_ds = Dataset.get_by_name(workspace=workspace, name=dataset_name)

# Load a TabularDataset into pandas DataFrame
df = titanic_ds.to_pandas_dataframe()
```

## <a name="next-steps"></a>Pasos siguientes

* Use el aprendizaje automático automatizado para [entrenar con TabularDatasets](https://aka.ms/automl-dataset).
* Para ver más ejemplos del uso del entrenamiento con conjuntos de datos, consulte los [cuadernos de ejemplo](https://aka.ms/dataset-tutorial).
