---
title: Requisitos del almacenamiento de blobs de Microsoft Azure Data Box | Microsoft Docs
description: Obtenga información sobre las versiones compatibles para las API, SDK y bibliotecas de cliente para el almacenamiento de blobs de Azure Data Box
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 12/11/2018
ms.author: alkohli
ms.openlocfilehash: e7c2cc0c0ffaae11bd7bf5113c942cdb98397201
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53551156"
---
# <a name="azure-data-box-blob-storage-requirements"></a>Requisitos de almacenamiento de blobs de Azure Data Box

En este artículo se enumeran las versiones de las herramientas, SDK y API de Azure compatibles con el almacenamiento de blobs de Azure Data Box. El almacenamiento de blobs de Azure Data Box proporciona la funcionalidad de administración de blobs con una semántica coherente con Azure. En este artículo también se resumen las diferencias conocidas entre el almacenamiento de blobs de Azure Data Box y los servicios de Azure Storage.

Se recomienda que revise cuidadosamente la información antes de conectarse al almacenamiento de blobs de Azure Data Box y que luego vuelva a ella si es necesario.


## <a name="storage-differences"></a>Diferencias de almacenamiento

|     Característica                                             |     Azure Storage                                     |     Almacenamiento de blobs de Azure Data Box |
|---------------------------------------------------------|-------------------------------------------------------|---------------------------|
|    Almacenamiento de archivos de Azure                                   |    Recursos compartidos de archivos SMB basado en la nube admitidos              |    No compatible      |
|    Cifrado de servicio para datos en reposo                  |    Cifrado de AES de 256 bits                             |    Cifrado de AES de 256 bits |
|    Tipo de cuenta de almacenamiento                                 |    Cuentas de Azure Blob Storage y de uso general    |    Solo de uso general v1|
|    Nombre de blob                                            |    1 024 caracteres (2 048 bytes)                     |    880 caracteres (1 760 bytes)|
|    Tamaño máximo del blob en bloques                              |    4,75 TB (100 MB x 50 000 bloques)                   |    4,75 TB (100 MB x 50 000 bloques) para Azure Data Box v 1.7 y versiones posteriores.|
|    Tamaño máximo del blob en páginas                               |    8 TB                                               |    1 TB                   |
|    Tamaño de página de blob en página                                  |    512 bytes                                          |    4 KB                   |

## <a name="supported-api-versions"></a>Versiones de API admitidas

Se admiten las siguientes versiones de las API del servicio de Azure Storage con almacenamiento de blobs de Azure Data Box:

Versión preliminar pública (Azure Data Box 1.7 y versiones posteriores)

- [2017-04-17](/rest/api/storageservices/version-2017-04-17)
- [2016-05-31](/rest/api/storageservices/version-2016-05-31)
- [2015-12-11](/rest/api/storageservices/version-2015-12-11)
- [2015-07-08](/rest/api/storageservices/version-2015-07-08)
- [2015-04-05](/rest/api/storageservices/version-2015-04-05)

## <a name="supported-sdk-versions"></a>Versiones de SDK compatibles

