---
title: Transformación Filtrar en el flujo de datos de asignación de Azure Data Factory
description: Transformación Filtrar en el flujo de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 2afe079c346a15ec212664ce022ac5e2926b12d4
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387807"
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
