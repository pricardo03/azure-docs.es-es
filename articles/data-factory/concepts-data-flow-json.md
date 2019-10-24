---
title: Uso de JSON en flujo de datos de asignación en Azure Data Factory
description: El flujo de datos de asignación de Azure Data Factory incluye funcionalidades integradas para administrar documentos JSON con jerarquías.
author: kromerm
ms.author: makromer
ms.review: djpmsft
ms.service: data-factory
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: fe412e9e682fb55f1664c546e6b6c5a347527adb
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72387360"
---
# <a name="mapping-data-flow-json-handling"></a>Administración de JSON con flujo de datos de asignación

## <a name="creating-json-structures-in-derived-column"></a>Creación de estructuras JSON en columna derivada

Puede agregar una columna compleja al flujo de datos mediante el generador de expresiones de columna derivada. En la transformación de columna derivada, agregue una nueva columna y abra el generador de expresiones haciendo clic en el cuadro azul. Para que una columna sea compleja, puede escribir la estructura JSON manualmente o usar la experiencia de usuario para agregar subcolumnas de forma interactiva.

### <a name="using-the-expression-builder-ux"></a>Uso de la experiencia de usuario del generador de expresiones

En el panel lateral del esquema de salida, mantenga el puntero sobre una columna y haga clic en el icono de signo más. Seleccione **Agregar subcolumna** para convertir la columna en un tipo complejo.

![Agregar subcolumna](media/data-flow/addsubcolumn.png "Agregar subcolumna")

Puede agregar columnas y subcolumnas adicionales de la misma manera. Para cada campo no complejo, se puede agregar una expresión en el editor de expresiones a la derecha.

![Columna compleja](media/data-flow/complexcolumn.png "Columna compleja")

### <a name="entering-the-json-structure-manually"></a>Introducción manual de la estructura JSON

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

#### <a name="sample-manual-script-for-complete-hierarchical-definition"></a>Script manual de ejemplo para una definición jerárquica completa
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

El uso de un conjunto de datos JSON como origen en el flujo de datos le permite establecer cinco opciones de configuración adicionales. Esta configuración se puede encontrar en el acordeón **Configuración de JSON** en la pestaña **Opciones de origen**.  

![Configuración de JSON](media/data-flow/json-settings.png "Configuración de JSON")

### <a name="default"></a>Valor predeterminado

De forma predeterminada, los datos JSON se leen en el formato siguiente.

```
{ "json": "record 1" }
{ "json": "record 2" }
{ "json": "record 3" }
```

### <a name="single-document"></a>Documento único

Si se selecciona **Documento único**, los flujos de datos de asignación leen un documento JSON de cada archivo. 

``` json
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

Si se selecciona **Nombres de columnas sin comillas**, los flujos de datos de asignación leen columnas JSON que no están entre comillas. 

```
{ json: "record 1" }
{ json: "record 2" }
{ json: "record 3" }
```

### <a name="has-comments"></a>Tiene comentarios

Seleccione **Tiene comentarios** si los datos JSON tienen comentarios de estilo C o C++.

``` json
{ "json": /** comment **/ "record 1" }
{ "json": "record 2" }
{ /** comment **/ "json": "record 3" }
```

### <a name="single-quoted"></a>Con comillas simples

Seleccione **Con comillas simples** si los valores y campos JSON usan comillas simples en lugar de comillas dobles.

```
{ 'json': 'record 1' }
{ 'json': 'record 2' }
{ 'json': 'record 3' }
```

### <a name="backslash-escaped"></a>Barra diagonal inversa con escape

Seleccione **Barra diagonal inversa con escape** si se usan barras diagonales inversas como caracteres de escape en los datos JSON.

```
{ "json": "record 1" }
{ "json": "\} \" \' \\ \n \\n record 2" }
{ "json": "record 3" }
```

## <a name="higher-order-functions"></a>Funciones de orden superior

Una función de orden superior es una función que toma una o más funciones como argumento. A continuación se muestra una lista de las funciones de orden superior admitidas en flujos de datos de asignación que permiten realizar operaciones de matriz.

### <a name="filter"></a>filter
Filtra los elementos de la matriz que no cumplen el predicado proporcionado. "Filter" espera una referencia a un elemento de la función de predicado como #item.

#### <a name="examples"></a>Ejemplos
```
filter([1, 2, 3, 4], #item > 2) => [3, 4]
filter(['a', 'b', 'c', 'd'], #item == 'a' || #item == 'b') => ['a', 'b']
```

### <a name="map"></a>map
Asigna cada elemento de la matriz a un nuevo elemento mediante la expresión proporcionada. La asignación espera una referencia a un elemento de la función de expresión como #item.

#### <a name="examples"></a>Ejemplos
```
map([1, 2, 3, 4], #item + 2) => [3, 4, 5, 6]
map(['a', 'b', 'c', 'd'], #item + '_processed') => ['a_processed', 'b_processed', 'c_processed', 'd_processed']
```

### <a name="reduce"></a>reduce
Acumula los elementos de una matriz. "Reduce" espera una referencia a un acumulador y a un elemento en la primera función de expresión, como #acc y #item, y espera que el valor resultante sea #result para poder usarlo en la segunda función de expresión.

#### <a name="examples"></a>Ejemplos
```
reduce([1, 2, 3, 4], 0, #acc + #item, #result) => 10
reduce(['1', '2', '3', '4'], '0', #acc + #item, #result) => '01234'
reduce([1, 2, 3, 4], 0, #acc + #item, #result + 15) => 25
```

### <a name="sort"></a>sort
Ordena la matriz mediante la función de predicado proporcionada. "Sort" espera una referencia a dos elementos consecutivos de la función de expresión, como #item1 y #item2.

#### <a name="examples"></a>Ejemplos
```
sort([4, 8, 2, 3], compare(#item1, #item2)) => [2, 3, 4, 8]
sort(['a3', 'b2', 'c1'],
        iif(right(#item1, 1) >= right(#item2, 1), 1, -1)) => ['c1', 'b2', 'a3']
sort(['a3', 'b2', 'c1'],
        iif(#item1 >= #item2, 1, -1)) => ['a3', 'b2', 'c1']
```

### <a name="contains"></a>contains
Devuelve true si algún elemento de la matriz proporcionada se evalúa como true en el predicado proporcionado. "Contains" espera una referencia a un elemento de la función de predicado como #item.

#### <a name="examples"></a>Ejemplos
```
contains([1, 2, 3, 4], #item == 3) => true
contains([1, 2, 3, 4], #item > 5) => false
```

## <a name="next-steps"></a>Pasos siguientes

* [Uso de la transformación de columna derivada para compilar estructuras jerárquicas](data-flow-derived-column.md)