|     Biblioteca de cliente     |     Versión admitida de almacenamiento de blobs de Azure Data Box     |     Vínculo             |     Especificación de punto de conexión         |
|------------------------|-------------------------------------------------|---------------------------------------------|------------------------------------|
|    .NET                |    De la 6.2.0 a la 8.7.0.                         |    Paquete NuGet:   https://www.nuget.org/packages/WindowsAzure.Storage/ <br>Versión de GitHub:   https://github.com/Azure/azure-storage-net/releases                                                                      |    archivo app.config                 |
|    Java                |    De la 4.1.0 a la 6.1.0                          |    Paquete Maven:   http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage   <br>Versión de GitHub:   https://github.com/Azure/azure-storage-java/releases                                                      |    Configuración de la cadena de conexión         |
|    Node.js             |    De la 1.1.0 a la 2.7.0                          |    Vínculo NPM:   https://www.npmjs.com/package/azure-storage   (Por ejemplo: ejecute "npm install azure-storage@2.7.0")   <br>Versión de GitHub:   https://github.com/Azure/azure-storage-node/releases                            |    Declaración de instancia de servicio    |
|    C++                 |    De la 2.4.0 a la 3.1.0                          |    Paquete NuGet:   https://www.nuget.org/packages/wastorage.v140/   <br>Versión de GitHub:   https://github.com/Azure/azure-storage-cpp/releases                                                                            |    Configuración de la cadena de conexión         |
|    PHP                 |    De la 0.15.0 a la 1.0.0                         |    Versión de GitHub:   https://github.com/Azure/azure-storage-php/releases   <br>Instalación a través de Composer (consulte los detalles a continuación)                                                                                                   |    Configuración de la cadena de conexión         |
|    Python              |    De la 0.30.0 a la 1.0.0                         |    Versión de GitHub:   https://github.com/Azure/azure-storage-python/releases                                                                                                                                              |    Declaración de instancia de servicio    |
|    Ruby                |    De la 0.12.1 a la 1.0.1                         |    Paquete de RubyGems:<br>Común:   https://rubygems.org/gems/azure-storage-common/   <br>Blob: https://rubygems.org/gems/azure-storage-blob/      <br>Versión de GitHub:   https://github.com/Azure/azure-storage-ruby/releases    |                                   |

## <a name="supported-azure-client-libraries"></a>Bibliotecas de clientes de Azure compatibles

Para el almacenamiento en blobs de Azure Data Box, hay requisitos de bibliotecas de cliente y de sufijos de puntos de conexión que son específicos.

Las versiones compatibles de la API REST para el almacenamiento en blobs de Azure Data Box son 2017-04-17, 2016-05-31, 2015-12-11, 2015-07-08 y 2015-04-05 para Azure Data Box versión 1.7 y versiones posteriores. Los puntos de conexión del almacenamiento en blobs de Azure Data Box no tienen una paridad completa con la versión más reciente de la API REST de Azure Blob Storage. En lo que respecta a las bibliotecas de cliente de almacenamiento, debe conocer cuál es la versión compatible con la API REST.

### <a name="azure-data-box-17-onwards"></a>Azure Data Box versión 1.7 y posteriores

| Biblioteca de cliente     |Versión admitida de almacenamiento de blobs de Azure Data Box     | Vínculo   |     Especificación de punto de conexión      |
|--------------------|--------------------------------------------|--------|---------------------------------|
|    .NET                |    8.7.0                                           |    Paquete NuGet:   https://www.nuget.org/packages/WindowsAzure.Storage/8.7.0    <br>Versión de GitHub:   https://github.com/Azure/azure-storage-net/releases/tag/v8.7.0                                                                                                                                                                                               |    archivo app.config                 |
|    Java                |    6.1.0                                           |    Paquete Maven:   http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/6.1.0   <br>Versión de GitHub:   https://github.com/Azure/azure-storage-java/releases/tag/v6.1.0                                                                                                                                                                              |    Configuración de la cadena de conexión         |
|    Node.js             |    2.7.0                                           |    Vínculo NPM:   https://www.npmjs.com/package/azure-storage   (Ejecute: npm install azure-storage@2.7.0)   <br>Versión de GitHub:   https://github.com/Azure/azure-storage-node/releases/tag/v2.7.0                                                                                                                                                                        |    Declaración de instancia de servicio    |
|    C++                 |    3.1.0                                           |    Paquete NuGet:   https://www.nuget.org/packages/wastorage.v140/3.1.0   <br>Versión de GitHub:   https://github.com/Azure/azure-storage-cpp/releases/tag/v3.1.0                                                                                                                                                                                                     |    Configuración de la cadena de conexión         |
|    PHP                 |    1.0.0                                           |    Versión de GitHub:<br>Comunes: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-common   <br>Blob: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-blob      <br>Instalación mediante el compositor (para obtener más información, vea los siguientes detalles).                                                                                                             |    Configuración de la cadena de conexión         |
|    Python              |    1.0.0                                           |    Versión de GitHub:<br>Común:   https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-common <br>Blob:   https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-blob                                                                                                                                                                          |    Declaración de instancia de servicio    |
|    Ruby                |    1.0.1                                           |    Paquete de RubyGems:<br>Común:   https://rubygems.org/gems/azure-storage-common/versions/1.0.1   <br>Blob: https://rubygems.org/gems/azure-storage-blob/versions/1.0.1         <br>Versión de GitHub:<br>Comunes: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-common   <br>Blob: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-blob          |    Configuración de la cadena de conexión         |



