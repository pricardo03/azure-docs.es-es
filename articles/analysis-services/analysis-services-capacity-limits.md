---
title: Límites de recursos y objetos de Azure Analysis Services | Microsoft Docs
description: Describe los límites de recursos y objetos de Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 08/23/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 40a5b68a12724f2574af19bb10c276c54c5afba0
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "69997600"
---
# <a name="analysis-services-resource-and-object-limits"></a>Límites de recursos y objetos de Azure Analysis Services

En este artículo se describen los límites de recursos y objetos de modelo.

## <a name="tier-limits"></a>Límites por nivel

Para QPU y los límites de memoria para los niveles de desarrollador, básico y estándar, consulte la [página de precios de Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="object-limits"></a>Límites de objetos

Estos límites son teóricos. El rendimiento se verá reducido con números más bajos.

|Object|Tamaños/números máximos|  
|------------|----------------------------|  
|Bases de datos en una instancia|16 000|  
|Número combinado de tablas y columnas en una base de datos|16 000|  
|Filas en una tabla|Ilimitado<br /><br /> **Advertencia:** con la restricción de que ninguna columna de la tabla puede tener más de 1 999 999 997 valores distintos.|  
|Jerarquías en una tabla|15 999|  
|Niveles en una jerarquía|15 999|  
|Relaciones|8\.000|  
|Columnas de clave en toda la tabla|15 999|  
|Medidas en las tablas|2^31-1 = 2 147 483 647|  
|Celdas devueltas por una consulta|2^31-1 = 2 147 483 647|  
|Tamaño del registro de la consulta de origen|64 K|  
|Longitud de los nombres de objeto|512 caracteres|  


