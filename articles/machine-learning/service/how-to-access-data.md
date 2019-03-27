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
ms.date: 02/25/2019
ms.custom: seodec18
ms.openlocfilehash: 25da234e4210c98ce17bdeb502493c5c649dab28
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58481644"
---
# <a name="access-data-from-your-datastores"></a>Acceder a los datos de los almacenes de datos

 En el servicio de Azure Machine Learning, almacenes de datos son de proceso independiente de la ubicación de mecanismos para acceder al almacenamiento sin necesidad de realizar cambios en el código fuente. Si escribir código de entrenamiento para tomar una ruta de acceso como un parámetro, o proporcionar un almacén de datos directamente a un Estimador, asegúrese de flujos de trabajo de Azure Machine Learning las ubicaciones de almacén de datos son accesibles y disposición para el contexto de proceso.

Este tema de procedimientos muestra ejemplos de las tareas siguientes:
* [Elegir un almacén de datos](#access)
* [Obtención de datos](#get)
* [Cargar y descargar datos en almacenes de datos](#up-and-down)
* [Almacén de datos de acceso durante el entrenamiento](#train)

## <a name="prerequisites"></a>Requisitos previos

Para usar almacenes de datos, antes se necesita un [área de trabajo](concept-azure-machine-learning-architecture.md#workspace).

Para empezar puede [crear una nueva área de trabajo](setup-create-workspace.md#sdk), o bien puede recuperar una existente:

```Python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()
```

<a name="access"></a>

## <a name="choose-a-datastore"></a>Elegir un almacén de datos

Puede usar el almacén de datos predeterminado o el suyo propio.

### <a name="use-the-default-datastore-in-your-workspace"></a>Usar el almacén de datos de forma predeterminada en el área de trabajo

 Cada área de trabajo tiene un almacén de datos predeterminado registrado, puede usar de inmediato.

Para obtener el almacén de datos predeterminado del área de trabajo:

```Python
ds = ws.get_default_datastore()
```

### <a name="register-your-own-datastore-with-the-workspace"></a>Registrar su propio almacén de datos con el área de trabajo

Si ya tiene una instancia de Azure Storage, puede registrarla como almacén de datos en el área de trabajo.   Todos los métodos de registro están en el [ `Datastore` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) clase y tiene el formato register_azure_ *. 

Los ejemplos siguientes muestran registrar un contenedor de blobs de Azure o un recurso compartido de archivos de Azure como un almacén de datos.

+ Para un **almacén de datos del contenedor de blobs de Azure**, usar [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py)

  ```Python
  ds = Datastore.register_azure_blob_container(workspace=ws, 
                                               datastore_name='your datastore name', 
                                               container_name='your azure blob container name',
                                               account_name='your storage account name', 
                                               account_key='your storage account key',
                                               create_if_not_exists=True)
  ```

+ Para un **almacén de datos del recurso compartido de archivos de Azure**, utilice [ `register_azure_file_share()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-). Por ejemplo:  
  ```Python
  ds = Datastore.register_azure_file_share(workspace=ws, 
                                           datastore_name='your datastore name', 
                                           container_name='your file share name',
                                           account_name='your storage account name', 
                                           account_key='your storage account key',
                                           create_if_not_exists=True)
  ```

<a name="get"></a>

## <a name="find--define-datastores"></a>Buscar & definir almacenes de datos

Para obtener un almacén de datos especificado registrado en el área de trabajo actual, use [ `get()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) :

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

Para definir un almacén de datos predeterminado diferente para el área de trabajo actual, use [ `set_default_datastore()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-):

```Python
#define default datastore for current workspace
ws.set_default_datastore('your datastore name')
```

<a name="up-and-down"></a>
## <a name="upload--download-data"></a>Cargar y descargar datos
El [ `upload()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) y [ `download()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) métodos descritos en los ejemplos siguientes son específicos y operar de forma idéntica para la [AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py) y [AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py) clases.

### <a name="upload"></a>Cargar

 Cargue un directorio o archivos individuales en el almacén de datos mediante el SDK de Python.

Para cargar un directorio en un almacén de datos `ds`:

```Python
import azureml.data
from azureml.data import AzureFileDatastore, AzureBlobDatastore

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
## <a name="access-datastores-during-training"></a>Acceso a almacenes de datos durante el entrenamiento

Una vez que el almacén de datos esté disponible en el destino de proceso, puede tener acceso durante las ejecuciones de entrenamiento (por ejemplo, los datos de entrenamiento o validación) pasando simplemente la ruta de acceso a él como un parámetro en el script de entrenamiento.

La siguiente tabla se enumeran los [ `DataReference` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py) métodos que indiquen el destino de proceso de cómo usar el almacén de datos durante las ejecuciones.

forma|Método|DESCRIPCIÓN|
----|-----|--------
Montaje| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--)| Usar para montar el almacén de datos en el destino de proceso.
Descargar|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-)|Usar para descargar el contenido de su almacén de datos en la ubicación especificada por `path_on_compute`. <br> Contexto de ejecución de aprendizaje, esta descarga se produce antes de la ejecución.
Cargar|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)| Usar para cargar un archivo desde la ubicación especificada por `path_on_compute` en su almacén de datos. <br> Contexto de ejecución de aprendizaje, esta carga se produce después de su ejecución.

 ```Python
import azureml.data
from azureml.data import DataReference

ds.as_mount()
ds.as_download(path_on_compute='your path on compute')
ds.as_upload(path_on_compute='yourfilename')
```  

Para hacer referencia a una carpeta o archivo específico en el almacén de datos y que esté disponible en el destino de proceso, use el almacén de datos [ `path()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#path-path-none--data-reference-name-none-) función.

```Python
#download the contents of the `./bar` directory in ds to the compute target
ds.path('./bar').as_download()
```

> [!NOTE]
> Cualquier `ds` o `ds.path` objeto se resuelve en un nombre de variable de entorno del formato `"$AZUREML_DATAREFERENCE_XXXX"` cuyo valor representa la ruta de acceso de montaje y descarga en el proceso de destino. La ruta de acceso del almacén de datos en el proceso de destino no puede ser el mismo que la ruta de acceso de ejecución para el script de entrenamiento.

### <a name="compute-context-and-datastore-type-matrix"></a>Matriz de tipo de contexto y el almacén de datos de proceso

La matriz siguiente muestra las funcionalidades de acceso de datos disponibles para los escenarios de contexto y el almacén de datos de proceso diferentes. El término "Canalización" en esta matriz se refiere a la capacidad para utilizar almacenes de datos como una entrada o salida en [canalizaciones de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/concept-ml-pipelines).

||Proceso local|Proceso de Azure Machine Learning|Transferencias de datos|Databricks|HDInsight|Azure Batch|Azure DataLake Analytics|Virtual Machines|
-|--|-----------|----------|---------|-----|--------------|---------|---------|
|AzureBlobDatastore|[`as_download()`] [`as_upload()`]|[`as_mount()`]<br> [`as_download()`] [`as_upload()`] <br> Canalización|Canalización|Canalización|[`as_download()`] <br> [`as_upload()`]|Canalización||[`as_download()`] <br> [`as_upload()`]|
|AzureFileDatastore|[`as_download()`] [`as_upload()`]|[`as_mount()`]<br> [`as_download()`] [`as_upload()`] Canalización |||[`as_download()`] [`as_upload()`]|||[`as_download()`] [`as_upload()`]|
|AzureDataLakeDatastore|||Canalización|Canalización|||Canalización||
|AzureDataLakeGen2Datastore|||Canalización||||||
|AzureDataPostgresSqlDatastore|||Canalización||||||
|AzureSqlDatabaseDataDatastore|||Canalización||||||


> [!NOTE]
> Puede haber escenarios en que altamente iterativo, los procesos de datos de gran tamaño ejecutan más rápidamente mediante la [`as_download()`] en lugar de [`as_mount()`]; Esto se puede validar de manera experimental.

### <a name="examples"></a>Ejemplos 

Ejemplos de código siguientes son específicos para la [ `Estimator` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) clase para tener acceso a su almacén de datos durante el entrenamiento.

Este código crea un Estimador mediante el script de entrenamiento, `train.py`, desde el directorio de origen indicado con los parámetros definidos en `script_params`, todo en el destino de proceso especificado.

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

También puede pasar en una lista de almacenes de datos para el constructor de Estimador `inputs` parámetro montar o copiar a y desde los almacenes de datos. Este ejemplo de código:
* Descargas de todo el contenido en el almacén de datos `ds1` al destino de proceso antes de su script de entrenamiento `train.py` se ejecuta
* La carpeta descargas `'./foo'` en el almacén de datos `ds2` al destino de proceso antes de `train.py` se ejecuta
* Carga el archivo `'./bar.pkl'` desde el destino de proceso hasta el almacén de datos `ds3` después de ejecuta la secuencia de comandos

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[ds1.as_download(), ds2.path('./foo').as_download(), ds3.as_upload(path_on_compute='./bar.pkl')])
```

## <a name="next-steps"></a>Pasos siguientes

* [Entrenamiento de un modelo](how-to-train-ml-models.md)

* [Implementar un modelo](how-to-deploy-and-where.md)
