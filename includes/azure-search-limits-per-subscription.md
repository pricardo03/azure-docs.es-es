---
title: archivo de inclusión
description: archivo de inclusión
services: search
author: HeidiSteen
ms.service: search
ms.topic: include
ms.date: 05/06/2019
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: 1e147e8bd9260cd1ece60b70641968a229995ec1
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186694"
---
Puede crear varios servicios dentro de una suscripción. Cada uno de ellos se puede aprovisionar en un nivel concreto. La única limitación es el número de servicios permitidos en cada nivel. Por ejemplo, puede crear hasta doce servicios en el nivel básico y otros doce en el nivel de S1 dentro de la misma suscripción. Para más información sobre los niveles, consulte [Selección de una SKU o de un plan de tarifa de Azure Search](../articles/search/search-sku-tier.md).

El límite máximo de servicios se puede elevar a petición. Si necesita tener más servicios en la misma suscripción, póngase en contacto con el soporte técnico de Azure.

| Recurso            | Gratis<sup></sup>1 | Básica | S1  | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| ------------------- | ---- | ----- | --- | -- | -- | ----- | -- | -- |
| Servicios máximos    |1     | 16    | 16  | 8  | 6  | 6     | 6  | 6  |
| Escala máxima en unidades de búsqueda (SU)<sup>2</sup> |N/D |3 unidades de búsqueda |36 unidades de búsqueda |36 unidades de búsqueda |36 unidades de búsqueda |36 unidades de búsqueda |36 unidades de búsqueda |36 unidades de búsqueda |

<sup>1</sup>Gratis se basa en recursos compartidos, no dedicados. El escalado vertical no se admite en los recursos compartidos.

<sup>2</sup> Las unidades de búsqueda son unidades facturables, asignadas como *réplica* o como *partición*. Ambos recursos se necesitan para las operaciones de almacenamiento, indexación y consulta. Para más información sobre los cálculos de SU, consulte [Escalado de niveles de recursos para cargas de trabajo de indexación y consulta en Azure Search](../articles/search/search-capacity-planning.md). 