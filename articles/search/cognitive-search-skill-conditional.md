---
title: Habilidades de búsqueda cognitiva condicional (Azure Search) | Microsoft Docs
description: Aptitud condicional que permite el filtrado, crear valores predeterminados y combinar los valores.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: luisca
ms.openlocfilehash: 6a203a38437ccb6a9c325e6594289744e0148c84
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028431"
---
#   <a name="conditional-skill"></a>Aptitudes condicional

El **aptitud condicional** permite una variedad de escenarios que requieren una operación booleana para decidir los datos que se deben asignar a una salida. Estos escenarios incluyen: filtrado, asignar un valor predeterminado y combinar datos según una condición.

El siguiente pseudocódigo explica lo que lleva a cabo la aptitud condicional:

```
if (condition) 
    { output = whenTrue } 
else 
    { output = whenFalse } 
```

> [!NOTE]
> Esta aptitud no está enlazada a una API de Cognitive Services y no se le cobrará por usarla. Sin embargo, debe [adjuntar un recurso de Cognitive Services](cognitive-search-attach-cognitive-services.md) para invalidar la opción del recurso **Gratis**, que tiene un límite de unos pocos enriquecimientos al día.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ConditionalSkill


## <a name="evaluated-fields"></a>Campos evaluados

Esta habilidad es especial porque sus entradas son campos evaluados.

Los siguientes son los valores válidos de una expresión:

-   Las rutas de acceso de anotación (rutas de acceso en las expresiones deben delimitarse con "$(" and ")") <br/>
    Ejemplos:
    ```
        "= $(/document)"
        "= $(/document/content)"
    ```

-  Literales (cadenas, números, true, false, null) <br/>
    Ejemplos:
    ```
       "= 'this is a string'"   // string, note the single quotes
       "= 34"                   // number
       "= true"                 // boolean
       "= null"                 // null value
    ```

-  Las expresiones que usan un operador de comparación (==,! =, > =, >, < =, <) <br/>
    Ejemplos:
    ```
        "= $(/document/language) == 'en'"
        "= $(/document/sentiment) >= 0.5"
    ```

-   Las expresiones que usan los operadores booleanos (& &, ||,!, ^) <br/>
    Ejemplos:
    ```
        "= $(/document/language) == 'en' && $(/document/sentiment) > 0.5"
        "= !true"
    ```

-   Las expresiones que usan un operador numérico (+, -, \*, /, %) <br/>
    Ejemplos: 
    ```
        "= $(/document/sentiment) + 0.5"         // addition
        "= $(/document/totalValue) * 1.10"       // multiplication
        "= $(/document/lengthInMeters) / 0.3049" // division
    ```

