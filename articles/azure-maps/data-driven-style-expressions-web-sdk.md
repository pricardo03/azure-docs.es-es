---
title: Controladas por datos expresiones de estilo en el SDK de Web de Azure Maps | Microsoft Docs
description: Cómo usar expresiones de estilo controladas por datos en el SDK de Web de Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 4/4/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.custom: codepen
ms.openlocfilehash: 0f63ed7d00f1ae3e30cdac76606559a4e9f49f04
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288102"
---
# <a name="data-driven-style-expressions-web-sdk"></a>Expresiones de estilo controlada por datos (Web SDK)

Las expresiones permiten aplicar la lógica de negocios a las opciones de estilo que observar las propiedades definidas en cada forma de un origen de datos. También se pueden usar expresiones para filtrar los datos de un origen de datos o una capa. Las expresiones pueden constar de lógica condicional, como instrucciones if y también pueden usarse para manipular los datos con; cadena, operadores lógicos y matemáticos. 

Estilos de datos pueden reducir la cantidad de código necesario para implementar la lógica de negocios en torno a la aplicación de estilos. Cuando se usa con capas, las expresiones se evalúan en tiempo de procesamiento en un subproceso independiente que proporciona un mayor rendimiento en comparación con la evaluación de la lógica de negocios en el subproceso de interfaz de usuario.

El vídeo siguiente proporciona información general de estilo controladas por datos en el SDK de Web de Azure Maps.

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Data-Driven-Styling-with-Azure-Maps/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

Las expresiones se representan como matrices JSON. El primer elemento de una expresión de la matriz es una cadena que especifica el nombre del operador de expresión. Por ejemplo, "+" o "case". Los elementos siguientes (si existe) son los argumentos para la expresión. Cada argumento es un valor literal (cadena, número, booleano, o `null`), o una matriz de otra expresión. El pseudocódigo siguiente define la estructura básica de una expresión. 

```javascript
[ 
    expression_operator, 
    argument0, 
    argument1, 
    …
] 
```

El SDK de Azure Maps Web admite muchos tipos de expresiones que pueden utilizarse por sí solas o en combinación con otras expresiones.

