---
title: 'Guía de inicio rápido de Azure: Creación de un blob en el almacenamiento de objetos con el SDK de Storage para Java V10 | Microsoft Docs'
description: En esta guía de inicio rápido, se va a crear un contenedor en el almacenamiento de objetos (Blob), se va a cargar un archivo, se van a enumerar objetos y se van a descargar con el SDK de Storage para Java.
services: storage
author: roygara
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 07/02/2018
ms.author: rogarana
ms.openlocfilehash: a789269e73e1817f6a45e1e5948dbfaa21efd283
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38704477"
---
# <a name="quickstart-upload-download-and-list-blobs-using-the-java-sdk-v10-preview"></a>Guía de inicio rápido: Carga, descarga y enumeración de blobs mediante el SDK de Java V10 (versión preliminar)

En esta guía de inicio rápido, aprenderá a usar el nuevo SDK de Storage para Java con el fin de cargar, descargar y enumerar blobs en bloques en un contenedor en Azure Blob Storage. El nuevo SDK para Java usa el modelo de programación reactivo con RxJava que proporciona operaciones asincrónicas. Más información sobre RxJava [aquí](https://github.com/ReactiveX/RxJava). 

## <a name="prerequisites"></a>requisitos previos

Para completar esta guía de inicio rápido:

* Instalación y configuración de [Maven](http://maven.apache.org/download.cgi) para que funcione desde la línea de comandos, o cualquier IDE de Java que prefiera
* Instalación y configuración de [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Descarga de la aplicación de ejemplo

La [aplicación de ejemplo](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) utilizada en este tutorial de inicio rápido es una aplicación de consola básica. 

Use [git](https://git-scm.com/) para descargar una copia de la aplicación en su entorno de desarrollo.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart.git
```

Este comando clona el repositorio en la carpeta git local.

Cuando el proyecto finalice la importación, abra **Quickstart.java** (ubicado en **src/main/java/quickstart**).

[!INCLUDE [storage-copy-account-key-portal](../../../includes/storage-copy-account-key-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Configuración de la cadena de conexión de almacenamiento.
Esta solución requiere que el nombre y la clave de la cuenta de almacenamiento estén guardadas de forma segura en variables de entorno locales de la máquina en la que se ejecuta el programa de ejemplo. Siga uno de estos ejemplos dependiendo de su sistema operativo para crear las variables de entorno.

### <a name="for-linux"></a>Para Linux

```
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

### <a name="for-windows"></a>Para Windows

```
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

## <a name="run-the-sample"></a>Ejecución del ejemplo

En este ejemplo se crea un archivo de prueba en el directorio predeterminado (AppData\Local\Temp, para usuarios de Windows), después le solicita que especifique los comandos para cargar el archivo de prueba en el almacenamiento de blobs, se enumeran los blobs del contenedor y luego se descarga el archivo con un nombre nuevo para poder comparar los archivos antiguos y nuevos. 

Si desea ejecutar el ejemplo mediante Maven en la línea de comandos, abra un shell y vaya a **storage-blobs-java-v10-quickstart** dentro de su directorio clonado. A continuación, escriba `mvn compile exec:java`.

Lo siguiente es un ejemplo de salida si fuera a ejecutar la aplicación en Windows.

```
Created quickstart container
Enter a command
(P)utBlob | (L)istBlobs | (G)etBlob | (D)eleteBlobs | (E)xitSample
# Enter a command :
P
Uploading the sample file into the container: https://<storageaccount>.blob.core.windows.net/quickstart
# Enter a command :
L
Listing blobs in the container: https://<storageaccount>.blob.core.windows.net/quickstart
Blob name: SampleBlob.txt
# Enter a command :
G
Get the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
The blob was downloaded to C:\Users\<useraccount>\AppData\Local\Temp\downloadedFile13097087873115855761.txt
# Enter a command :
D
Delete the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt

# Enter a command :
>> Blob deleted: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
E
Cleaning up the sample and exiting!
```

Tiene el control del ejemplo, por lo tanto, escriba los comandos para que se ejecute el código. Tenga en cuenta que las entradas distinguen mayúsculas de minúsculas.

También puede usar una herramienta como [Explorador de Azure Storage](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para ver los archivos de Blob Storage. El Explorador de Azure Storage es una herramienta gratuita multiplataforma que permite acceder a la información de la cuenta de almacenamiento. 

Después de haber comprobado los archivos, presione la tecla **Entrar** para finalizar la demostración y eliminar los archivos de prueba. Ahora que sabe lo que hace el ejemplo, abra el archivo **Quickstart.java** para examinar el código. 

## <a name="understand-the-sample-code"></a>Descripción del código de ejemplo

A continuación, explicaremos el código de ejemplo para ayudarle a comprender cómo funciona.

### <a name="get-references-to-the-storage-objects"></a>Obtención de referencias a los objetos de almacenamiento

Lo primero que hay que hacer es crear las referencias a los objetos usados para acceder a Blob Storage y administrarlo. Estos objetos dependen unos de otros: cada uno es utilizado por el siguiente de la lista.

* Cree una instancia del objeto StorageURL que apunte a la cuenta de almacenamiento.

    El objeto [**StorageURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._storage_u_r_l?view=azure-java-preview) es una representación de la cuenta de almacenamiento y permite generar una nueva canalización. Una canalización ([canalización HTTP](https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview#url-types--http-pipeline)) es un conjunto de directivas que se usan para manipular solicitudes y respuestas con los mecanismos de autorización, registro y reintento. Con la canalización puede crear una instancia del objeto [**ServiceURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._service_u_r_l?view=azure-java-preview), que le permite crear una instancia de [**ContainerURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-preview), que es necesario para ejecutar operaciones en contenedores de blobs.

* Cree una instancia del objeto ContainerURL, que representa el contenedor al que está accediendo. Los contenedores se usan para organizar los blobs al igual que se usan las carpetas en el equipo para organizar los archivos.

    El objeto **ContainerURL** proporciona un punto de acceso al servicio de contenedor. Con el objeto **ContainerURL** se puede crear una instancia del objeto [**BlobURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-preview), que es necesario para crear blobs.

* Cree una instancia del objeto [BlobURL](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._blob_u_r_l?view=azure-java-preview), que señala al blob específico en el que está interesado.

> [!IMPORTANT]
> Los nombres de contenedor deben estar en minúsculas. Para más información sobre la nomenclatura de contenedores y blobs, consulte [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Asignación de nombres y referencia a contenedores, blobs y metadatos).

### <a name="create-a-container"></a>Crear un contenedor 

En esta sección, va a crear una instancia del objeto ContainerURL y a crear un nuevo contenedor con él. El contenedor del ejemplo se llama **quickstart**. 

En este ejemplo se utiliza [containerURL.create](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l.create?view=azure-java-preview) porque queremos crear un nuevo contenedor cada vez que se ejecuta el ejemplo. O bien se puede crear el contenedor con antelación para que no necesite crearlo en el código.

```java
// Create a ServiceURL to call the Blob service. We will also use this to construct the ContainerURL
SharedKeyCredentials creds = new SharedKeyCredentials(accountName, accountKey);
// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final ServiceURL serviceURL = new ServiceURL(new URL("http://" + accountName + ".blob.core.windows.net"), StorageURL.createPipeline(creds, new PipelineOptions()));

// Let's create a container using a blocking call to Azure Storage
// If container exists, we'll catch and continue
containerURL = serviceURL.createContainerURL("quickstart");

try {
    ContainersCreateResponse response = containerURL.create(null, null).blockingGet();
    System.out.println("Container Create Response was " + response.statusCode());
} catch (RestException e){
    if (e instanceof RestException && ((RestException)e).response().statusCode() != 409) {
        throw e;
    } else {
        System.out.println("quickstart container already exists, resuming...");
    }
}
```

### <a name="upload-blobs-to-the-container"></a>Carga de blobs al contenedor

Blob Storage admite blobs en bloques, blobs en anexos y blobs en páginas. Los blobs en bloques son los que se usan con más frecuencia y serán los que utilicemos en este tutorial de inicio rápido. 

Para cargar un archivo a un blob, obtenga una referencia al blob en el contenedor de destino. Cuando ya tenga la referencia del blob, puede cargar un archivo en él mediante las API de bajo nivel como [Upload](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.upload?view=azure-java-preview) (también conocida como PutBlob), [StageBlock](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.stageblock?view=azure-java-preview#com_microsoft_azure_storage_blob__block_blob_u_r_l_stageBlock_String_Flowable_ByteBuffer__long_LeaseAccessConditions_) (también conocida como PutBLock) en la instancia de BlockBlobURL, o las API de alto nivel proporcionadas en la clase [TransferManager](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._transfer_manager?view=azure-java-preview), como el método [TransferManager.uploadFileToBlockBlob](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._transfer_manager.uploadfiletoblockblob?view=azure-java-preview). Esta operación crea el blob si todavía no existe o lo sobrescribe si ya existe.

El código de ejemplo crea un archivo local que se usará para las operaciones de carga y descarga. El archivo que se va a cargar se guarda como **sourceFile** y la dirección URL del blob es **blob**. En el ejemplo siguiente se carga el archivo al contenedor llamado **quickstart**.

```java
static void uploadFile(BlockBlobURL blob, File sourceFile) throws IOException {
    
    FileChannel fileChannel = FileChannel.open(sourceFile.toPath());
            
    // Uploading a file to the blobURL using the high-level methods available in TransferManager class
    // Alternatively call the Upload/StageBlock low-level methods from BlockBlobURL type
    TransferManager.uploadFileToBlockBlob(fileChannel, blob, 8*1024*1024, null)
        .subscribe(response-> {
            System.out.println("Completed upload request.");
            System.out.println(response.response().statusCode());
        });
}
```

Los blobs en bloques pueden ser cualquier tipo de archivo de texto o binario. Los blobs en páginas se utilizan principalmente para los archivos VHD usados para respaldar VM IaaS. La anexión de blobs se utiliza para anexar datos al final y, a menudo, se usa para el registro. La mayoría de los objetos almacenados en Blob Storage son blobs en bloques.

### <a name="list-the-blobs-in-a-container"></a>Enumerar los blobs de un contenedor

Puede obtener una lista de objetos en un contenedor mediante [containerURL.listBlobsFlatSegment](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l.listblobsflatsegment?view=azure-java-preview). Este método devuelve hasta 5000 objetos a la vez junto con un marcador de continuación (marcador siguiente) si hay más para enumerar en el contenedor. Para ello, creamos una función de aplicación auxiliar que se llama a sí misma repetidamente siempre y cuando haya un marcador en la respuesta anterior listBlobsFlatSegment.

```java
static void listBlobs(ContainerURL containerURL) {
    // Each ContainerURL.listBlobsFlatSegment call return up to maxResults (maxResults=10 passed into ListBlobOptions below).
    // To list all Blobs, we are creating a helper static method called listAllBlobs, 
    // and calling it after the initial listBlobsFlatSegment call
    ListBlobsOptions options = new ListBlobsOptions(null, null, 10);

    containerURL.listBlobsFlatSegment(null, options)
        .flatMap(containersListBlobFlatSegmentResponse -> 
            listAllBlobs(containerURL, containersListBlobFlatSegmentResponse))    
                .subscribe(response-> {
                    System.out.println("Completed list blobs request.");
                    System.out.println(response.statusCode());
                });
}

private static Single <ContainersListBlobFlatSegmentResponse> listAllBlobs(ContainerURL url, ContainersListBlobFlatSegmentResponse response) {                
    // Process the blobs returned in this result segment (if the segment is empty, blobs() will be null.
    if (response.body().blobs() != null) {
        for (Blob b : response.body().blobs().blob()) {
            String output = "Blob name: " + b.name();
            if (b.snapshot() != null) {
                output += ", Snapshot: " + b.snapshot();
            }
            System.out.println(output);
        }
    }
    else {
        System.out.println("There are no more blobs to list off.");
    }
    
    // If there is not another segment, return this response as the final response.
    if (response.body().nextMarker() == null) {
        return Single.just(response);
    } else {
        /*
        IMPORTANT: ListBlobsFlatSegment returns the start of the next segment; you MUST use this to get the next
        segment (after processing the current result segment
        */
            
        String nextMarker = response.body().nextMarker();

        /*
        The presence of the marker indicates that there are more blobs to list, so we make another call to
        listBlobsFlatSegment and pass the result through this helper function.
        */
            
    return url.listBlobsFlatSegment(nextMarker, new ListBlobsOptions(null, null,1))
        .flatMap(containersListBlobFlatSegmentResponse ->
            listAllBlobs(url, containersListBlobFlatSegmentResponse));
    }
}
```

### <a name="download-blobs"></a>Descargar blobs

Descargue blobs en el disco local con el método [blobURL.download](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._blob_u_r_l.download?view=azure-java-preview).

El código siguiente descarga el blob cargado en una sección anterior, agregando el sufijo "_DOWNLOADED" al nombre de blob para que pueda ver ambos archivos en el disco local. 

```java
static void getBlob(BlockBlobURL blobURL, File sourceFile) {
    try {
        // Get the blob using the low-level download method in BlockBlobURL type
        // com.microsoft.rest.v2.util.FlowableUtil is a static class that contains helpers to work with Flowable
        blobURL.download(new BlobRange(0, Long.MAX_VALUE), null, false)
            .flatMapCompletable(response -> {
                AsynchronousFileChannel channel = AsynchronousFileChannel.open(Paths
                    .get(sourceFile.getPath()), StandardOpenOption.CREATE,  StandardOpenOption.WRITE);
                        return FlowableUtil.writeFile(response.body(), channel);
            }).doOnComplete(()-> System.out.println("The blob was downloaded to " + sourceFile.getAbsolutePath()))
            // To call it synchronously add .blockingAwait()
            .subscribe();
    } catch (Exception ex){
        System.out.println(ex.toString());
    }
}
```

### <a name="clean-up-resources"></a>Limpieza de recursos

Si ya no necesita los blobs cargados en esta guía de inicio rápido, puede eliminar todo el contenedor mediante [containerURL.delete](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l.delete?view=azure-java-preview). Este método también elimina los archivos del contenedor.

```java
containerURL.delete(null).blockingGet();
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial de inicio rápido aprendió a transferir archivos entre un disco local y Azure Blob Storage mediante Java. 

> [!div class="nextstepaction"]
> [SDK de Storage V10 para código fuente de Java](https://github.com/Azure/azure-storage-java/tree/New-Storage-SDK-V10-Preview)
> [Referencia de API](https://docs.microsoft.com/en-us/java/api/storage/client?view=azure-java-preview)
> [Más información sobre RxJava](https://github.com/ReactiveX/RxJava)