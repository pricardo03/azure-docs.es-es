---
title: Transformación Filtrar de Azure Data Factory Mapping Data Flow
description: Transformación Filtrar de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: dd29753ff14d16081a46eebbc2ea02d94e5985f1
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029363"
---
# <a name="azure-data-factory-filter-transformation"></a>Transformación Filtrar de Azure Data Factory



Las transformaciones Filtrar permiten filtrar las filas. Creación de una expresión que define la condición del filtro. Haga clic en el cuadro de texto para iniciar el Generador de expresiones. En el Generador de expresiones, cree una expresión de filtro para controlar qué filas de la secuencia de datos actual pueden pasar a través del filtro hacia la siguiente transformación. Piense en la transformación Filtrar como la cláusula WHERE de una instrucción SQL.

## <a name="filter-on-loan_status-column"></a>Filtrar la columna loan_status:

```
in([‘Default’, ‘Charged Off’, ‘Fully Paid’], loan_status).
```

Filtrar la columna de año en la demostración de películas:

```
year > 1980
```

## <a name="next-steps"></a>Pasos siguientes

Pruebe una transformación de filtro de columna, la [transformación Selección](data-flow-select.md).