Debido a la evaluación compatibles, la aptitud condicional puede utilizarse para escenarios de transformación secundaria. Vea el ejemplo [definición habilidades 4](#transformation-examples) para obtener un ejemplo.

## <a name="skill-inputs"></a>Entradas de la aptitud
Las entradas distinguen mayúsculas de minúsculas.

| Entradas      | DESCRIPCIÓN |
|-------------|-------------|
| condition   | Esta entrada es un [evalúa campo](#evaluated-fields) que representa la condición para evaluar. Esta condición se debe evaluar como un valor booleano (true o false).   <br/>  Ejemplos: <br/> "= true" <br/> "= $(/document/language) == 'fr'" <br/> "= $(/documentos/páginas/ \* /Language) == $(/ documentos/expectedLanguage)" <br/> |
| whenTrue    | Esta entrada es un [evalúa el campo](#evaluated-fields). Valor que se devuelve si la condición se evalúa en true. Las cadenas constantes se deben devolver en ' ' comillas. <br/>Valores de ejemplo: <br/> "="contrato""<br/>"= $(/document/contractType)" <br/> "= $(/documentos/entidades/\*)" <br/> |
| whenFalse   | Esta entrada es un [evalúa el campo](#evaluated-fields). Valor que se devuelve si la condición se evalúa como false.  <br/>Valores de ejemplo: <br/> "="contrato""<br/>"= $(/document/contractType)" <br/> "= $(/documentos/entidades/\*)" <br/>

## <a name="skill-outputs"></a>Salidas de la aptitud
Hay una única salida denominada 'salida'. Si la condición es true, devolverá el valor de whenFalse si la condición es false o whenTrue.

## <a name="examples"></a>Ejemplos

### <a name="sample-skill-definition-1-filtering-documents-to-return-only-french-documents"></a>Definición de la habilidad de ejemplo 1: Filtrado de documentos para devolver únicamente los documentos "Francés"

La salida siguiente devolverá una matriz de las oraciones ("/ documentos/frenchSentences") si el idioma del documento es francés. Si no, el idioma es francés, ese valor se establecerá en null.

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == 'fr'" },
        { "name": "whenTrue", "source": "/document/sentences" },
        { "name": "whenFalse", "source": "= null" }
    ],
    "outputs": [ { "name": "output", "targetName": "frenchSentences" } ]
}
```
Si se usa "/ documentos/frenchSentences" como el *contexto* de otra habilidad, solo se ejecutará dicha aptitud si "/ documentos/frenchSentences" no se establece en null


### <a name="sample-skill-definition-2-setting-a-default-value-when-it-does-not-exist"></a>Definición de la habilidad de ejemplo 2: Establecer un valor predeterminado cuando no existe.

La salida siguiente creará una anotación ("/ documentos/languageWithDefault") que se establece en el lenguaje del documento, o "es" Si no se establece el idioma.

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == null" },
        { "name": "whenTrue", "source": "= 'es'" },
        { "name": "whenFalse", "source": "= $(/document/language)" }
    ],
    "outputs": [ { "name": "output", "targetName": "languageWithDefault" } ]
}
```

### <a name="sample-skill-definition-3-merging-values-from-two-different-fields-into-a-single-field"></a>Definición de la habilidad de ejemplo 3: Combinar valores de dos campos distintos en un solo campo

En este ejemplo, tienen algunas oraciones un *frenchSentiment* propiedad. Cada vez que el *frenchSentiment* propiedad es null, nos gustaría usar el *englishSentiment* valor. Asignamos a la salida a un miembro denominado simplemente *opiniones* ("/ documentos/opiniones / * / opinión").

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document/sentences/*",
    "inputs": [
        { "name": "condition", "source": "= $(/document/sentences/*/frenchSentiment) == null" },
        { "name": "whenTrue", "source": "/document/sentences/*/englishSentiment" },
        { "name": "whenFalse", "source": "/document/sentences/*/frenchSentiment" }
    ],
    "outputs": [ { "name": "output", "targetName": "sentiment" } ]
}
```

## <a name="transformation-examples"></a>Ejemplos de transformación
### <a name="sample-skill-definition-4-performing-data-transformations-on-a-single-field"></a>Definición de la habilidad de ejemplo 4: Realización de transformaciones de datos en un solo campo

En este ejemplo, recibimos una opinión entre 0 y 1, y nos gustaría transformarlos para que esté comprendido entre -1 y 1. Se trata de una transformación matemática pequeño que podríamos hacer uso de la aptitud condicional.

En este ejemplo concreto, se usa nunca el aspecto de la aptitud condicional como la condición siempre es true. 

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document/sentences/*",
    "inputs": [
        { "name": "condition", "source": "= true" },
        { "name": "whenTrue", "source": "= $(/document/sentences/*/sentiment) * 2 - 1" },
        { "name": "whenFalse", "source": "= 0" }
    ],
    "outputs": [ { "name": "output", "targetName": "normalizedSentiment" } ]
}
```


## <a name="special-considerations"></a>Consideraciones especiales
Tenga en cuenta que algunos de los parámetros se evalúan, por lo que deberá ser especialmente cuidadoso siguiendo el patrón documentado. Las expresiones deben empezar por un igual signo "=" y las rutas de acceso deben estar delimitados por "$(" and ")". Asegúrese de que poner las cadenas de comillas' ' le ayudará a del evaluador de expresiones distingue entre cadenas y rutas de acceso y los operadores. Además, asegúrese de colocar un espacio en blanco alrededor de operadores (por ejemplo un * en una ruta de acceso tiene un significado distinto que el operador de multiplicación).


## <a name="next-steps"></a>Pasos siguientes

+ [Aptitudes predefinidas](cognitive-search-predefined-skills.md)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