| Tipo de expresiones | DESCRIPCIÓN |
|---------------------|-------------|
| [Expresiones booleanas](#boolean-expressions) | Expresiones booleanas proporcionan un conjunto de expresiones de operadores booleanos para evaluar las comparaciones de tipo boolean. |
| [Expresiones de color](#color-expressions) | Expresiones de color facilitan crear y manipular valores de color. |
| [Expresiones condicionales](#conditional-expressions) | Expresiones condicionales proporcionan operaciones lógicas que son similares a las instrucciones if. |
| [Expresiones de datos](#data-expressions) | Proporciona acceso a los datos de propiedad en una característica. |
| [Interpolar y expresiones de paso](#interpolate-and-step-expressions) | Interpolar y se pueden usar expresiones de paso para calcular valores a lo largo de una función de paso o curva interpolada. |
| [Expresiones específicas de capa](#layer-specific-expressions) | Expresiones especiales que solo son aplicables a una sola capa. |
| [Expresiones matemáticas](#math-expressions) | Proporciona operadores matemáticos para llevar a cabo cálculos controladas por datos dentro del marco de la expresión. |
| [Expresiones de cadena de operador](#string-operator-expressions) | Expresiones de cadena de operador realizan operaciones de conversión en cadenas como concatenación y convertir las mayúsculas y minúsculas. |
| [Expresiones de tipo](#type-expressions) | Expresiones de tipo proporcionan herramientas para pruebas y la conversión de tipos de datos diferentes, como cadenas, números y valores booleanos. |
| [Expresiones de enlace de variables](#variable-binding-expressions) | Expresiones de enlace de variables permiten que los resultados de un cálculo se almacena en una variable y se hace referencia en otro lugar en una expresión varias veces sin tener que volver a calcular el valor almacenado. |
| [Expresión de zoom](#zoom-expression) | Recupera el nivel de zoom actual de la asignación en tiempo de procesamiento. |

Todos los ejemplos de este documento usará la siguiente función para mostrar diferentes maneras en que se pueden usar los diferentes tipos de expresiones. 

```javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.13284, 47.63699]
    },
    "properties": {     
        "entityType": "restaurant",
        "revenue": 12345,
        "subTitle": "Building 40", 
        "temperature": 72,
        "title": "Cafeteria", 
        "zoneColor": "red"
    }
}
```

## <a name="data-expressions"></a>Expresiones de datos

Las expresiones de datos proporcionan acceso a los datos de propiedad en una característica. 

| Expression | Tipo de valor devuelto | DESCRIPCIÓN |
|------------|-------------|-------------|
| `['at', number, array]` | objeto | Recupera un elemento de matriz. |
| `['geometry-type']` | string | Obtiene el tipo de geometría de la característica: Punto, MultiPoint, MultiLineString, LineString, Polygon, MultiPolygon. |
| `['get', string]` | value | Obtiene el valor de propiedad de las propiedades de la característica actual. Devuelve null si no se encuentra la propiedad solicitada. |
| `['get', string, object]` | value | Obtiene el valor de propiedad de las propiedades del objeto proporcionado. Devuelve null si no se encuentra la propiedad solicitada. |
| `['has', string]` | boolean | Determina si las propiedades de una característica tienen la propiedad especificada. |
| `['has', string, object]` | boolean | Determina si las propiedades del objeto tienen la propiedad especificada. |
| `['id']` | value | Obtiene el identificador de la característica si lo tiene. |
| `['length', string | array]` | número | Obtiene la longitud de una cadena o matriz. |

**Ejemplos**

Pueden tener acceso a propiedades de una característica directamente en una expresión mediante el uso de un `get` expresión. El ejemplo siguiente usa el valor de "zoneColor" de la característica para especificar la propiedad de color de una capa de burbuja. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: ['get', 'zoneColor'] //Get the zoneColor value.
});
```

El ejemplo anterior funcionará bien si todas las características de punto tienen el `zoneColor` propiedad, pero si no es así, el color probablemente recurrirá a "black". Para modificar el color de la reserva un `case` expresión puede utilizarse en combinación con el `has` expresión para comprobar si existe la propiedad, y si no se devuelve un color de reserva en su lugar.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'case', //Use a conditional case expression.

        ['has', 'zoneColor'],   //Check to see if feature has a "zoneColor" property
        ['get', 'zoneColor'],   //If it does, use it.

        'blue'  //If it doesn't, default to blue.
    ]
});
```

Capas de burbujas y símbolos representarán las coordenadas de todas las formas en un origen de datos de forma predeterminada. Esto puede hacerse para resaltar los vértices de una línea o polígono. El `filter` opción de la capa puede utilizarse para limitar el tipo de geometría de las características que representa mediante un `['geometry-type']` expresión dentro de una expresión booleana. El siguiente ejemplo limita para que solo una capa de burbuja `Point` se representan características.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    filter: ['==', ['geometry-type'], 'Point']
});
```

En el siguiente ejemplo permitirá ambos `Point` y `MultiPoint` características va a representar. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    filter: ['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]
});
```

De forma similar, se representará el contorno de polígonos en las capas de la línea. Para deshabilitar este comportamiento en una capa de línea, agregue un filtro que solo permite `LineString` y `MultiLineString` características.  

## <a name="math-expressions"></a>Expresiones matemáticas

Expresiones matemáticas proporcionan operadores matemáticos para llevar a cabo cálculos controladas por datos dentro del marco de la expresión.

| Expression | Tipo de valor devuelto | DESCRIPCIÓN |
|------------|-------------|-------------|
| `['+', number, number, …]` | número | Calcula la suma de los números especificados. |
| `['-', number]` | número | Resta 0 el número especificado. |
| `['-', number, number]` | número | Resta las primeras cifras por el segundo número. |
| `['*', number, number, …]` | número | Multiplica a los números especificados juntos. |
| `['/', number, number]` | número | Divide el primer número por el segundo número. |
| `['%', number, number]` | número | Calcula el resto al dividir el primer número por el segundo número. |
| `['^', number, number]` | número | Calcula el valor del primer valor elevado a la potencia del segundo número. |
| `['abs', number]` | número | Calcula el valor absoluto del número especificado. |
| `['acos', number]` | número | Calcula el arco coseno del número especificado. |
| `['asin', number]` | número | Calcula el arco seno del número especificado. |
| `['atan', number]` | número | Calcula el arco tangente del número especificado. |
| `['ceil', number]` | número | Redondea el número hasta el siguiente número entero. |
| `['cos', number]` | número | Calcula el coseno del número especificado. |
| `['e']` | número | Devuelve la constante matemática `e`. |
| `['floor', number]` | número | Redondea el número hacia abajo hasta el número entero anterior. |
| `['ln', number]` | número | Calcula el logaritmo natural del número especificado. |
| `['ln2']` | número | Devuelve la constante matemática `ln(2)`. |
| `['log10', number]` | número | Calcula el logaritmo en base 10 del número especificado. |
| `['log2', number]` | número | Calcula el logaritmo en base dos del número especificado. |
| `['max', number, number, …]` | número | Calcula el número máximo en el conjunto especificado de números. |
| `['min', number, number, …]` | número | Calcula el número mínimo del conjunto especificado de números. |
| `['pi']` | número | Devuelve la constante matemática `PI`. |
| `['round', number]` | número | Redondea el número al entero más cercano. Los valores a medio camino se redondean alejándose de cero. Por ejemplo, `['round', -1.5]` se evalúa como -2. |
| `['sin', number]` | número | Calcula el seno del número especificado. |
| `['sqrt', number]` | número | Calcula la raíz cuadrada del número especificado. |
| `['tan', number]` | número | Calcula la tangente del número especificado. |
## <a name="boolean-expressions"></a>Expresiones booleanas

Expresiones booleanas proporcionan un conjunto de expresiones de operadores booleanos para evaluar las comparaciones de tipo boolean.

Cuando se comparan valores, se deben escribirse de la comparación. Los valores de distintos tipos siempre se consideran distintos. Los casos donde se conocen los tipos sea diferente en tiempo de análisis se consideran no válidos y producen un error de análisis. 

| Expression | Tipo de valor devuelto | DESCRIPCIÓN |
|------------|-------------|-------------|
| `['! ', boolean]` | boolean | Negación lógica. Devuelve `true` si la entrada es `false`, y `false` si la entrada es `true`. |
| `['!= ', value, value]` | boolean | Devuelve `true` si los valores de entrada no son iguales, `false` en caso contrario. |
| `['<', value, value]` | boolean | Devuelve `true` si la primera entrada es estrictamente menor que el segundo, `false` en caso contrario. Los argumentos son necesarios para ser cadenas o ambos números. |
| `['<=', value, value]` | boolean | Devuelve `true` si la primera entrada es menor o igual que el segundo, `false` en caso contrario. Los argumentos son necesarios para ser cadenas o ambos números. |
| `['==', value, value]` | boolean | Devuelve `true` si los valores de entrada son iguales, `false` en caso contrario. Los argumentos son necesarios para ser cadenas o ambos números. |
| `['>', value, value]` | boolean | Devuelve `true` si la primera entrada es estrictamente mayor que el segundo, `false` en caso contrario. Los argumentos son necesarios para ser cadenas o ambos números. |
| `['>=' value, value]` | boolean | Devuelve `true` si la primera entrada es mayor o igual que el segundo, `false` en caso contrario. Los argumentos son necesarios para ser cadenas o ambos números. |
| `['all', boolean, boolean, …]` | boolean | Devuelve `true` si todas las entradas son `true`, `false` en caso contrario. |
| `['any', boolean, boolean, …]` | boolean | Devuelve `true` si cualquiera de las entradas son `true`, `false` en caso contrario. |

## <a name="conditional-expressions"></a>Expresiones condicionales

Expresiones condicionales proporcionan operaciones lógicas que son similares a las instrucciones if.

Las expresiones siguientes realizan operaciones de lógica condicional en los datos de entrada. Por ejemplo, el `case` expresión proporciona la lógica "if/then/else" mientras el `match` expresión es como una "instrucción de modificador". 

### <a name="case-expression"></a>Expresión Case

Un `case` expresión es un tipo de expresión condicional que proporciona la instrucción if tales como lógica (if/then/else). Este tipo de expresión le ayudará a obtener una lista de condiciones booleanas y devuelve el valor de salida de la primera condición booleano que es true.

El pseudocódigo siguiente define la estructura de la `case` expresión. 

```javascript
[
    'case',
    condition1: boolean, 
    output1: value,
    condition2: boolean, 
    output2: value,
    ...,
    fallback: value
]
```

**Ejemplo**

El ejemplo siguiente los pasos a través de diferentes condiciones booleanas hasta encontrar uno que se evalúa como `true`y, a continuación, devuelve dicho valor asociado. Si ninguna condición booleana se evalúa como `true`, se devolverá un valor de reserva. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'case',

        //Check to see if the first boolean expression is true, and if it is, return its assigned result.
        ['has', 'zoneColor'],
        ['get', 'zoneColor'],

        //Check to see if the second boolean expression is true, and if it is, return its assigned result.
        ['all', ['has', ' temperature '], ['>', ['get', 'temperature'], 100]],
        'red',

        //Specify a default value to return.
        'green'
    ]
});
```

### <a name="match-expression"></a>Expresión de coincidencia

Un `match` expresión es un tipo de expresión condicional que proporciona la instrucción switch tales como lógica. La entrada puede ser cualquier expresión como `['get', 'entityType']` que devuelve una cadena o un número. Cada etiqueta debe ser un único valor literal o una matriz de valores literales, cuyos valores deben ser todas las cadenas o todos los números. Coincide con la entrada si alguno de los valores de la coincidencia de la matriz. Cada etiqueta debe ser única. Si el tipo de entrada no coincide con el tipo de las etiquetas, el resultado será el valor de reserva.

El pseudocódigo siguiente define la estructura de la `match` expresión. 

```javascript
[
    'match',
    input: number | string,
    label1: number | string | (number | string)[], 
    output1: value,
    label2: number | string | (number | string)[], 
    output2: value,
    ...,
    fallback: value
]
```

**Ejemplos**

El ejemplo siguiente se examina el `entityType` propiedad de una característica de punto en una capa de burbuja busca una coincidencia. Si encuentra a una coincidencia, que especifica el valor se devuelve o no devuelve el valor de reserva.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'match',

        //Get the property to match.
        ['get', 'entityType'],

        //List the values to match and the result to return for each match.
        'restaurant', 'red',
        'park', 'green',

        //Specify a default value to return if no match is found.
        'black'
    ]
});
```

El ejemplo siguiente usa una matriz para mostrar un conjunto de etiquetas que se deben devolver el mismo valor. Esto es mucho más eficiente que enumerar cada etiqueta individualmente. En este caso, si el `entityType` propiedad es "restaurante" o "grocery_store", se devolverá el color "rojo".

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'match',

        //Get the property to match.
        ['get', 'entityType'],

        //List the values to match and the result to return for each match.
        ['restaurant', 'grocery_store'], 'red',

        'park', 'green',

        //Specify a default value to return if no match is found.
        'black'
    ]
});
```

### <a name="coalesce-expression"></a>Expresión de fusión

Un `coalesce` expresión pasos a través de un conjunto de expresiones hasta que se obtiene el primer valor distinto de null y devuelve ese valor. 

El pseudocódigo siguiente define la estructura de la ` coalesce` expresión. 

```javascript
[
    'coalesce', 
    value1, 
    value2, 
    …
]
```

**Ejemplo**

En el ejemplo siguiente se usa un `coalesce` expresión para establecer el `textField` opción de una capa de símbolos. Si el `title` falta la propiedad de la característica o conjunto a `null`, la expresión, a continuación, intentará buscar el `subtitle` propiedad, si su falta o `null`, a continuación, volverá a una cadena vacía. 

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: [
            'coalesce',

            //Try getting the title property.
            ['get', 'title'],

            //If there is no title, try getting the subtitle. 
            ['get', 'subtitle'],

            //Default to an empty string.
            ''
        ]
    }
});
```

## <a name="type-expressions"></a>Expresiones de tipo

Expresiones de tipo proporcionan herramientas para pruebas y la conversión de tipos de datos diferentes, como cadenas, números y valores booleanos.

| Expression | Tipo de valor devuelto | DESCRIPCIÓN |
|------------|-------------|-------------|
| `['literal', array]`<br/><br/>`['literal', object]` | matriz \| objeto | Devuelve un valor literal de matriz u objeto. Use esta expresión para evitar que una matriz o un objeto que se evalúa como una expresión. Esto es necesario cuando un objeto o matriz debe ser emitido por una expresión. |
| `['to-boolean', value]` | boolean | Convierte el valor de entrada en un valor booleano. El resultado es `false` cuando la entrada es una cadena vacía, `0`, `false`, `null`, o `NaN`; en caso contrario, su `true`. |
| `['to-color', value]`<br/><br/>`['to-color', value1, value2…]` | color | Convierte el valor de entrada en un color. Si se proporcionan varios valores, cada uno de ellos se evalúa en orden hasta que se obtiene la primera conversión correcta. Si ninguna de las entradas se pueden convertir, la expresión es un error. |
| `['to-number', value]`<br/><br/>`['to-number', value1, value2, …]` | número | Convierte el valor de entrada en un número, si es posible. Si la entrada es `null` o `false`, el resultado es 0. Si la entrada es `true`, el resultado es 1. Si la entrada es una cadena, se convierte en un número mediante la [ToNumber](https://tc39.github.io/ecma262/#sec-tonumber-applied-to-the-string-type) cadena de función de la especificación del lenguaje ECMAScript. Si se proporcionan varios valores, cada uno de ellos se evalúa en orden hasta que se obtiene la primera conversión correcta. Si ninguna de las entradas se pueden convertir, la expresión es un error. |
| `['to-string', value]` | string | Convierte el valor de entrada en una cadena. Si la entrada es `null`, el resultado es `""`. Si la entrada es un valor booleano, el resultado es `"true"` o `"false"`. Si la entrada es un número, se convierte en una cadena mediante la [ToString](https://tc39.github.io/ecma262/#sec-tostring-applied-to-the-number-type) número en función de la especificación del lenguaje ECMAScript. Si la entrada es un color, se convierte en cadena de color CSS RGBA `"rgba(r,g,b,a)"`. En caso contrario, la entrada se convierte en una cadena mediante la [JSON.stringify](https://tc39.github.io/ecma262/#sec-json.stringify) función de la especificación del lenguaje ECMAScript. |
| `['typeof', value]` | string | Devuelve una cadena que describe el tipo del valor especificado. |

> [!TIP]
> Si un mensaje de error similar al `Expression name must be a string, but found number instead. If you wanted a literal array, use ["literal", [...]].` aparece en la consola del explorador significa que hay una expresión en algún lugar en el código que tiene una matriz que no tiene una cadena para su primer valor. Si desea que la expresión para devolver una matriz, ajuste la matriz con el `literal` expresión. En el ejemplo siguiente se establece el icono de `offset` opción de una capa de símbolos, que debe ser una matriz que contiene dos números, mediante el uso de un `match` expresión para elegir entre dos valores de desplazamiento en función del valor de la `entityType` propiedad del punto característica.
>
> ```javascript
> var layer = new atlas.layer.SymbolLayer(datasource, null, {
>     iconOptions: {
>         offset: [
>             'match',
>
>             //Get the entityType value.
>             ['get', 'entityType'],
>
>             //If there is no title, try getting the subtitle. 
>             'restaurant', ['literal', [0, -10]],
>
>             //Default to value.
>             ['literal', [0, 0]]
>         ]
>     }
> });
> ```

## <a name="color-expressions"></a>Expresiones de color

Expresiones de color facilitan crear y manipular valores de color.

| Expression | Tipo de valor devuelto | DESCRIPCIÓN |
|------------|-------------|-------------|
| `['rgb', number, number, number]` | color | Crea un valor de color de *rojo*, *verde*, y *azul* componentes que deben oscilar entre `0` y `255`y un componente alfa de `1`. Si alguno de los componentes está fuera del intervalo, la expresión es un error. |
| `['rgba', number, number, number, number]` | color | Crea un valor de color de *rojo*, *verde*, *azul* componentes que deben oscilar entre `0` y `255`y un componente alfa dentro de un intervalo de `0` y `1`. Si alguno de los componentes está fuera del intervalo, la expresión es un error. |
| `['to-rgba']` | \[number, number, number, number\] | Devuelve una matriz de cuatro elementos que contiene el color de entrada *rojo*, *verde*, *azul*, y *alfa* componentes, en ese orden. |

**Ejemplo**

En el ejemplo siguiente se crea y el valor de color RGB que tiene un *rojo* valor `255`, y *verde* y *azul* valores que se calculan multiplicando `2.5` por el valor de la `temperature` propiedad. Como los cambios de temperatura se cambia el color a diferentes sombras de *rojo*.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'rgb', //Create a RGB color value.

        255,    //Set red value to 255.

        ['*', 2.5, ['get', 'temperature']], //Multiple the temperature by 2.5 and set the green value.

        ['*', 2.5, ['get', 'temperature']]  //Multiple the temperature by 2.5 and set the blue value.
    ]
});
```

## <a name="string-operator-expressions"></a>Expresiones de cadena de operador

Expresiones de cadena de operador realizan operaciones de conversión en cadenas como concatenación y convertir las mayúsculas y minúsculas. 

| Expression | Tipo de valor devuelto | DESCRIPCIÓN |
|------------|-------------|-------------|
| `['concat', string, string, …]` | string | Concatena varias cadenas. Cada valor debe ser una cadena. Use el `to-string` escriba la expresión para convertir otros tipos de valor en cadena, si es necesario. |
| `['downcase', string]` | string | Convierte la cadena especificada a minúsculas. |
| `['upcase', string]` | string | Convierte la cadena especificada a mayúsculas. |

**Ejemplo**

El ejemplo siguiente convierte la `temperature` característica en una cadena de propiedad del punto y, a continuación, concatena "° F" al final de la misma.

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: ['concat', ['to-string', ['get', 'temperature']], '°F'],

        //Some additional style options.
        offset: [0, -1.5],
        size: 12,
        color: 'white'
    }
});
```

