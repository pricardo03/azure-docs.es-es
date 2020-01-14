---
title: Acceso a los datos en los servicios de Azure Storage
titleSuffix: Azure Machine Learning
description: Aprenda a usar los almacenes de datos para conectarse de forma segura a los servicios de Azure Storage durante el entrenamiento con Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: ylxiong
author: YLXiong1125
ms.reviewer: nibaccam
ms.date: 12/10/2019
ms.custom: seodec18
ms.openlocfilehash: ac6ef6341013ca13d5a9f27be8897365c1c2155d
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2019
ms.locfileid: "75535452"
---
# <a name="access-data-in-azure-storage-services"></a>Acceso a los datos en los servicios de almacenamiento de Azure
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo, aprenderá a obtener acceso fácilmente a los datos en los servicios de Azure Storage a través de almacenes de datos de Azure Machine Learning. Los almacenes de datos se usan para almacenar información de conexión, como el identificador de suscripción y la autorización de token. Al usar almacenes de datos, puede obtener acceso al almacenamiento sin tener que codificar de forma rígida la información de conexión en los scripts. 

Puede crear almacenes de datos a partir de estas [soluciones de Azure Storage](#matrix). En el caso de las soluciones de almacenamiento no compatibles y para ahorrar el costo de salida de datos durante los experimentos de aprendizaje automático, se recomienda [mover los datos](#move) a nuestras soluciones de Azure Storage compatibles. 

## <a name="prerequisites"></a>Prerequisites
Necesitará:
- Suscripción a Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

- Una cuenta de Azure Storage con un [contenedor de blobs de Azure](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) o un [recurso compartido de archivos de Azure](https://docs.microsoft.com/azure/storage/files/storage-files-introduction).

- [SDK de Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) o acceso a [Azure Machine Learning Studio](https://ml.azure.com/).

- Un área de trabajo de Azure Machine Learning.
  
  [Cree un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md) o use una existente mediante el SDK de Python:

   ```Python
   import azureml.core
   from azureml.core import Workspace, Datastore
        
   ws = Workspace.from_config()
   ```

<a name="access"></a>

## <a name="create-and-register-datastores"></a>Creación y registro de almacenes de datos

Cuando se registra una solución de Azure Storage como un almacén de datos, se crea automáticamente ese almacén de datos en un área de trabajo específica. Puede crear y registrar almacenes de datos en un área de trabajo mediante el SDK de Python o Azure Machine Learning Studio.

### <a name="python-sdk"></a>SDK de Python

Todos los métodos de registro están en la clase [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) y tienen la forma `register_azure_*`.

Puede encontrar la información necesaria para rellenar el método `register()` mediante [Azure Portal](https://portal.azure.com):

1. Seleccione **Cuentas de almacenamiento** en el panel izquierdo y elija la cuenta de almacenamiento que quiere registrar. 
2. Para obtener información como el nombre de la cuenta, el contenedor y el nombre del recurso compartido de archivos, vaya a la página **Información general**. Para obtener información de autenticación, como la clave de cuenta o el token de SAS, vaya a **Claves de acceso** en el panel de **Configuración**. 

> [!IMPORTANT]
> Si la cuenta de almacenamiento se encuentra en una red virtual, solo se admite la creación del almacén de datos de blobs de Azure. Para conceder a la cuenta de almacenamiento acceso a su área de trabajo, establezca el parámetro `grant_workspace_access` en `True`.

Los ejemplos siguientes muestran cómo registrar una instancia de Azure Blob Container, un recurso compartido de archivos de Azure y datos de Azure SQL como un almacén de datos.

#### <a name="blob-container"></a>Contenedor de blobs

Para registrar un almacén de datos de un contenedor de blobs de Azure, use [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false--blob-cache-timeout-none--grant-workspace-access-false--subscription-id-none--resource-group-none-).

En el código siguiente se crea y registra el almacén de datos `blob_datastore_name` en el área de trabajo `ws`. Este almacén de datos tiene acceso al contenedor de blobs `my-container-name` en la cuenta de almacenamiento `my-account-name` con la clave de cuenta proporcionada.

```Python
blob_datastore_name='azblobsdk' # Name of the datastore to workspace
container_name=os.getenv("BLOB_CONTAINER", "<my-container-name>") # Name of Azure blob container
account_name=os.getenv("BLOB_ACCOUNTNAME", "<my-account-name>") # Storage account name
account_key=os.getenv("BLOB_ACCOUNT_KEY", "<my-account-key>") # Storage account key

blob_datastore = Datastore.register_azure_blob_container(workspace=ws, 
                                                         datastore_name=blob_datastore_name, 
                                                         container_name=container_name, 
                                                         account_name=account_name,
                                                         account_key=account_key)
```

#### <a name="file-share"></a>Recurso compartido de archivos

Para registrar un recurso compartido de archivos de Azure como almacén de datos, use [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-). 

En el código siguiente se crea y registra el almacén de datos `file_datastore_name` en el área de trabajo `ws`. Este almacén de datos tiene acceso al recurso compartido de archivos `my-fileshare-name` en la cuenta de almacenamiento `my-account-name` con la clave de cuenta proporcionada.

```Python
file_datastore_name='azfilesharesdk' # Name of the datastore to workspace
file_share_name=os.getenv("FILE_SHARE_CONTAINER", "<my-fileshare-name>") # Name of Azure file share container
account_name=os.getenv("FILE_SHARE_ACCOUNTNAME", "<my-account-name>") # Storage account name
account_key=os.getenv("FILE_SHARE_ACCOUNT_KEY", "<my-account-key>") # Storage account key

file_datastore = Datastore.register_azure_file_share(workspace=ws,
                                                 datastore_name=file_datastore_name, 
                                                 file_share_name=file_share_name, 
                                                 account_name=account_name,
                                                 account_key=account_key)
```

#### <a name="sql-data"></a>SQL data

Para un almacén de datos de Azure SQL, use [register_azure_sql_database()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore.datastore?view=azure-ml-py#register-azure-sql-database-workspace--datastore-name--server-name--database-name--tenant-id-none--client-id-none--client-secret-none--resource-url-none--authority-url-none--endpoint-none--overwrite-false--username-none--password-none-) para registrar un almacén de datos de credenciales conectado a una base de datos de Azure SQL con permisos de entidad de servicio o autenticación de SQL. 

Para registrarse a través de la autenticación de SQL:

```python
sql_datastore_name="azsqlsdksql"
server_name=os.getenv("SQL_SERVERNAME", "<my-server-name>") # Name of the Azure SQL server
database_name=os.getenv("SQL_DATBASENAME", "<my-database-name>") # Name of the Azure SQL database
username=os.getenv("SQL_USER_NAME", "<my-sql-user-name>") # Username of the database user to access the database
password=os.getenv("SQL_USER_PASSWORD", "<my-sql-user-password>") # Password of the database user to access the database

sql_datastore = Datastore.register_azure_sql_database(workspace=ws,
                                                  datastore_name=sql_datastore_name,
                                                  server_name=server_name,
                                                  database_name=database_name,
                                                  username=username,
                                                  password=password)

```

Para registrarse a través de una entidad de servicio:

```python 
sql_datastore_name="azsqlsdksp"
server_name=os.getenv("SQL_SERVERNAME", "<my-server-name>") # Name of the SQL server
database_name=os.getenv("SQL_DATBASENAME", "<my-database-name>") # Name of the SQL database
client_id=os.getenv("SQL_CLIENTNAME", "<my-client-id>") # Client ID of the service principal with permissions to access the database
client_secret=os.getenv("SQL_CLIENTSECRET", "<my-client-secret>") # Secret of the service principal
tenant_id=os.getenv("SQL_TENANTID", "<my-tenant-id>") # Tenant ID of the service principal

sql_datastore = Datastore.register_azure_sql_database(workspace=ws,
                                                      datastore_name=sql_datastore_name,
                                                      server_name=server_name,
                                                      database_name=database_name,
                                                      client_id=client_id,
                                                      client_secret=client_secret,
                                                      tenant_id=tenant_id)
```

#### <a name="storage-guidance"></a>Orientación sobre el almacenamiento

Recomendamos un contenedor de blobs de Azure. El almacenamiento Estándar y Premium están disponibles para blobs. Aunque el almacenamiento premium sea más costoso, su mayor velocidad de rendimiento puede mejorar la velocidad de las ejecuciones de entrenamiento, sobre todo si usa un gran conjunto de datos. Para obtener información sobre el costo de las cuentas de almacenamiento, vea la [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service).

### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio 

Cree un nuevo almacén de datos en unos cuantos pasos en Azure Machine Learning Studio:

1. Inicie sesión en [Azure Machine Learning Studio](https://ml.azure.com/).
1. Seleccione **Almacenes de datos** en el panel izquierdo en **Administrar**.
1. Seleccione **+ Nuevo almacén de datos**.
1. Complete el formulario para un nuevo almacén de datos. El formulario se actualiza de forma inteligente según las selecciones de tipo de Azure Storage y tipo de autenticación.
  
Puede encontrar la información necesaria para rellenar el formulario en [Azure Portal](https://portal.azure.com). Seleccione **Cuentas de almacenamiento** en el panel izquierdo y elija la cuenta de almacenamiento que quiere registrar. La página **Información general** proporciona información como el nombre de la cuenta, el contenedor y el nombre del recurso compartido de archivos. Para obtener elementos de autenticación, como la clave de cuenta o el token de SAS, vaya a **Claves de acceso** en el panel de **Configuración**.

En el ejemplo siguiente se muestra el aspecto que tendría el formulario al crear un almacén de datos de blobs de Azure: 
    
![Formulario para un nuevo almacén de datos](media/how-to-access-data/new-datastore-form.png)


<a name="get"></a>

## <a name="get-datastores-from-your-workspace"></a>Obtención de almacenes de almacenamiento del área de trabajo

Para obtener un almacén de datos específico registrado en el área de trabajo actual, utilice el método estático [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) en la clase `Datastore`:

```Python
# Get a named datastore from the current workspace
datastore = Datastore.get(ws, datastore_name='your datastore name')
```
Para obtener la lista de almacenes de datos registrados con un área de trabajo determinada, puede utilizar la propiedad [`datastores`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py#datastores) en un objeto del área de trabajo:

```Python
# List all datastores registered in the current workspace
datastores = ws.datastores
for name, datastore in datastores.items():
    print(name, datastore.datastore_type)
```

Al crear un área de trabajo, se registra automáticamente un contenedor de blobs de Azure y un recurso compartido de archivos de Azure en el área de trabajo. Se denominan `workspaceblobstore` y `workspacefilestore`, respectivamente. Almacenan la información de conexión del contenedor de blobs y el recurso compartido de archivos que se aprovisionan en la cuenta de almacenamiento asociada al área de trabajo. El contenedor `workspaceblobstore` se establece como el almacén de datos predeterminado.

Para obtener el almacén de datos predeterminado del área de trabajo, use esta línea:

```Python
datastore = ws.get_default_datastore()
```

Para definir un almacén de datos predeterminado diferente para el área de trabajo actual, use el método [`set_default_datastore()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-) en el objeto del área de trabajo:

```Python
# Define the default datastore for the current workspace
ws.set_default_datastore('your datastore name')
```

<a name="up-and-down"></a>
## <a name="upload-and-download-data"></a>Carga y descarga de datos

Los métodos [`upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#upload-src-dir--target-path-none--overwrite-false--show-progress-true-) y [`download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) descritos en los ejemplos siguientes son específicos para las clases [AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py) y [AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py), y funcionan de forma idéntica para ambas.

### <a name="upload"></a>Cargar

Cargue un directorio o archivos individuales en el almacén de datos mediante el SDK de Python:

```Python
datastore.upload(src_dir='your source directory',
                 target_path='your target path',
                 overwrite=True,
                 show_progress=True)
```

El parámetro `target_path` especifica la ubicación en el recurso compartido de archivos (o el contenedor de blobs) en que se realiza la carga. El valor predeterminado es `None`, por lo que los datos se cargan en la raíz. Si `overwrite=True`, cualquier dato existente en `target_path` se sobrescribe.

También puede cargar una lista de archivos individuales en el almacén de datos mediante el método `upload_files()`.

### <a name="download"></a>Descargar

Descargue los datos desde un almacén de datos en el sistema de archivos local:

```Python
datastore.download(target_path='your target path',
                   prefix='your prefix',
                   show_progress=True)
```

El parámetro `target_path` es la ubicación del directorio local en la que se descargan los datos. Para especificar una ruta de acceso a la carpeta en el recurso compartido de archivos (o el contenedor de blobs) para realizar la descarga, especifique la ruta de acceso a `prefix`. Si `prefix` es `None`, se descargará todo el contenido del recurso compartido de archivos (o contenedor de blobs).

<a name="train"></a>
## <a name="access-your-data-during-training"></a>Acceso a los datos durante el aprendizaje

> [!IMPORTANT]
> Se recomienda usar los [conjuntos de datos de Azure Machine Learning](how-to-create-register-datasets.md) para obtener acceso a los datos en el entrenamiento. Los conjuntos de datos proporcionan funciones que cargan datos tabulares en un DataFrame de Pandas o Spark. Los conjuntos de datos también proporcionan la capacidad de descargar o montar archivos de cualquier formato desde Azure Blob Storage, Azure Files, Azure Data Lake Storage Gen1, Azure Data Lake Storage Gen2, Azure SQL Database y Azure Database for PostgreSQL. [Obtenga más información sobre cómo entrenar con conjuntos de datos](how-to-train-with-datasets.md).

En la siguiente tabla se enumeran los métodos que indican al destino de proceso cómo usar el almacén de datos durante las ejecuciones: 

Función|Método|Descripción|
----|-----|--------
Montaje| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-mount--)| Se usa para montar el almacén de datos en el destino de proceso. Cuando se monta el almacén de datos, todos los archivos incluidos en él son accesibles para el destino de proceso.
Descargar|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-download-path-on-compute-none-)|Se usa para descargar el contenido del almacén de datos en la ubicación especificada por `path_on_compute`. <br><br> Esta descarga se produce antes de la ejecución.
Cargar|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-upload-path-on-compute-none-)| Se usa para cargar un archivo desde la ubicación especificada por `path_on_compute` en el almacén de datos. <br><br> Esta carga se produce después de la ejecución.

Para hacer referencia a una carpeta o archivo concretos del almacén de datos y que estén disponibles en el destino de proceso, use el método [`path()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#path-path-none--data-reference-name-none-) de este:

```Python
# To mount the full contents in your storage to the compute target
datastore.as_mount()

# To download the contents of only the `./bar` directory in your storage to the compute target
datastore.path('./bar').as_download()
```
> [!NOTE]
> Cualquier objeto `datastore` o `datastore.path` especificado se resuelve en un nombre de variable de entorno con el formato `"$AZUREML_DATAREFERENCE_XXXX"`. El valor de este nombre representa la ruta de acceso de montaje o descarga en el destino de proceso. La ruta de acceso del almacén de datos en el destino de proceso puede no ser la misma que la ruta de acceso de ejecución del script de entrenamiento.

### <a name="examples"></a>Ejemplos 

Se recomienda usar la clase [`Estimator`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) para tener acceso a los datos durante el entrenamiento. 

La variable `script_params` es un diccionario que contiene parámetros en `entry_script`. Úselo para pasar un almacén de datos y describir cómo estarán disponibles los datos en el destino de proceso. Obtenga más información en el [tutorial completo](tutorial-train-models-with-aml.md).

```Python
from azureml.train.estimator import Estimator

# Notice that '/' is in front, which indicates the absolute path
script_params = {
    '--data_dir': datastore.path('/bar').as_mount()
}

est = Estimator(source_directory='your code directory',
                entry_script='train.py',
                script_params=script_params,
                compute_target=compute_target
                )
```

También puede pasar una lista de almacenes de datos al parámetro `inputs` del constructor `Estimator` para montar o copiar datos tanto en el almacén de datos como desde él. Este ejemplo de código:
* Descarga todo el contenido de `datastore1` al destino de proceso antes de que se ejecute el script de entrenamiento `train.py`.
* Descarga la carpeta `'./foo'` de `datastore2` al destino de proceso antes de que se ejecute `train.py`.
* Carga el archivo `'./bar.pkl'` desde el destino de proceso a `datastore3` después de que el script se haya ejecutado.

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[datastore1.as_download(), datastore2.path('./foo').as_download(), datastore3.as_upload(path_on_compute='./bar.pkl')])
```
Si prefiere usar un objeto `RunConfig` para el entrenamiento, debe configurar un objeto [`DataReference`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py). 

En el código siguiente se muestra cómo trabajar con un objeto `DataReference` en una canalización de estimación. Para ver el ejemplo completo, consulte [este cuaderno](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-how-to-use-estimatorstep.ipynb).

```Python
from azureml.core import Datastore
from azureml.data.data_reference import DataReference
from azureml.pipeline.core import PipelineData

def_blob_store = Datastore(ws, "workspaceblobstore")

input_data = DataReference(
       datastore=def_blob_store,
       data_reference_name="input_data",
       path_on_datastore="20newsgroups/20news.pkl")

output = PipelineData("output", datastore=def_blob_store)
```
<a name="matrix"></a>

### <a name="compute-and-datastore-matrix"></a>Proceso y matriz de almacén de datos

Los almacenes de datos admiten actualmente el almacenamiento de la información de conexión a los servicios de almacenamiento que se enumeran en la siguiente matriz. Esta matriz muestra las funcionalidades de acceso a datos disponibles para los diferentes destinos de proceso y escenarios de almacenes de datos. [Obtenga más información sobre los destinos de proceso de Azure Machine Learning](how-to-set-up-training-targets.md#compute-targets-for-training).

|Proceso|[AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py)                                       |[AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py)                                      |[AzureDataLakeDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakedatastore?view=azure-ml-py) |[AzureDataLakeGen2Datastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakegen2datastore?view=azure-ml-py) [AzurePostgreSqlDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_postgre_sql_datastore.azurepostgresqldatastore?view=azure-ml-py) [AzureSqlDatabaseDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_sql_database_datastore.azuresqldatabasedatastore?view=azure-ml-py) |
|--------------------------------|----------------------------------------------------------|----------------------------------------------------------|------------------------|----------------------------------------------------------------------------------------|
| Local|[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|N/D         |N/D                                                                         |
| Proceso de Azure Machine Learning |[as_mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [canalizaciones de Machine Learning](concept-ml-pipelines.md)|[as_mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [canalizaciones de Machine Learning](concept-ml-pipelines.md)|N/D         |N/D                                                                         |
| Máquinas virtuales               |[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                           | [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |N/D         |N/D                                                                         |
| HDInsight de Azure                      |[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            | [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |N/D         |N/D                                                                         |
| Transferencia de datos                  |[Canalizaciones de Machine Learning](concept-ml-pipelines.md)                                               |N/D                                           |[Canalizaciones de Machine Learning](concept-ml-pipelines.md)            |[Canalizaciones de Machine Learning](concept-ml-pipelines.md)                                                                            |
| Azure Databricks                     |[Canalizaciones de Machine Learning](concept-ml-pipelines.md)                                              |N/D                                           |[Canalizaciones de Machine Learning](concept-ml-pipelines.md)             |N/D                                                                         |
| Azure Batch                    |[Canalizaciones de Machine Learning](concept-ml-pipelines.md)                                               |N/D                                           |N/D         |N/D                                                                         |
| Análisis con Azure Data Lake       |N/D                                           |N/D                                           |[Canalizaciones de Machine Learning](concept-ml-pipelines.md)             |N/D                                                                         |

> [!NOTE]
> Es posible que haya escenarios en que los procesos de datos de gran tamaño y altamente iterativos se ejecuten más rápidamente mediante `as_download()` en lugar de `as_mount()`. Esto se puede validar de manera experimental.

### <a name="accessing-source-code-during-training"></a>Acceso al código fuente durante el entrenamiento

Azure Blob Storage presenta mayores velocidades de rendimiento que un recurso compartido de archivos de Azure y se escala a un gran número de trabajos iniciados en paralelo. Por esta razón, se recomienda configurar las ejecuciones de manera que usen Blob Storage para transferir archivos de código fuente.

En el ejemplo de código siguiente se especifica en la configuración de ejecución el almacén de datos de blobs que se usará para las transferencias de código fuente:

```python 
# workspaceblobstore is the default blob storage
run_config.source_directory_data_store = "workspaceblobstore" 
```

## <a name="access-data-during-scoring"></a>Acceso a los datos durante la puntuación

Azure Machine Learning dispone de varios métodos para usar los modelos para puntuación. Algunos de estos métodos no proporcionan acceso a los almacenes de datos. Use la tabla siguiente para saber qué métodos le permiten acceder a los almacenes de datos durante la puntuación:

| Método | Acceso a almacén de datos | Descripción |
| ----- | :-----: | ----- |
| [Predicción por lotes](how-to-run-batch-predictions.md) | ✔ | Realice predicciones sobre grandes cantidades de datos asincrónicamente. |
| [Servicio web](how-to-deploy-and-where.md) | &nbsp; | Implemente modelos como servicios web. |
| [Módulo de Azure IoT Edge](how-to-deploy-and-where.md) | &nbsp; | Implemente modelos en dispositivos IoT Edge. |

En situaciones en las que el SDK no proporciona acceso a los almacenes de datos, es posible que pueda crear código personalizado mediante el SDK de Azure correspondiente para obtener acceso a los datos. Por ejemplo, el [SDK de Azure Storage para Python](https://github.com/Azure/azure-storage-python) es una biblioteca cliente que puede usar para acceder a los datos almacenados en blobs o archivos.

<a name="move"></a>
## <a name="move-data-to-supported-azure-storage-solutions"></a>Movimiento de datos a soluciones de Azure Storage compatibles

Azure Machine Learning admite el acceso a datos desde Azure Blob Storage, Azure Files, Azure Data Lake Storage Gen1, Azure Data Lake Storage Gen2, Azure SQL Database y Azure Database for PostgreSQL. Si usa almacenamiento no compatible, se recomienda que mueva los datos a soluciones de Azure Storage compatibles mediante Azure Data Factory. El movimiento de datos a un almacenamiento compatible puede ayudarle a ahorrar costos de salida de datos durante los experimentos de aprendizaje automático. 

Azure Data Factory proporciona una transferencia de datos eficaz y resistente con más de 80 conectores pregenerados sin costo adicional. Estos conectores incluyen servicios de datos de Azure, orígenes de datos locales, Amazon S3 y Redshift, y Google BigQuery. [Siga la guía paso a paso para mover los datos mediante Azure Data Factory](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-copy-data-tool).

## <a name="next-steps"></a>Pasos siguientes

* [Entrenamiento de un modelo](how-to-train-ml-models.md)
* [Implementar un modelo](how-to-deploy-and-where.md)
