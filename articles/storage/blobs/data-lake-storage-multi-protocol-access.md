---
title: Acceso multiprotocolo en Azure Data Lake Storage | Microsoft Docs
description: Use API de blob y aplicaciones que emplean API de blob con Azure Data Lake Storage Gen2.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/01/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 3470e27f64a672db0165b0132996db91a1447c08
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74033983"
---
# <a name="multi-protocol-access-on-azure-data-lake-storage"></a>Acceso multiprotocolo en Azure Data Lake Storage

Las API de blob ya funcionan con las cuentas que tienen un espacio de nombres jerárquico. Esto desbloquea el ecosistema de herramientas, aplicaciones y servicios, así como varias características de Blob Storage para las cuentas que tienen un espacio de nombres jerárquico.

Hasta hace poco, es posible que tuviera que mantener soluciones de almacenamiento independientes para los objetos y para los análisis. Esto se debe a que Azure Data Lake Storage Gen2 tenía una compatibilidad limitada con el ecosistema. Además, tenía acceso limitado a características de Blob service, como el registro de diagnóstico. Las soluciones de almacenamiento fragmentado son difíciles de mantener porque es necesario trasladar los datos entre las cuentas para llevar a cabo distintos escenarios. Pero esto ya no es necesario.

Con el acceso multiprotocolo en Data Lake Storage, puede trabajar con los datos mediante el ecosistema de herramientas, aplicaciones y servicios. Además, incluye herramientas y aplicaciones de terceros. Puede hacer que apunten a cuentas que tienen un espacio de nombres jerárquico sin necesidad de modificarlas. Estas aplicaciones funcionan *tal cual* aunque llamen a las API de blob, dado que estas API ya pueden operar en los datos de cuentas que tienen un espacio de nombres jerárquico.

Las características de Blob Storage, como el [registro de diagnóstico](../common/storage-analytics-logging.md), los [niveles de acceso](storage-blob-storage-tiers.md) y las [directivas de administración del ciclo de vida de Blob Storage](storage-lifecycle-management-concepts.md), ya funcionan con las cuentas que tienen un espacio de nombres jerárquico. Por tanto, puede habilitar los espacios de nombres jerárquicos en las cuentas de Blob Storage sin perder el acceso a estas características importantes. 

> [!NOTE]
> El acceso multiprotocolo en Data Lake Storage está disponible de forma general y en todas las regiones. Algunos servicios de Azure o características de Blob Storage habilitados mediante el acceso multiprotocolo permanecen en versión preliminar. Para obtener más información, vea las tablas de cada sección de este artículo. 

## <a name="how-multi-protocol-access-on-data-lake-storage-works"></a>Funcionamiento del acceso multiprotocolo en Data Lake Storage

Las API de blob y las API de Data Lake Storage Gen2 pueden operar en los mismos datos en las cuentas de almacenamiento que tienen un espacio de nombres jerárquico. Data Lake Storage Gen2 enruta las API de blob a través del espacio de nombres jerárquico para que pueda aprovechar las ventajas de las operaciones de directorio de primera clase y las listas de control de acceso (ACL) compatibles con POSIX. 

![Diagrama conceptual del acceso multiprotocolo en Data Lake Storage](./media/data-lake-storage-interop/interop-concept.png) 

Las herramientas y aplicaciones existentes que usan la API de blob obtienen estos beneficios de forma automática. No es necesario que los desarrolladores las modifiquen. Data Lake Storage Gen2 aplica de forma coherente listas de control de acceso de nivel de archivo y de directorio independientemente del protocolo que usan las herramientas y las aplicaciones para acceder a los datos. 

## <a name="blob-storage-feature-support"></a>Compatibilidad de características de Blob Storage

El acceso multiprotocolo en Data Lake Storage permite usar más características de Blob Storage con Data Lake Storage. En esta tabla se indican las características habilitadas mediante el acceso multiprotocolo en Data Lake Storage. 

Los elementos que aparecen en esta tabla van a cambiar con el tiempo a medida que se siga ampliando la compatibilidad con las características de Blob Storage. 

> [!NOTE]
> Aunque el acceso multiprotocolo en Data Lake Storage está disponible con carácter general, la compatibilidad con algunas de estas características permanece en versión preliminar. 

|Característica de Blob Storage | Nivel de compatibilidad |
|---|---|
|[Nivel de acceso esporádico](storage-blob-storage-tiers.md)|Disponibilidad general|
|SDK de Blob |Disponibilidad general|
|API de REST de Blob|Disponibilidad general|
|[PowerShell](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-powershell) |Disponibilidad general|
|[CLI](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-cli) |Disponibilidad general|
|[Registros de diagnóstico](../common/storage-analytics-logging.md)| Vista previa|
|[Directivas de administración del ciclo de vida](storage-lifecycle-management-concepts.md)| Vista previa|
|[Notificaciones a través de Azure Event Grid](data-lake-storage-events.md)|Vista previa|
|[Nivel de acceso de archivo](storage-blob-storage-tiers.md)| Vista previa|
|[Blobfuse](storage-how-to-mount-container-linux.md)|Todavía no se admite|
|[Almacenamiento inmutable](storage-blob-immutable-storage.md)|Todavía no se admite|
|[Instantáneas](storage-blob-snapshots.md)|Todavía no se admite|
|[Eliminación temporal](storage-blob-soft-delete.md)|Todavía no se admite|
|[Sitios web estáticos](storage-blob-static-website.md)|Todavía no se admite|

Para obtener más información sobre los problemas y las limitaciones generales conocidos con Azure Data Lake Storage Gen2, vea [Problemas conocidos](data-lake-storage-known-issues.md).

## <a name="azure-ecosystem-support"></a>Compatibilidad con el ecosistema de Azure

El acceso multiprotocolo en Data Lake Storage además permite conectarse a más servicios de Azure con Data Lake Storage. En esta tabla se indican los servicios habilitados mediante el acceso multiprotocolo en Data Lake Storage. 

Al igual que ocurre con la lista de características compatibles con Blob Storage, los elementos que aparecen en esta tabla van a cambiar con el tiempo a medida que se siga ampliando la compatibilidad con los servicios de Azure. 

> [!NOTE]
> Aunque el acceso multiprotocolo en Data Lake Storage está disponible con carácter general, la compatibilidad con algunos de estos servicios permanece en versión preliminar. 

|Servicio de Azure | Nivel de compatibilidad |
|---|---|
|[Azure Data Box](data-lake-storage-migrate-on-premises-hdfs-cluster.md)|Disponibilidad general|
|[Captura de Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)|Disponibilidad general|
|[Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-portal)|Disponibilidad general|
|[IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)|Disponibilidad general|
|[Aplicaciones lógicas](https://azure.microsoft.com/services/logic-apps/)|Disponibilidad general|
|[Azure Cognitive Search](https://docs.microsoft.com/azure/search/search-howto-index-azure-data-lake-storage)|Vista previa|

Para obtener la lista completa de compatibilidad del ecosistema Azure con Azure Data Lake Storage Gen2, vea [Integración de Azure Data Lake Storage con los servicios de Azure](data-lake-storage-integrate-with-azure-services.md).

Para obtener más información sobre los problemas y las limitaciones generales conocidos con Azure Data Lake Storage Gen2, vea [Problemas conocidos](data-lake-storage-known-issues.md).

## <a name="next-steps"></a>Pasos siguientes

Consulte [Problemas conocidos](data-lake-storage-known-issues.md).




