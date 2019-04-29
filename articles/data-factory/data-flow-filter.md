---
title: Transformación Filtrar de Azure Data Factory Mapping Data Flow
description: Transformación Filtrar de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: d1751c47ad4507260d9f8d6ea44fcb32ed0e7338
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61347436"
---
# <a name="azure-data-factoryfilter-transformation"></a>Transformación de datos de Azure FactoryFilter

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