La expresión anterior representa un pin en el mapa con el texto "64° F" superpuesta sobre él tal como se muestra en la imagen siguiente.

<center>

![Ejemplo de cadena de expresión de operador](media/how-to-expressions/string-operator-expression.png) </center>

## <a name="interpolate-and-step-expressions"></a>Interpolar y expresiones de paso

Interpolar y se pueden usar expresiones de paso para calcular valores a lo largo de una función de paso o curva interpolada. Tome estas expresiones en una expresión que devuelve un valor numérico como su entrada, por ejemplo `['get',  'temperature']`. El valor de entrada se evalúa con pares de valores de entrada y salidos, denominados "detiene", para determinar el valor que mejor se adapte a la función de paso o curva interpolada. Los valores para cada punto de entrada deben ser un número y estar en orden ascendente. Los valores de salida deben ser un número y la matriz de números o un color.

### <a name="interpolate-expression"></a>Interpolar expresión

Un `interpolate` expresión puede utilizarse para calcular un conjunto de valores continuo y sin problemas mediante la interpolación entre valores de detención. Un `interpolate` expresión que devuelve valores de color produce en qué resultado se seleccionan los valores de un degradado de color.

Hay tres tipos de métodos de interpolación que se pueden usar en un `interpolate` expresión:
 
