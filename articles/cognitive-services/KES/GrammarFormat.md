---
title: Formato de gramática de Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: Más información sobre el formato de gramática en Knowledge Exploration Service (KES) API.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 844bd9a88c52fd398fc66c71e59da513c0d7d90d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60814864"
---
# <a name="grammar-format"></a>Formato de gramática

La gramática es un archivo XML que especifica el conjunto ponderado de consultas en lenguaje natural que puede interpretar el servicio, así como la manera en que estas consultas en lenguaje natural se convierten en expresiones de consulta semántica.  La sintaxis de gramática se basa en [SRGS](https://www.w3.org/TR/speech-grammar/), un estándar de W3C para las gramáticas de reconocimiento de voz, con extensiones para admitir la integración de índices de datos y funciones semánticas.

A continuación se describen los elementos sintácticos que pueden utilizarse en una gramática.  Consulte [este ejemplo](#example) para obtener una gramática completa que muestra el uso de estos elementos en contexto.

### <a name="grammar-element"></a>Elemento grammar

El elemento `grammar` es el elemento de nivel superior en la especificación de gramática XML.  El atributo `root` obligatorio especifica el nombre de la regla raíz que define el punto de partida de la gramática.

```xml
<grammar root="GetPapers">
```

### <a name="import-element"></a>Elemento import

El elemento `import` importa una definición de esquema desde un archivo externo para habilitar las referencias de atributo. El elemento debe ser un elemento secundario del elemento `grammar` de nivel superior y aparece antes que cualquier otro elemento `attrref`. El atributo `schema` obligatorio especifica el nombre de un archivo de esquema que se encuentra en el mismo directorio que el archivo XML de gramática. El elemento `name` obligatorio especifica el alias del esquema que los elementos `attrref` posteriores utilizan para hacer referencia a los atributos definidos en este esquema.

```xml
  <import schema="academic.schema" name="academic"/>
```

### <a name="rule-element"></a>Elemento rule

El elemento `rule` define una regla de gramática, una unidad estructural que especifica un conjunto de expresiones de consulta que el sistema puede interpretar.  El elemento debe ser un elemento secundario del elemento de nivel superior `grammar`.  El atributo `id` obligatorio especifica el nombre de la regla, a la que se hace referencia desde los elementos `grammar` o `ruleref`.

Un elemento `rule` define un conjunto de expansiones legales.  Los tokens de texto se corresponden directamente con la consulta de entrada.  Los elementos `item` especifican las repeticiones y modifican las probabilidades de interpretación.  Los elementos `one-of` indican las opciones alternativas.  Los elementos `ruleref` permiten la construcción de expansiones más complejas a partir otras más sencillas.  Los elementos `attrref` permiten establecer coincidencias con los valores de atributo desde el índice.  Los elementos `tag` especifican la semántica de la interpretación y pueden modificar la probabilidad de interpretación.

```xml
<rule id="GetPapers">...</rule>
```

### <a name="example-element"></a>Elemento example

El elemento `example` opcional especifica frases de ejemplo que pueden ser aceptadas por la definición `rule` contenedora.  Esto puede utilizarse para documentación y pruebas automatizadas.

```xml
<example>papers about machine learning by michael jordan</example>
```

### <a name="item-element"></a>Elemento item

El elemento `item` agrupa una secuencia de construcciones gramaticales.  Se puede utilizar para indicar las repeticiones de la secuencia de expansión, o para especificar alternativas junto con el elemento `one-of`.

Cuando un elemento `item` no es un elemento secundario de `one-of`, la repetición de la secuencia entre comillas se especifica asignando al atributo `repeat` un valor de recuento.  Un valor de recuento "*n*" (donde *n* es un entero) indica que la secuencia debe ocurrir exactamente *n* veces.  Un valor de recuento "*m*-*n*" permite que la secuencia aparezca entre *m* y *n* veces, ambos valores incluidos.  Un valor de recuento "*m*-" especifica que la secuencia debe aparecer al menos *m* veces.  El elemento `repeat-logprob` opcional se puede usar para modificar la probabilidad de interpretación de cada repetición adicional más allá del mínimo.

```xml
<item repeat="1-" repeat-logprob="-10">...</item>
```

Cuando los elementos `item` aparecen como elementos secundarios de `one-of`, definen el conjunto de alternativas de expansión.  En este uso, el atributo `logprob` opcional especifica la probabilidad logarítmica relativa entre las distintas opciones.  Dada una probabilidad *p* entre 0 y 1, la probabilidad logarítmica correspondiente se puede calcular como log(*p*), donde log() es la función logarítmica natural.  Si no se especifica, el valor predeterminado de `logprob` es 0, que no modifica la probabilidad de interpretación.  Tenga en cuenta que la probabilidad logarítmica siempre es un valor negativo de punto flotante o 0.

```xml
<one-of>
  <item>by</item>
  <item logprob="-0.5">written by</item>
  <item logprob="-1">authored by</item>
</one-of>
```

### <a name="one-of-element"></a>Elemento one-of

El elemento `one-of` especifica expansiones alternativas entre uno de los elementos `item` secundarios.  Solo pueden aparecer elementos `item` dentro de un elemento `one-of`.  Las probabilidades relativas entre las distintas opciones se pueden especificar mediante el atributo `logprob` en cada elemento secundario `item`.

```xml
<one-of>
  <item>by</item>
  <item logprob="-0.5">written by</item>
  <item logprob="-1">authored by</item>
</one-of>
```

### <a name="ruleref-element"></a>Elemento ruleref

El elemento `ruleref` especifica expansiones válidas mediante referencias a otro elemento `rule`.  El uso de elementos `ruleref` permite crear expresiones más complejas a partir de reglas más sencillas.  El atributo `uri` obligatorio indica el nombre del `rule` al que se hacer referencia, utilizando la sintaxis "#*rulename*".  Para capturar la salida semántica de la regla a la que se hace referencia, utilice el atributo `name` opcional para especificar el nombre de una variable a la que se asigna la salida semántica.
 
```xml
<ruleref uri="#GetPaperYear" name="year"/>
```

### <a name="attrref-element"></a>Elemento attrref

El elemento `attrref` hace referencia a un atributo de índice, lo que permite establecer coincidencias con los valores de atributo observados en el índice.  El atributo `uri` obligatorio especifica el nombre del esquema de índice y el nombre de atributo, utilizando la sintaxis "*schemaName*#*attrName*".  Debe haber un elemento `import` anterior que importa el esquema llamado *schemaName*.  El nombre del atributo es el nombre de un atributo definido en el esquema correspondiente.

Además de coincidir con los datos de entrada del usuario, el elemento `attrref` también devuelve un objeto de consulta estructurada como resultado, que selecciona el subconjunto de objetos del índice que coinciden con el valor de entrada.  Use el atributo `name` opcional para especificar el nombre de la variable donde se debe almacenar la salida del objeto de consulta.  El objeto de consulta puede estar compuesto por otros objetos de consulta para formar expresiones más complejas.  Consulte [Interpretación semántica](SemanticInterpretation.md) para obtener más información.  

```xml
<attrref uri="academic#Keyword" name="keyword"/>
```

#### <a name="query-completion"></a>Finalización de consultas

Para admitir la finalización de consultas al interpretar consultas de usuario parciales, cada atributo al que se hace referencia debe incluir "starts_with" como operación en la definición del esquema.  Dado un prefijo de consulta de usuario, `attrref` establecerá coincidencias con todos los valores del índice que se completen el prefijo y producirá cada valor completo como una interpretación distinta de la gramática.  

Ejemplos:
* El establecimiento de coincidencias de `<attrref uri="academic#Keyword" name="keyword"/>` con el prefijo de consulta "dat" genera una interpretación para artículos acerca de "base de datos", una interpretación para artículos sobre "minería de datos", etc.
* El establecimiento de coincidencias de `<attrref uri="academic#Year" name="year"/>` con el prefijo de consulta "200" genera una interpretación para artículos acerca de "2000", una interpretación para artículos sobre "2001", etc.

#### <a name="matching-operations"></a>Operaciones de establecimiento de coincidencias

Además de las coincidencias exactas, algunos tipos de atributo también permiten establecer coincidencias de prefijo y desigualdad con el atributo `op` opcional.  Si ningún objeto del índice tiene un valor que coincida, se bloquea la ruta de acceso de la gramática y el servicio no generará ninguna interpretación en la ruta de acceso de esta gramática.   El valor predeterminado del atributo `op` es "eq".

```xml
in <attrref uri="academic#Year" name="year"/>
before <attrref uri="academic#Year" op="lt" name="year"/
```

En la tabla siguiente se enumeran los valores `op` admitidos para cada tipo de atributo.  Su uso requiere que se incluya operación index correspondiente en la definición de atributo del esquema.

| Tipo de atributo | Valor de operación | DESCRIPCIÓN | Operación index
|----|----|----|----|
| Cadena | eq | Coincidencia exacta de cadena | equals |
| Cadena | starts_with | Coincidencia de prefijo de cadena | starts_with |
| Int32, Int64, doble | eq |  Coincidencia de igualdad numérica | equals |
| Int32, Int64, doble | lt, le, gt, ge | Coincidencia de desigualdad numérica (<, <=, >, >=) | is_between |
| Int32, Int64, doble | starts_with | Coincidencia de prefijo del valor en notación decimal | starts_with |

Ejemplos:
* `<attrref uri="academic#Year" op="lt" name="year"/>` establece coincidencias con la cadena de entrada "2000" y devuelve todos los artículos publicados antes del año 2000, sin incluir este.
* `<attrref uri="academic#Year" op="lt" name="year"/>` no establece coincidencias con la cadena de entrada "20" porque no hay ningún artículo en el índice publicado antes del año 20.
* `<attrref uri="academic#Keyword" op="starts_with" name="keyword"/>` establece coincidencias con la cadena de entrada "dat" y devuelve artículos con una única interpretación sobre "base de datos", "minería de datos", etc.  Se trata de un caso de uso poco frecuente.
* `<attrref uri="academic#Year" op="starts_with" name="year"/>` establece coincidencias con la cadena de entrada "20" y devuelve artículos con una única interpretación publicados en 200-299, 2000 a 2999, etc.  Se trata de un caso de uso poco frecuente.

### <a name="tag-element"></a>Elemento tag

El elemento `tag` especifica cómo se interpretará una ruta de acceso en la gramática.  Contiene una secuencia de instrucciones que terminan con punto y coma.  Una instrucción puede ser una asignación de un literal o una variable a otra variable.  También puede asignar la salida de una función con 0 o más parámetros a una variable.  Se puede especificar cada parámetro de la función mediante un literal o una variable.  Si la función no devuelve ninguna salida, se omite la asignación.  El ámbito de la variable es local para la regla contenedora.

```xml
<tag>x = 1; y = x;</tag>
<tag>q = All(); q = And(q, q2);</tag>
<tag>AssertEquals(x, 1);</tag>
```

Cada `rule` en la gramática tiene una variable predefinida llamada "out", que representa la salida semántica de la regla.  Para calcular su valor, se evalúa cada una de las instrucciones semánticas atravesadas por la ruta mediante los elementos `rule` que coincidan con la entrada de la consulta del usuario.  El valor asignado a la variable "out" al final de la evaluación es la salida semántica de la regla.  La salida semántica de la interpretación de una consulta de usuario con relación a la gramática es la salida semántica de la regla raíz.

Algunas instrucciones pueden modificar la probabilidad de una ruta de interpretación porque agregan una compensación de la probabilidad logarítmica.  Algunas instrucciones pueden rechazar la interpretación por completo si no se cumplen las condiciones especificadas.

Para obtener una lista de las funciones semánticas admitidas, consulte [Funciones semántica](SemanticInterpretation.md#semantic-functions).

## <a name="interpretation-probability"></a>Probabilidad de interpretación

La probabilidad de una ruta de interpretación a través de la gramática es la probabilidad logarítmica acumulada de todos los elementos `<item>` y las funciones semánticas encontradas en el camino.  Describe la probabilidad relativa de que una secuencia de entrada determinada coincida.

Dada una probabilidad *p* entre 0 y 1, la probabilidad logarítmica correspondiente se puede calcular como log(*p*), donde log() es la función logarítmica natural.  El uso de probabilidades logarítmicas permite al sistema acumular la probabilidad conjunta de una ruta de interpretación mediante una sencilla suma.  También evita el subdesbordamiento de punto flotante común a estos cálculos de probabilidad conjunta.  Tenga en cuenta que, por su naturaleza, la probabilidad logarítmica es siempre un valor de punto flotante negativo o 0, donde los valores mayores indican mayor probabilidad.

## <a name="example"></a>Ejemplo

El siguiente es un ejemplo XML del dominio de publicaciones académicas que muestra los distintos elementos de una gramática:

```xml
<grammar root="GetPapers">

  <!-- Import academic data schema-->
  <import schema="academic.schema" name="academic"/>
  
  <!-- Define root rule-->
  <rule id="GetPapers">
    <example>papers about machine learning by michael jordan</example>
    
    papers
    <tag>
      yearOnce = false;
      isBeyondEndOfQuery = false;
      query = All();
    </tag>
  
    <item repeat="1-" repeat-logprob="-10">
      <!-- Do not complete additional attributes beyond end of query -->
      <tag>AssertEquals(isBeyondEndOfQuery, false);</tag>
        
      <one-of>
        <!-- about <keyword> -->
        <item logprob="-0.5">
          about <attrref uri="academic#Keyword" name="keyword"/>
          <tag>query = And(query, keyword);</tag>
        </item>
        
        <!-- by <authorName> [while at <authorAffiliation>] -->
        <item logprob="-1">
          by <attrref uri="academic#Author.Name" name="authorName"/>
          <tag>authorQuery = authorName;</tag>
          <item repeat="0-1" repeat-logprob="-1.5">
            while at <attrref uri="academic#Author.Affiliation" name="authorAffiliation"/>
            <tag>authorQuery = And(authorQuery, authorAffiliation);</tag>
          </item>
          <tag>
            authorQuery = Composite(authorQuery);
            query = And(query, authorQuery);
          </tag>
        </item>
        
        <!-- written (in|before|after) <year> -->
        <item logprob="-1.5">
          <!-- Allow this grammar path to be traversed only once -->
          <tag>
            AssertEquals(yearOnce, false);
            yearOnce = true;
          </tag>
          <ruleref uri="#GetPaperYear" name="year"/>
          <tag>query = And(query, year);</tag>
        </item>
      </one-of>

      <!-- Determine if current parse position is beyond end of query -->
      <tag>isBeyondEndOfQuery = GetVariable("IsBeyondEndOfQuery", "system");</tag>
    </item>
    <tag>out = query;</tag>
  </rule>
  
  <rule id="GetPaperYear">
    <tag>year = All();</tag>
    written
    <one-of>
      <item>
        in <attrref uri="academic#Year" name="year"/>
      </item>
      <item>
        before
        <one-of>
          <item>[year]</item>
          <item><attrref uri="academic#Year" op="lt" name="year"/></item>
        </one-of>
      </item>
      <item>
        after
        <one-of>
          <item>[year]</item>
          <item><attrref uri="academic#Year" op="gt" name="year"/></item>
        </one-of>
      </item>
    </one-of>
    <tag>out = year;</tag>
  </rule>
</grammar>
```
