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
ms.date: 05/21/2019
ms.openlocfilehash: b4c22caae86e20b8379db2b7feffb1ca82001239
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66753158"
---
# <a name="create-and-access-datasets-preview-in-azure-machine-learning"></a>Creación y acceso a conjuntos de datos (versión preliminar) en Azure Machine Learning

En este artículo aprenderá a crear conjuntos de datos de Azure Machine Learning (versión preliminar) y cómo acceder a los datos de los experimentos locales y remotos.

Con los conjuntos de datos administrados, puede: 
* **Tener acceso fácilmente a los datos durante el entrenamiento de modelos** sin volver a conectarse a los almacenes subyacentes

* **Garantizar la coherencia y reproducibilidad de los datos** con el mismo puntero en todos los experimentos: blocs de notas, ML automatizado, canalizaciones, interfaz visual

* **Compartir datos y colaborar** con otros usuarios

* **Explorar datos** y administrar el ciclo de vida de las instantáneas de datos y versiones

* **Comparar datos** en el entrenamiento para producción


## <a name="prerequisites"></a>Requisitos previos

Para crear y trabajar con conjuntos de datos, necesita:

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Un [área de trabajo de Azure Machine Learning Service](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace)

* El [SDK de Azure Machine Learning para Python instalado](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), que incluye el paquete azureml-datasets.

> [!Note]
> Algunas clases Dataset (versión preliminar) tienen dependencias en el paquete [azureml dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) (disponibilidad general). Para los usuarios de Linux, estas clases solo se admiten en las siguientes distribuciones:  Red Hat Enterprise Linux, Ubuntu, Fedora y CentOS.

## <a name="data-formats"></a>Formatos de datos

Puede crear un conjunto de datos de Azure Machine Learning a partir de los datos siguientes:
+ [delimited](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset#from-delimited-files-path--separator------header--promoteheadersbehavior-all-files-have-same-headers--3---encoding--fileencoding-utf8--0---quoting-false--infer-column-types-true--skip-rows-0--skip-mode--skiplinesbehavior-no-rows--0---comment-none--include-path-false--archive-options-none-)
+ [binary](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-binary-files-path-)
+ [json](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-json-files-path--encoding--fileencoding-utf8--0---flatten-nested-arrays-false--include-path-false-)
+ [Excel](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-excel-files-path--sheet-name-none--use-column-headers-false--skip-rows-0--include-path-false--infer-column-types-true-)
+ [Parquet](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-parquet-files-path--include-path-false-)
+ [Azure SQL Database](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-sql-query-data-source--query-)
+ [Azure Data Lake gen. 1](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-sql-query-data-source--query-)

## <a name="create-datasets"></a>Creación de conjuntos de datos 

Puede interactuar con los conjuntos de datos con el paquete azureml-datasets en el [SDK de Python para Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) y específicamente [la clase `Dataset` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py).

### <a name="create-from-local-files"></a>Creación a partir de archivos locales

Cargue los archivos desde la máquina local especificando la ruta del archivo o de la carpeta con el método [`auto_read_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#auto-read-files-path--include-path-false-) de la clase `Dataset`.  Este método realiza los siguientes pasos sin necesidad de especificar el tipo de archivo o los argumentos de análisis:

* Inferir y establecer el delimitador
* Omitir los registros vacíos en la parte superior del archivo
* Deducir y establecer la fila de encabezado
* Inferir y conversión de tipos de datos de columna

```Python
from azureml.core.dataset import Dataset

dataset = Dataset.auto_read_files('./data/crime.csv')
```

Además, utilice las funciones específicas del archivo para controlar explícitamente el análisis del archivo. 


### <a name="create-from-azure-datastores"></a>Creación a partir de almacenes de datos de Azure

Para crear conjuntos de datos desde un almacén de datos de Azure:

* Compruebe que dispone de acceso `contributor` o `owner` para el almacén de datos de Azure registrado.

* Importe los paquetes [`Workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py), [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#definition) y `Dataset` desde el SDK.

```Python
from azureml.core.workspace import Workspace
from azureml.core.datastore import Datastore
from azureml.core.dataset import Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()
```

 El método `get()` recupera un almacén de datos existente en el área de trabajo.

```
dstore = Datastore.get(workspace, datastore_name)
```

Use el método `from_delimited_files()` para leer archivos delimitados y crear un conjunto de datos no registrado.

```Python
# create an in-memory Dataset on your local machine
datapath = dstore.path('data/src/crime.csv')
dataset = Dataset.from_delimited_files(datapath)

# returns the first 5 rows of the Dataset as a pandas Dataframe.
dataset.head(5)
```

## <a name="register-datasets"></a>Registro de conjuntos de datos

Para completar el proceso de creación, registre los conjuntos de datos con el área de trabajo:

Use el método [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-) para registrar los conjuntos de datos en el área de trabajo de modo que puedan compartirse con otros usuarios y reutilizarse en varios experimentos.

```Python
dataset = dataset.register(workspace = workspace,
                           name = 'dataset_crime',
                           description = 'Training data',
                           exist_ok = False
                           )
```

>[!NOTE]
> Si `exist_ok = False` (valor predeterminado) e intenta registrar un conjunto de datos con el mismo nombre que otro, se produce un error. Establézcalo en `True` para sobrescribir el existente.

## <a name="access-data-in-datasets"></a>Acceso a los datos de los conjuntos de datos

Los conjuntos de datos registrados son accesibles y consumibles localmente, de forma remota y en clusters de proceso como el proceso de Azure Machine Learning. Para reutilizar el conjunto de datos registrado en experimentos y entornos de proceso, utilice el siguiente código para obtener el área de trabajo y el conjunto de datos registrado por nombre.

```Python
workspace = Workspace.from_config()

# See list of datasets registered in workspace.
Dataset.list(workspace)

# Get dataset by name
dataset = workspace.datasets['dataset_crime']
```

## <a name="next-steps"></a>Pasos siguientes

* [Exploración y preparación de los conjuntos de datos](how-to-explore-prepare-data.md).
* [Administración del ciclo de vida de las definiciones del conjunto de datos](how-to-manage-dataset-definitions.md).
* Para ver un ejemplo del uso de los conjuntos de datos, consulte los [cuadernos de ejemplo](https://aka.ms/dataset-tutorial).
