---
title: Creación de conjuntos de datos para acceder a los datos con conjuntos de datos de Azure ML
titleSuffix: Azure Machine Learning
description: Aprenda a crear conjuntos de datos desde diversos orígenes y registre los conjuntos de datos con el área de trabajo.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 11/04/2019
ms.openlocfilehash: aabbac60acc53cfccc29fc3dbd06575e09840d83
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73497118"
---
# <a name="create-and-access-datasets-preview-in-azure-machine-learning"></a>Creación de conjuntos de datos y acceso a ellos (versión preliminar) en Azure Machine Learning

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo aprenderá a crear conjuntos de datos de Azure Machine Learning (versión preliminar) y a acceder a los datos de los experimentos locales o remotos.

Con los conjuntos de datos de Azure Machine Learning, puede:

* **Mantener una copia de datos única en el almacenamiento** que los conjuntos de datos usan como referencia.

* **Acceder fácilmente a los datos durante el entrenamiento de un modelo** sin preocuparse por la ruta de acceso a los datos ni la cadena de conexión.

* **Compartir datos y colaborar** con otros usuarios.

## <a name="prerequisites"></a>Requisitos previos

Para crear y trabajar con conjuntos de datos, necesita:

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

* Un [área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

* El [SDK de Azure Machine Learning para Python instalado](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), que incluye el paquete azureml-datasets.

> [!Note]
> Algunas clases Dataset (versión preliminar) tienen dependencias en el paquete [azureml dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py). Para los usuarios de Linux, estas clases solo se admiten en las siguientes distribuciones:  Red Hat Enterprise Linux, Ubuntu, Fedora y CentOS.

## <a name="dataset-types"></a>Tipos de conjuntos de datos

Los conjuntos de valores se clasifican en dos tipos en función de cómo los usuarios los consumen en el entrenamiento.

* [TabularDatasets](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) representa los datos en formato tabular mediante el análisis del archivo o la lista de archivos proporcionados. Esto le proporciona la capacidad de materializar los datos en un DataFrame de Pandas o Spark. Se puede crear un objeto `TabularDataset` a partir de archivos csv, tsv o parquet, resultados de consultas SQL, etc. Para obtener una lista completa, consulte nuestra [documentación](https://aka.ms/tabulardataset-api-reference).

* [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) hace referencia a uno o varios archivos de sus almacenes de archivos o direcciones URL públicas. Este proporciona la capacidad de descargar o montar los archivos en el proceso. Los archivos pueden tener cualquier formato, lo que le ofrece una amplia gama de escenarios de aprendizaje automático, incluido el aprendizaje profundo.

Para obtener más información sobre los próximos cambios en la API, consulte [esto](https://aka.ms/tabular-dataset).

## <a name="create-datasets"></a>Creación de conjuntos de datos

Mediante la creación de un conjunto de datos, puede crear una referencia a la ubicación del origen de datos, junto con una copia de sus metadatos. Los datos se mantienen en la ubicación existente, por lo que no se genera ningún costo de almacenamiento adicional. Es posible crear TabularDatasets y FileDatasets mediante el SDK de Python o mediante la página de aterrizaje del área de trabajo (versión preliminar). 

Para que Azure Machine Learning pueda acceder a los datos, hay que crear los conjuntos de datos a partir de las rutas de acceso de los [almacenes de datos de Azure](how-to-access-data.md) o las direcciones URL web públicas.

### <a name="using-the-sdk"></a>Uso del SDK

Para crear conjuntos de datos desde un [almacén de datos de Azure](how-to-access-data.md) mediante el SDK de Python:

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

#### <a name="create-a-tabulardataset"></a>Creación de un objeto TabularDataset

Es posible crear TabularDatasets a través del SDK o mediante Azure Machine Learning Studio. Se puede especificar una marca de tiempo desde una columna de los datos o el patrón de la ruta de acceso donde se almacenan los datos para habilitar un rasgo de serie temporal, lo que permite un filtrado por tiempo sencillo y eficaz.

Use el método [`from_delimited_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-delimited-files-path--validate-true--include-path-false--infer-column-types-true--set-column-types-none--separator------header--promoteheadersbehavior-all-files-have-same-headers--3---partition-format-none-) en la clase `TabularDatasetFactory` para leer archivos en formato csv o tsv y cree una clase TabularDataset sin registrar. Si está leyendo de varios archivos, los resultados se agregarán en una representación tabular.

```Python
# create a TabularDataset from multiple paths in datastore
datastore_paths = [
                  (datastore, 'weather/2018/11.csv'),
                  (datastore, 'weather/2018/12.csv'),
                  (datastore, 'weather/2019/*.csv')
                 ]
weather_ds = Dataset.Tabular.from_delimited_files(path=datastore_paths)
```

De forma predeterminada, al crear un elemento TabularDataset, los tipos de datos de columna se deducen automáticamente. Si los tipos deducidos no son los esperados, puede especificar los tipos de columna mediante el código siguiente. Obtenga más información sobre los tipos de datos admitidos [aquí](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.datatype?view=azure-ml-py).

```Python
from azureml.data.dataset_factory import DataType

# create a TabularDataset from a delimited file behind a public web url and convert column "Survived" to boolean
web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path, set_column_types={'Survived': DataType.to_bool()})

# preview the first 3 rows of titanic_ds
titanic_ds.take(3).to_pandas_dataframe()
```

| |PassengerId|Survived|Pclass|NOMBRE|Sex|Edad|SibSp|Parch|Vale|Fare|Cabin|Embarked
-|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
0|1|False|3|Braund, Mr. Owen Harris|hombre|22,0|1|0|A/5 21171|7,2500||S
1|2|True|1|Cumings, Mrs. John Bradley (Florence Briggs Th...|mujer|38,0|1|0|PC 17599|71,2833|C85|C
2|3|True|3|Heikkinen, Miss. Laina|mujer|26,0|0|0|STON/O2. 3101282|7,9250||S

Use el método [`from_sql_query()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-sql-query-query--validate-true--set-column-types-none-) en la clase `TabularDatasetFactory` para leer desde Azure SQL Database.

```Python

from azureml.core import Dataset, Datastore

# create tabular dataset from a SQL database in datastore
sql_datastore = Datastore.get(workspace, 'mssql')
sql_ds = Dataset.Tabular.from_sql_query((sql_datastore, 'SELECT * FROM my_table'))
```

En TabularDatasets, se puede especificar una marca de tiempo desde una columna de los datos o el patrón de la ruta de acceso donde se almacenan los datos para habilitar un rasgo de serie temporal, lo que permite un filtrado por tiempo sencillo y eficaz.

Use el método [`with_timestamp_columns()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-fine-grain-timestamp--coarse-grain-timestamp-none--validate-false-) en la clase `TabularDataset` para especificar la columna de marca de tiempo y habilitar el filtrado por tiempo. Puede encontrar más ejemplos y detalles [aquí](https://aka.ms/azureml-tsd-notebook).

```Python
# create a TabularDataset with time series trait
datastore_paths = [(datastore, 'weather/*/*/*/data.parquet')]

# get a coarse timestamp column from the path pattern
dataset = Dataset.Tabular.from_parquet_files(path=datastore_path, partition_format='weather/{coarse_time:yyy/MM/dd}/data.parquet')

# set coarse timestamp to the virtual column created, and fine grain timestamp from a column in the data
dataset = dataset.with_timestamp_columns(fine_grain_timestamp='datetime', coarse_grain_timestamp='coarse_time')

# filter with time-series-trait-specific methods
data_slice = dataset.time_before(datetime(2019, 1, 1))
data_slice = dataset.time_after(datetime(2019, 1, 1))
data_slice = dataset.time_between(datetime(2019, 1, 1), datetime(2019, 2, 1))
data_slice = dataset.time_recent(timedelta(weeks=1, days=1))
```

#### <a name="create-a-filedataset"></a>Creación de un objeto FileDataset

Use el método [`from_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py#from-files-path--validate-true-) en la clase `FileDatasetFactory` para cargar archivos en cualquier formato y crear un elemento FileDataset sin registrar.

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

#### <a name="on-the-web"></a>En la Web 
En los pasos y la animación siguientes se muestra cómo crear un conjunto de datos en Azure Machine Learning Studio, https://ml.azure.com.

![Creación de un conjunto de datos con la interfaz de usuario](media/how-to-create-register-datasets/create-dataset-ui.gif)

Para crear un conjunto de datos en el estudio:
1. Inicie sesión en https://ml.azure.com.
1. Seleccione **Conjuntos de datos** en la sección **Recursos** del panel de la izquierda. 
1. Seleccione **+ Crear conjunto de datos** para elegir el origen del conjunto de datos, que puede ser archivos locales, almacenes de datos o direcciones URL web públicas.
1. Seleccione **Tabular** o **Archivo** para el tipo de conjunto de datos.
1. Seleccione **Siguiente** para revisar los formularios **Settings and preview** (Configuración y vista previa), **Esquema** y **Confirmar detalles**; se rellenan de forma inteligente en función del tipo de archivo. Use estos formularios para comprobar las selecciones y para configurar más el conjunto de datos antes de la creación.  
1. Seleccione **Crear** para completar la creación del conjunto de datos.

## <a name="register-datasets"></a>Registro de conjuntos de datos

Para completar el proceso de creación, registre los conjuntos de datos con un área de trabajo.

Use el método [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-) para registrar los conjuntos de datos con el área de trabajo de modo que puedan compartirse con otros usuarios y reutilizarse en varios experimentos.

```Python
titanic_ds = titanic_ds.register(workspace=workspace,
                                 name='titanic_ds',
                                 description='titanic training data')
```

> [!Note]
> Los conjuntos de datos creados a través de Azure Machine Learning Studio se registran automáticamente en el área de trabajo.

## <a name="create-datasets-with-azure-open-datasets"></a>Creación de conjuntos de datos con Azure Open Datasets

[Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/) son conjuntos de datos públicos mantenidos que puede usar para agregar características de escenarios específicos a soluciones de aprendizaje automático a fin de obtener modelos más precisos. Los conjuntos de datos incluyen datos de dominio público para el clima, censos, días festivos, seguridad pública y ubicación, que le ayudarán a entrenar los modelos de Machine Learning y enriquecer las soluciones predictivas. Open Datasets se encuentra en la nube en Microsoft Azure y se incluye en el SDK y en la interfaz de usuario del área de trabajo.

### <a name="using-the-sdk"></a>Uso del SDK

Para crear conjuntos de datos con Azure Open Datasets desde el SDK, asegúrese de que ha instalado el paquete con `pip install azureml-opendatasets`. Cada conjunto de datos discretos se representa mediante su propia clase en el SDK y ciertas clases están disponibles como `TabularDataset`, `FileDataset` o ambos. Consulte la [documentación de referencia](https://docs.microsoft.com/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py) para obtener una lista completa de clases.

La mayoría de las clases se heredan y devuelven una instancia de `TabularDataset`. Entre los ejemplos de estas clases se incluyen `PublicHolidays`, `BostonSafety` y `UsPopulationZip`. Para crear un elemento `TabularDataset` a partir de estos tipos de clases, use el constructor sin argumentos. Cuando se registra un conjunto de datos creado desde Open Datasets, los datos no se descargan inmediatamente, pero se tendrá acceso a ellos más adelante cuando se soliciten (durante el entrenamiento, por ejemplo) desde una ubicación de almacenamiento central. 

```python
from azureml.opendatasets import UsPopulationZip

tabular_dataset = UsPopulationZip()
tabular_dataset = tabular_dataset.register(workspace=workspace, name="pop data", description="US population data by zip code")
```

Ciertas clases se pueden recuperar como `TabularDataset` o `FileDataset`, lo que permite manipular y descargar los archivos directamente. Otras clases solo pueden obtener un conjunto de datos mediante las funciones `get_tabular_dataset()` **o** `get_file_dataset()`. En el ejemplo de código siguiente se muestran algunos ejemplos de estos tipos de clases.

```python
from azureml.opendatasets import MNIST

# MNIST class can return either TabularDataset or FileDataset
tabular_dataset = MNIST.get_tabular_dataset()
file_dataset = MNIST.get_file_dataset()

from azureml.opendatasets import Diabetes

# Diabetes class can return ONLY return TabularDataset and must be called from the static function
diabetes_tabular = Diabetes.get_tabular_dataset()
```

### <a name="using-the-ui"></a>Uso de la interfaz de usuario

También puede crear conjuntos de datos desde clases de Open Datasets mediante la interfaz de usuario. En el área de trabajo, navegue a la pestaña **Conjunto de datos** en *Recursos*. Haga clic en la lista desplegable **Crear conjunto de datos** y, a continuación, haga clic en **From Open Datasets** (Desde Open Datasets).

![Abrir un conjunto de datos con la interfaz de usuario](media/how-to-create-register-datasets/open-datasets-1.png)

A continuación, seleccione un conjunto de datos seleccionando el icono y, opcionalmente, filtrando con la barra de búsqueda. A continuación, haga clic en **Siguiente**.

![Elegir conjunto de datos](media/how-to-create-register-datasets/open-datasets-2.png)

A continuación, elija un nombre para registrar el conjunto de datos y, opcionalmente, filtre los datos mediante los filtros disponibles. En este caso, para el conjunto de datos de días festivos públicos, filtre el período de tiempo a un año y el código de país a solo EE. UU. A continuación, haga clic en **Crear**.

![Establecer parámetros del conjunto de datos y crear conjunto de datos](media/how-to-create-register-datasets/open-datasets-3.png)

Ahora se crea el conjunto de datos y está disponible en el área de trabajo, en **Conjuntos de datos**, y se puede usar de la misma manera que los demás conjuntos de datos que se hayan creado.

## <a name="version-datasets"></a>Conjuntos de datos de versión

Puede registrar un nuevo conjunto de datos con el mismo nombre mediante la creación de una nueva versión. La versión del conjunto de datos es una manera de marcar el estado de los datos, por lo que puede aplicar una versión específica del conjunto de datos para experimentación o reproducción futura. Obtenga más información sobre las [versiones del conjunto de datos](how-to-version-track-datasets.md).
```Python
# create a TabularDataset from Titanic training data
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


## <a name="access-datasets-in-your-script"></a>Obtener acceso a los conjuntos de datos del script

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

* Obtenga más información sobre [cómo entrenar con conjuntos de datos](how-to-train-with-datasets.md).
* Use el aprendizaje automático automatizado para [entrenar con TabularDatasets](https://aka.ms/automl-dataset).
* Para ver más ejemplos del uso del entrenamiento con conjuntos de datos, consulte los [cuadernos de ejemplo](https://aka.ms/dataset-tutorial).
