---
title: Uso de almacenes de datos en Azure Machine Learning para acceder a los datos
description: Uso de almacenes de datos al almacenamiento de datos de acceso durante el aprendizaje
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 615ab592c040eedf7d31e3a3036f558ea6c09740
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990814"
---
# <a name="how-to-access-data-during-training"></a>Acceso a los datos durante el aprendizaje
En Azure Machine Learning Services, un almacén de datos es una abstracción de [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction). El almacén de datos puede hacer referencia a un contenedor de [blobs de Azure](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) o a un [recurso compartido de archivos de Azure](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) como almacenamiento subyacente. Los almacenes de datos permiten acceder e interactuar fácilmente con un almacenamiento de datos durante los flujos de trabajo de Azure Machine Learning mediante el SDK de Python o la CLI.

## <a name="create-a-datastore"></a>Creación de un almacén de datos
Para usar almacenes de datos, antes se necesita un [área de trabajo](concept-azure-machine-learning-architecture.md#workspace), que se puede crear, o bien se puede recuperar uno existente:

```Python
import azureml.core
from azureml.core import Workspace

ws = Workspace.from_config()
```

### <a name="use-the-default-datastore"></a>Uso del almacén de datos predeterminado
Cada área de trabajo tiene un almacén de datos predeterminado que puede empezar a usar inmediatamente, lo que le ahorra el trabajo de crear y configurar sus propias cuentas de almacenamiento.

Para obtener el almacén de datos predeterminado del área de trabajo:
```Python
ds = ws.get_default_datastore()
```

### <a name="register-a-datastore"></a>Registro de un almacén de datos
Si ya tiene una instancia de Azure Storage, puede registrarla como almacén de datos en el área de trabajo. Azure Machine Learning proporciona la funcionalidad para registrar un contenedor de blobs de Azure o un recurso compartido de archivos de Azure como un almacén de datos. Todos los métodos de registro están en la clase `Datastore` y tienen la forma `register_azure_*`.

#### <a name="azure-blob-container-datastore"></a>Almacén de datos de un contenedor de blobs de Azure
Para registrar un almacén de datos de un contenedor de blobs de Azure:

```Python
ds = Datastore.register_azure_blob_container(workspace=ws, 
                                             datastore_name='your datastore name', 
                                             container_name='your azure blob container name',
                                             account_name='your storage account name', 
                                             account_key='your storage account key',
                                             create_if_not_exists=True)
```

#### <a name="azure-file-share-datastore"></a>Almacén de datos de un recurso compartido de archivos de Azure
Para registrar un almacén de datos de un recurso compartido de archivos de Azure:

```Python
ds = Datastore.register_azure_file_share(workspace=ws, 
                                         datastore_name='your datastore name', 
                                         container_name='your file share name',
                                         account_name='your storage account name', 
                                         account_key='your storage account key',
                                         create_if_not_exists=True)
```

### <a name="get-an-existing-datastore"></a>Obtención de un almacén de datos existente
Para consultar un almacén de datos registrado por nombre:
```Python
ds = Datastore.get(ws, datastore_name='your datastore name')
```

También puede obtener todos los almacenes de datos de un área de trabajo:
```Python
datastores = ws.datastores()
for name, ds in datastores.items(),
    print(name, ds.datastore_type)"
```

Por comodidad, si desea establecer uno de los almacenes de datos registrado como almacén de datos predeterminado del área de trabajo, puede hacerlo como se indica a continuación:
```Python
ws.set_default_datastore('your datastore name')
```

## <a name="upload-and-download-data"></a>Carga y descarga de datos
### <a name="upload"></a>Cargar
Puede cargar un directorio o archivos individuales en el almacén de datos mediante el SDK de Python.

Para cargar un directorio en un almacén de datos `ds`:
```Python
ds.upload(src_dir='your source directory',
          target_path='your target path',
          overwrite=True,
          show_progress=True)
```
`target_path` especifica la ubicación en el recurso compartido de archivos (o el contenedor de blobs) en que se realiza la carga. El valor predeterminado es `None`, en cuyo caso los datos se cargan en la raíz. `overwrite=True` sobrescribirá todos los datos existente en `target_path`.

Como alternativa, puede cargar una lista de archivos individuales en el almacén de datos mediante el método `upload_files()` de este.

### <a name="download"></a>Descargar
También puede descargar datos desde un almacén de datos en el sistema de archivos local.

```Python
ds.download(target_path='your target path',
            prefix='your prefix',
            show_progress=True)
```
`target_path` es la ubicación del directorio local en la que se descargan los datos. Para especificar una ruta de acceso a la carpeta en el recurso compartido de archivos (o el contenedor de blobs) para realizar la descarga, especifique la ruta de acceso a `prefix`. Si `prefix` es `None`, se descargará todo el contenido del recurso compartido de archivos (o contenedor de blobs).

## <a name="access-datastores-for-training"></a>Acceso a almacenes de datos para el aprendizaje
Puede acceder a un almacén de datos durante una ejecución de aprendizaje (por ejemplo, para los datos de aprendizaje o validación) en un destino de proceso remoto mediante el SDK de Python. 

Hay dos formas de que el almacén de datos esté disponible en el proceso remoto:
* **Montaje**  
`ds.as_mount()`: mediante la especificación de este modo de montaje, el almacén de datos obtener se montará automáticamente en el proceso remoto. 
* **Carga o descarga**  
    * `ds.as_download(path_on_compute='your path on compute')`: con este modo de descarga, los datos se descargarán de su almacén de datos al proceso remoto en la ubicación especificada por `path_on_compute`.
    * Por el contrario, también puede cargar los datos que se han producido desde la ejecución del aprendizaje a un almacén de datos. Por ejemplo, si el script de aprendizaje crea un archivo `foo.pkl` en el directorio de trabajo actual del proceso remoto, puede especificar que se cargue en el almacén de datos una vez que el script se haya ejecutado: `ds.as_upload(path_on_compute='./foo.pkl')`. Así se cargará el archivo en la raíz del almacén de datos.
    
Si desea hacer referencia a una carpeta o archivo concretos del almacén de datos, puede usar la función **`path`** de este. Por ejemplo, si el almacén de datos tiene un directorio con una ruta de acceso relativa `./bar`y solo desea descargar el contenido de esta carpeta en el destino del proceso, puede hacerlo como se indica a continuación: `ds.path('./bar').as_download()`

Cualquier objeto `ds` o `ds.path` se resuelve en un nombre de variable de entorno con el formato `"$AZUREML_DATAREFERENCE_XXXX"` cuyo valor representa la ruta de acceso de montaje y descarga en el proceso remoto. La ruta de acceso del almacén de datos en el proceso remoto puede no ser la misma que la ruta de acceso de ejecución del script.

Para acceder al almacén de datos durante el aprendizaje, puede pasarlo al script de aprendizaje como un argumento de línea de comandos a través de `script_params`:

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data_dir': ds.as_mount()
}

est = Estimator(source_directory='your code directory',
                script_params=script_params,
                compute_target=compute_target,
                entry_script='train.py')
```
`as_mount()` es el modo predeterminado de un almacén de datos, por lo que también puede pasar directamente `ds` al argumento `'--data_dir'`.

Como alternativa, puede pasar una lista de almacenes de datos al parámetro `inputs` del constructor Estimador para montar o copiar tanto en los almacenes de datos como desde ellos:

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[ds1.as_download(), ds2.path('./foo').as_download(), ds3.as_upload(path_on_compute='./bar.pkl')])
```
El código anterior:
* descargará todo el contenido del almacén de datos `ds1` en el proceso remoto antes de que se ejecute su script de aprendizaje `train.py`
* descargará la carpeta `'./foo'` del almacén de datos `ds2` en el proceso remoto antes de `train.py` se ejecute
* cargará el archivo `'./bar.pkl'` desde el proceso remoto al almacén de datos `d3` después de que el script se haya ejecutado

## <a name="next-steps"></a>Pasos siguientes
* [Entrenamiento de un modelo](how-to-train-ml-models.md)
