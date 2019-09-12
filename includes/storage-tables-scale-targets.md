---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/20/2019
ms.author: tamram
ms.openlocfilehash: 5ab03b682dd0ed1dc7b198e89c86e7a74c6275cd
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "67457535"
---
| Recurso | Destino |
|----------|---------------|
| Tamaño máximo de una tabla individual | 500 TiB |
| Tamaño máximo de una entidad de tabla | 1 MiB |
| Número máximo de propiedades de una entidad de tabla | 255, que incluye tres propiedades del sistema: PartitionKey, RowKey y Timestamp |
| Tamaño máximo total de valores de propiedad de una entidad | 1 MiB |
| Tamaño máximo total de una propiedad individual en una entidad | Varía en función del tipo de propiedad. Para obtener más información, consulte los **tipos de propiedades** en la [descripción del modelo de datos del servicio Tabla](/rest/api/storageservices/understanding-the-table-service-data-model). |
| Número máximo de directivas de acceso almacenadas por tabla | 5 |
| Tasa de solicitud total por cuenta de almacenamiento | 20 000 transacciones por segundo, lo que supone un tamaño de entidad de 1 KiB |
| Rendimiento de destino de una sola partición de tabla (entidades de 1 KiB) | Hasta 2000 entidades por segundo |