---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/23/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 8908ded31b96aac50db1fc25e92c2c0a8e960453
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219048"
---
El almacenamiento con redundancia local (LRS) replica los datos tres veces dentro de un solo centro de datos. El almacenamiento con redundancia local proporciona una durabilidad mínima del 99,999999999 % (once nueves) de los objetos en un año determinado. LRS es la opción de replicación de costo más bajo y ofrece la menor durabilidad en comparación con otras opciones.

Si se produce un desastre en el nivel de centro de datos (por ejemplo, un incendio o una inundación), es posible que todas las réplicas de una cuenta de almacenamiento con redundancia local se pierdan o que no se puedan recuperar. Para mitigar este riesgo, Microsoft recomienda el uso del almacenamiento con redundancia de zona (ZRS), el almacenamiento con redundancia geográfica (GRS) o el almacenamiento con redundancia de zona geográfica (GZRS).

Una solicitud de escritura a una cuenta de almacenamiento de LRS se devuelve correctamente solo después de que los datos se escriben en las tres réplicas.

Puede que desee usar el almacenamiento con redundancia local en los escenarios siguientes:

* Si la aplicación almacena los datos que se pueden reconstruir fácilmente si se produce una pérdida de datos, puede optar por LRS.
* Algunas aplicaciones están restringidas a la replicación de datos solo dentro de un país o región debido a requisitos de gobernanza de datos. En algunos casos, las regiones emparejadas en las que los datos se replican para las cuentas de GRS pueden estar en otro país o región. Para más información sobre las regiones emparejadas, consulte [Regiones de Azure](https://azure.microsoft.com/regions/).
