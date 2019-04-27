---
title: Formato de esquema de Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: Más información sobre el formato de esquema en Knowledge Exploration Service (KES) API.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 51a812762659bcc67762b82e9c120772065aab53
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60814416"
---
# <a name="schema-format"></a>Formato de esquema

El esquema se especifica en un archivo JSON que describe la estructura de atributos de los objetos del archivo de datos utilizado para crear el índice.  Para cada atributo, el esquema especifica el nombre, el tipo de datos, las operaciones opcionales y la lista de sinónimos opcional.  Un objeto puede tener 0 o más valores para cada atributo.  A continuación se muestra un ejemplo simplificado de un dominio de publicación académico:

``` json
{
  "attributes":[
    {"name":"Title", "type":"String"},
    {"name":"Year", "type":"Int32"},
    {"name":"Author", "type":"Composite"},
    {"name":"Author.Id", "type":"Int64", "operations":["equals"]},
    {"name":"Author.Name", "type":"String"},
    {"name":"Author.Affiliation", "type":"String"},
    {"name":"Keyword", "type":"String", "synonyms":"Keyword.syn"}
  ]
}
```

Los nombres de atributo son identificadores que distinguen entre mayúsculas y minúsculas que empiezan por una letra y constan únicamente de letras (A-Z), números (0-9) y guiones bajos (\_).  El atributo reservado "logprob" se utiliza para especificar las probabilidades de registro natural relativas entre objetos.

## <a name="attribute-type"></a>Tipo de atributo

A continuación se muestra una lista de los tipos de datos de atributos admitidos:

| Type | DESCRIPCIÓN | Operaciones | Ejemplo |
|------|-------------|------------|---------|
| `String` | Cadena (1-1024 caracteres) | equals, starts_with | "hello world" |
| `Int32` | Entero de 32 bits con signo | equals, starts_with, is_between | 2016 |
| `Int64` | Entero de 64 bits con signo | equals, starts_with, is_between | 9876543210 |
| `Double` | Valor de punto flotante de doble precisión | equals, starts_with, is_between | 1.602e-19 |
| `Date` | Fecha (de 01-01-1400 a 31-12-9999) | equals, is_between | "2016-03-14" |
| `Guid` | Identificador único global | equals | "602DD052-CC47-4B23-A16A-26B52D30C05B" |
| `Blob` | Datos comprimidos internamente no indexados | *None* | "Permitir que todas las personas y organizaciones del mundo puedan llegar aún más lejos" |
| `Composite` | Composición de varios atributos secundarios| *N/D* | { "Name":"harry shum", "Affiliation":"microsoft" } |

Los atributos de cadena se usan para representar valores de cadena que pueden formar parte de la consulta del usuario.  Estos atributos admiten la operación *equals* de coincidencias exactas, así como la operación *starts_with* para escenarios de finalización de consultas, como, por ejemplo, la coincidencia de "micros" con "microsoft".  En próximas versiones se admitirá la no distinción entre mayúsculas y minúsculas y la coincidencia aproximada para manejar los errores ortográficos.

Los atributos Int32/Int64/Double se usan para representar valores numéricos.  La operación *is_between* permite la compatibilidad de desigualdad (lt, le, gt, ge) en tiempo de ejecución.  La operación *starts_with* es compatible con escenarios de finalización de consultas, como la coincidencia de "20" con "2016" o de "3," con "3,14".

Los atributos de fecha se usan para codificar eficazmente los valores de fecha.  La operación *is_between* permite la compatibilidad de desigualdad (lt, le, gt, ge) en tiempo de ejecución.
  
Los atributos de GUID se usan para representar eficazmente valores GUID con compatibilidad predeterminada con la operación *equals*.

Los atributos de blobs se usan para codificar eficazmente blobs de datos potencialmente grandes para la búsqueda en tiempo de ejecución del objeto correspondiente, sin compatibilidad con ninguna operación de indexación basada en el contenido de los valores del blob.

### <a name="composite-attributes"></a>Atributos compuestos

Los atributos compuestos se utilizan para representar una agrupación de valores de atributo.  El nombre de cada atributo secundario empieza por el nombre del atributo compuesto seguido de ".".  Los valores de los atributos compuestos se especifican como un objeto JSON que contiene los valores de los atributos anidados.  Los atributos compuestos pueden tener varios valores de objeto.  Sin embargo, los atributos compuestos no pueden tener atributos secundarios que sean, a su vez, atributos compuestos.

En el ejemplo anterior de publicación académica, esto permite al servicio consultar los documentos por "harry shum" mientras este está en "microsoft".  Sin los atributos compuestos, el servicio solo puede consultar los documentos en los que uno de los autores es "harry shum" y otro de los autores es "microsoft".  Para más información, consulte la información sobre [consultas de composición](SemanticInterpretation.md#composite-function).

## <a name="attribute-operations"></a>Operaciones de atributo

De forma predeterminada, cada atributo está indexado para admitir todas las operaciones disponibles para el tipo de datos de atributo.  Si no se requiere una operación determinada, se puede especificar explícitamente el conjunto de operaciones indexadas para reducir el tamaño del índice.  En el siguiente fragmento de código del esquema de ejemplo anterior, el atributo Author.Id está indexado para que admita solo la operación *equals*, pero no las operaciones adicionales *starts_with* y *is_between* para los atributos Int32.
```json
{"name":"Author.Id", "type":"Int32", "operations":["equals"]}
```

Cuando se hace referencia a un atributo dentro de una gramática, la operación *starts_with* se debe especificar en el esquema para que el servicio genere finalizaciones a partir de consultas parciales.  

## <a name="attribute-synonyms"></a>Sinónimos de atributo

A menudo, es conveniente hacer referencia a un valor de atributo de cadena determinado mediante un sinónimo.  Por ejemplo, los usuarios pueden referirse a "Microsoft" como "MSFT" o "MS".  En estos casos, la definición del atributo puede especificar el nombre de un archivo de esquema que se encuentra en el mismo directorio que este.  Cada línea del archivo de sinónimos representa una entrada de sinónimo en el siguiente formato JSON: `["<canonical>", "<synonym>"]`.  En el esquema de ejemplo, "AuthorName.syn" es un archivo JSON que contiene los valores de sinónimos para el atributo Author.Name.

`{"name":"Author.Name", "type":"String", "synonyms":"AuthorName.syn"}`


Un valor canónico puede tener varios sinónimos.  Varios valores canónicos pueden compartir un sinónimo común.  En tales casos, el servicio resolverá la ambigüedad a través de varias interpretaciones.  A continuación se muestra un archivo de sinónimos AuthorName.syn de ejemplo que se corresponde con el esquema anterior:
```json
["harry shum","heung-yeung shum"]
["harry shum","h shum"]
["henry shum","h shum"]
...
```
