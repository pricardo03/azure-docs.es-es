---
title: Creación de un blob en Azure Storage mediante la biblioteca cliente de Node.js v2
description: Cree una cuenta de almacenamiento y un contenedor en el almacenamiento de objetos (Blob). Después, puede usar la biblioteca de clientes de Azure Storage para Node.js V2 a fin de cargar un blob en Azure Storage, descargar un blob o enumerar los blobs de un contenedor.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 02/04/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.openlocfilehash: 89dbd6cc9a03398427b157fa207adb898d3bfc56
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68721964"
---
# <a name="how-to-upload-download-and-list-blobs-using-the-client-library-for-nodejs-v2"></a>Carga, descarga y enumeración de blobs mediante la biblioteca cliente de Node.js v2

En esta guía paso a paso, aprenderá a usar la biblioteca cliente de Node.js v2 para cargar, descargar y enumerar blobs con Azure Blob Storage.

> [!TIP]
> La versión más reciente de la biblioteca cliente de Azure Storage para Node.js es la v10. Microsoft recomienda que utilice la versión más reciente de la biblioteca de cliente siempre que sea posible. Para comenzar a usar la versión v10, consulte [Inicio rápido: Carga, descarga, enumeración y eliminación de blobs mediante la biblioteca cliente de Azure Storage para JavaScript v10 (versión preliminar)](storage-quickstart-blobs-nodejs-v10.md).

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

