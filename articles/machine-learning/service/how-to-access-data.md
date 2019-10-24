---
title: Acceso a los datos en los servicios de almacenamiento de Azure
titleSuffix: Azure Machine Learning
description: Aprenda a usar los almacenes de datos para acceder a los servicios de almacenamiento de Azure durante el entrenamiento con Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.reviewer: nibaccam
ms.date: 08/2/2019
ms.custom: seodec18
ms.openlocfilehash: 3576f7cc0297ff1e9b10373ccc27b09e1a0ae8ae
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72436699"
---
# <a name="access-data-in-azure-storage-services"></a>Acceso a los datos en los servicios de almacenamiento de Azure

En este artículo, aprenderá a acceder fácilmente a los datos en los servicios de almacenamiento de Azure a través de almacenes de datos de Azure Machine Learning. Los almacenes de datos se usan para almacenar información de conexión, como el identificador de suscripción y la autorización de token. El uso de almacenes de datos permite acceder al almacenamiento sin tener que codificar de forma rígida la información de conexión en los scripts. Puede crear almacenes de datos a partir de estas [soluciones de Azure Storage](#matrix). En el caso de las soluciones de almacenamiento no compatibles, para ahorrar el costo de salida de datos durante los experimentos de aprendizaje automático, se recomienda mover los datos a nuestras soluciones de Azure Storage compatibles. [Obtenga información sobre cómo mover los datos](#move). 

En este procedimiento se muestran ejemplos de las tareas siguientes:
* [Registro de almacenes de datos](#access)
* [Obtención de almacenes de almacenamiento del área de trabajo](#get)
* [Carga y descarga de datos con almacenes de datos](#up-and-down)
* [Acceso a los datos durante el aprendizaje](#train)
* [Migrar datos a Azure](#move)

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

- Una cuenta de Azure Storage con un [contenedor de blobs de Azure](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) o un [recurso compartido de archivos de Azure](https://docs.microsoft.com/azure/storage/files/storage-files-introduction).

- [SDK de Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) o acceso a la [página de aterrizaje del área de trabajo (versión preliminar)](https://ml.azure.com/).

- Un área de trabajo de Azure Machine Learning. 
    - [Cree un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md) o use una existente mediante el SDK de Python.

        ```Python
        import azureml.core
        from azureml.core import Workspace, Datastore
        
        ws = Workspace.from_config()
        ```

<a name="access"></a>

## <a name="create-and-register-datastores"></a>Creación y registro de almacenes de datos

Cuando se registra una solución de Azure Storage como un almacén de datos, se crea automáticamente ese almacén de datos en un área de trabajo específica. Puede crear y registrar almacenes de datos en un área de trabajo mediante el SDK de Python o la página de aterrizaje del área de trabajo.

### <a name="using-the-python-sdk"></a>Uso del SDK de Python

Todos los métodos de registro están en la clase [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) y tienen la forma register_azure_*.

La información necesaria para rellenar el método register() se puede encontrar a través de [Azure Portal](https://ms.portal.azure.com). Seleccione **Cuentas de almacenamiento** en el panel izquierdo y elija la cuenta de almacenamiento que quiere registrar. La página **Información general** proporciona información como el nombre de la cuenta y el nombre del recurso compartido de archivos o el contenedor. Para obtener información de autenticación, como la clave de cuenta o el token de SAS, vaya a **Claves de cuenta** en la opción **Configuración** del panel izquierdo. 

Los ejemplos siguientes le muestran cómo registrar una instancia de Azure Blob Container o un recurso compartido de archivos de Azure como un almacén de datos.

+ Para un **almacén de datos de un contenedor de blobs de Azure**, use [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false--blob-cache-timeout-none--grant-workspace-access-false--subscription-id-none--resource-group-none-)

    En el código siguiente se crea y registra el almacén de datos, `my_datastore`, en el área de trabajo, `ws`. Este almacén de datos tiene acceso al contenedor de blobs de Azure, `my_blob_container`, en la cuenta de Azure Storage, `my_storage_account`, con la clave de cuenta proporcionada.

    ```Python
       datastore = Datastore.register_azure_blob_container(workspace=ws, 
                                                          datastore_name='my_datastore', 
                                                          container_name='my_blob_container',
                                                          account_name='my_storage_account', 
                                                          account_key='your storage account key',
                                                          create_if_not_exists=True)
    ```

+ Para un **almacén de datos de un recurso compartido de archivos de Azure**, use [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-). 

    En el código siguiente se crea y registra el almacén de datos, `my_datastore`, en el área de trabajo, `ws`. Este almacén de datos tiene acceso al recurso compartido de archivos de Azure, `my_file_share`, en la cuenta de Azure Storage, `my_storage_account`, con la clave de cuenta proporcionada.

    ```Python
       datastore = Datastore.register_azure_file_share(workspace=ws, 
                                                      datastore_name='my_datastore', 
                                                      file_share_name='my_file_share',
                                                      account_name='my_storage account', 
                                                      account_key='your storage account key',
                                                      create_if_not_exists=True)
    ```

####  <a name="storage-guidance"></a>Orientación sobre el almacenamiento

Recomendamos el contenedor de blobs de Azure. El almacenamiento Estándar y Premium están disponibles para blobs. Aunque sea más costoso, se recomienda el almacenamiento premium debido a que la velocidad de rendimiento es mayor y puede mejorar la velocidad de las ejecuciones de entrenamiento, sobre todo si usa un gran conjunto de datos. Consulte la [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service) para obtener información sobre el costo de la cuenta de almacenamiento.

### <a name="using-the-workspace-landing-page"></a>Uso de la página de aterrizaje del área de trabajo 

Cree un nuevo almacén de datos en unos pocos pasos en la página de aterrizaje del área de trabajo.

1. Inicie sesión en la [página de aterrizaje del área de trabajo](https://ml.azure.com/).
1. Seleccione **Almacenes de datos** en el panel izquierdo en **Administrar**.
1. Seleccione **+ Nuevo almacén de datos**.
1. Complete el nuevo formulario de almacén de datos. El formulario se actualiza de forma inteligente según el tipo de almacenamiento de Azure y las selecciones de tipo de autenticación.
  
La información necesaria para rellenar el formulario se puede encontrar a través de [Azure Portal](https://ms.portal.azure.com). Seleccione **Cuentas de almacenamiento** en el panel izquierdo y elija la cuenta de almacenamiento que quiere registrar. La página **Información general** proporciona información como el nombre de la cuenta y el nombre del recurso compartido de archivos o el contenedor. Para los elementos de autenticación, como la clave de cuenta o el token de SAS, vaya a **Claves de cuenta** en la opción **Configuración** del panel izquierdo.

En el ejemplo siguiente se muestra el aspecto que tendría el formulario para crear un almacén de datos de Azure Blob. 
    
 ![Nuevo almacén de datos](media/how-to-access-data/new-datastore-form.png)


<a name="get"></a>

## <a name="get-datastores-from-your-workspace"></a>Obtención de almacenes de almacenamiento del área de trabajo

Para obtener un almacén de datos específico registrado en el área de trabajo actual, utilice el método estático [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) en la clase Datastore:

```Python
#get named datastore from current workspace
datastore = Datastore.get(ws, datastore_name='your datastore name')
```
Para obtener la lista de almacenes de datos registrados con un área de trabajo determinada, puede utilizar la propiedad [`datastores`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py#datastores) en un objeto del área de trabajo:

```Python
#list all datastores registered in current workspace
datastores = ws.datastores
for name, datastore in datastores.items():
    print(name, datastore.datastore_type)
```

Al crear un área de trabajo, se registra un contenedor de blobs de Azure y un recurso compartido de archivos de Azure en el área de trabajo denominada `workspaceblobstore` y `workspacefilestore`, respectivamente. Almacenan la información de conexión del contenedor de blobs y el recurso compartido de archivos que se aprovisiona en la cuenta de almacenamiento asociada al área de trabajo. `workspaceblobstore` se establece como el almacén de datos predeterminado.

Para obtener el almacén de datos predeterminado del área de trabajo:

```Python
datastore = ws.get_default_datastore()
```

Para definir un almacén de datos predeterminado diferente para el área de trabajo actual, use el método [`set_default_datastore()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-) en el objeto del área de trabajo:

```Python
#define default datastore for current workspace
ws.set_default_datastore('your datastore name')
```

<a name="up-and-down"></a>
## <a name="upload--download-data"></a>Carga y descarga de datos
Los métodos [`upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#upload-src-dir--target-path-none--overwrite-false--show-progress-true-) y [`download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) descritos en los ejemplos siguientes son específicos para las clases [AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py) y [AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py), y funcionan de forma idéntica para ambas.

### <a name="upload"></a>Cargar

 Cargue un directorio o archivos individuales en el almacén de datos mediante el SDK de Python.

Para cargar un directorio en un almacén de datos `datastore`:

```Python
import azureml.data
from azureml.data.azure_storage_datastore import AzureFileDatastore, AzureBlobDatastore

datastore.upload(src_dir='your source directory',
                 target_path='your target path',
                 overwrite=True,
                 show_progress=True)
```

El parámetro `target_path` especifica la ubicación en el recurso compartido de archivos (o el contenedor de blobs) en que se realiza la carga. El valor predeterminado es `None`, en cuyo caso los datos se cargan en la raíz. Cuando `overwrite=True`, cualquier dato existente en `target_path` se sobrescribe.

También puede cargar una lista de archivos individuales en el almacén de datos mediante el método `upload_files()`.

### <a name="download"></a>Descargar

De forma similar, descargue los datos desde un almacén de datos en el sistema de archivos local.

```Python
datastore.download(target_path='your target path',
                   prefix='your prefix',
                   show_progress=True)
```

El parámetro `target_path` es la ubicación del directorio local en la que se descargan los datos. Para especificar una ruta de acceso a la carpeta en el recurso compartido de archivos (o el contenedor de blobs) para realizar la descarga, especifique la ruta de acceso a `prefix`. Si `prefix` es `None`, se descargará todo el contenido del recurso compartido de archivos (o contenedor de blobs).

<a name="train"></a>
## <a name="access-your-data-during-training"></a>Acceso a los datos durante el aprendizaje

> [!IMPORTANT]
> La nueva forma recomendada de acceder a los datos en el entrenamiento son los [conjuntos de datos de Azure Machine Learning (versión preliminar) ](how-to-create-register-datasets.md). Los conjuntos de datos proporcionan funciones que cargan datos tabulares en pandas u objetos DataFrame de Spark, así como la posibilidad de descargar o montar archivos de cualquier formato desde Azure Blob Storage, Azure Files, Azure Data Lake gen. 1, Azure Data Lake gen. 2, Azure SQL y Azure PostgreSQL. Obtenga más información sobre [entrenar con conjuntos de datos](how-to-train-with-datasets.md).

En la siguiente tabla se enumeran los métodos que indican al destino de proceso cómo usar el almacén de datos durante las ejecuciones. 

Función|Método|DESCRIPCIÓN|
----|-----|--------
Montaje| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-mount--)| Se usa para montar el almacén de datos en el destino de proceso.
Descargar|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-download-path-on-compute-none-)|Se usa para descargar el contenido del almacén de datos en la ubicación especificada por `path_on_compute`. <br><br> Esta descarga se produce antes de la ejecución.
Cargar|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-upload-path-on-compute-none-)| Se usa para cargar un archivo desde la ubicación especificada por `path_on_compute` en el almacén de datos. <br><br> Esta carga se produce después de la ejecución.

Para hacer referencia a una carpeta o archivo concretos del almacén de datos y que estén disponibles en el destino de proceso, use el método [`path()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#path-path-none--data-reference-name-none-) de este.

```Python
#to mount the full contents in your storage to the compute target
datastore.as_mount()

#to download the contents of the `./bar` directory in your storage to the compute target
datastore.path('./bar').as_download()
```
> [!NOTE]
> Cualquier objeto `datastore` o `datastore.path` especificado se resuelve en un nombre de variable de entorno con el formato `"$AZUREML_DATAREFERENCE_XXXX"`, cuyo valor representa la ruta de acceso de montaje y descarga en el proceso de destino. La ruta de acceso del almacén de datos en el proceso de destino puede no ser la misma que la ruta de acceso de ejecución del script de entrenamiento.

### <a name="examples"></a>Ejemplos 

Los siguientes ejemplos de código son específicos para la clase [`Estimator`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) para tener acceso a los datos durante el entrenamiento. 

`script_params` es un diccionario que contiene parámetros para entry_script. Úselo para pasar un almacén de datos y describir cómo estarán disponibles los datos en el destino de proceso. Obtenga más información en nuestro completo [tutorial](tutorial-train-models-with-aml.md).

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data_dir': datastore.path('/bar').as_mount()
}

est = Estimator(source_directory='your code directory',
                entry_script='train.py',
                script_params=script_params,
                compute_target=compute_target
                )
```

También puede pasar una lista de almacenes de datos al parámetro `inputs` del constructor Estimador para montar o copiar datos tanto en los almacenes de datos como desde ellos. Este ejemplo de código:
* Descarga todo el contenido de `datastore1` al destino de proceso antes de que se ejecute su script de entrenamiento `train.py`.
* Descarga la carpeta `'./foo'` de `datastore2` al destino de proceso antes de que se ejecute `train.py`.
* Carga el archivo `'./bar.pkl'` desde el destino de proceso a `datastore3` después de que el script se haya ejecutado.

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[datastore1.as_download(), datastore2.path('./foo').as_download(), datastore3.as_upload(path_on_compute='./bar.pkl')])
```
<a name="matrix"></a>

### <a name="compute-and-datastore-matrix"></a>Proceso y matriz de almacén de datos

Los almacenes de datos admiten actualmente el almacenamiento de la información de conexión a los servicios de almacenamiento que se enumeran en la siguiente matriz. Esta matriz muestra las funcionalidades de acceso a datos disponibles para los diferentes destinos de proceso y escenarios de almacenes de datos. Obtenga más información sobre los [destinos de proceso de Azure Machine Learning](how-to-set-up-training-targets.md#compute-targets-for-training).

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

### <a name="accessing-source-code-during-training"></a>Acceso al código fuente durante el entrenamiento

Azure Blob Storage presenta mayores velocidades de rendimiento que los recursos compartidos de archivos de Azure y se escala a un gran número de trabajos iniciados en paralelo. Por esta razón, se recomienda configurar las ejecuciones de manera que usen el almacenamiento de blobs para transferir archivos de código fuente.

En el ejemplo de código siguiente se especifica en la configuración de ejecución el almacén de datos de blobs que se usará para las transferencias de código fuente.

```python 
# workspaceblobstore is the default blob storage
run_config.source_directory_data_store = "workspaceblobstore" 
```

## <a name="access-data-during-scoring"></a>Acceso a los datos durante la puntuación

Azure Machine Learning dispone de varios métodos para usar los modelos para puntuación. Algunos de estos métodos no proporcionan acceso a los almacenes de datos. Use la tabla siguiente para saber qué métodos le permiten acceder a los almacenes de datos durante la puntuación:

| Método | Acceso a almacén de datos | DESCRIPCIÓN |
| ----- | :-----: | ----- |
| [Predicción por lotes](how-to-run-batch-predictions.md) | ✔ | Realice predicciones sobre grandes cantidades de datos asincrónicamente. |
| [Servicio web](how-to-deploy-and-where.md) | &nbsp; | Implemente modelos como un servicio web. |
| [Módulo IoT Edge](how-to-deploy-and-where.md) | &nbsp; | Implemente modelos en dispositivos IoT Edge. |

En situaciones en las que el SDK no proporciona acceso a los almacenes de datos, es posible que pueda crear código personalizado mediante el SDK de Azure correspondiente para acceder a los datos. Por ejemplo, el [SDK de Azure Storage para Python](https://github.com/Azure/azure-storage-python) es una biblioteca cliente que puede usar para acceder a los datos almacenados en blobs o archivos.

<a name="move"></a>
## <a name="move-data-to-supported-azure-storage-solutions"></a>Movimiento de datos a soluciones de Azure Storage compatibles

El servicio Azure Machine Learning admite el acceso a datos desde Azure Blob, Azure File, Azure Data Lake Gen 1, Azure Data Lake Gen 2, Azure SQL, Azure PostgreSQL. En el caso del almacenamiento no compatible, para ahorrar el costo de salida de datos durante los experimentos de aprendizaje automático, se recomienda mover los datos a nuestras soluciones de Azure Storage compatibles con Azure Data Factory. Azure Data Factory proporciona una transferencia de datos eficaz y resistente con más de 80 conectores precompilados, como los servicios de datos de Azure, los orígenes de datos locales, Amazon S3 y Redshift, y Google BigQuery, sin costo adicional. [Siga la guía paso a paso para mover los datos mediante Azure Data Factory](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-copy-data-tool).

## <a name="next-steps"></a>Pasos siguientes

* [Entrenamiento de un modelo](how-to-train-ml-models.md)

* [Implementar un modelo](how-to-deploy-and-where.md)
