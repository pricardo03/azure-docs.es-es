---
title: Transformación Filtrar de Azure Data Factory Mapping Data Flow
description: Transformación Filtrar de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: e0b41850c149ff7095333cf77b780dec1f03b882
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66234409"
---
# <a name="azure-data-factory-filter-transformation"></a>Transformación de filtro de factoría de datos de Azure

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Las transformaciones Filtrar permiten filtrar las filas. Creación de una expresión que define la condición del filtro. Haga clic en el cuadro de texto para iniciar el Generador de expresiones. En el Generador de expresiones, cree una expresión de filtro para controlar qué filas de la secuencia de datos actual pueden pasar a través del filtro hacia la siguiente transformación. Piense en la transformación de filtro como la cláusula WHERE de una instrucción SQL.

## <a name="filter-on-loanstatus-column"></a>Filtrar columna loan_status:

```
in([‘Default’, ‘Charged Off’, ‘Fully Paid’], loan_status).
```

Filtrar la columna de año en la demostración de películas:

```
year > 1980
```

## <a name="next-steps"></a>Pasos siguientes

Pruebe una columna de filtrado de transformación, la [Seleccionar transformación](data-flow-select.md)
