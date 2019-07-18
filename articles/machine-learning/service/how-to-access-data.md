---
title: Acceso a datos en almacenes de datos o blobs durante el entrenamiento
titleSuffix: Azure Machine Learning service
description: Aprenda a usar los almacenes de datos para acceder al almacenamiento de datos de blobs con Azure Machine Learning Service
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 05/24/2019
ms.custom: seodec18
ms.openlocfilehash: 638d7bfb0e396874415c1055c4b707a65caffa4e
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67269303"
---
# <a name="access-data-from-your-datastores"></a>Datos de acceso desde almacenes de datos

 En Azure Machine Learning Service, los almacenes de datos son mecanismos de proceso independientes de la ubicación que permiten acceder a almacenamiento sin necesidad de realizar cambios en el código fuente. Tanto si escribe código de entrenamiento para tomar una ruta de acceso como un parámetro, o proporciona un almacén de datos directamente a un estimador, los flujos de trabajo de Azure Machine Learning aseguran que las ubicaciones del almacén de datos son accesibles y a disposición del contexto de proceso.

En este procedimiento se muestran ejemplos de las tareas siguientes:
* [Elección de un almacén de datos](#access)
* [Obtención de datos](#get)
* [Carga y descarga de datos en almacenes de datos](#up-and-down)
* [Acceso al almacén de datos durante el aprendizaje](#train)

## <a name="prerequisites"></a>Requisitos previos

Para usar almacenes de datos, antes se necesita un [área de trabajo](concept-workspace.md).

Para empezar puede [crear una nueva área de trabajo](setup-create-workspace.md#sdk), o bien puede recuperar una existente:

```Python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()
```

<a name="access"></a>

## <a name="choose-a-datastore"></a>Elección de un almacén de datos

Puede usar el almacén de datos predeterminado o el suyo propio.

### <a name="use-the-default-datastore-in-your-workspace"></a>Uso del almacén de datos predeterminado en el área de trabajo

 Cada área de trabajo tiene un almacén de datos predeterminado registrado que puede empezar a usar de forma inmediata.

Para obtener el almacén de datos predeterminado del área de trabajo:

```Python
ds = ws.get_default_datastore()
```

### <a name="register-your-own-datastore-with-the-workspace"></a>Registro de su propio almacén de datos en el área de trabajo

Si ya tiene una instancia de Azure Storage, puede registrarla como almacén de datos en el área de trabajo. 

<a name="store"></a>

####  <a name="storage-guidance"></a>Orientación sobre el almacenamiento

Se recomiendan Blob Storage y almacenes de datos de blobs. El almacenamiento Estándar y Premium están disponibles para blobs. Aunque sea más costoso, se recomienda el almacenamiento premium debido a que la velocidad de rendimiento es mayor y puede mejorar la velocidad de las ejecuciones de entrenamiento, sobre todo si usa un gran conjunto de datos. Consulte la [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service) para obtener información sobre el costo de la cuenta de almacenamiento.

>[!NOTE]
> Azure Machine Learning Service admite otros tipos de almacenes de datos, que podrían resultar útiles para determinados escenarios. Por ejemplo, si necesita entrenar con datos almacenados en una base de datos, podría usar AzureSQLDatabaseDatastore o AzurePostgreSqlDatastore. Consulte [esta tabla](#matrix) para ver los tipos de almacén de datos disponibles.

#### <a name="register-your-datastore"></a>Registro del almacén de datos
Todos los métodos de registro están en la clase [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) y tienen la forma register_azure_*.

Los ejemplos siguientes le muestran cómo registrar una instancia de Azure Blob Container o un recurso compartido de archivos de Azure como un almacén de datos.

+ Para un **almacén de datos de un contenedor de blobs de Azure**, use [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py)

  ```Python
  ds = Datastore.register_azure_blob_container(workspace=ws, 
                                               datastore_name='your datastore name', 
                                               container_name='your azure blob container name',
                                               account_name='your storage account name', 
                                               account_key='your storage account key',
                                               create_if_not_exists=True)
  ```

+ Para un **almacén de datos de un recurso compartido de archivos de Azure**, use [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-). Por ejemplo: 
  ```Python
  ds = Datastore.register_azure_file_share(workspace=ws, 
                                           datastore_name='your datastore name', 
                                           file_share_name='your file share name',
                                           account_name='your storage account name', 
                                           account_key='your storage account key',
                                           create_if_not_exists=True)
  ```

<a name="get"></a>

## <a name="find--define-datastores"></a>Búsqueda y definición de almacenes de datos

Para registrar un almacén de datos especificado en el área de trabajo actual, use [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-):

```Python
#get named datastore from current workspace
ds = Datastore.get(ws, datastore_name='your datastore name')
```

Para obtener una lista de todos los almacenes de datos en un área de trabajo determinada, use este código:

```Python
#list all datastores registered in current workspace
datastores = ws.datastores
for name, ds in datastores.items():
    print(name, ds.datastore_type)
```

Para definir un almacén de datos predeterminado diferente para el área de trabajo actual, use [`set_default_datastore()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-):

```Python
#define default datastore for current workspace
ws.set_default_datastore('your datastore name')
```

<a name="up-and-down"></a>
## <a name="upload--download-data"></a>Carga y descarga de datos
Los métodos [`upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) y [`download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) descritos en los ejemplos siguientes son específicos para las clases [AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py) y [AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py), y funcionan de forma idéntica para ambas.

### <a name="upload"></a>Cargar

 Cargue un directorio o archivos individuales en el almacén de datos mediante el SDK de Python.

Para cargar un directorio en un almacén de datos `ds`:

```Python
import azureml.data
from azureml.data.azure_storage_datastore import AzureFileDatastore, AzureBlobDatastore

ds.upload(src_dir='your source directory',
          target_path='your target path',
          overwrite=True,
          show_progress=True)
```

`target_path` especifica la ubicación en el recurso compartido de archivos (o el contenedor de blobs) en que se realiza la carga. El valor predeterminado es `None`, en cuyo caso los datos se cargan en la raíz. `overwrite=True` sobrescribirá todos los datos existente en `target_path`.

También puede cargar una lista de archivos individuales en el almacén de datos mediante el método `upload_files()` de este.

### <a name="download"></a>Descargar
De forma similar, descargue los datos desde un almacén de datos en el sistema de archivos local.

```Python
ds.download(target_path='your target path',
            prefix='your prefix',
            show_progress=True)
```

`target_path` es la ubicación del directorio local en la que se descargan los datos. Para especificar una ruta de acceso a la carpeta en el recurso compartido de archivos (o el contenedor de blobs) para realizar la descarga, especifique la ruta de acceso a `prefix`. Si `prefix` es `None`, se descargará todo el contenido del recurso compartido de archivos (o contenedor de blobs).

<a name="train"></a>
## <a name="access-datastores-during-training"></a>Acceso al almacén de datos durante el entrenamiento

Una vez que el almacén de datos esté disponible en el destino de proceso del entrenamiento, puede acceder durante las ejecuciones de entrenamiento (por ejemplo, para los datos de aprendizaje o validación) pasando simplemente la ruta de acceso a él como un parámetro en el script de entrenamiento.

En la siguiente tabla se enumeran los métodos [`DataReference`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py) que indican al destino de proceso cómo usar el almacén de datos durante las ejecuciones.

Función|Método|DESCRIPCIÓN|
----|-----|--------
Montaje| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--)| Se usa para montar el almacén de datos en el destino de proceso.
Descargar|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-)|Se usa para descargar el contenido del almacén de datos en la ubicación especificada por `path_on_compute`. <br> Para el contexto de ejecución de entrenamiento, esta descarga se produce antes de la ejecución.
Cargar|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)| Se usa para cargar un archivo desde la ubicación especificada por `path_on_compute` en el almacén de datos. <br> Para el contexto de ejecución de entrenamiento, esta carga se produce después de su ejecución.

 ```Python
import azureml.data
from azureml.data.data_reference import DataReference

ds.as_mount()
ds.as_download(path_on_compute='your path on compute')
ds.as_upload(path_on_compute='yourfilename')
```  

Para hacer referencia a una carpeta o archivo concretos del almacén de datos y que estén disponibles en el destino de proceso, use la función [`path()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#path-path-none--data-reference-name-none-) de este.

```Python
#download the contents of the `./bar` directory in ds to the compute target
ds.path('./bar').as_download()
```

> [!NOTE]
> Cualquier objeto `ds` o `ds.path` se resuelve en un nombre de variable de entorno con el formato `"$AZUREML_DATAREFERENCE_XXXX"` cuyo valor representa la ruta de acceso de montaje y descarga en el proceso de destino. La ruta de acceso del almacén de datos en el proceso de destino puede no ser la misma que la ruta de acceso de ejecución del script de entrenamiento.

<a name="matrix"></a>
### <a name="training-compute-and-datastore-matrix"></a>Proceso de entrenamiento y matriz de almacén de datos

La matriz siguiente muestra las funcionalidades de acceso a datos disponibles para los diferentes escenarios de almacén de datos y destinos de proceso de entrenamiento. Obtenga más información sobre los [destinos de proceso de entrenamiento de Azure Machine Learning](how-to-set-up-training-targets.md#compute-targets-for-training).

|Proceso|[AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py)                                       |[AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py)                                      |[AzureDataLakeDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakedatastore?view=azure-ml-py) |[AzureDataLakeGen2Datastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakegen2datastore?view=azure-ml-py) [AzurePostgreSqlDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_postgre_sql_datastore.azurepostgresqldatastore?view=azure-ml-py) [AzureSqlDatabaseDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_sql_database_datastore.azuresqldatabasedatastore?view=azure-ml-py) |
|--------------------------------|----------------------------------------------------------|----------------------------------------------------------|------------------------|----------------------------------------------------------------------------------------|
| Local|[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|N/D         |N/D                                                                         |
| Proceso de Azure Machine Learning |[as_mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [ML&nbsp;pipelines](concept-ml-pipelines.md)|[as_mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [ML&nbsp;pipelines](concept-ml-pipelines.md)|N/D         |N/D                                                                         |
| Máquinas virtuales               |[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                           | [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |N/D         |N/D                                                                         |
| HDInsight                      |[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            | [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |N/D         |N/D                                                                         |
| Transferencia de datos                  |[ML&nbsp;pipelines](concept-ml-pipelines.md)                                               |N/D                                           |[ML&nbsp;pipelines](concept-ml-pipelines.md)            |[ML&nbsp;pipelines](concept-ml-pipelines.md)                                                                            |
| Databricks                     |[ML&nbsp;pipelines](concept-ml-pipelines.md)                                              |N/D                                           |[ML&nbsp;pipelines](concept-ml-pipelines.md)             |N/D                                                                         |
| Azure Batch                    |[ML&nbsp;pipelines](concept-ml-pipelines.md)                                               |N/D                                           |N/D         |N/D                                                                         |
| Azure DataLake Analytics       |N/D                                           |N/D                                           |[ML&nbsp;pipelines](concept-ml-pipelines.md)             |N/D                                                                         |

> [!NOTE]
> Es posible que haya escenarios altamente iterativos, los procesos de datos de gran tamaño se ejecutan más rápidamente mediante `as_download()` en lugar de `as_mount()`; esto se puede validar de manera experimental.

### <a name="examples"></a>Ejemplos 

Los siguientes ejemplos de código son específicos para la clase [`Estimator`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) para tener acceso a su almacén de datos durante el entrenamiento.

Este código crea un estimador mediante el script de entrenamiento, `train.py`, desde el directorio de origen indicado con los parámetros definidos en `script_params`, todos en el destino de proceso de entrenamiento especificado.

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data_dir': ds.as_mount()
}

est = Estimator(source_directory='your code directory',
                entry_script='train.py',
                script_params=script_params,
                compute_target=compute_target
                )
```

También puede pasar una lista de almacenes de datos al parámetro `inputs` del constructor Estimador para montar o copiar tanto en los almacenes de datos como desde ellos. Este ejemplo de código:
* Descarga todo el contenido del almacén de datos `ds1` al destino de proceso antes de que se ejecute su script de entrenamiento `train.py`
* Descarga la carpeta `'./foo'` del almacén de datos `ds2` al destino de proceso antes de `train.py` se ejecute
* Carga el archivo `'./bar.pkl'` desde el destino de proceso al almacén de datos `ds3` después de que el script se haya ejecutado

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[ds1.as_download(), ds2.path('./foo').as_download(), ds3.as_upload(path_on_compute='./bar.pkl')])
```

## <a name="next-steps"></a>Pasos siguientes

* [Entrenamiento de un modelo](how-to-train-ml-models.md)

* [Implementar un modelo](how-to-deploy-and-where.md)
