---
title: archivo de inclusión
description: archivo de inclusión
services: search
author: HeidiSteen
ms.service: search
ms.topic: include
ms.date: 04/04/2018
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: a466ea29fa31c4c628724f3d5138a1612ef7a0f4
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553913"
---
Puede crear varios servicios dentro de una suscripción. Cada uno de ellos se puede aprovisionar en un nivel concreto. Está limitado únicamente por el número de servicios permitidos en cada nivel. Por ejemplo, puede crear hasta doce servicios en el nivel básico y otros doce en el nivel de S1 dentro de la misma suscripción. Para obtener más información acerca de los niveles, consulte [elegir un nivel o SKU de Azure Search](../articles/search/search-sku-tier.md).

El límite máximo de servicios se puede elevar a petición. Si necesita más servicios en la misma suscripción, póngase en contacto con soporte técnico de Azure.

| Recurso            | Libre<sup>1</sup> | Básica | S1  | S2 | S3 | S3&nbsp;HD |
| ------------------- | ---- | ----- | --- | -- | -- | ----- |
| Servicios máximos    |1     | 12    | 12  | 6  | 6  | 6     |
| Escala máxima en unidades de búsqueda (SU)<sup>2</sup> |N/D |3 unidades de búsqueda |36 unidades de búsqueda |36 unidades de búsqueda |36 unidades de búsqueda |36 unidades de búsqueda |

<sup>1</sup>gratis se basa en los recursos compartidos, no dedicados. El escalado vertical no se admite en los recursos compartidos.

<sup>2</sup>unidades de búsqueda son unidades facturables, asignadas como un *réplica* o un *partición*. Ambos recursos se necesitan para las operaciones de almacenamiento, indexación y consulta. Para más información sobre los cálculos de SU, consulte [Escalado de niveles de recursos para cargas de trabajo de indexación y consulta en Azure Search](../articles/search/search-capacity-planning.md). 