### <a name="install-php-client-via-composer---current"></a>Instalación del cliente PHP a través de compositor: actual

Para instalar a través de compositor: (usaremos blob como ejemplo).
Cree un archivo llamado composer.json en la raíz del proyecto con el siguiente código:

```
 {
   "require": {
   "Microsoft/azure-storage-blob":"1.0.0"
   }
```

Descargue `composer.phar` a la raíz del proyecto.

Ejecute: php composer.phar install.

### <a name="endpoint-declaration"></a>Declaración de punto de conexión

Un punto de conexión de almacenamiento en blobs de Azure Data Box incluye dos partes: el nombre de una región y el dominio de Azure Data Box. En el SDK de almacenamiento en blobs de Azure Data Box, el punto de conexión predeterminado es <serial no. of the device>.microsoftdatabox.com.  Para obtener más información sobre el punto de conexión de servicio de blob, vaya a [Connect via Data Box Blob storage](data-box-deploy-copy-data-via-rest.md) (Conectase mediante el almacenamiento en blobs de Azure Data Box).
 
## <a name="examples"></a>Ejemplos

### <a name="net"></a>.NET

Para el almacenamiento en blobs de Azure Data Box, se especifica el sufijo de punto de conexión en el archivo `app.config`:

```
<add key="StorageConnectionString"
value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;
EndpointSuffix=<<serial no. of the device>.microsoftdatabox.com  />
```

### <a name="java"></a>Java

Para el almacenamiento en blobs de Azure Data Box, se especifica el sufijo de punto de conexión en la configuración de la cadena de conexión:

```
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key;" +
    "EndpointSuffix=<serial no. of the device>.microsoftdatabox.com ";
```

### <a name="nodejs"></a>Node.js

Para el almacenamiento en blobs de Azure Data Box, se especifica el sufijo de punto de conexión en la instancia de declaración:

```
var blobSvc = azure.createBlobService('myaccount', 'mykey',
'myaccount.blob. <serial no. of the device>.microsoftdatabox.com ');
```

### <a name="c"></a>C++

Para el almacenamiento en blobs de Azure Data Box, se especifica el sufijo de punto de conexión en la configuración de la cadena de conexión:

```
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;
AccountName=your_storage_account;
AccountKey=your_storage_account_key;
EndpointSuffix=<serial no. of the device>.microsoftdatabox.com "));
```

### <a name="php"></a>PHP

Para el almacenamiento en blobs de Azure Data Box, se especifica el sufijo de punto de conexión en la configuración de la cadena de conexión:

```
$connectionString = 'BlobEndpoint=http://<storage account name>.blob.<serial no. of the device>.microsoftdatabox.com /;
AccountName=<storage account name>;AccountKey=<storage account key>'
```

### <a name="python"></a>Python

Para el almacenamiento en blobs de Azure Data Box, se especifica el sufijo de punto de conexión en la instancia de declaración:

```
block_blob_service = BlockBlobService(account_name='myaccount',
account_key='mykey',
endpoint_suffix=’<serial no. of the device>.microsoftdatabox.com’)
```

### <a name="ruby"></a>Ruby

Para el almacenamiento en blobs de Azure Data Box, se especifica el sufijo de punto de conexión en la configuración de la cadena de conexión:

```
set
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;
AccountName=myaccount;
AccountKey=mykey;
EndpointSuffix=<serial no. of the device>.microsoftdatabox.com
```

## <a name="next-steps"></a>Pasos siguientes

* [Implementación de Azure Data Box](data-box-deploy-ordered.md)