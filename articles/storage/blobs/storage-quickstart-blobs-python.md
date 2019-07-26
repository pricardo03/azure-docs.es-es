---
title: 'Guía de inicio rápido de Azure: Creación de un blob en el almacenamiento de objetos con Python | Microsoft Docs'
description: En esta guía de inicio rápido, creará una cuenta de almacenamiento y un contenedor en el almacenamiento de objetos (Blob). Después, puede usar la biblioteca de clientes de almacenamiento para Python a fin de cargar un blob en Azure Storage, descargar un blob o enumerar los blobs de un contenedor.
services: storage
author: mhopkins-msft
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 12/14/2018
ms.author: mhopkins
ms.reviewer: seguler
ms.openlocfilehash: 5fe011d740b1c08ae3b9cf4e3ea67d2cdd4fee66
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68360096"
---
# <a name="quickstart-upload-download-and-list-blobs-with-python"></a>Inicio rápido: Carga, descarga y enumeración de blobs mediante Python

En este artículo de inicio rápido aprenderá a usar Python para cargar, descargar y enumerar blobs en bloques en un contenedor en Azure Blob Storage. Los blobs son simplemente los objetos que pueden contener cualquier cantidad de texto o datos binarios (por ejemplo, imágenes, documentos, medios de transmisión, datos de archivo, etc.) y en Azure Storage son distintos de los recursos compartidos de archivos, las tablas sin esquema y las colas de mensajes. Para más información, consulte [Introducción a Azure Storage](/azure/storage/common/storage-introduction).

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

Asegúrese de tener instalados los siguientes requisitos previos adicionales:

