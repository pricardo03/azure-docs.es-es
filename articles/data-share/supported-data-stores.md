---
title: Almacenes de datos admitidos en Azure Data Share
description: Obtenga información sobre los almacenes de datos que se admiten para usar Azure Data Share.
ms.service: data-share
author: joannapea
ms.author: joanpo
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: 56103ed89d2e7813fd60bc50ecca7271f5421a4a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75438690"
---
# <a name="supported-data-stores-in-azure-data-share"></a>Almacenes de datos admitidos en Azure Data Share

Azure Data Share proporciona un uso compartido de datos abierto y flexible, lo que incluye la habilidad de compartir desde y hacia diferentes almacenes de datos. Los proveedores de datos pueden compartir datos de un tipo de almacén de datos y sus consumidores pueden elegir en qué almacén se van a recibir los datos. 

En este artículo, obtendrá información sobre el amplio conjunto de almacenes de datos de Azure que se admiten en Azure Data Share. También puede encontrar información sobre las combinaciones de almacenes de datos que pueden aprovechar los proveedores y los consumidores de datos. 

## <a name="what-data-stores-are-supported-in-azure-data-share"></a>¿Qué almacenes de datos se admiten en Azure Data Share? 

La siguiente tabla detalla los orígenes de datos admitidos en Azure Data Share. 

| Almacén de datos | Uso compartido basado en instantáneas | Uso compartido en contexto 
|:--- |:--- |:--- |:--- |:--- |:--- |
| Azure Blob Storage |✓ | |
| Azure Data Lake Storage Gen1 |✓ | |
| Azure Data Lake Storage Gen2 |✓ ||
| Azure SQL Database |Vista previa pública | |
| Azure Synapse Analytics (anteriormente Azure SQL DW) |Vista previa pública | |
| Explorador de datos de Azure | |[Versión preliminar limitada](https://aka.ms/azuredatasharepreviewsignup) |

## <a name="data-store-support-matrix"></a>Matriz de soporte de almacén de datos

Azure Data Share ofrece a los consumidores de datos flexibilidad a la hora de decidir un almacén en donde aceptar datos. Por ejemplo, los datos compartidos de Azure SQL Database se pueden recibir en Azure Data Lake Store Gen2, Azure SQL Database o Azure Synapse Analytics. Los clientes pueden elegir en qué formato recibir los datos cuando configuran un recurso compartido de datos recibidos. 

La siguiente tabla detalla las diferentes combinaciones y opciones que tienen los consumidores de datos cuando aceptan y configuran sus datos compartidos. Para más información sobre cómo configurar asignaciones de conjuntos de datos, consulte [cómo configurar asignaciones de conjuntos de datos](how-to-configure-mapping.md).

|  | Azure Blob Storage | Azure SQL Data Lake Gen1 | Azure SQL Data Lake Gen2 | Azure SQL Database | Azure Synapse Analytics 
|:--- |:--- |:--- |:--- |:--- |:--- |
| Azure Blob Storage |✓ ||✓|
| Azure Data Lake Storage Gen1 |✓ | |✓|
| Azure Data Lake Storage Gen2 |✓ | |✓|
| Azure SQL Database |✓ | |✓|✓|✓|
| Azure Synapse Analytics |✓ | |✓|✓|✓|

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de cómo empezar a compartir datos, vaya al tutorial que cubre cómo [compartir sus datos](share-your-data.md).
