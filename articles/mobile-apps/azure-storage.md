---
title: Almacenamiento en la nube para crear aplicaciones muy escalables, seguras y duraderas con Azure Storage
description: Conozca los servicios para almacenar en la nube grandes volúmenes de datos (estructurados y no estructurados) de aplicaciones móviles.
author: elamalani
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: 01407f843ef36095fc87feb3722e85dc477ad8bb
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795136"
---
# <a name="cloud-storage-for-highly-secure-durable-scalable-apps-with-azure-storage"></a>Almacenamiento en la nube de aplicaciones muy escalables, seguras y duraderas con Azure Storage
[Azure Storage](https://azure.microsoft.com/services/storage/) es la solución de almacenamiento en la nube de Microsoft para las aplicaciones modernas que ofrece un almacén de objetos escalable de forma masiva para objetos de datos, un servicio de sistema de archivos para la nube, un almacén de mensajería confiable y un almacén NoSQL. Azure Storage se caracteriza por:
- **Durabilidad y alta disponibilidad**: la redundancia garantiza que los datos están seguros en caso de errores de hardware transitorios. También puede optar por replicar datos entre centros de datos o regiones geográficas para obtener protección adicional frente a catástrofes locales o desastres naturales. Los datos replicados de esta manera permanecen con una alta disponibilidad en caso de una interrupción inesperada.
- **Seguridad**: el servicio cifra todos los datos escritos en Azure Storage. Azure Storage proporciona un control pormenorizado sobre quién tiene acceso a los datos.
- **Escalabilidad**: los servicios están diseñados para escalarse de forma masiva con el fin de satisfacer las necesidades de rendimiento y almacenamiento de datos de las aplicaciones de hoy en día.
- **Administración**: Azure controla automáticamente el mantenimiento, las actualizaciones y los problemas críticos del hardware.
- Es posible **acceder** a los datos desde cualquier parte del mundo a través de HTTP o HTTPS. Microsoft proporciona bibliotecas cliente en diversos lenguajes, como .NET, Java, Node.js, Python, PHP, Ruby, Go y otros, así como una API REST consolidada. El scripting se admite en Azure PowerShell o la CLI de Azure, y Azure Portal y el Explorador de Azure Storage ofrecen soluciones visuales fáciles para trabajar con los datos.

Use los servicios siguientes para habilitar el almacenamiento en la nube en sus aplicaciones móviles.

## <a name="azure-blob-storage"></a>Azure Blob Storage
[Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/) ofrece una solución de almacenamiento de objetos para la nube y está optimizado para almacenar grandes cantidades de datos no estructurados que no se ajustan a una definición o un modelo de datos determinados, por ejemplo, de texto o binarios. Admite la variedad de lenguajes de las bibliotecas cliente y está diseñado para:
- Servicio de imágenes o documentos directamente a un explorador.
- Almacenamiento de archivos para acceso distribuido.
- Streaming de audio y vídeo.
- Escribir en archivos de registro.
- Almacenamiento de datos para copia de seguridad y restauración, recuperación ante desastres y archivado.
- Almacenamiento de datos para el análisis en local o en un servicio hospedado de Azure.

**Referencias**
- [Azure Portal](https://portal.azure.com)
- [Documentación](/azure/storage/blobs/storage-blobs-introduction)
- [Guías de inicio rápido](/azure/storage/blobs/storage-quickstart-blobs-portal)
- [Muestras](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

## <a name="azure-table-storage"></a>Azure Table Storage
[Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) es un servicio que almacena datos NoSQL estructurados en la nube y proporciona un almacén de claves y atributos con un diseño sin esquema. Azure Table Storage permite almacenar una gran cantidad de datos estructurados. El servicio es un almacén de datos NoSQL que acepta llamadas autenticadas desde dentro y fuera de la nube de Azure. Las tablas de Azure son ideales para el almacenamiento de datos estructurados no relacionales. Table Storage suele usarse para realizar las siguientes tareas:
- Almacenamiento de TB de datos estructurados capaces de ofrecer servicio a aplicaciones de escala web.
- Almacenamiento de conjuntos de datos que no requieren uniones complejas, claves externas o procedimientos almacenados y que pueden desnormalizarse para un acceso rápido.
- Consulta rápida de datos mediante un índice agrupado.
- Acceso a datos mediante el protocolo OData y las consultas LINQ con la bibliotecas .NET de Data Service de WCF.

Puede usar Table Storage para almacenar grandes conjuntos de datos estructurados no relacionales y realizar consultas sobre ellos, y las tablas se escalarán a medida que aumente la demanda.

**Referencias**
- [Azure Portal](https://portal.azure.com)
- [Documentación](/azure/storage/tables/table-storage-overview)
- [Muestras](/azure/cosmos-db/tutorial-develop-table-dotnet?toc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fstorage%2Ftables%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [Guías de inicio rápido](/azure/storage/tables/table-storage-quickstart-portal)

## <a name="azure-files"></a>Archivos de Azure
[Azure Files](https://azure.microsoft.com/services/storage/files/) permite configurar recursos compartidos de archivos de red de alta disponibilidad a los que se puede acceder mediante el protocolo Bloque de mensajes del servidor (SMB) estándar. Varias máquinas virtuales pueden compartir los mismos archivos con acceso de lectura y escritura. También puede leer los archivos mediante la interfaz REST o las bibliotecas de cliente de Storage. Puede acceder a los archivos desde cualquier lugar del mundo mediante una dirección URL que apunte al archivo e incluya un token de firma de acceso compartido (SAS). Puede generar tokens SAS; permiten el acceso específico a un recurso privado durante un período de tiempo determinado.

Los recursos compartidos de archivos de Azure se pueden usar para:
- Reemplazar o complementar servidores de archivos locales: Desde sistemas operativos tan extendidos como Windows, macOS y Linux se puede montar directamente un recurso compartido de Azure Files desde cualquier lugar del mundo. Los recursos compartidos de Azure Files se pueden replicar también con Azure File Sync en servidores de Windows Server, locales o en la nube, para obtener un almacenamiento en caché eficiente y distribuido de los datos allí donde se usan.
- **Migración mediante "lift and shift" de aplicaciones** a la nube que esperan un recurso compartido de archivos para almacenar datos de la aplicación del usuario.
- **Simplificar el desarrollo en la nube**: Azure Files también se puede utilizar de muchas formas para simplificar los nuevos proyectos de desarrollo en la nube. Por ejemplo:
    - Configuración de aplicaciones compartidas: Un patrón habitual entre las aplicaciones distribuidas es contar con archivos de configuración en una ubicación centralizada que permite tener acceso a ellos desde muchas instancias de aplicaciones. Las instancias de la aplicación pueden cargar su configuración mediante la API de REST de Azure Files y los usuarios pueden acceder a ella según sea necesario montando el recurso compartido SMB localmente.
    - Recurso compartido de diagnóstico: Un recurso compartido de Azure Files es un lugar adecuado para que las aplicaciones en la nube escriban sus registros, métricas y volcados de memoria. Las instancias de la aplicación pueden escribir los registros mediante la API de REST de Azure Files, y los desarrolladores pueden acceder a ellos montando el recurso compartido de archivos en su máquina local. Esto permite una gran flexibilidad, ya que los desarrolladores pueden adoptar el desarrollo en la nube sin tener que abandonar las herramientas existentes que ya conocen y disfrutan.

**Referencias**
- [Azure Portal](https://portal.azure.com)
- [Documentación](/azure/storage/files/storage-files-introduction)
- [Guías de inicio rápido](/azure/storage/files/storage-files-quick-create-use-windows)

## <a name="azure-queues"></a>Colas de Azure
[Azure Queue Storage](https://azure.microsoft.com/services/storage/queues/) es un servicio para almacenar grandes cantidades de mensajes, a los que se puede acceder desde cualquier lugar del mundo a través de llamadas autenticadas mediante HTTP o HTTPS. Un único mensaje en cola puede tener un tamaño de hasta 64 KB y una cola puede contener millones de mensajes, hasta el límite de capacidad total de una cuenta de almacenamiento. Las colas se utilizan normalmente para crear un trabajo pendiente del trabajo que se va a procesar de forma asincrónica.

**Referencias**
- [Azure Portal](https://portal.azure.com)
- [Documentación](/azure/storage/queues/)
- [Guías de inicio rápido](/azure/storage/queues/storage-quickstart-queues-portal)
- [Muestras](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
