---
title: Acceso multiprotocolo en Azure Data Lake Storage (versión preliminar) | Microsoft Docs
description: Use API de blob y aplicaciones que emplean API de blob con Azure Data Lake Storage Gen2.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 471e3008c25ddef83dd9b502dd8f677ae31cc71b
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70259371"
---
# <a name="multi-protocol-access-on-azure-data-lake-storage-preview"></a>Acceso multiprotocolo en Azure Data Lake Storage (versión preliminar)

Las API de blob ya funcionan con las cuentas que tienen un espacio de nombres jerárquico. Esto permite aprovechar todo el ecosistema de herramientas, aplicaciones y servicios, así como todas las características de Blob Storage para las cuentas que tienen un espacio de nombres jerárquico.

Hasta hace poco, es posible que tuviera que mantener soluciones de almacenamiento independientes para los objetos y para los análisis. Esto se debe a que Azure Data Lake Storage Gen2 tenía una compatibilidad limitada con el ecosistema. Además, tenía acceso limitado a características de Blob service, como el registro de diagnóstico. Las soluciones de almacenamiento fragmentado son difíciles de mantener porque es necesario trasladar los datos entre las cuentas para llevar a cabo distintos escenarios. Pero esto ya no es necesario.

> [!NOTE]
> El acceso multiprotocolo en Data Lake Storage está en versión preliminar pública y se encuentra disponible en [estas regiones](#region-availability). Para conocer las limitaciones, consulte el artículo [Problemas conocidos](data-lake-storage-known-issues.md). Para inscribirse en la versión preliminar, visite [esta página](https://aka.ms/blobinteropsignup).

## <a name="use-the-entire-ecosystem-of-applications-tools-and-services"></a>Uso de todo el ecosistema de aplicaciones, herramientas y servicios

Si se inscribe en la versión preliminar del acceso multiprotocolo en Data Lake Storage, podrá trabajar con todos los datos mediante el uso del ecosistema completo de herramientas, aplicaciones y servicios. Esto engloba servicios de Azure como [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-introduction), [IOT Hub](https://docs.microsoft.com/azure/iot-hub/), [Power BI](https://docs.microsoft.com/power-bi/desktop-data-sources) y muchos otros. 

Además, incluye herramientas y aplicaciones de terceros. Puede hacer que apunten a cuentas que tienen un espacio de nombres jerárquico sin necesidad de modificarlas. Estas aplicaciones funcionan *tal cual* aunque llamen a las API de blob, dado que estas API ya pueden operar en los datos de cuentas que tienen un espacio de nombres jerárquico.

> [!NOTE]
> Para conocer las limitaciones, consulte el artículo [Problemas conocidos](data-lake-storage-known-issues.md).

## <a name="use-all-blob-storage-features"></a>Uso de todas las características de Blob Storage

Las características de Blob Storage, como el [registro de diagnóstico](../common/storage-analytics-logging.md), los [niveles de acceso](storage-blob-storage-tiers.md) y las [directivas de administración del ciclo de vida de Blob Storage](storage-lifecycle-management-concepts.md), ya funcionan con las cuentas que tienen un espacio de nombres jerárquico. Por lo tanto, puede habilitar los espacios de nombres jerárquicos en las cuentas de almacenamiento de blobs sin perder el acceso a estas características importantes. 

> [!NOTE]
> Para conocer las limitaciones, consulte el artículo [Problemas conocidos](data-lake-storage-known-issues.md).

## <a name="how-multi-protocol-access-on-data-lake-storage-works"></a>Funcionamiento del acceso multiprotocolo en Data Lake Storage

Las API de blob y las API de Data Lake Storage Gen2 pueden operar en los mismos datos en las cuentas de almacenamiento que tienen un espacio de nombres jerárquico. Data Lake Storage Gen2 enruta las API de blob a través del espacio de nombres jerárquico para que pueda aprovechar las ventajas de las operaciones de directorio de primera clase y las listas de control de acceso (ACL) compatibles con POSIX. 

![Diagrama conceptual del acceso multiprotocolo en Data Lake Storage](./media/data-lake-storage-interop/interop-concept.png) 

Las herramientas y aplicaciones existentes que usan la API de blob obtienen estos beneficios de forma automática. No es necesario que los desarrolladores las modifiquen. Data Lake Storage Gen2 aplica de forma coherente listas de control de acceso de nivel de archivo y de directorio independientemente del protocolo que usan las herramientas y las aplicaciones para acceder a los datos. 

<a id="region-availability" />

## <a name="region-availability"></a>Disponibilidad en regiones

El acceso multiprotocolo en Azure Data Lake Storage (versión preliminar) está disponible en las regiones siguientes:

|||||
|-|-|-|-|
|Centro de EE. UU.|Centro occidental de EE.UU.|Centro de Canadá|
|East US|Asia oriental|Europa del Norte|
|Este de EE. UU. 2|Sudeste asiático|Europa occidental|
|Oeste de EE. UU.|Este de Australia|Este de Japón|
|Oeste de EE. UU. 2|Sur de Brasil||

## <a name="next-steps"></a>Pasos siguientes

Consulte [Problemas conocidos](data-lake-storage-known-issues.md).




