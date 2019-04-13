---
title: Límites de recursos y objetos de Azure Analysis Services | Microsoft Docs
description: Describe los límites de recursos y objetos de Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 0ffbffc788baaffd4a0532c3918ed82cc3eaf5c3
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "59527819"
---
# <a name="analysis-services-resource-and-object-limits"></a>Límites de recursos y objetos de Azure Analysis Services

En este artículo se describen los límites de recursos y objetos de modelo.

## <a name="tier-limits"></a>Límites por nivel

### <a name="developer-tier"></a>Nivel Developer

Este nivel se recomienda para fines de evaluación, desarrollo y prueba. Un plan individual incluye la misma funcionalidad que el nivel Standard, pero tiene limitaciones en cuanto a la potencia de procesamiento, las QPU y el tamaño de la memoria. El escalado horizontal de la réplica de consultas no está disponible para este nivel. Este nivel no ofrece ningún Acuerdo de Nivel de Servicio.

|Plan  |QPU  |Memoria (GB)  |
|---------|---------|---------|
|D1    |    20     |    3     |


### <a name="basic-tier"></a>Nivel Basic

El nivel se recomienda para soluciones de producción con modelos tabulares más pequeños, concurrencia de usuarios limitada y requisitos sencillos de actualización de datos. El escalado horizontal de la réplica de consultas *no está disponible* para este nivel. En este nivel no se admiten perspectivas, varias particiones ni características de modelo tabular de DirectQuery.  

|Plan  |QPU  |Memoria (GB)  |
|---------|---------|---------|
|B1    |    40     |    10     |
|B2    |    80     |    20     |

### <a name="standard-tier"></a>Nivel Standard

Este nivel es para aplicaciones de producción críticas que requieren concurrencia de usuarios elástica y tienen modelos de datos que crecen con rapidez. Admite la actualización de datos avanzada para realización de actualizaciones de modelos de datos casi en tiempo real y admite todas las características del modelado tabular.

|Plan  |QPU  |Memoria (GB)  |
|---------|---------|---------|
|S1    |    40     |    10     |
|S2    |    100     |    25     |
|S3    |    200     |    50     |
|S4    |    400     |    100     |
|S8*    |    320     |    200     |
|S9*    |    640    |    400     |

\* No está disponible en todas las regiones.  

## <a name="object-limits"></a>Límites de objetos

Estos límites son teóricos. El rendimiento se verá reducido con números más bajos.

|Objeto|Tamaños/números máximos|  
|------------|----------------------------|  
|Bases de datos en una instancia|16 000|  
|Número combinado de tablas y columnas en una base de datos|16 000|  
|Filas en una tabla|Ilimitado<br /><br /> **Advertencia:** con la restricción de que ninguna columna de la tabla puede tener más de 1 999 999 997 valores distintos.|  
|Jerarquías en una tabla|15 999|  
|Niveles en una jerarquía|15 999|  
|Relaciones|8.000|  
|Columnas de clave en toda la tabla|15 999|  
|Medidas en las tablas|2^31-1 = 2 147 483 647|  
|Celdas devueltas por una consulta|2^31-1 = 2 147 483 647|  
|Tamaño del registro de la consulta de origen|64 K|  
|Longitud de los nombres de objeto|512 caracteres|  