* `['linear']` -Se interpola linealmente entre el par de delimitadores.
* `['exponential', base]` -Interpola exponencialmente entre los delimitadores. El `base` valor controla la velocidad a la que aumenta la salida. Los valores más altos que la salida aumentar más hacia el extremo superior del intervalo. Un `base` valor cercano a 1 genera una salida que aumenta linealmente más.
* `['cubic-bezier', x1, y1, x2, y2]` -Interpola mediante un [curva Bézier cúbica](https://developer.mozilla.org/docs/Web/CSS/timing-function) definido por los puntos de control determinado.

Este es un ejemplo del aspecto de estos tipos de interpolaciones. 

| Lineal  | Exponencial | Bézier cúbica |
|---------|-------------|--------------|
| ![Gráfico de interpolación lineal](media/how-to-expressions/linear-interpolation.png) | ![Gráfico de interpolación exponencial](media/how-to-expressions/exponential-interpolation.png) | ![Gráfico de interpolación de Bézier cúbica](media/how-to-expressions/bezier-curve-interpolation.png) |

El pseudocódigo siguiente define la estructura de la `interpolate` expresión. 

```javascript
[
    'interpolate',
    interpolation: ['linear'] | ['exponential', base] | ['cubic-bezier', x1, y1, x2, y2],
    input: number,
    stopInput1: number, 
    stopOutput1: value1,
    stopInput2: number, 
    stopOutput2: value2, 
    ...
]
```

**Ejemplo**

En el ejemplo siguiente se usa un `linear interpolate` expresión para establecer el `color` propiedad de una capa de burbuja según el `temperature` propiedad de la característica de punto. Si el `temperature` valor es menor que "blue" 60, se devolverá si entre 60 y menor que 70, amarillo si se devolverá, entre el 70 y menor que 80, "orange" se devuelve, si "rojo" 80 o superior, se devolverán.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'interpolate',
        ['linear'],
        ['get', 'temperature'],
        50,        
        'blue',
        60,
        'yellow',
        70,
        'orange',
        80,
        'red'
    ]
});
```

La siguiente imagen muestra cómo se eligen los colores de la expresión anterior.
 
<center>

![Ejemplo de expresiones de interpolar](media/how-to-expressions/interpolate-expression-example.png) </center>

### <a name="step-expression"></a>Expresión de paso

Un `step` expresión puede utilizarse para calcular los valores discretos, escalonado resultados mediante la evaluación de un [constante a trozos función](http://mathworld.wolfram.com/PiecewiseConstantFunction.html) definido por se detiene. 

El pseudocódigo siguiente define la estructura de la `step` expresión. 

```javascript
[
    'step',
    input: number,
    output0: value0,
    stop1: number, 
    output1: value1,
    stop2: number, 
    output2: value2, 
    ...
]
```

Las expresiones de paso devuelven el valor de salida de la detención antes de ser el valor de entrada o el primer valor de entrada si la entrada es menor que la primera parada. 

**Ejemplo**

En el ejemplo siguiente se usa un `step` expresión para establecer el `color` propiedad de una capa de burbuja según el `temperature` propiedad de la característica de punto. Si el `temperature` valor es menor que "blue" 60, se devolverá si entre 60 y menor que 70, "amarillo" si se devolverá, entre el 70 y menor que 80, "orange" se devuelve, si "rojo" 80 o superior, se devolverán.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'step',
        ['get', 'temperature'],
        'blue',
        60,
        'yellow',
        70,
        'orange',
        80,
        'red'
    ]
});
```

