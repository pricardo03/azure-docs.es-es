---
title: Límites de recursos y objetos de Azure Analysis Services | Microsoft Docs
description: Describe los límites de recursos y objetos de Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 4c2cebe2225e475ccd40460e7b10a6ba3ed428d5
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "44723865"
---
# <a name="analysis-services-resource-and-object-limits"></a>Límites de recursos y objetos de Azure Analysis Services

En este artículo se describen los límites de recursos y objetos de modelo.

## <a name="tier-limits"></a>Límites por nivel

### <a name="developer-tier"></a>Nivel Developer

Este nivel se recomienda para fines de evaluación, desarrollo y prueba. Un plan individual incluye la misma funcionalidad que el nivel Standard, pero tiene limitaciones en cuanto a la potencia de procesamiento, las QPU y el tamaño de la memoria. El escalado horizontal de la réplica de consultas *no está disponible* para este nivel. Este nivel no ofrece ningún Acuerdo de Nivel de Servicio.

|Plan  |QPU  |Memoria (GB)  |
|---------|---------|---------|
|D1    |    20     |    3     |


### <a name="basic-tier"></a>Nivel Basic

El nivel se recomienda para soluciones de producción con modelos tabulares más pequeños, concurrencia de usuarios limitada y requisitos sencillos de actualización de datos. El escalado horizontal de la réplica de consultas *no está disponible* para este nivel. En este nivel *no se admiten* perspectivas, varias particiones y características de modelo tabular de DirectQuery.  

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

Estos son límites teóricos. El rendimiento se verá reducido con números más bajos.

|Objeto|Tamaños/números máximos|  
|------------|----------------------------|  
|Bases de datos en una instancia|16 000|  
|Número combinado de tablas y columnas en una base de datos|16 000|  
|Filas en una tabla|Ilimitado<br /><br /> **Advertencia:** Con la restricción de que ninguna columna de la tabla puede tener más de 1 999 999 997 valores distintos.|  
|Jerarquías en una tabla|15 999|  
|Niveles en una jerarquía|15 999|  
|Relaciones|8.000|  
|Columnas de clave en toda la tabla|15 999|  
|Medidas en una tabla|2^31-1 = 2 147 483 647|  
|Celdas devueltas por una consulta|2^31-1 = 2 147 483 647|  
|Tamaño del registro de la consulta de origen|64 K|  
|Longitud de los nombres de objeto|512 caracteres|  


