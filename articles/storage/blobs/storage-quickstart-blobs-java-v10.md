---
title: 'Inicio rápido: Uso del SDK de Storage para Java para crear un blob'
description: En esta guía de inicio rápido, se va a crear un contenedor en el almacenamiento de objetos (Azure Blob), se va a cargar un archivo, se van a enumerar objetos y se van a descargar con el SDK de Storage para Java.
services: storage
author: mhopkins-msft
ms.custom: mvc, seo-java-july2019
ms.service: storage
ms.topic: quickstart
ms.date: 11/14/2018
ms.author: mhopkins
ms.reviewer: seguler
ms.openlocfilehash: 96da1f8903896f932fa63aa8497eaff69f893559
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489662"
---
# <a name="quickstart-upload-download-and-list-blobs-by-using-the-java-storage-sdk-v10"></a>Inicio rápido: Carga, descarga y enumeración de blobs mediante el SDK de Storage para Java V10

En esta guía de inicio rápido, aprenderá a usar el nuevo SDK de Storage para Java con el fin de cargar, descargar y enumerar blobs en bloques en un contenedor en Azure Blob Storage. El nuevo SDK para Java usa el modelo de programación reactivo con RxJava que proporciona operaciones asincrónicas. Conozca más información sobre las [extensiones reactivas para la máquina virtual Java](https://github.com/ReactiveX/RxJava). 

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

Asegúrese de tener instalados los siguientes requisitos previos adicionales:

* [Maven](https://maven.apache.org/download.cgi) para trabajar desde la línea de comandos o cualquier entorno de desarrollo integrado de Java que prefiera.
* [JDK](https://aka.ms/azure-jdks)

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
Esta solución requiere que almacene de forma segura el nombre y la clave de la cuenta de almacenamiento. Almacene esta información en variables de entorno locales de la máquina en la que se ejecuta el ejemplo. Siga el ejemplo para Linux o el de Windows, según su sistema operativo, para crear las variables de entorno.

### <a name="linux-example"></a>Ejemplo de Linux

```bash
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

### <a name="windows-example"></a>Ejemplo de Windows

```CMD
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

## <a name="run-the-sample"></a>Ejecución del ejemplo

Este ejemplo crea un archivo de prueba en el directorio predeterminado, **AppData\Local\Temp**, para los usuarios de Windows. A continuación, le pedirá que realice los pasos siguientes:

1. Escriba los comandos para cargar el archivo de prueba en Azure Blob Storage.
2. Enumere los blobs del contenedor.
3. Descargue el archivo cargado con un nombre nuevo para poder comparar los archivos antiguos y nuevos. 

Si desea ejecutar el ejemplo mediante Maven en la línea de comandos, abra un shell y vaya a **storage-blobs-java-v10-quickstart** dentro de su directorio clonado. A continuación, escriba `mvn compile exec:java`.

En este ejemplo aparece la salida si ejecuta la aplicación en Windows.

```Output
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

Tiene el control del ejemplo, por lo tanto, escriba los comandos para que se ejecute el código. Las entradas distinguen mayúsculas de minúsculas.

También puede usar una herramienta como [Explorador de Azure Storage](https://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para ver los archivos de Blob Storage. El Explorador de Azure Storage es una herramienta gratuita multiplataforma que le permite acceder a la información de la cuenta de almacenamiento. 

Compruebe los archivos. Seleccione **E** y seleccione **ENTRAR** para finalizar la demostración y eliminar los archivos de prueba. Ahora que sabe lo que hace el ejemplo, abra el archivo **Quickstart.java** para examinar el código. 

## <a name="understand-the-sample-code"></a>Descripción del código de ejemplo

En las siguientes secciones se explica el código de ejemplo para ayudarle a comprender cómo funciona.

### <a name="get-references-to-the-storage-objects"></a>Obtención de referencias a los objetos de almacenamiento

En primer lugar, cree las referencias a los objetos usados para acceder a Blob Storage y administrarlo. Estos objetos dependen unos de otros. Cada uno es utilizado por el siguiente de la lista.

1. Cree una instancia del objeto **StorageURL** que apunte a la cuenta de almacenamiento.

    * El objeto [StorageURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._storage_u_r_l?view=azure-java-stable) es una representación de la cuenta de almacenamiento. Úselo para generar una nueva canalización. 
    * Una canalización es un conjunto de directivas que se usan para manipular solicitudes y respuestas con los mecanismos de autorización, registro y reintento. Para más información, consulte [Canalización HTTP](https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview#url-types--http-pipeline).  
    * Mediante el uso de la canalización, cree una instancia del objeto [ServiceURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._service_u_r_l?view=azure-java-stable).
    * Mediante el uso del objeto **ServiceURL**, cree una instancia de [ContainerURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-stable).
    * El objeto **ContainerURL** es necesario para ejecutar operaciones en contenedores de blobs.

2. Cree una instancia del objeto **ContainerURL**, que representa el contenedor al que está accediendo. Los contenedores se usan para organizar los blobs al igual que se usan las carpetas en el equipo para organizar los archivos.

    * El objeto **ContainerURL** proporciona un punto de acceso al servicio de contenedor. 
    * Mediante el uso del objeto [ContainerURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._blob_u_r_l?view=azure-java-stable), puede crear una instancia de [BlobURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-stable).
    * El objeto **BlobURL** es necesario crear los blobs.

3. Cree una instancia del objeto **BlobURL** que señale al blob específico en el que está interesado. 

> [!IMPORTANT]
> Los nombres de contenedor deben estar en minúsculas. Para más información sobre la nomenclatura de contenedores y blobs, consulte [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Asignación de nombres y referencia a contenedores, blobs y metadatos).

### <a name="create-a-container"></a>Crear un contenedor 

En esta sección, va a crear una instancia del objeto **ContainerURL**. Va a crear un nuevo contenedor con él. El contenedor del ejemplo se llama **quickstart**. 

En este ejemplo se utilizará [ContainerURL.create](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l.create?view=azure-java-stable#com_microsoft_azure_storage_blob__container_u_r_l_create_Metadata_PublicAccessType_Context_) para que pueda crear un nuevo contenedor cada vez que se ejecute el ejemplo. O bien, se puede crear el contenedor con antelación para que no necesite crearlo en el código.

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

Blob Storage admite blobs en bloques, blobs en anexos y blobs en páginas. Los blobs en bloques son los que se usan con más frecuencia. Los va a usar en esta guía de inicio rápido. 

1. Para cargar un archivo a un blob, obtenga una referencia al blob en el contenedor de destino. 
2. Después de obtener la referencia de blob, puede cargar un archivo en él mediante el uso de las siguientes API:

   * API de bajo nivel. Algunos ejemplos son [BlockBlobURL.upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.upload?view=azure-java-stable#com_microsoft_azure_storage_blob__block_blob_u_r_l_upload_Flowable_ByteBuffer__long_BlobHTTPHeaders_Metadata_BlobAccessConditions_Context_), también denominado PutBlob, y [BlockBlobURL.stageBlock](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.stageblock?view=azure-java-stable), también denominado PutBLock, en la instancia de **BlockBlobURL**. 

   * Las API de alto nivel se proporcionan en la [clase TransferManager](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._transfer_manager?view=azure-java-stable). Un ejemplo es el método [TransferManager.uploadFileToBlockBlob](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._transfer_manager.uploadfiletoblockblob?view=azure-java-stable). 

     Esta operación crea el blob si todavía no existe. Si ya existe, lo sobrescribe.

El código de ejemplo crea un archivo local que se usará para las operaciones de carga y descarga. Este almacena el archivo que se va a cargar como **sourceFile** y almacena la dirección URL del blob en **blob**. En el ejemplo siguiente se carga el archivo al contenedor llamado **quickstart**.

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

Puede obtener una lista de objetos en un contenedor mediante [containerURL.listBlobsFlatSegment](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l.listblobsflatsegment?view=azure-java-stable). Este método devuelve hasta 5000 objetos a la vez junto con un marcador de continuación o de siguiente si hay más para enumerar en el contenedor. Cree una función de aplicación auxiliar que se llame a sí misma repetidamente cuando haya un marcador de siguiente en la respuesta anterior **listBlobsFlatSegment**.

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

Descargue blobs en el disco local con el método [blobURL.download](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._blob_u_r_l.download?view=azure-java-stable).

El código siguiente descarga el blob cargado en una sección anterior. **_DOWNLOADED** se agrega como un sufijo al nombre de blob para que pueda ver ambos archivos en el disco local. 

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

Si no necesita los blobs cargados en esta guía de inicio rápido, puede eliminar todo el contenedor mediante [ContainerURL.delete](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l.delete?view=azure-java-stable). Este método también elimina los archivos del contenedor.

```java
containerURL.delete(null).blockingGet();
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido aprendió a transferir archivos entre un disco local y Azure Blob Storage mediante Java. 

> [!div class="nextstepaction"]
> [SDK de Storage V10 para código fuente de Java](https://github.com/Azure/azure-storage-java/)
> [Referencia de API](https://docs.microsoft.com/java/api/overview/azure/storage/client?view=azure-java-stable)
> [Más información sobre RxJava](https://github.com/ReactiveX/RxJava)