La siguiente imagen muestra cómo se eligen los colores de la expresión anterior.
 
<center>

![Ejemplo de expresiones de paso](media/how-to-expressions/step-expression-example.png)
</center>

## <a name="layer-specific-expressions"></a>Expresiones específicas de capa

Expresiones especiales que solo se aplican a capas específicas.

### <a name="heat-map-density-expression"></a>Expresión de densidad de mapa de calor

Recupera el valor de densidad de mapa de calor para cada píxel de una capa de mapa térmico de una expresión de densidad de mapa de calor y se define como `['heatmap-density']`. Este valor es un número entre `0` y `1` y se usa en combinación con un `interpolation` o `step` expresión para definir el degradado de color que se utiliza para colorear el mapa de calor. Solo se puede usar esta expresión en el [, opción de color](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest#color) de la capa de mapa de calor.

> [!TIP]
> El color en el índice 0 en una expresión de interpolación o el color predeterminado de un paso, define el color del área donde no hay ningún dato y puede utilizarse para definir un color de fondo. Muchos prefieren establecer este valor en transparente o en un negro semitransparente. 

**Ejemplo**

Este ejemplo utiliza una expresión de interpolación marítimas para crear un degradado de color suave para representar el mapa de calor. 

```javascript 
var layer = new atlas.layer.HeatMapLayer(datasource, null, {
    color: [
        'interpolate',
        ['linear'],
        ['heatmap-density'],
        0, 'transparent',
        0.01, 'purple',
        0.5, '#fb00fb',
        1, '#00c3ff'
    ]
});
```

Además de utilizar un degradado suave para colorear un mapa de calor, colores pueden especificarse dentro de un conjunto de intervalos mediante una `step` expresión. Mediante un `step` expresión para colorear el mapa térmico desglosa la densidad visualmente en intervalos que más se parezca a un mapa de estilo de contorno o radial.  

```javascript 
var layer = new atlas.layer.HeatMapLayer(datasource, null, {
    color: [
        'step',
        ['heatmap-density'],
        'transparent',
        0.01, 'navy',
        0.25, 'navy',
        0.5, 'green',
        0.75, 'yellow',
        1, 'red'
    ]
});
```

Para obtener más información, consulte el [agregar una capa de mapa térmico](map-add-heat-map-layer.md) documentación.

### <a name="line-progress-expression"></a>Expresión de progreso de la línea

Recupera el progreso de una línea de degradado en una capa de línea de una expresión de progreso de la línea y se define como `['line-progress']`. Este valor es un número entre 0 y 1 y se usa en combinación con un `interpolation` o `step` expresión. Esta expresión puede utilizarse solo con el [strokeGradient opción]( https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest#strokegradient) de la capa de línea. 

> [!NOTE]
> El `strokeGradient` requiere la opción de la capa de línea del `lineMetrics` opción del origen de datos se establezca en `true`.

**Ejemplo**

En el ejemplo siguiente se usa el `['line-progress']` expresión para aplicar un degradado de color en el trazo de una línea.

```javascript
var layer = new atlas.layer.LineLayer(datasource, null, {
    strokeGradient: [
        'interpolate',
        ['linear'],
        ['line-progress'],
        0, "blue",
        0.1, "royalblue",
        0.3, "cyan",
        0.5, "lime",
        0.7, "yellow",
        1, "red"
    ]
});
```

[Vea el ejemplo en vivo](map-add-shape.md#line-stroke-gradient)

### <a name="text-field-format-expression"></a>Expresión de formato de campo de texto

La expresión de formato de campo de texto puede utilizarse con el `textField` opción de las capas de símbolos `textOptions` propiedad para proporcionar un formato mixto de texto. Esta expresión permite un conjunto de cadenas de entrada y las opciones de formato que se especifique. Las siguientes opciones pueden especificarse para cada cadena de entrada en esta expresión.

 * `'font-scale'` : Especifica el factor de escala para el tamaño de fuente. Si se especifica, este valor invalida el `size` propiedad de la `textOptions` para la cadena individual.
 * `'text-font'` : Especifica uno o más familias de fuentes que se deben usar para esta cadena. Si se especifica, este valor invalida el `font` propiedad de la `textOptions` para la cadena individual.

El siguiente pseudocódigo define la estructura de la expresión de formato de campo de texto. 

```javascript
[
    'format', 
    input1: string, 
    options1: { 
        'font-scale': number, 
        'text-font': string[] 
    },
    input2: string, 
    options2: { 
        'font-scale': number, 
        'text-font': string[] 
    },
    …
]
```

**Ejemplo**

En el ejemplo siguiente se da el campo de texto mediante la adición de una fuente en negrita y escalar verticalmente el tamaño de fuente de la `title` propiedad de la característica. En este ejemplo también agrega el `subtitle` propiedad de la característica en una nueva línea, con una escala hacia abajo el tamaño de fuente.

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: [
            'format',

            //Bold the title property and scale its font size up.
            ['get', 'title'],
            {
                'text-font': ['literal', ['StandardFont-Bold']],
                'font-scale': 1.25
            },

            '\n', {},   //Add a new line without any formatting.

            //Scale the font size down of the subtitle property. 
            ['get', 'subtitle'],
            { 'font-scale': 0.75 }
        ]
    }
});
```

Esta capa representará de la característica punto tal como se muestra en la imagen siguiente:
 
<center>

![Imagen de la característica de punto con el campo de texto con formato](media/how-to-expressions/text-field-format-expression.png) </center>

### <a name="number-format-expression"></a>Expresión de formato de número

El `number-format` expresión solo se puede usar con el `textField` opción de una capa de símbolos. Esta expresión convierte al número proporcionado en una cadena con formato. Esta expresión de JavaScript ajusta [Number.toLocalString](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString) funcionando y es compatible con el siguiente conjunto de opciones.

 * `locale` -Especifique esta opción para convertir números en cadenas de forma que se alinea con el idioma especificado. Pasar un [etiqueta de idioma BCP 47](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Intl#Locale_identification_and_negotiation) en esta opción.
 * `currency` -Para convertir al número en una cadena que representa una moneda. Los valores posibles son el [códigos de divisa ISO 4217](https://en.wikipedia.org/wiki/ISO_4217), como "USD" para el dólar estadounidense, "EUR" euros o "CNY" para el RMB chino.
 * `'min-fraction-digits'` : Especifica el número mínimo de posiciones decimales que se incluyen en la versión de cadena del número.
 * `'max-fraction-digits'` : Especifica el número máximo de posiciones decimales que se incluyen en la versión de cadena del número.

El siguiente pseudocódigo define la estructura de la expresión de formato de campo de texto. 

```javascript
[
    'number-format', 
    input: number, 
    options: {
        locale: string, 
        currency: string, 
        'min-fraction-digits': number, 
        'max-fraction-digits': number
    }
]
```

**Ejemplo**

En el ejemplo siguiente se usa un `number-format` expresión para modificar el modo `revenue` propiedad de la característica de punto se representa en el `textField` capas de opción de un símbolo que parece un valor en dólares estadounidenses.

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: [
            'number-format', 
            ['get', 'revenue'], 
            { ‘currency’: 'USD' }
        ],

        offset: [0, 0.75]
    }
});
```

