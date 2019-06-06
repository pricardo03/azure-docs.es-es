---
title: Crear conjuntos de datos para tener acceso a datos con conjuntos de datos de aprendizaje automático de Azure
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo crear conjuntos de datos desde diversos orígenes y registre los conjuntos de datos con el área de trabajo
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/21/2019
ms.openlocfilehash: 021b9035b10ad5a08bca1842e8d0714b33c84271
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66492440"
---
# <a name="create-and-access-datasets-preview-in-azure-machine-learning"></a>Crear y acceder a conjuntos de datos (versión preliminar) en Azure Machine Learning

En este artículo, obtendrá información sobre cómo crear conjuntos de datos de Azure Machine Learning (versión preliminar) y cómo tener acceso a los datos de los experimentos locales y remotos.

Con los conjuntos de datos administrado, puede: 
* **Tener acceso fácilmente a los datos durante el entrenamiento del modelo** sin volver a conectarse a almacenes subyacentes

* **Garantizar la coherencia de los datos y reproducibilidad** con el mismo puntero a través de los experimentos: blocs de notas, ml automatizada, canalizaciones, interfaz visual

* **Compartir datos y colaborar** con otros usuarios

* **Explorar datos** & Administrar el ciclo de vida de las instantáneas de datos y de versiones

* **Comparar datos** en formación para producción


## <a name="prerequisites"></a>Requisitos previos

Para crear y trabajar con conjuntos de datos, necesita:

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Un [área de trabajo de servicio de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace)

* El [SDK de Azure Machine Learning de Python instalado](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), que incluye el paquete de conjuntos de datos de aprendizaje automático de Azure.

> [!Note]
> Algunas clases de conjunto de datos (versión preliminar) tienen dependencias en el [azureml dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) (GA) del paquete. Los usuarios de Linux, estas clases solo se admiten en las siguientes distribuciones:  Red Hat Enterprise Linux, Ubuntu, Fedora y CentOS.

## <a name="data-formats"></a>Formatos de datos

Puede crear un conjunto de datos de Azure Machine Learning desde los siguientes datos:
+ [delimited](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-delimited-files-path--separator------header--promoteheadersbehavior-allfileshavesameheaders--3---encoding--fileencoding-utf8--0---quoting-false--infer-column-types-true--skip-rows-0--skip-mode--skiplinesbehavior-norows--0---comment-none--include-path-false--archive-options-none-)
+ [binary](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-binary-files-path-)
+ [json](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-json-files-path--encoding--fileencoding-utf8--0---flatten-nested-arrays-false--include-path-false-)
+ [Excel](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-excel-files-path--sheet-name-none--use-column-headers-false--skip-rows-0--include-path-false--infer-column-types-true-)
+ [Parquet](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-parquet-files-path--include-path-false-)
+ [Azure SQL Database](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-sql-query-data-source--query-)
+ [Uso de Azure Data Lake. 1](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-sql-query-data-source--query-)

## <a name="create-datasets"></a>Creación de conjuntos de datos 

Puede interactuar con los conjuntos de datos con el paquete de conjuntos de datos de aprendizaje automático de Azure en el [SDK de Python de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) y específicamente [el `Dataset` clase](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py).

### <a name="create-from-local-files"></a>Crear a partir de archivos locales

Cargar archivos desde el equipo local mediante la especificación de la ruta de acceso de archivo o carpeta con el [ `auto_read_files()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#auto-read-files-path--include-path-false-) método desde el `Dataset` clase.  Este método lleva a cabo los siguientes pasos sin necesidad de especificar el tipo de archivo o analizar los argumentos:

* Inferir y estableciendo el delimitador.
* Omitiendo los registros vacíos en la parte superior del archivo.
* Inferir y la configuración de la fila de encabezado.
* Inferir y conversión de tipos de datos de columna.

```Python
from azureml.core.dataset import Dataset

dataset = Dataset.auto_read_files('./data/crime.csv')
```

Como alternativa, puede usar las funciones específicas del archivo para controlar explícitamente el análisis del archivo. 


### <a name="create-from-azure-datastores"></a>Crear a partir de almacenes de datos de Azure

Para crear conjuntos de datos desde un almacén de datos de Azure:

* Compruebe que dispone de `contributor` o `owner` acceso para el almacén de datos registrados de Azure.

* Importar el [ `Workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) y [ `Datastore` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#definition) y `Dataset` paquetes desde el SDK.

```Python
from azureml.core.workspace import Workspace
from azureml.core.datastore import Datastore
from azureml.core.dataset import Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()
```

 El `get()` método recupera un almacén de datos existente en el área de trabajo.

```
dstore = Datastore.get(workspace, datastore_name)
```

Use el `from_delimited_files()` método para leer archivos delimitados y crear un conjunto de datos no registrado.

```Python
# create an in-memory Dataset on your local machine
datapath = dstore.path('data/src/crime.csv')
dataset = Dataset.from_delimited_files(datapath)

# returns the first 5 rows of the Dataset as a pandas Dataframe.
dataset.head(5)
```

## <a name="register-datasets"></a>Registrar los conjuntos de datos

Para completar el proceso de creación, registre los conjuntos de datos con el área de trabajo:

Use la [ `register()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-) método para registrar los conjuntos de datos al área de trabajo para que puedan compartir con otros usuarios y reutilizar en varios experimentos.

```Python
dataset = dataset.register(workspace = workspace,
                           name = 'dataset_crime',
                           description = 'Training data',
                           exist_ok = False
                           )
```

>[!NOTE]
> Si `exist_ok = False` (valor predeterminado), e intenta registrar un conjunto de datos con el mismo nombre que otro, se produce un error. Establecido en `True` para sobrescribir la existente.

## <a name="access-data-in-datasets"></a>Acceder a los datos en conjuntos de datos

Los conjuntos de datos registrados sean accesibles y consumibles localmente, remotamente y en clústeres de cálculo, como el proceso de Azure Machine Learning. Para reutilizar el conjunto de datos registrado en experimentos y entornos de proceso, utilice el código siguiente para obtener el área de trabajo y el conjunto de datos registrado por el nombre.

```Python
workspace = Workspace.from_config()

# See list of datasets registered in workspace.
Dataset.list(workspace)

# Get dataset by name
dataset = workspace.datasets['dataset_crime']
```

## <a name="next-steps"></a>Pasos siguientes

* [Explorar y preparar los conjuntos de datos](how-to-explore-prepare-data.md).
* [Administrar el ciclo de vida de las definiciones de conjunto de datos](how-to-manage-dataset-definitions.md).
* Para obtener un ejemplo del uso de conjuntos de datos, vea el [cuadernos de ejemplo](https://aka.ms/dataset-tutorial).