Cree una cuenta de almacenamiento de Azure en [Azure Portal](https://portal.azure.com/#create/Microsoft.StorageAccount-ARM). Si necesita ayuda para crearla, consulte [Creación de una cuenta de almacenamiento](../common/storage-quickstart-create-account.md).

## <a name="download-the-sample-application"></a>Descarga de la aplicación de ejemplo

La [aplicación de ejemplo](https://github.com/Azure-Samples/storage-blobs-node-quickstart.git) es una sencilla aplicación de consola de Node.js. Para comenzar, clone el repositorio en el equipo con el comando siguiente:

```bash
git clone https://github.com/Azure-Samples/storage-blobs-node-quickstart.git
```

Para abrir la aplicación, busque la carpeta *storage-blobs-node-quickstart* y ábrala en el entorno de edición de código de su elección.

[!INCLUDE [storage-copy-connection-string-portal](../../../includes/storage-copy-connection-string-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Configuración de la cadena de conexión de almacenamiento.

Antes de ejecutar la aplicación, debe proporcionar la cadena de conexión de la cuenta de almacenamiento. El repositorio de ejemplo incluye un archivo llamado *.env.example*. Puede cambiar el nombre de este archivo quitando la extensión *.example*, lo que resulta en un archivo llamado *.env*. Dentro del archivo *.env*, agregue el valor de la cadena de conexión después de la clave *AZURE_STORAGE_CONNECTION_STRING*.

## <a name="install-required-packages"></a>Instalación de los paquetes requeridos

En el directorio de la aplicación, ejecute *npm install* para instalar los paquetes necesarios para la aplicación.

```bash
npm install
```

## <a name="run-the-sample"></a>Ejecución del ejemplo
Ahora que las dependencias están instaladas, puede ejecutar el ejemplo si emite el siguiente comando:

```bash
npm start
```

La salida del script será similar a la siguiente:

```bash
Containers:
 - container-one
 - container-two
Container "demo" is created
Blob "quickstart.txt" is uploaded
Local file "./readme.md" is uploaded
Blobs in "demo" container:
 - quickstart.txt
 - readme.md
Blob downloaded blob content: "hello Blob SDK"
Blob "quickstart.txt" is deleted
Container "demo" is deleted
Done
```

Si usa una nueva cuenta de almacenamiento para este ejemplo, puede que no vea ninguno de los nombres de contenedor en la etiqueta "*Contenedores*".

## <a name="understanding-the-code"></a>Descripción del código
La primera expresión se usa para cargar valores en variables de entorno.

```javascript
if (process.env.NODE_ENV !== 'production') {
    require('dotenv').load();
}
```

El módulo *dotenv* carga variables de entorno al ejecutar la aplicación en local para la depuración. Los valores se definen en un archivo denominado *.env* y se cargan en el contexto de ejecución actual. En contextos de producción, la configuración del servidor proporciona estos valores y por eso este código se ejecuta solo cuando el script no se está ejecutando en un contexto de "producción".

```javascript
const path = require('path');
const storage = require('azure-storage');
```

El propósito de los módulos es el siguiente: 

archivo denominado *.env* en el contexto de ejecución actual
- *path* es necesario para determinar la ruta de acceso absoluta del archivo que se va a cargar en Blob Storage
- *azure-storage* es el módulo de la [biblioteca cliente de Azure Storage](https://docs.microsoft.com/javascript/api/azure-storage) para Node.js

Luego, la variable **blobService** se inicializa como una nueva instancia de Azure Blob Service.

```javascript
const blobService = storage.createBlobService();
```

En la implementación siguiente, cada una de las funciones de *blobService* se encapsula en una *Promesa*, lo que permite el acceso a la función *asincrónica* de JavaScript y al operador *await* para ajustarse a la naturaleza de devolución de llamada de la [API de Azure Storage](/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest). Cuando se devuelve una respuesta correcta para cada función, la promesa resuelve con los datos pertinentes junto con un mensaje específico para la acción.

### <a name="list-containers"></a>Enumerar contenedores

La función *listContainers* llama a [listContainersSegmented](/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest), que devuelve colecciones de contenedores de grupos.

```javascript
const listContainers = async () => {
    return new Promise((resolve, reject) => {
        blobService.listContainersSegmented(null, (err, data) => {
            if (err) {
                reject(err);
            } else {
                resolve({ message: `${data.entries.length} containers`, containers: data.entries });
            }
        });
    });
};
```

El tamaño de los grupos se puede configurar mediante [ListContainersOptions](/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice.listcontaineroptions?view=azure-node-latest). La llamada a *listContainersSegmented* devuelve metadatos de blob como una matriz de instancias de [ContainerResult](/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice.containerresult?view=azure-node-latest). Los resultados se devuelven en lotes de un incremento de 5 000 (segmentos). Si hay más de 5 000 blobs en un contenedor, los resultados incluyen un valor para *continuationToken*. Para enumerar los segmentos posteriores del contenedor de blobs, puede pasar el token de continuación a *listContainersSegment* como segundo argumento.

### <a name="create-a-container"></a>Crear un contenedor

La función *createContainer* llama a [createContainerIfNotExists](/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest) y establece el nivel de acceso adecuado para el blob.

```javascript
const createContainer = async (containerName) => {
    return new Promise((resolve, reject) => {
        blobService.createContainerIfNotExists(containerName, { publicAccessLevel: 'blob' }, err => {
            if (err) {
                reject(err);
            } else {
                resolve({ message: `Container '${containerName}' created` });
            }
        });
    });
};
```

El segundo parámetro (*options*) de **createContainerIfNotExists** acepta un valor para [publicAccessLevel](https://docs.microsoft.com/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest). El valor *blob* para *publicAccessLevel* especifica que los datos del blob en cuestión se exponen al público. Esta configuración es opuesta al nivel de acceso *container*, que otorga la capacidad de enumerar el contenido del contenedor.

El uso de **createContainerIfNotExists** permite a la aplicación ejecutar el comando *createContainer* varias veces sin devolver errores cuando el contenedor ya existe. En un entorno de producción, a menudo solo se llama a **createContainerIfNotExists** una vez, ya que se usa el mismo contenedor a lo largo de la aplicación. En estos casos, puede crear el contenedor con antelación a través del portal o mediante la CLI de Azure.

### <a name="upload-text"></a>Cargar texto

La función *uploadString* llama a [createBlockBlobFromText](/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest) para escribir (o sobrescribir) una cadena arbitraria en el contenedor de blobs.

```javascript
const uploadString = async (containerName, blobName, text) => {
    return new Promise((resolve, reject) => {
        blobService.createBlockBlobFromText(containerName, blobName, text, err => {
            if (err) {
                reject(err);
            } else {
                resolve({ message: `Text "${text}" is written to blob storage` });
            }
        });
    });
};
```
### <a name="upload-a-local-file"></a>Cargar un archivo local

La función *uploadLocalFile* usa [createBlockBlobFromLocalFile](/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest#createblockblobfromlocalfile-string--string--string--errororresult-blobresult--) para cargar y escribir (o sobrescribir) un archivo del sistema de archivos en Blob Storage. 

```javascript
const uploadLocalFile = async (containerName, filePath) => {
    return new Promise((resolve, reject) => {
        const fullPath = path.resolve(filePath);
        const blobName = path.basename(filePath);
        blobService.createBlockBlobFromLocalFile(containerName, blobName, fullPath, err => {
            if (err) {
                reject(err);
            } else {
                resolve({ message: `Local file "${filePath}" is uploaded` });
            }
        });
    });
};
```
Otros métodos disponibles para cargar contenido en blobs incluyen trabajar con [texto](/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest-string--string--string---buffer--errororresult-blobresult--) y [secuencias](/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest#createblockblobfromstream-string--string--stream-readable--number--errororresult-blobresult--). Para comprobar que el archivo se ha cargado en Blob Storage, puede usar el [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/) para ver los datos de la cuenta.

### <a name="list-the-blobs"></a>Enumerar los blobs

La función *listBlobs* llama al método [listBlobsSegmented](/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest#listblobssegmented-string--continuationtoken--errororresult-listblobsresult--) para devolver una lista de metadatos de blobs de un contenedor. 

```javascript
const listBlobs = async (containerName) => {
    return new Promise((resolve, reject) => {
        blobService.listBlobsSegmented(containerName, null, (err, data) => {
            if (err) {
                reject(err);
            } else {
                resolve({ message: `${data.entries.length} blobs in '${containerName}'`, blobs: data.entries });
            }
        });
    });
};
```

Al llamar a *listBlobsSegmented*, devuelve los metadatos de los blobs como una matriz de instancias de [BlobResult](https://docs.microsoft.com/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice.blobresult?view=azure-node-latest). Los resultados se devuelven en lotes de un incremento de 5 000 (segmentos). Si hay más de 5 000 blobs en un contenedor, los resultados incluyen un valor para **continuationToken**. Para enumerar los segmentos posteriores del contenedor de blobs, puede pasar el token de continuación a **listBlobSegmented** como segundo argumento.

### <a name="download-a-blob"></a>Descarga de un blob

La función *downloadBlob* usa [getBlobToText](/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest) para descargar el contenido del blob en la ruta de acceso de archivo absoluta especificada.

```javascript
const downloadBlob = async (containerName, blobName) => {
    const dowloadFilePath = path.resolve('./' + blobName.replace('.txt', '.downloaded.txt'));
    return new Promise((resolve, reject) => {
        blobService.getBlobToText(containerName, blobName, (err, data) => {
            if (err) {
                reject(err);
            } else {
                resolve({ message: `Blob downloaded "${data}"`, text: data });
            }
        });
    });
};
```
La implementación que se muestra aquí cambia el origen y devuelve el contenido del blob como una cadena. También puede descargar el blob como una [secuencia](/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest), además de directamente en un [archivo local](/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest).

### <a name="delete-a-blob"></a>Eliminar un blob

La función *deleteBlob* llama a la función [deleteBlobIfExists](/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest#deleteblobifexists-string--string--errororresult-boolean--). Como su nombre indica, esta función no devuelve un error si el blob ya se ha eliminado.

```javascript
const deleteBlob = async (containerName, blobName) => {
    return new Promise((resolve, reject) => {
        blobService.deleteBlobIfExists(containerName, blobName, err => {
            if (err) {
                reject(err);
            } else {
                resolve({ message: `Block blob '${blobName}' deleted` });
            }
        });
    });
};
```

### <a name="delete-a-container"></a>Eliminar un contenedor

Los contenedores se eliminan al retirar el método *deleteContainer* del servicio de blob y pasar el nombre del contenedor.

```javascript
const deleteContainer = async (containerName) => {
    return new Promise((resolve, reject) => {
        blobService.deleteContainer(containerName, err => {
            if (err) {
                reject(err);
            } else {
                resolve({ message: `Container '${containerName}' deleted` });
            }
        });
    });
};
```

### <a name="calling-code"></a>Llamadas al código

Para admitir la sintaxis *async/await* de JavaScript, todo el código de llamada se encapsula en una función denominada *execute*. Luego se llama a execute, que se trata como una promesa.

```javascript
async function execute() {
    // commands 
}

execute().then(() => console.log("Done")).catch((e) => console.log(e));
```
Todo el código siguiente se ejecuta dentro de la función execute, donde se encuentra el comentario `// commands`.

En primer lugar, se declaran las variables correspondientes para asignar nombres, probar contenido y apuntar al archivo local que se va a cargar en Blob Storage.

```javascript
const containerName = "demo";
const blobName = "quickstart.txt";
const content = "hello Node SDK";
const localFilePath = "./readme.md";
let response;
```

Para enumerar los contenedores de la cuenta de almacenamiento, se llama a la función listContainers y la lista de contenedores devuelta se registra en la ventana de salida.

```javascript
console.log("Containers:");
response = await listContainers();
response.containers.forEach((container) => console.log(` -  ${container.name}`));
```

Una vez que la lista de contenedores está disponible, puede usar el método *findIndex* de matriz para ver si el contenedor que quiere crear ya existe. Si el contenedor no existe, se crea.

```javascript
const containerDoesNotExist = response.containers.findIndex((container) => container.name === containerName) === -1;

if (containerDoesNotExist) {
    await createContainer(containerName);
    console.log(`Container "${containerName}" is created`);
}
```
Luego, se cargan una cadena y un archivo local en Blob Storage.

```javascript
await uploadString(containerName, blobName, content);
console.log(`Blob "${blobName}" is uploaded`);

response = await uploadLocalFile(containerName, localFilePath);
console.log(response.message);
```
El proceso para enumerar blobs es el mismo que para enumerar contenedores. La llamada a *listBlobs* devuelve una matriz de blobs del contenedor que se registra en la ventana de salida.

```javascript
console.log(`Blobs in "${containerName}" container:`);
response = await listBlobs(containerName);
response.blobs.forEach((blob) => console.log(` - ${blob.name}`));
```

Para descargar un blob, la respuesta se captura y se usa para acceder al valor del blob. El elemento readableStreamBody de respuesta se convierte en una cadena y se registra en la ventana de salida.

```javascript
response = await downloadBlob(containerName, blobName);
console.log(`Downloaded blob content: "${response.text}"`);
```

Por último, el blob y el contenedor se eliminan de la cuenta de almacenamiento.

```javascript
await deleteBlob(containerName, blobName);
console.log(`Blob "${blobName}" is deleted`);

await deleteContainer(containerName);
console.log(`Container "${containerName}" is deleted`);
```

## <a name="clean-up-resources"></a>Limpieza de recursos
Todos los datos escritos en la cuenta de almacenamiento se eliminan automáticamente al final del ejemplo de código. 

## <a name="resources-for-developing-nodejs-applications-with-blobs"></a>Recursos para el desarrollo de aplicaciones Node.js con blobs

Consulte estos recursos adicionales para el desarrollo de Node.js con Blob Storage:

### <a name="binaries-and-source-code"></a>Archivos binarios y código fuente

- Consulte e instale el [código fuente de la biblioteca de cliente de Node.js](https://github.com/Azure/azure-storage-node) para Azure Storage en GitHub.

### <a name="client-library-reference-and-samples"></a>Referencia a la biblioteca de clientes y ejemplos

- Para más información acerca de la biblioteca de cliente de Node.js, consulte la [referencia de la API de Node.js](https://docs.microsoft.com/javascript/api/overview/azure/storage).
- Explore los [ejemplos de Blob Storage](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=nodejs&term=blob) escritos mediante la biblioteca de cliente de Node.js.

## <a name="next-steps"></a>Pasos siguientes

En este artículo se muestra cómo cargar un archivo entre un disco local y Azure Blob Storage mediante Node.js. Para obtener más información sobre cómo trabajar con Blob Storage, vaya al repositorio de GitHub.

> [!div class="nextstepaction"]
> [SDK de Microsoft Azure Storage para Node.js y JavaScript for Browsers](https://github.com/Azure/azure-storage-node)