Esta capa representará de la característica punto tal como se muestra en la imagen siguiente:

<center>

![Ejemplo de expresión de formato de número](media/how-to-expressions/number-format-expression.png) </center>

## <a name="zoom-expression"></a>Expresión de zoom

Un `zoom` expresión se usa para recuperar el nivel de zoom actual de la asignación en tiempo de procesamiento y se define como `['zoom']`. Esta expresión devuelve un número entre el intervalo de nivel de zoom mínimo y máximo del mapa. Mediante la siguiente expresión permite estilos que se va a modificar de forma dinámica cuando se cambia el nivel de zoom del mapa. El `zoom` expresión solo puede usarse con `interpolate` y `step` expresiones.

**Ejemplo**

De forma predeterminada, los radios de puntos de datos representados en la capa de mapa térmico tienen un radio de píxel fijo para todos los niveles de zoom. Como el mapa se amplía los agregados de datos juntos y la capa de mapa térmico tiene un aspecto diferente. Un `zoom` expresión puede utilizarse para escalar el radio para cada nivel de zoom de forma que cada punto de datos trata de la misma área física de la asignación. Esto hará que la capa de mapa térmico buscar más coherente y estático. Cada nivel de zoom del mapa tiene dos veces tantas píxeles vertical y horizontalmente como nivel de zoom anterior. Escala el radio de forma que duplica con cada nivel de zoom, se creará un mapa térmico que tiene un aspecto coherente en todos los niveles de zoom. Esto puede realizarse mediante el uso de la `zoom` expresión con un `base 2 exponential interpolation` expresión tal como se muestra a continuación. 

