---
title: Transformación Clave suplente de Azure Data Factory Mapping Data Flow
description: Transformación Clave suplente de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 62e9879f6bc75f042669ecb931ca7459d1317cc7
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271528"
---
# <a name="azure-data-factory-mapping-data-flow-surrogate-key-transformation"></a>Transformación Clave suplente de Azure Data Factory Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Para agregar un valor de clave incremental arbitrario no empresarial al conjunto de filas de su flujo de datos, utilice la transformación Clave suplente. Resulta útil para diseñar tablas de dimensiones en un modelo de datos analíticos con un esquema de estrella, donde cada miembro de las tablas de dimensión debe tener una clave única que no sea empresarial, como parte de la metodología Kimball DW.

![Transformación Clave suplente](media/data-flow/surrogate.png "Surrogate Key Transformation")

"Columna de clave" es el nombre que dará a la nueva columna de clave suplente.

"Valor inicial" es el punto de partida del valor incremental.