* [Python](https://www.python.org/downloads/)
* [SDK de Azure Storage para Python](https://github.com/Azure/azure-sdk-for-python)

## <a name="download-the-sample-application"></a>Descarga de la aplicación de ejemplo
La [aplicación de ejemplo](https://github.com/Azure-Samples/storage-blobs-python-quickstart.git) usada en esta guía de inicio rápido es una aplicación Python básica.  

Use [git](https://git-scm.com/) para descargar una copia de la aplicación en su entorno de desarrollo. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-python-quickstart.git 
```

Este comando clona el repositorio *Azure-Samples/storage-blobs-python-quickstart* en la carpeta git local. Para ejecutar el programa de Python, abra el archivo *example.py* en la raíz del repositorio.  

[!INCLUDE [storage-copy-account-key-portal](../../../includes/storage-copy-account-key-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Configuración de la cadena de conexión de almacenamiento.
En la aplicación, proporcione el nombre y la clave de la cuenta de almacenamiento para crear un objeto `BlockBlobService`. Abra el archivo *example.py* desde el Explorador de soluciones en el IDE. Reemplace los valores `accountname` y `accountkey` por el nombre y la clave de la cuenta. 

```python
block_blob_service = BlockBlobService(
    account_name='accountname', account_key='accountkey')
```

## <a name="run-the-sample"></a>Ejecución del ejemplo
En este ejemplo se crea un archivo de prueba en la carpeta *Documents*. El programa de ejemplo carga el archivo de prueba en Blob Storage, enumera los blobs en el contenedor y descarga el archivo con un nuevo nombre. 

En primer lugar, instale las dependencias mediante la ejecución de `pip install`:

```python
    pip install azure-storage-blob
```

Después, ejecute el ejemplo. Verá mensajes similares a la siguiente salida:
  
```output
Temp file = C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Uploading to Blob storage as blobQuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

List blobs in the container
         Blob name: QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Downloading blob to C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078_DOWNLOADED.txt
```
Antes de continuar, compruebe la carpeta *Documents* de los dos archivos. Puede abrirlos y ver que son idénticos.

También puede usar una herramienta como [Explorador de Azure Storage](https://storageexplorer.com) para ver los archivos de Blob Storage. El Explorador de Azure Storage es una herramienta gratuita multiplataforma que permite acceder a la información de la cuenta de almacenamiento. 

Después de haber comprobado los archivos, presione cualquier tecla para finalizar la demostración y eliminar los archivos de prueba. Ahora que sabe lo que hace el ejemplo, abra el archivo *example.py* para examinar el código. 

## <a name="understand-the-sample-code"></a>Descripción del código de ejemplo

Veamos el código de ejemplo para entender cómo funciona.

### <a name="get-references-to-the-storage-objects"></a>Obtención de referencias a los objetos de almacenamiento
En primer lugar, creará las referencias a los objetos usados para acceder a Blob Storage y administrarlo. Estos objetos dependen unos de otros, y cada uno es utilizado por el siguiente de la lista.

* Cree una instancia del objeto **BlockBlobService**, que apunta a Blob service en la cuenta de almacenamiento. 

* Cree una instancia del objeto **CloudBlobContainer**, que representa el contenedor al que está accediendo. Los contenedores se usan para organizar los blobs al igual que se usan las carpetas en el equipo para organizar los archivos.

Una vez que tenga el contenedor de blobs en la nube, cree una instancia del objeto **CloudBlockBlob** que apunte al blob específico en el que está interesado. Luego, puede cargar, descargar o copiar el blob según sea necesario.

> [!IMPORTANT]
> Los nombres de contenedor deben estar en minúsculas. Para más información sobre los nombres de contenedores y blobs, consulte [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Nomenclatura y referencias de contenedores, blobs y metadatos).

En esta sección, se crean instancias de los objetos, se crea un contenedor y luego se establecen permisos en el contenedor para que los blobs sean públicos. El contenedor se denomina **quickstartblobs**. 

```python
# Create the BlockBlockService that is used to call the Blob service for the storage account.
block_blob_service = BlockBlobService(
    account_name='accountname', account_key='accountkey')

# Create a container called 'quickstartblobs'.
container_name = 'quickstartblobs'
block_blob_service.create_container(container_name)

# Set the permission so the blobs are public.
block_blob_service.set_container_acl(
    container_name, public_access=PublicAccess.Container)
```
### <a name="upload-blobs-to-the-container"></a>Carga de blobs al contenedor

Blob Storage admite blobs en bloques, blobs en anexos y blobs en páginas. Los blobs en bloques son los que se usan con más frecuencia y serán los que utilicemos en este tutorial de inicio rápido.  

Para cargar un archivo en un blob, obtenga la ruta de acceso completa del archivo combinando el nombre del directorio y el nombre de archivo en la unidad local. Luego, puede cargar el archivo en la ruta de acceso especificada con el método `create_blob_from_path`. 

El código de ejemplo crea un archivo local que se usará para la carga y descarga, almacenando el archivo que se va a cargar como *full_path_to_file* y el nombre del blob como *local_file_name*. En el ejemplo siguiente se carga el archivo al contenedor denominado **quickstartblobs**.

```python
# Create a file in Documents to test the upload and download.
local_path = os.path.expanduser("~\Documents")
local_file_name = "QuickStart_" + str(uuid.uuid4()) + ".txt"
full_path_to_file = os.path.join(local_path, local_file_name)

# Write text to the file.
file = open(full_path_to_file, 'w')
file.write("Hello, World!")
file.close()

print("Temp file = " + full_path_to_file)
print("\nUploading to Blob storage as blob" + local_file_name)

# Upload the created file, use local_file_name for the blob name.
block_blob_service.create_blob_from_path(
    container_name, local_file_name, full_path_to_file)
```

Existen varios métodos de carga que puede usar con Blob Storage. Por ejemplo, si tiene una secuencia de memoria, puede usar el método `create_blob_from_stream` en lugar de `create_blob_from_path`. 

Los blobs en bloques pueden tener un tamaño de hasta 4,7 TB y pueden ser cualquier cosa, desde hojas de cálculo de Excel a archivos de vídeo grandes. Los blobs en páginas se usan principalmente con los archivos VHD que respaldan las máquinas virtuales IaaS. Los blobs en anexos se utilizan para el registro, por ejemplo, cuando desea escribir en un archivo y luego sigue agregando más información. La mayoría de los objetos almacenados en Blob Storage son blobs en bloques.

### <a name="list-the-blobs-in-a-container"></a>Enumerar los blobs de un contenedor

Obtenga una lista de archivos del contenedor con el método `list_blobs`. Este método devuelve un generador. El código siguiente recupera la lista de blobs, luego los recorre y se muestran los nombres de los blobs encontrados en un contenedor.  

```python
# List the blobs in the container.
print("\nList blobs in the container")
generator = block_blob_service.list_blobs(container_name)
for blob in generator:
    print("\t Blob name: " + blob.name)
```

### <a name="download-the-blobs"></a>Descarga de los blobs

Descargue los blobs en el disco local con el método `get_blob_to_path`. El código siguiente descarga el blob cargado en una sección anterior. *_DOWNLOADED* se agrega como un sufijo al nombre de blob para que pueda ver ambos archivos en el disco local. 

```python
# Download the blob(s).
# Add '_DOWNLOADED' as prefix to '.txt' so you can see both files in Documents.
full_path_to_file2 = os.path.join(local_path, string.replace(
    local_file_name, '.txt', '_DOWNLOADED.txt'))
print("\nDownloading blob to " + full_path_to_file2)
block_blob_service.get_blob_to_path(
    container_name, local_file_name, full_path_to_file2)
```

### <a name="clean-up-resources"></a>Limpieza de recursos
Si ya no necesita los blobs cargados en esta guía de inicio rápido, puede eliminar todo el contenedor mediante el método `delete_container`. Si lo que quiere es eliminar archivos individuales, use el método `delete_blob`.

```python
# Clean up resources. This includes the container and the temp files.
block_blob_service.delete_container(container_name)
os.remove(full_path_to_file)
os.remove(full_path_to_file2)
```
## <a name="resources-for-developing-python-applications-with-blobs"></a>Recursos para el desarrollo de aplicaciones Python con blobs

Para más información sobre el desarrollo de Python con almacenamiento de blobs, consulte estos recursos adicionales:

### <a name="binaries-and-source-code"></a>Archivos binarios y código fuente

- Consulte, descargue e instale el [código fuente de la biblioteca de cliente de Python](https://github.com/Azure/azure-storage-python) para Azure Storage en GitHub.

### <a name="client-library-reference-and-samples"></a>Referencia a la biblioteca de clientes y ejemplos

- Para más información sobre la biblioteca de cliente de Python, consulte la [referencia de la API de Python](https://docs.microsoft.com/python/api/overview/azure/storage).
- Explore los [ejemplos de Blob Storage](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=python&term=blob) escritos mediante la biblioteca de cliente de Python.

## <a name="next-steps"></a>Pasos siguientes
 
En esta guía de inicio rápido aprendió a transferir archivos entre un disco local y Azure Blob Storage mediante Python. Para más información sobre cómo trabajar con Blob Storage, continúe con los procedimientos de Blob Storage.

> [!div class="nextstepaction"]
> [Procedimientos de las operaciones de Blob Storage](./storage-python-how-to-use-blob-storage.md)
 
Para más información sobre el Explorador de Storage y los blobs, consulte [Administración de recursos de Azure Blob Storage con el Explorador de Storage (versión preliminar)](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