```javascript 
var layer = new atlas.layer.HeatMapLayer(datasource, null, {
    radius: [
        'interpolate',
        ['exponential', 2],
        ['zoom'],
        
        //For zoom level 1 set the radius to 2 pixels.
        10, 2,

        //Between zoom level 1 and 19, exponentially scale the radius from 2 pixels to 10,000 pixels.
        19, 10000
    ]
};
```

[Vea el ejemplo en vivo](map-add-heat-map-layer.md#consistent-zoomable-heat-map)

## <a name="variable-binding-expressions"></a>Expresiones de enlace de variables

Las expresiones de enlace de variables almacenan los resultados de un cálculo en una variable para que se puede hacer referencia en otro lugar en una expresión varias veces sin tener que volver a calcularlo. Esto supone una optimización útil para las expresiones que implican muchos cálculos

| Expression | Tipo de valor devuelto | DESCRIPCIÓN |
|--------------|---------------|--------------|
| \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'let',<br/>&nbsp;&nbsp;&nbsp;&nbsp;nombre1: cadena,<br/>&nbsp;&nbsp;&nbsp;&nbsp;valor1: cualquier,<br/>&nbsp;&nbsp;&nbsp;&nbsp;nombre2: cadena,<br/>&nbsp;&nbsp;&nbsp;&nbsp;value2: cualquier,<br/>&nbsp;&nbsp;&nbsp;&nbsp;…<br/>&nbsp;&nbsp;&nbsp;&nbsp;childExpression<br/>\] | | Almacena uno o más valores como variables para su uso por el `var` expresión en la expresión secundaria que devuelve el resultado. |
| `['var', name: string]` | cualquiera | Hace referencia a una variable que se creó utilizando el `let` expresión. |

