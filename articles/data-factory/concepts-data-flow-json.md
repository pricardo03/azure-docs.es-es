---
title: Conceptos JSON de Azure Data Factory Mapping Data Flow
description: Data Factory Mapping Data Flow incluye funcionalidades integradas para administrar documentos JSON con jerarquías.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: cde42dda47d54c03c50895bc625f99c9350b53e3
ms.sourcegitcommit: d470d4e295bf29a4acf7836ece2f10dabe8e6db2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2019
ms.locfileid: "70210514"
---
# <a name="mapping-data-flow-json-handling"></a>Administración de JSON con Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

## <a name="creating-json-structures-in-expression-editor"></a>Creación de estructuras JSON en el editor de expresiones
### <a name="derived-column-transformation"></a>Transformación de una columna derivada
Agregar una columna compleja al flujo de datos es más fácil mediante el editor de expresiones de columna derivada. Después de agregar una nueva columna y abrir el editor, hay dos opciones: escribir la estructura JSON manualmente o usar la interfaz de usuario para agregar subcolumnas de forma interactiva.

#### <a name="interactive-ui-json-design"></a>Diseño JSON con interfaz de usuario interactiva
En el panel lateral del esquema de salida se pueden agregar nuevas subcolumnas mediante el menú `+`: ![Agregar subcolumna](media/data-flow/addsubcolumn.png "Add Subcolumn")

A partir de ahí, se pueden agregar nuevas columnas y subcolumnas de la misma manera. Para cada campo no complejo, se puede agregar una expresión en el editor de expresiones a la derecha.

![Columna compleja](media/data-flow/complexcolumn.png "Complex column")

#### <a name="manual-json-design"></a>Diseño JSON manual
Para agregar manualmente una estructura JSON, agregue una nueva columna y escriba la expresión en el editor. La expresión tiene el siguiente formato general:
```
@(
    field1=0,
    field2=@(
        field1=0
    )
)
```
Si esta expresión se especificara para una columna denominada "complexColumn", se escribiría en el receptor como el siguiente JSON:
```
{
    "complexColumn": {
        "field1": 0,
        "field2": {
            "field1": 0
        }
    }
}
```

#### <a name="sample-manual-dsl"></a>Ejemplo de lenguaje específico de dominio manual
```
@(
    title=Title,
    firstName=FirstName,
    middleName=MiddleName,
    lastName=LastName,
    suffix=Suffix,
    contactDetails=@(
        email=EmailAddress,
        phone=Phone
    ),
    address=@(
        line1=AddressLine1,
        line2=AddressLine2,
        city=City,
        state=StateProvince,
        country=CountryRegion,
        postCode=PostalCode
    ),
    ids=[
        toString(CustomerID), toString(AddressID), rowguid
    ]
)
```

## <a name="source-format-options"></a>Opciones de formato de origen
### <a name="default"></a>Valor predeterminado
```
{ "json": "record 1" }
{ "json": "record 2" }
{ "json": "record 3" }
```

### <a name="single-document"></a>Documento único
* Opción uno
```
[
    {
        "json": "record 1"
    },
    {
        "json": "record 2"
    },
    {
        "json": "record 3"
    }
]
```

* Opción dos
```
File1.json
{
    "json": "record 1"
}
File2.json
{
    "json": "record 2"
}
File3.json
{
    "json": "record 3"
}
```

### <a name="unquoted-column-names"></a>Nombres de columnas sin comillas
```
{ json: "record 1" }
{ json: "record 2" }
{ json: "record 3" }
```

### <a name="has-comments"></a>Tiene comentarios
```
{ "json": /** comment **/ "record 1" }
{ "json": "record 2" }
{ /** comment **/ "json": "record 3" }
```

### <a name="single-quoted"></a>Con comillas simples
```
{ 'json': 'record 1' }
{ 'json': 'record 2' }
{ 'json': 'record 3' }
```

### <a name="backslash-escaped"></a>Barra diagonal inversa con escape
```
{ "json": "record 1" }
{ "json": "\} \" \' \\ \n \\n record 2" }
{ "json": "record 3" }
```

# <a name="higher-order-functions"></a>Funciones de orden superior
## <a name="filter"></a>filter
Filtra los elementos de la matriz que no cumplen el predicado proporcionado. "Filter" espera una referencia a un elemento de la función de predicado como #item.

### <a name="examples"></a>Ejemplos
```
filter([1, 2, 3, 4], #item > 2) => [3, 4]
filter(['a', 'b', 'c', 'd'], #item == 'a' || #item == 'b') => ['a', 'b']
```

## <a name="map"></a>map
Asigna cada elemento de la matriz a un nuevo elemento mediante la expresión proporcionada. La asignación espera una referencia a un elemento de la función de expresión como #item.

### <a name="examples"></a>Ejemplos
```
map([1, 2, 3, 4], #item + 2) => [3, 4, 5, 6]
map(['a', 'b', 'c', 'd'], #item + '_processed') => ['a_processed', 'b_processed', 'c_processed', 'd_processed']
```

## <a name="reduce"></a>reduce
Acumula los elementos de una matriz. "Reduce" espera una referencia a un acumulador y a un elemento en la primera función de expresión, como #acc y #item, y espera que el valor resultante sea #result para poder usarlo en la segunda función de expresión.

### <a name="examples"></a>Ejemplos
```
reduce([1, 2, 3, 4], 0, #acc + #item, #result) => 10
reduce(['1', '2', '3', '4'], '0', #acc + #item, #result) => '01234'
reduce([1, 2, 3, 4], 0, #acc + #item, #result + 15) => 25
```

## <a name="sort"></a>sort
Ordena la matriz mediante la función de predicado proporcionada. "Sort" espera una referencia a dos elementos consecutivos de la función de expresión, como #item1 y #item2.

### <a name="examples"></a>Ejemplos
```
sort([4, 8, 2, 3], compare(#item1, #item2)) => [2, 3, 4, 8]
sort(['a3', 'b2', 'c1'],
        iif(right(#item1, 1) >= right(#item2, 1), 1, -1)) => ['c1', 'b2', 'a3']
sort(['a3', 'b2', 'c1'],
        iif(#item1 >= #item2, 1, -1)) => ['a3', 'b2', 'c1']
```

## <a name="contains"></a>contains
Devuelve true si algún elemento de la matriz proporcionada se evalúa como true en el predicado proporcionado. "Contains" espera una referencia a un elemento de la función de predicado como #item.

### <a name="examples"></a>Ejemplos
```
contains([1, 2, 3, 4], #item == 3) => true
contains([1, 2, 3, 4], #item > 5) => false
```

## <a name="next-steps"></a>Pasos siguientes

* [Uso de la transformación de columna derivada para compilar estructuras jerárquicas](data-flow-derived-column.md)
