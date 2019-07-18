---
title: Operadores de consultas SQL para Azure Cosmos DB
description: Obtenga información acerca de los operadores SQL para Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: eecc1522f8c260286c7dd7fc4c2e58d5d8caa8fb
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2019
ms.locfileid: "67343187"
---
# <a name="operators-in-azure-cosmos-db"></a>Operadores en Azure Cosmos DB

En este artículo se detallan los diversos operadores admitidos por Azure Cosmos DB.

## <a name="equality-and-comparison-operators"></a>Operadores de igualdad y de comparación

En la siguiente tabla se muestra el resultado de las comparaciones de igualdad en la API de SQL entre dos tipos JSON cualesquiera.

| **Op** | **Undefined** | **Null** | **Boolean** | **Number** | **String** | **Object** | **Array** |
|---|---|---|---|---|---|---|---|
| **Undefined** | Undefined | Undefined | Undefined | Undefined | Undefined | Undefined | Undefined |
| **Null** | Undefined | **OK (CORRECTO)** | Undefined | Undefined | Undefined | Undefined | Undefined |
| **Boolean** | Undefined | Undefined | **OK (CORRECTO)** | Undefined | Undefined | Undefined | Undefined |
| **Number** | Undefined | Undefined | Undefined | **OK (CORRECTO)** | Undefined | Undefined | Undefined |
| **String** | Undefined | Undefined | Undefined | Undefined | **OK (CORRECTO)** | Undefined | Undefined |
| **Object** | Undefined | Undefined | Undefined | Undefined | Undefined | **OK (CORRECTO)** | Undefined |
| **Array** | Undefined | Undefined | Undefined | Undefined | Undefined | Undefined | **OK (CORRECTO)** |

Para los operadores de comparación como `>`, `>=`, `!=`, `<` y `<=`, la comparación entre tipos o entre dos objetos o matrices genera `Undefined`.  

Si el resultado de la expresión escalar es `Undefined`, el elemento no se incluye en el resultado, porque `Undefined` no es igual a `true`.

## <a name="logical-and-or-and-not-operators"></a>Operadores lógicos (Y, O y NO)

Los operadores lógicos operan en valores booleanos. En las tablas siguientes se muestran las tablas de verdad lógica para estos operadores:

**operator OR**

| OR | True | False | Undefined |
| --- | --- | --- | --- |
| True |True |True |True |
| False |True |False |Undefined |
| Undefined |True |Undefined |Undefined |

**operator AND**

| Y | True | False | Undefined |
| --- | --- | --- | --- |
| True |True |False |Undefined |
| False |False |False |False |
| Undefined |Undefined |False |Undefined |

**operator NOT**

| NO |  |
| --- | --- |
| True |False |
| False |True |
| Undefined |Undefined |


## <a name="-operator"></a>Operador *

El operador especial * proyecta el elemento completo tal cual. Al usarse, debe ser el único campo proyectado. Una consulta como `SELECT * FROM Families f` es válida, `SELECT VALUE * FROM Families f` y `SELECT *, f.id FROM Families f` no lo son.

## <a name="-and--operators"></a>? y ?? Operadores

Puede usar el operador ternario (?) y el operador de combinación (??) para crear expresiones condicionales, al igual que en lenguajes de programación populares como C# y JavaScript. 

Puede usar el operador ? para construir nuevas propiedades JSON sobre la marcha. Por ejemplo, la siguiente consulta clasifica los niveles de calificación en `elementary` o `other`:

```sql
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel
     FROM Families.children[0] c
```

Además, puede anidar llamadas en el operador ?, como en la consulta siguiente: 

```sql
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high") AS gradeLevel
    FROM Families.children[0] c
```

Al igual que con otros operadores de consulta, el operador ? excluye elementos si faltan las propiedades a las que se hace referencia o si los tipos que se comparan son diferentes.

Use el operador ?? para comprobar de manera eficaz una propiedad en un elemento al consultar datos semiestructurados o de tipos mixtos. Por ejemplo, la consulta siguiente devuelve `lastName` si está presente, o `surname` si `lastName` no está presente.

```sql
    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f
```

## <a name="next-steps"></a>Pasos siguientes

- [Ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Palabras clave](sql-query-keywords.md)
- [Cláusula SELECT](sql-query-select.md)