**Ejemplo**

Este ejemplo utiliza una expresión que calcula los ingresos en relación con la proporción de temperatura y, a continuación, usa un `case` expresión para evaluar diferentes operaciones booleanas en este valor. El `let` expresión se utiliza para almacenar los ingresos en relación con la proporción de temperatura de modo que solo debe calcularse una vez y `var` expresión hace referencia a esta variable tantas veces como sea necesario sin tener que volver a calcularlo.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        //Divide the point features `revenue` property by the `temperature` property and store it in a variable called `ratio`.
        'let', 'ratio', ['/', ['get', 'revenue'], ['get', 'temperature']],
        //Evaluate the child expression in which the stored variable will be used.
        [
            'case',

            //Check to see if the ratio is less than 100, return 'red'.
            ['<', ['var', 'ratio'], 100],
            'red',

            //Check to see if the ratio is less than 200, return 'green'.
            ['<', ['var', 'ratio'], 200],
            'green',

            //Return `blue` for values greater or equal to 200.
            'blue'
        ]
    ]
});
```

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para obtener más ejemplos de código que implementan expresiones:

> [!div class="nextstepaction"] 
> [Adición de una capa de símbolo](map-add-pin.md)

> [!div class="nextstepaction"] 
> [Adición de una capa de burbuja](map-add-bubble-layer.md)

> [!div class="nextstepaction"] 
> [Agregar formas](map-add-shape.md)

> [!div class="nextstepaction"] 
> [Adición de una capa de mapa térmico](map-add-heat-map-layer.md)

Más información sobre las opciones de capa que admiten expresiones:

> [!div class="nextstepaction"] 
> [BubbleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [HeatMapLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest) 
