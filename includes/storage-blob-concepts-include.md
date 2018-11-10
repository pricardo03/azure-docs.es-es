---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/17/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 7889fbc9373cbdfdfab891bf8b1cd610523c7032
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50088098"
---
Azure Blob Storage es la solución de almacenamiento de objetos de Microsoft para la nube. Blob Storage está optimizado para el almacenamiento de cantidades masivas de datos no estructurados, como texto o datos binarios.

Blob Storage resulta muy conveniente para:

* Servicio de imágenes o documentos directamente a un explorador.
* Almacenamiento de archivos para acceso distribuido.
* Streaming de audio y vídeo.
* Escribir en archivos de registro.
* Almacenamiento de datos para copia de seguridad y restauración, recuperación ante desastres y archivado.
* Almacenamiento de datos para el análisis en local o en un servicio hospedado de Azure.

Los usuarios o las aplicaciones cliente pueden acceder a los objetos de Blob Storage con HTTP o HTTPS &mdash;desde cualquier lugar del mundo&mdash; mediante direcciones URL, la [API REST de Azure Storage](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), la [CLI de Azure](https://docs.microsoft.com/cli/azure/storage) o una biblioteca de cliente de Azure Storage. Las bibliotecas de cliente de almacenamiento están disponibles para varios lenguajes, como [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage/client), [Node.js](http://azure.github.io/azure-storage-node), [Python](https://docs.microsoft.com/python/azure/), [PHP](http://azure.github.io/azure-storage-php/) y [Ruby](http://azure.github.io/azure-storage-ruby).

## <a name="blob-service-concepts"></a>Conceptos de Blob service

Blob Storage expone tres recursos: la cuenta de almacenamiento, los contenedores de la cuenta y los blobs de un contenedor. En el siguiente diagrama se muestra la relación entre estos recursos.

![Diagrama de arquitectura de Blob Storage (objeto)](./media/storage-blob-concepts-include/blob1.png)

### <a name="storage-account"></a>Cuenta de almacenamiento

Todo el acceso a objetos de datos en Azure Storage tiene lugar a través de una cuenta de almacenamiento. Para más información, vea [Introducción a las cuentas de Azure Storage](../articles/storage/common/storage-account-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="container"></a>Contenedor

Un contenedor organiza un conjunto de blobs, de forma parecida a una carpeta en un sistema de archivos. Todos los blobs residen dentro de un contenedor. Una cuenta de almacenamiento puede contener un número ilimitado de contenedores y un contenedor puede almacenar un número ilimitado de blobs. 

  > [!NOTE]
  > El nombre del contenedor debe estar en minúsculas.

### <a name="blob"></a>Blob
 
Azure Storage ofrece tres tipos de blobs: &mdash;blobs en bloques, blobs en anexos y [blobs en páginas](../articles/storage/blobs/storage-blob-pageblob-overview.md) (usados para archivos VHD).

* Los blobs en bloques almacenan texto y datos binarios, hasta unos 4,7 TB. Los blobs en bloques se componen de bloques de datos que se pueden administrar de forma individual.
* Los blobs en anexos constan de bloques, como los blobs en bloques, pero están optimizados para operaciones de anexión. Los blobs en anexos resultan muy convenientes para escenarios como el registro de datos de máquinas virtuales.
* Los blobs en páginas almacenan archivos de acceso aleatorio con un tamaño de hasta 8 TB. Los blobs en páginas almacenan los archivos de disco duro virtual que respaldan las máquinas virtuales.

Todos los blobs residen dentro de un contenedor. Un contenedor es similar a una carpeta de un sistema de archivos. Puede organizar además los blobs en directorios virtuales y recorrerlos igual que haría con un sistema de archivos. 

Puede haber ocasiones en las que los grandes conjuntos de datos y las restricciones de red harán que la carga datos en Blob Storage mediante la red sea poco realista. Puede usar [discos de Azure Data Box](../articles/databox/data-box-disk-overview.md) para solicitar discos de estado sólido (SSD) a Microsoft. A continuación, puede copiar los datos en esos discos y enviarlos de vuelta a Microsoft para su carga en Blob Storage.

Si necesita exportar grandes cantidades de datos de la cuenta de almacenamiento, consulte [Uso del servicio Microsoft Azure Import/Export para transferir datos a Blob Storage](../articles/storage/common/storage-import-export-service.md).
  
Para más información acerca de la nomenclatura de contenedores y blobs, consulte [Asignación de nombres y referencias a contenedores, blobs y metadatos](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).
