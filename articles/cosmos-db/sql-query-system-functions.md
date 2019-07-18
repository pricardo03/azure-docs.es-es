---
title: Funciones del sistema
description: Obtenga información acerca de las funciones del sistema de SQL en Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: 11a6fdad187670bcb5af4c56198fd7343680690d
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2019
ms.locfileid: "67343239"
---
# <a name="system-functions"></a>Funciones del sistema

 Cosmos DB proporciona muchas funciones de SQL integradas. Las categorías de las funciones integradas aparecen a continuación.  
  
|Función|DESCRIPCIÓN|  
|--------------|-----------------|  
|[Funciones matemáticas](#mathematical-functions)|Las funciones matemáticas realizan un cálculo, basado normalmente en valores de entrada proporcionados como argumentos, y devuelven un valor numérico.|  
|[Funciones de comprobación de tipos](#type-checking-functions)|Las funciones de comprobación de tipos permiten comprobar el tipo de una expresión dentro de consultas SQL.|  
|[Funciones de cadena](#string-functions)|Las siguientes funciones de cadena realizan una operación sobre un valor de entrada de cadena y devuelven una cadena, un valor numérico o un booleano.|  
|[Funciones de matriz](#array-functions)|Las siguientes funciones de matriz realizan una operación en un valor de entrada de matriz y devuelven un valor numérico, booleano o de matriz.|
|[Funciones de fecha y hora](#date-time-functions)|Las funciones de fecha y hora le permiten obtener la fecha y la hora UTC actuales de dos formas: una marca de tiempo numérica cuyo valor es la época de Unix en milisegundos, o una cadena que se ajusta al formato ISO 8601.|
|[Funciones espaciales](#spatial-functions)|Las siguientes funciones espaciales realizan una operación en un valor de entrada de objeto espacial y devuelven un valor numérico o booleano.|  

A continuación se muestra una lista de funciones dentro de cada categoría:

| Grupo de funciones | Operaciones |
|---------|----------|
| Funciones matemáticas | ABS, CEILING, EXP, FLOOR, LOG, LOG10, POWER, ROUND, SIGN, SQRT, SQUARE, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COT, DEGREES, PI, RADIANS, SIN y TAN |
| Funciones de comprobación de tipos | IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED e IS_PRIMITIVE |
| Funciones de cadena | CONCAT, CONTAINS, ENDSWITH, INDEX_OF, LEFT, LENGTH, LOWER, LTRIM, REPLACE, REPLICATE, REVERSE, RIGHT, RTRIM, STARTSWITH, SUBSTRING y UPPER |
| Funciones de matriz | ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH y ARRAY_SLICE |
| Funciones de fecha y hora | GETCURRENTDATETIME, GETCURRENTTIMESTAMP,  |
| Funciones espaciales | ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID y ST_ISVALIDDETAILED |

Si actualmente usa una función definida por el usuario (UDF) para la que ahora hay disponible una función integrada, la función integrada correspondiente será más rápida de ejecutar y más eficaz.

La principal diferencia entre las funciones de Cosmos DB y SQL ANSI es que las funciones de Cosmos DB están diseñadas para funcionar bien con datos sin esquemas y datos de esquemas mixtos. Por ejemplo, si falta una propiedad o tiene un valor no numérico, como `unknown`, se omite el elemento en lugar de devolver un error.

##  <a name="mathematical-functions"></a> Funciones matemáticas  

Las funciones matemáticas realizan un cálculo, basado en valores de entrada proporcionados como argumentos, y devuelven un valor numérico.

Puede ejecutar consultas similares a las del ejemplo siguiente:

```sql
    SELECT VALUE ABS(-4)
```

El resultado es el siguiente:

```json
    [4]
```

Esta es una tabla de las funciones matemáticas integradas admitidas.
  
||||  
|-|-|-|  
|[ABS](#bk_abs)|[ACOS](#bk_acos)|[ASIN](#bk_asin)|  
|[ATAN](#bk_atan)|[ATN2](#bk_atn2)|[CEILING](#bk_ceiling)|  
|[COS](#bk_cos)|[COT](#bk_cot)|[DEGREES](#bk_degrees)|  
|[EXP](#bk_exp)|[FLOOR](#bk_floor)|[LOG](#bk_log)|  
|[LOG10](#bk_log10)|[PI](#bk_pi)|[POWER](#bk_power)|  
|[RADIANS](#bk_radians)|[ROUND](#bk_round)|[SIN](#bk_sin)|  
|[SQRT](#bk_sqrt)|[SQUARE](#bk_square)|[SIGN](#bk_sign)|  
|[TAN](#bk_tan)|[TRUNC](#bk_trunc)||  
  
####  <a name="bk_abs"></a> ABS  
 Devuelve el valor absoluto (positivo) de la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
ABS (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestran los resultados de usar la función ABS en tres números distintos.  
  
```  
SELECT ABS(-1) AS abs1, ABS(0) AS abs2, ABS(1) AS abs3 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{abs1: 1, abs2: 0, abs3: 1}]  
```  
  
####  <a name="bk_acos"></a> ACOS  
 Devuelve el ángulo, en radianes, cuyo coseno es la expresión numérica especificada; también se denomina arcocoseno.  
  
 **Sintaxis**  
  
```  
ACOS(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  El siguiente ejemplo devuelve el arcocoseno de -1:  
  
```  
SELECT ACOS(-1) AS acos 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"acos": 3.1415926535897931}]  
```  
  
####  <a name="bk_asin"></a> ASIN  
 Devuelve el ángulo, en radianes, cuyo seno es la expresión numérica especificada. También se denomina arcoseno.  
  
 **Sintaxis**  
  
```  
ASIN(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  El siguiente ejemplo devuelve el arcoseno de -1:  
  
```  
SELECT ASIN(-1) AS asin  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"asin": -1.5707963267948966}]  
```  
  
####  <a name="bk_atan"></a> ATAN  
 Devuelve el ángulo, en radianes, cuya tangente es la expresión numérica especificada. También se denomina arcotangente.  
  
 **Sintaxis**  
  
```  
ATAN(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  El siguiente ejemplo devuelve la arcotangente del valor especificado.  
  
```  
SELECT ATAN(-45.01) AS atan  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"atan": -1.5485826962062663}]  
```  
  
####  <a name="bk_atn2"></a> ATN2  
 Devuelve el valor principal de la arcotangente de y/x, expresado en radianes.  
  
 **Sintaxis**  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el siguiente ejemplo se calcula ATN2 para los componentes x e y especificados.  
  
```  
SELECT ATN2(35.175643, 129.44) AS atn2  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"atn2": 1.3054517947300646}]  
```  
  
####  <a name="bk_ceiling"></a> CEILING  
 Devuelve el valor entero más pequeño mayor o igual que la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
CEILING (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestran valores numéricos positivos, negativos y cero con la función CEILING.  
  
```  
SELECT CEILING(123.45) AS c1, CEILING(-123.45) AS c2, CEILING(0.0) AS c3  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{c1: 124, c2: -123, c3: 0}]  
```  
  
####  <a name="bk_cos"></a> COS  
 Devuelve el coseno trigonométrico del ángulo especificado, en radianes, en la expresión especificada.  
  
 **Sintaxis**  
  
```  
COS(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se calcula el coseno del ángulo especificado.  
  
```  
SELECT COS(14.78) AS cos  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"cos": -0.59946542619465426}]  
```  
  
####  <a name="bk_cot"></a> COT  
 Devuelve la cotangente trigonométrica del ángulo especificado, en radianes, en la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
COT(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se calcula la cotangente del ángulo especificado.  
  
```  
SELECT COT(124.1332) AS cot  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"cot": -0.040311998371148884}]  
```  
  
####  <a name="bk_degrees"></a> DEGREES  
 Devuelve el ángulo correspondiente en grados de un ángulo especificado en radianes.  
  
 **Sintaxis**  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelven los grados de un ángulo de pi/2 radianes.  
  
```  
SELECT DEGREES(PI()/2) AS degrees  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"degrees": 90}]  
```  
  
####  <a name="bk_floor"></a> FLOOR  
 Devuelve el valor entero más grande menor o igual que la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestran valores numéricos positivos, negativos y cero con la función FLOOP.  
  
```  
SELECT FLOOR(123.45) AS fl1, FLOOR(-123.45) AS fl2, FLOOR(0.0) AS fl3  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{fl1: 123, fl2: -124, fl3: 0}]  
```  
  
####  <a name="bk_exp"></a> EXP  
 Devuelve el valor exponencial de la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
EXP (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Comentarios:**  
  
  La constante **e** (2,718281 …) es la base de los logaritmos naturales.  
  
  El exponente de un número es la constante **e** elevada a la potencia del número. Por ejemplo EXP(1,0) = e^1,0 = 2,71828182845905 y EXP(10) = e^10 = 22026,4657948067.  
  
  El valor exponencial del logaritmo natural de un número es el mismo número: EXP (LOG [n]) = n. Y el logaritmo natural del valor exponencial de un número es el mismo número: LOG (EXP (n)) = n.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se declara una variable y se devuelve el valor exponencial de la variable especificada (10).  
  
```  
SELECT EXP(10) AS exp  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{exp: 22026.465794806718}]  
```  
  
 En el ejemplo siguiente se devuelve el valor exponencial del logaritmo natural de 20 y el logaritmo natural del valor exponencial de 20. Dado que estas funciones son inversas entre sí, el valor devuelto con redondeo para las operaciones matemáticas de punto flotante en ambos casos es 20.  
  
```  
SELECT EXP(LOG(20)) AS exp1, LOG(EXP(20)) AS exp2  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{exp1: 19.999999999999996, exp2: 20}]  
```  
  
####  <a name="bk_log"></a> LOG  
 Devuelve el algoritmo natural de la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
- `base`  
  
   Argumento numérico opcional que establece la base del logaritmo.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Comentarios**  
  
  De forma predeterminada, LOG() devuelve el logaritmo natural. Puede cambiar la base del logaritmo a otro valor mediante el parámetro base opcional.  
  
  El logaritmo natural es el logaritmo en base **e**, donde **e** es una constante irracional aproximadamente igual a 2,718 281 828.  
  
  El logaritmo natural del valor exponencial de un número es el mismo número: LOG( EXP( n ) ) = n. Y el valor exponencial del logaritmo natural de un número es el mismo número: EXP( LOG( n ) ) = n.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se declara una variable y se devuelve el valor logarítmico de la variable especificada (10).  
  
```  
SELECT LOG(10) AS log  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{log: 2.3025850929940459}]  
```  
  
 En el ejemplo siguiente se calcula el logaritmo del exponente de un número.  
  
```  
SELECT EXP(LOG(10)) AS expLog  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{expLog: 10.000000000000002}]  
```  
  
####  <a name="bk_log10"></a> LOG10  
 Devuelve el logaritmo de base 10 de la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Comentarios:**  
  
  Las funciones LOG10 y POWER tienen una relación inversa. Por ejemplo, 10 ^ LOG10 (n) = n.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se declara una variable y se devuelve el valor de LOG10 de la variable especificada (100).  
  
```  
SELECT LOG10(100) AS log10 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{log10: 2}]  
```  
  
####  <a name="bk_pi"></a> PI  
 Devuelve el valor constante de PI.  
  
 **Sintaxis**  
  
```  
PI ()  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  El siguiente ejemplo devuelve el valor de pi.  
  
```  
SELECT PI() AS pi 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"pi": 3.1415926535897931}]  
```  
  
####  <a name="bk_power"></a> POWER  
 Devuelve el valor de la expresión especificada a la potencia especificada.  
  
 **Sintaxis**  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
- `y`  
  
   Es la potencia a la que se eleva `numeric_expression`.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo elevar un número a la potencia de 3 (el cubo del número).  
  
```  
SELECT POWER(2, 3) AS pow1, POWER(2.5, 3) AS pow2  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{pow1: 8, pow2: 15.625}]  
```  
  
####  <a name="bk_radians"></a> RADIANS  
 Devuelve radianes cuando se especifica una expresión numérica en grados.  
  
 **Sintaxis**  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se toman unos ángulos como entrada y se devuelven sus correspondientes valores en radianes.  
  
```  
SELECT RADIANS(-45.01) AS r1, RADIANS(-181.01) AS r2, RADIANS(0) AS r3, RADIANS(0.1472738) AS r4, RADIANS(197.1099392) AS r5  
```  
  
  El conjunto de resultados es el siguiente:  
  
```  
[{  
       "r1": -0.7855726963226477,  
       "r2": -3.1592204790349356,  
       "r3": 0,  
       "r4": 0.0025704127119236249,  
       "r5": 3.4402174274458375  
   }]  
```  
  
####  <a name="bk_round"></a> ROUND  
 Devuelve un valor numérico, redondeado al valor entero más cercano.  
  
 **Sintaxis**  
  
```  
ROUND(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Comentarios:**
  
  La operación de redondeo realizada sigue el redondeo de punto medio alejado del cero. Si la entrada es una expresión numérica que se encuentra exactamente entre dos enteros, el resultado será el valor entero más cercano alejado de cero.  
  
  |<numeric_expression>|Redondeo|
  |-|-|
  |-6,5000|-7|
  |-0,5|-1|
  |0,5|1|
  |6,5000|7||
  
  **Ejemplos**  
  
  En el ejemplo siguiente se redondean los siguientes números positivos y negativos al entero más próximo.  
  
```  
SELECT ROUND(2.4) AS r1, ROUND(2.6) AS r2, ROUND(2.5) AS r3, ROUND(-2.4) AS r4, ROUND(-2.6) AS r5  
```  
  
  El conjunto de resultados es el siguiente:  
  
```  
[{r1: 2, r2: 3, r3: 3, r4: -2, r5: -3}]  
```  
  
####  <a name="bk_sign"></a> SIGN  
 Devuelve el signo positivo (+1), cero (0) o negativo (-1) de la expresión numérica especificada.  
  
 **Sintaxis**  
  
```  
SIGN(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelven los valores del signo de números entre -2 y 2.  
  
```  
SELECT SIGN(-2) AS s1, SIGN(-1) AS s2, SIGN(0) AS s3, SIGN(1) AS s4, SIGN(2) AS s5  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{s1: -1, s2: -1, s3: 0, s4: 1, s5: 1}]  
```  
  
####  <a name="bk_sin"></a> SIN  
 Devuelve el seno trigonométrico del ángulo especificado, en radianes, en la expresión especificada.  
  
 **Sintaxis**  
  
```  
SIN(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se calcula el seno del ángulo especificado.  
  
```  
SELECT SIN(45.175643) AS sin  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"sin": 0.929607286611012}]  
```  
  
####  <a name="bk_sqrt"></a> SQRT  
 Devuelve la raíz cuadrada del valor numérico especificado.  
  
 **Sintaxis**  
  
```  
SQRT(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelven las raíces cuadradas de los números 1-3.  
  
```  
SELECT SQRT(1) AS s1, SQRT(2.0) AS s2, SQRT(3) AS s3  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{s1: 1, s2: 1.4142135623730952, s3: 1.7320508075688772}]  
```  
  
####  <a name="bk_square"></a> SQUARE  
 Devuelve el cuadrado del valor numérico especificado.  
  
 **Sintaxis**  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelven los cuadrados de los números 1-3.  
  
```  
SELECT SQUARE(1) AS s1, SQUARE(2.0) AS s2, SQUARE(3) AS s3  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{s1: 1, s2: 4, s3: 9}]  
```  
  
####  <a name="bk_tan"></a> TAN  
 Devuelve la tangente del ángulo especificado, en radianes, en la expresión especificada.  
  
 **Sintaxis**  
  
```  
TAN (<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se calcula la tangente de pi()/2.  
  
```  
SELECT TAN(PI()/2) AS tan 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"tan": 16331239353195370 }]  
```  
  
####  <a name="bk_trunc"></a> TRUNC  
 Devuelve un valor numérico, truncado al valor entero más cercano.  
  
 **Sintaxis**  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 **Argumentos**  
  
- `numeric_expression`  
  
   Es una expresión numérica.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se truncan los siguientes números positivos y negativos al valor entero más próximo.  
  
```  
SELECT TRUNC(2.4) AS t1, TRUNC(2.6) AS t2, TRUNC(2.5) AS t3, TRUNC(-2.4) AS t4, TRUNC(-2.6) AS t5  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{t1: 2, t2: 2, t3: 2, t4: -2, t5: -2}]  
```

## <a id="type-checking-functions"></a>Funciones de comprobación de tipos

Las funciones de comprobación de tipos permiten comprobar el tipo de una expresión dentro de una consulta SQL. Puede usar las funciones de comprobación de tipos para determinar los tipos de propiedades dentro de los elementos sobre la marcha cuando son variables o desconocidos. Esta es una tabla de las funciones de comprobación de tipos integradas admitidas:

Las siguientes funciones admiten la comprobación de tipos de valores de entrada y devuelven un valor booleano.  
  
||||  
|-|-|-|  
|[IS_ARRAY](#bk_is_array)|[IS_BOOL](#bk_is_bool)|[IS_DEFINED](#bk_is_defined)|  
|[IS_NULL](#bk_is_null)|[IS_NUMBER](#bk_is_number)|[IS_OBJECT](#bk_is_object)|  
|[IS_PRIMITIVE](#bk_is_primitive)|[IS_STRING](#bk_is_string)||  
  
####  <a name="bk_is_array"></a> IS_ARRAY  
 Devuelve un valor booleano que indica si el tipo de la expresión especificada es una matriz.  
  
 **Sintaxis**  
  
```  
IS_ARRAY(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueban objetos de los tipos JSON booleano, numérico, cadena, null, objeto, matriz y undefined mediante la función IS_ARRAY.  
  
```  
SELECT   
 IS_ARRAY(true) AS isArray1,   
 IS_ARRAY(1) AS isArray2,  
 IS_ARRAY("value") AS isArray3,  
 IS_ARRAY(null) AS isArray4,  
 IS_ARRAY({prop: "value"}) AS isArray5,   
 IS_ARRAY([1, 2, 3]) AS isArray6,  
 IS_ARRAY({prop: "value"}.prop2) AS isArray7  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isArray1":false,"isArray2":false,"isArray3":false,"isArray4":false,"isArray5":false,"isArray6":true,"isArray7":false}]
```  
  
####  <a name="bk_is_bool"></a> IS_BOOL  
 Devuelve un valor booleano que indica si el tipo de la expresión especificada es un valor booleano.  
  
 **Sintaxis**  
  
```  
IS_BOOL(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueban objetos de los tipos JSON booleano, numérico, cadena, null, objeto, matriz y undefined mediante la función IS_BOOL.  
  
```  
SELECT   
    IS_BOOL(true) AS isBool1,   
    IS_BOOL(1) AS isBool2,  
    IS_BOOL("value") AS isBool3,   
    IS_BOOL(null) AS isBool4,  
    IS_BOOL({prop: "value"}) AS isBool5,   
    IS_BOOL([1, 2, 3]) AS isBool6,  
    IS_BOOL({prop: "value"}.prop2) AS isBool7  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isBool1":true,"isBool2":false,"isBool3":false,"isBool4":false,"isBool5":false,"isBool6":false,"isBool7":false}]
```  
  
####  <a name="bk_is_defined"></a> IS_DEFINED  
 Devuelve un valor booleano que indica si se ha asignado un valor a la propiedad.  
  
 **Sintaxis**  
  
```  
IS_DEFINED(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueba la presencia de una propiedad en el documento JSON especificado. En el primer caso se devuelve true, ya que "a" está presente, pero en el segundo, false, ya que "b" no lo está.  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a) AS isDefined1, IS_DEFINED({ "a" : 5 }.b) AS isDefined2 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isDefined1":true,"isDefined2":false}]  
```  
  
####  <a name="bk_is_null"></a> IS_NULL  
 Devuelve un valor booleano que indica si el tipo de la expresión especificada es nulo.  
  
 **Sintaxis**  
  
```  
IS_NULL(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueban objetos de los tipos JSON booleano, numérico, cadena, null, objeto, matriz y undefined mediante la función IS_NULL.  
  
```  
SELECT   
    IS_NULL(true) AS isNull1,   
    IS_NULL(1) AS isNull2,  
    IS_NULL("value") AS isNull3,   
    IS_NULL(null) AS isNull4,  
    IS_NULL({prop: "value"}) AS isNull5,   
    IS_NULL([1, 2, 3]) AS isNull6,  
    IS_NULL({prop: "value"}.prop2) AS isNull7  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isNull1":false,"isNull2":false,"isNull3":false,"isNull4":true,"isNull5":false,"isNull6":false,"isNull7":false}]
```  
  
####  <a name="bk_is_number"></a> IS_NUMBER  
 Devuelve un valor booleano que indica si el tipo de la expresión especificada es un número.  
  
 **Sintaxis**  
  
```  
IS_NUMBER(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueban objetos de los tipos JSON booleano, numérico, cadena, null, objeto, matriz y undefined mediante la función IS_NULL.  
  
```  
SELECT   
    IS_NUMBER(true) AS isNum1,   
    IS_NUMBER(1) AS isNum2,  
    IS_NUMBER("value") AS isNum3,   
    IS_NUMBER(null) AS isNum4,  
    IS_NUMBER({prop: "value"}) AS isNum5,   
    IS_NUMBER([1, 2, 3]) AS isNum6,  
    IS_NUMBER({prop: "value"}.prop2) AS isNum7  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isNum1":false,"isNum2":true,"isNum3":false,"isNum4":false,"isNum5":false,"isNum6":false,"isNum7":false}]  
```  
  
####  <a name="bk_is_object"></a> IS_OBJECT  
 Devuelve un valor booleano que indica si el tipo de la expresión especificada es un objeto JSON.  
  
 **Sintaxis**  
  
```  
IS_OBJECT(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueban objetos de los tipos JSON booleano, numérico, cadena, null, objeto, matriz y undefined mediante la función IS_OBJECT.  
  
```  
SELECT   
    IS_OBJECT(true) AS isObj1,   
    IS_OBJECT(1) AS isObj2,  
    IS_OBJECT("value") AS isObj3,   
    IS_OBJECT(null) AS isObj4,  
    IS_OBJECT({prop: "value"}) AS isObj5,   
    IS_OBJECT([1, 2, 3]) AS isObj6,  
    IS_OBJECT({prop: "value"}.prop2) AS isObj7  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isObj1":false,"isObj2":false,"isObj3":false,"isObj4":false,"isObj5":true,"isObj6":false,"isObj7":false}]
```  
  
####  <a name="bk_is_primitive"></a> IS_PRIMITIVE  
 Devuelve un valor booleano que indica si el tipo de la expresión especificada es primitivo (cadena, booleano, numérico o NULL).  
  
 **Sintaxis**  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueban los objetos de los tipos JSON Boolean, number, string, null, object, array y undefined, mediante la función IS_PRIMITIVE.  
  
```  
SELECT   
           IS_PRIMITIVE(true) AS isPrim1,   
           IS_PRIMITIVE(1) AS isPrim2,  
           IS_PRIMITIVE("value") AS isPrim3,   
           IS_PRIMITIVE(null) AS isPrim4,  
           IS_PRIMITIVE({prop: "value"}) AS isPrim5,   
           IS_PRIMITIVE([1, 2, 3]) AS isPrim6,  
           IS_PRIMITIVE({prop: "value"}.prop2) AS isPrim7  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isPrim1": true, "isPrim2": true, "isPrim3": true, "isPrim4": true, "isPrim5": false, "isPrim6": false, "isPrim7": false}]  
```  
  
####  <a name="bk_is_string"></a> IS_STRING  
 Devuelve un valor booleano que indica si el tipo de la expresión especificada es una cadena.  
  
 **Sintaxis**  
  
```  
IS_STRING(<expression>)  
```  
  
 **Argumentos**  
  
- `expression`  
  
   Es cualquier expresión válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueban objetos de los tipos JSON booleano, numérico, cadena, null, objeto, matriz y undefined mediante la función IS_STRING.  
  
```  
SELECT   
       IS_STRING(true) AS isStr1,   
       IS_STRING(1) AS isStr2,  
       IS_STRING("value") AS isStr3,   
       IS_STRING(null) AS isStr4,  
       IS_STRING({prop: "value"}) AS isStr5,   
       IS_STRING([1, 2, 3]) AS isStr6,  
       IS_STRING({prop: "value"}.prop2) AS isStr7  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"isStr1":false,"isStr2":false,"isStr3":true,"isStr4":false,"isStr5":false,"isStr6":false,"isStr7":false}] 
```  

## <a id="string-functions"></a>Funciones de cadena

Las siguientes funciones escalares realizan una operación sobre un valor de entrada de cadena y devuelven una cadena, un valor numérico o un booleano:
  
||||  
|-|-|-|  
|[CONCAT](#bk_concat)|[CONTAINS](#bk_contains)|[ENDSWITH](#bk_endswith)|  
|[INDEX_OF](#bk_index_of)|[LEFT](#bk_left)|[LENGTH](#bk_length)|  
|[LOWER](#bk_lower)|[LTRIM](#bk_ltrim)|[REPLACE](#bk_replace)|  
|[REPLICATE](#bk_replicate)|[REVERSE](#bk_reverse)|[RIGHT](#bk_right)|  
|[RTRIM](#bk_rtrim)|[STARTSWITH](#bk_startswith)|[StringToArray](#bk_stringtoarray)|
|[StringToBoolean](#bk_stringtoboolean)|[StringToNull](#bk_stringtonull)|[StringToNumber](#bk_stringtonumber)|
|[StringToObject](#bk_stringtoobject)|[SUBSTRING](#bk_substring)|[ToString](#bk_tostring)|
|[TRIM](#bk_trim)|[UPPER](#bk_upper)||
  
####  <a name="bk_concat"></a> CONCAT  
 Devuelve una cadena que es el resultado de concatenar dos o más valores de cadena.  
  
 **Sintaxis**  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  El siguiente ejemplo devuelve la cadena concatenada de los valores especificados.  
  
```  
SELECT CONCAT("abc", "def") AS concat  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"concat": "abcdef"}  
```  
  
####  <a name="bk_contains"></a> CONTAINS  
 Devuelve un valor booleano que indica si la primera expresión de cadena contiene la segunda.  
  
 **Sintaxis**  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueba si "abc" contiene "ab" y contiene "d".  
  
```  
SELECT CONTAINS("abc", "ab") AS c1, CONTAINS("abc", "d") AS c2 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"c1": true, "c2": false}]  
```  
  
####  <a name="bk_endswith"></a> ENDSWITH  
 Devuelve un valor booleano que indica si la primera expresión de cadena finaliza con la segunda.  
  
 **Sintaxis**  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  El siguiente ejemplo devuelve las terminaciones "b" y "bc"de "abc".  
  
```  
SELECT ENDSWITH("abc", "b") AS e1, ENDSWITH("abc", "bc") AS e2 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"e1": false, "e2": true}]  
```  
  
####  <a name="bk_index_of"></a> INDEX_OF  
 Devuelve la posición inicial de la primera aparición de la expresión de la segunda cadena dentro de la primera expresión de cadena especificada, o -1 si no se encuentra la cadena.  
  
 **Sintaxis**  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelve el índice de varias subcadenas dentro de "abc".  
  
```  
SELECT INDEX_OF("abc", "ab") AS i1, INDEX_OF("abc", "b") AS i2, INDEX_OF("abc", "c") AS i3 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"i1": 0, "i2": 1, "i3": -1}]  
```  
  
####  <a name="bk_left"></a> LEFT  
 Devuelve la parte izquierda de una cadena con el número especificado de caracteres.  
  
 **Sintaxis**  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
- `num_expr`  
  
   Es cualquier expresión numérica válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelve la parte izquierda de "abc" para distintos valores de longitud.  
  
```  
SELECT LEFT("abc", 1) AS l1, LEFT("abc", 2) AS l2 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"l1": "a", "l2": "ab"}]  
```  
  
####  <a name="bk_length"></a> LENGTH  
 Devuelve el número de caracteres de la expresión de cadena especificada.  
  
 **Sintaxis**  
  
```  
LENGTH(<str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelve la longitud de una cadena.  
  
```  
SELECT LENGTH("abc") AS len 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"len": 3}]  
```  
  
####  <a name="bk_lower"></a> LOWER  
 Devuelve una expresión de cadena después de convertir los datos de caracteres en mayúsculas a minúsculas.  
  
 **Sintaxis**  
  
```  
LOWER(<str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar LOWER en una consulta.  
  
```  
SELECT LOWER("Abc") AS lower
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"lower": "abc"}]  
  
```  
  
####  <a name="bk_ltrim"></a> LTRIM  
 Devuelve una expresión de cadena después de quitar los espacios en blanco iniciales.  
  
 **Sintaxis**  
  
```  
LTRIM(<str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar LTRIM en una consulta.  
  
```  
SELECT LTRIM("  abc") AS l1, LTRIM("abc") AS l2, LTRIM("abc   ") AS l3 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"l1": "abc", "l2": "abc", "l3": "abc   "}]  
```  
  
####  <a name="bk_replace"></a> REPLACE  
 Reemplaza todas las apariciones de un valor de cadena especificado por otro valor de cadena.  
  
 **Sintaxis**  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar REPLACE en una consulta.  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk") AS replace 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"replace": "This is a desk"}]  
```  
  
####  <a name="bk_replicate"></a> REPLICATE  
 Repite un valor de cadena un número especificado de veces.  
  
 **Sintaxis**  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
- `num_expr`  
  
   Es cualquier expresión numérica válida. Si el valor num_expr es negativo o no finito, el resultado es indefinido.

  > [!NOTE]
  > La longitud máxima del resultado es de 10 000 caracteres, es decir, (length(str_expr) * num_expr) <= 10 000.
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar REPLICATE en una consulta.  
  
```  
SELECT REPLICATE("a", 3) AS replicate  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"replicate": "aaa"}]  
```  
  
####  <a name="bk_reverse"></a> REVERSE  
 Devuelve el orden inverso de un valor de cadena.  
  
 **Sintaxis**  
  
```  
REVERSE(<str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar REVERSE en una consulta.  
  
```  
SELECT REVERSE("Abc") AS reverse  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"reverse": "cbA"}]  
```  
  
####  <a name="bk_right"></a> RIGHT  
 Devuelve la parte derecha de una cadena con el número especificado de caracteres.  
  
 **Sintaxis**  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
- `num_expr`  
  
   Es cualquier expresión numérica válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelve la parte derecha de "abc" para distintos valores de longitud.  
  
```  
SELECT RIGHT("abc", 1) AS r1, RIGHT("abc", 2) AS r2 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"r1": "c", "r2": "bc"}]  
```  
  
####  <a name="bk_rtrim"></a> RTRIM  
 Devuelve una expresión de cadena después de quitar los espacios en blanco finales.  
  
 **Sintaxis**  
  
```  
RTRIM(<str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar RTRIM en una consulta.  
  
```  
SELECT RTRIM("  abc") AS r1, RTRIM("abc") AS r2, RTRIM("abc   ") AS r3  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"r1": "   abc", "r2": "abc", "r3": "abc"}]  
```  
  
####  <a name="bk_startswith"></a> STARTSWITH  
 Devuelve un valor booleano que indica si la primera expresión de cadena empieza con la segunda.  
  
 **Sintaxis**  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se comprueba si la cadena "abc" empieza por "b" y "a".  
  
```  
SELECT STARTSWITH("abc", "b") AS s1, STARTSWITH("abc", "a") AS s2  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"s1": false, "s2": true}]  
```  

  ####  <a name="bk_stringtoarray"></a> StringToArray  
 Devuelve la expresión traducida a una matriz. Si no se puede traducir la expresión, devuelve undefined.  
  
 **Sintaxis**  
  
```  
StringToArray(<expr>)  
```  
  
 **Argumentos**  
  
- `expr`  
  
   Es cualquier expresión escalar válida que se evaluará como expresión de matriz JSON. Tenga en cuenta que los valores de cadena anidados deben escribirse entre comillas dobles para ser válidos. Para más información sobre el formato JSON, consulte [json.org](https://json.org/)
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de matriz o undefined.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo se comporta StringToArray en tipos diferentes. 
  
 Los siguientes son ejemplos con entradas válidas.

```
SELECT 
    StringToArray('[]') AS a1, 
    StringToArray("[1,2,3]") AS a2,
    StringToArray("[\"str\",2,3]") AS a3,
    StringToArray('[["5","6","7"],["8"],["9"]]') AS a4,
    StringToArray('[1,2,3, "[4,5,6]",[7,8]]') AS a5
```

El conjunto de resultados es el siguiente:

```
[{"a1": [], "a2": [1,2,3], "a3": ["str",2,3], "a4": [["5","6","7"],["8"],["9"]], "a5": [1,2,3,"[4,5,6]",[7,8]]}]
```

El siguiente es un ejemplo de entrada no válida. 
   
 Las comillas simples dentro de la matriz no son válidas en JSON.
Aunque son válidas dentro de una consulta, no se analizarán en matrices válidas. Las cadenas dentro de la cadena de matriz deben escaparse "[\\"\\"]" o la comilla que la rodea debe ser simple '[""]'.

```
SELECT
    StringToArray("['5','6','7']")
```

El conjunto de resultados es el siguiente:

```
[{}]
```

Los siguientes son ejemplos de entrada no válida.
   
 La expresión pasada se analizará como una matriz JSON; a continuación no se evalúa para el tipo de matriz y, por tanto, se devuelven undefined.
   
```
SELECT
    StringToArray("["),
    StringToArray("1"),
    StringToArray(NaN),
    StringToArray(false),
    StringToArray(undefined)
```

El conjunto de resultados es el siguiente:

```
[{}]
```

####  <a name="bk_stringtoboolean"></a> StringToBoolean  
 Devuelve la expresión traducida a un valor booleano. Si no se puede traducir la expresión, devuelve undefined.  
  
 **Sintaxis**  
  
```  
StringToBoolean(<expr>)  
```  
  
 **Argumentos**  
  
- `expr`  
  
   Es cualquier expresión escalar válida que se evaluará como expresión booleana.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión booleana o undefined.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo se comporta StringToBoolean en tipos diferentes. 
 
 Los siguientes son ejemplos con entradas válidas.

Se admiten espacios en blanco únicamente antes o después de "true" o "false".

```  
SELECT 
    StringToBoolean("true") AS b1, 
    StringToBoolean("    false") AS b2,
    StringToBoolean("false    ") AS b3
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"b1": true, "b2": false, "b3": false}]
```  

Los siguientes son ejemplos con una entrada no válida.

 Los valores booleanos distinguen mayúsculas de minúsculas y deben escribirse con caracteres todos en minúsculas; es decir, "true" y "false".

```  
SELECT 
    StringToBoolean("TRUE"),
    StringToBoolean("False")
```  

El conjunto de resultados es el siguiente:  
  
```  
[{}]
``` 

La expresión pasada se analizará como una expresión booleana; estas entradas no se evalúan para el tipo booleano y, por tanto, se devuelven undefined.

```  
SELECT 
    StringToBoolean("null"),
    StringToBoolean(undefined),
    StringToBoolean(NaN), 
    StringToBoolean(false), 
    StringToBoolean(true)
```  

El conjunto de resultados es el siguiente:  
  
```  
[{}]
```  

####  <a name="bk_stringtonull"></a> StringToNull  
 Devuelve la expresión traducida a null. Si no se puede traducir la expresión, devuelve undefined.  
  
 **Sintaxis**  
  
```  
StringToNull(<expr>)  
```  
  
 **Argumentos**  
  
- `expr`  
  
   Es cualquier expresión escalar válida que se evaluará como expresión null.
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión null o undefined.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo se comporta StringToNull en tipos diferentes. 

Los siguientes son ejemplos con entradas válidas.

 Se admiten espacios en blanco únicamente antes o después de "null".

```  
SELECT 
    StringToNull("null") AS n1, 
    StringToNull("  null ") AS n2,
    IS_NULL(StringToNull("null   ")) AS n3
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"n1": null, "n2": null, "n3": true}]
```  

Los siguientes son ejemplos con una entrada no válida.

Null distingue mayúsculas de minúsculas y debe escribirse con caracteres todos en minúsculas; es decir, "null".

```  
SELECT    
    StringToNull("NULL"),
    StringToNull("Null")
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{}]
```  

La expresión pasada se analizará como una expresión null; estas entradas no se evalúan para el tipo null y, por tanto, se devuelven undefined.

```  
SELECT    
    StringToNull("true"), 
    StringToNull(false), 
    StringToNull(undefined),
    StringToNull(NaN) 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{}]
```  

####  <a name="bk_stringtonumber"></a> StringToNumber  
 Devuelve la expresión traducida a un número. Si no se puede traducir la expresión, devuelve undefined.  
  
 **Sintaxis**  
  
```  
StringToNumber(<expr>)  
```  
  
 **Argumentos**  
  
- `expr`  
  
   Es cualquier expresión escalar válida que se evaluará como expresión de número JSON. Los números en JSON deben ser un número entero o de punto flotante. Para más información sobre el formato JSON, consulte [json.org](https://json.org/)  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de número o undefined.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo se comporta StringToNumber en tipos diferentes. 

Se admiten espacios en blanco únicamente antes o después del número.

```  
SELECT 
    StringToNumber("1.000000") AS num1, 
    StringToNumber("3.14") AS num2,
    StringToNumber("   60   ") AS num3, 
    StringToNumber("-1.79769e+308") AS num4
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
{{"num1": 1, "num2": 3.14, "num3": 60, "num4": -1.79769e+308}}
```  

En JSON, un número válido debe ser un número entero o de punto flotante.

```  
SELECT   
    StringToNumber("0xF")
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
{{}}
```  

La expresión pasada se analizará como una expresión de número; estas entradas no se evalúan para el tipo número y, por tanto, se devuelven undefined. 

```  
SELECT 
    StringToNumber("99     54"),   
    StringToNumber(undefined),
    StringToNumber("false"),
    StringToNumber(false),
    StringToNumber(" "),
    StringToNumber(NaN)
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
{{}}
```  

####  <a name="bk_stringtoobject"></a> StringToObject  
 Devuelve la expresión traducida a un objeto. Si no se puede traducir la expresión, devuelve undefined.  
  
 **Sintaxis**  
  
```  
StringToObject(<expr>)  
```  
  
 **Argumentos**  
  
- `expr`  
  
   Es cualquier expresión escalar válida que se evaluará como expresión de objeto JSON. Tenga en cuenta que los valores de cadena anidados deben escribirse entre comillas dobles para ser válidos. Para más información sobre el formato JSON, consulte [json.org](https://json.org/)  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de objeto o undefined.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo se comporta StringToObject en tipos diferentes. 
  
 Los siguientes son ejemplos con entradas válidas.

``` 
SELECT 
    StringToObject("{}") AS obj1, 
    StringToObject('{"A":[1,2,3]}') AS obj2,
    StringToObject('{"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]}') AS obj3, 
    StringToObject("{\"C\":[{\"c1\":[5,6,7]},{\"c2\":8},{\"c3\":9}]}") AS obj4
``` 

El conjunto de resultados es el siguiente:

```
[{"obj1": {}, 
  "obj2": {"A": [1,2,3]}, 
  "obj3": {"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]},
  "obj4": {"C":[{"c1":[5,6,7]},{"c2":8},{"c3":9}]}}]
```

 Los siguientes son ejemplos con una entrada no válida.
Aunque son válidas dentro de una consulta, no se analizarán en objetos válidos. Las cadenas dentro de la cadena de objeto deben escaparse "{\\"a\\":\\"str\\"}" o la comilla que la rodea debe ser simple '{"a": "str"}'.

Las comillas simples que rodean a los nombres de propiedad no son válidos en JSON.

``` 
SELECT 
    StringToObject("{'a':[1,2,3]}")
```

El conjunto de resultados es el siguiente:

```  
[{}]
```  

Los nombres de propiedad sin comillas circundantes no son válidos en JSON.

``` 
SELECT 
    StringToObject("{a:[1,2,3]}")
```

El conjunto de resultados es el siguiente:

```  
[{}]
``` 

Los siguientes son ejemplos con una entrada no válida.

 La expresión pasada se analizará como un objeto JSON; estas entradas no se evalúan para el tipo de objeto y, por tanto, se devuelven undefined.

``` 
SELECT 
    StringToObject("}"),
    StringToObject("{"),
    StringToObject("1"),
    StringToObject(NaN), 
    StringToObject(false), 
    StringToObject(undefined)
``` 
 
 El conjunto de resultados es el siguiente:

```
[{}]
```

####  <a name="bk_substring"></a> SUBSTRING  
 Devuelve parte de una expresión de cadena a partir de la posición de base cero del carácter especificado y continúa hasta la longitud especificada, o hasta el final de la cadena.  
  
 **Sintaxis**  
  
```  
SUBSTRING(<str_expr>, <num_expr>, <num_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
- `num_expr`  
  
   Cualquier expresión numérica válida para denotar el carácter inicial y final.    
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se devuelve la subcadena de "abc" que empieza por 1 y tiene una longitud de 1 carácter.  
  
```  
SELECT SUBSTRING("abc", 1, 1) AS substring  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"substring": "b"}]  
```  
####  <a name="bk_tostring"></a> ToString  
 Devuelve una representación de cadena de una expresión escalar. 
  
 **Sintaxis**  
  
```  
ToString(<expr>)
```  
  
 **Argumentos**  
  
- `expr`  
  
   Es cualquier expresión escalar válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo se comporta ToString en tipos diferentes.   
  
```  
SELECT 
    ToString(1.0000) AS str1, 
    ToString("Hello World") AS str2, 
    ToString(NaN) AS str3, 
    ToString(Infinity) AS str4,
    ToString(IS_STRING(ToString(undefined))) AS str5, 
    ToString(0.1234) AS str6, 
    ToString(false) AS str7, 
    ToString(undefined) AS str8
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"str1": "1", "str2": "Hello World", "str3": "NaN", "str4": "Infinity", "str5": "false", "str6": "0.1234", "str7": "false"}]  
```  
 Dada la entrada siguiente:
```  
{"Products":[{"ProductID":1,"Weight":4,"WeightUnits":"lb"},{"ProductID":2,"Weight":32,"WeightUnits":"kg"},{"ProductID":3,"Weight":400,"WeightUnits":"g"},{"ProductID":4,"Weight":8999,"WeightUnits":"mg"}]}
```    
 En el ejemplo siguiente se muestra cómo se puede usar ToString con otras funciones de cadena como CONCAT.   
 
```  
SELECT 
CONCAT(ToString(p.Weight), p.WeightUnits) 
FROM p in c.Products 
```  

El conjunto de resultados es el siguiente:  
  
```  
[{"$1":"4lb" },
{"$1":"32kg"},
{"$1":"400g" },
{"$1":"8999mg" }]

```  
Dada la entrada siguiente:
```
{"id":"08259","description":"Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX","nutrients":[{"id":"305","description":"Caffeine","units":"mg"},{"id":"306","description":"Cholesterol, HDL","nutritionValue":30,"units":"mg"},{"id":"307","description":"Sodium, NA","nutritionValue":612,"units":"mg"},{"id":"308","description":"Protein, ABP","nutritionValue":60,"units":"mg"},{"id":"309","description":"Zinc, ZN","nutritionValue":null,"units":"mg"}]}
```
En el ejemplo siguiente se muestra cómo se puede usar ToString con otras funciones de cadena como REPLACE.   
```
SELECT 
    n.id AS nutrientID,
    REPLACE(ToString(n.nutritionValue), "6", "9") AS nutritionVal
FROM food 
JOIN n IN food.nutrients
```
El conjunto de resultados es el siguiente:  
 ```
[{"nutrientID":"305"},
{"nutrientID":"306","nutritionVal":"30"},
{"nutrientID":"307","nutritionVal":"912"},
{"nutrientID":"308","nutritionVal":"90"},
{"nutrientID":"309","nutritionVal":"null"}]
``` 
 
####  <a name="bk_trim"></a> TRIM  
 Devuelve una expresión de cadena después de quitar los espacios en blanco iniciales y finales.  
  
 **Sintaxis**  
  
```  
TRIM(<str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar TRIM en una consulta.  
  
```  
SELECT TRIM("   abc") AS t1, TRIM("   abc   ") AS t2, TRIM("abc   ") AS t3, TRIM("abc") AS t4
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"t1": "abc", "t2": "abc", "t3": "abc", "t4": "abc"}]  
``` 
####  <a name="bk_upper"></a> UPPER  
 Devuelve una expresión de cadena después de convertir datos de caracteres en minúsculas a mayúsculas.  
  
 **Sintaxis**  
  
```  
UPPER(<str_expr>)  
```  
  
 **Argumentos**  
  
- `str_expr`  
  
   Es cualquier expresión de cadena válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de cadena.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo utilizar UPPER en una consulta.  
  
```  
SELECT UPPER("Abc") AS upper  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"upper": "ABC"}]  
```

## <a id="array-functions"></a>Funciones de matriz

Las siguientes funciones escalares realizan una operación en un valor de entrada de matriz y devuelven un valor numérico, booleano o de matriz:
  
||||  
|-|-|-|  
|[ARRAY_CONCAT](#bk_array_concat)|[ARRAY_CONTAINS](#bk_array_contains)|[ARRAY_LENGTH](#bk_array_length)|  
|[ARRAY_SLICE](#bk_array_slice)|||  
  
####  <a name="bk_array_concat"></a> ARRAY_CONCAT  
 Devuelve una matriz que es el resultado de concatenar dos o más valores de la matriz.  
  
 **Sintaxis**  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 **Argumentos**  
  
- `arr_expr`  
  
   Es cualquier expresión de matriz válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión de matriz.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo concatenar dos matrices.  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"]) AS arrayConcat 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"arrayConcat": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <a name="bk_array_contains"></a> ARRAY_CONTAINS  
Devuelve un valor booleano que indica si la matriz contiene el valor especificado. Puede buscar una coincidencia parcial o total de un objeto mediante el uso de una expresión booleana dentro del comando. 

**Sintaxis**  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr> [, bool_expr])  
```  
  
 **Argumentos**  
  
- `arr_expr`  
  
   Es cualquier expresión de matriz válida.  
  
- `expr`  
  
   Es cualquier expresión válida.  

- `bool_expr`  
  
   Es cualquier expresión booleana. Si se establece en ' true' y el valor de búsqueda especificado es un objeto, el comando busca una coincidencia parcial (el objeto de búsqueda es un subconjunto de uno de los objetos). Si se establece en 'false', el comando busca una coincidencia total de todos los objetos dentro de la matriz. Si no se especifica, el valor predeterminado es 'false'. 
  
  **Tipos de valor devuelto**  
  
  Devuelve un valor booleano.  
  
  **Ejemplos**  
  
  En el siguiente ejemplo se muestra cómo comprobar la pertenencia a una matriz con ARRAY_CONTAINS.  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples") AS b1,  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes") AS b2  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"b1": true, "b2": false}]  
```  

En el siguiente ejemplo se indica cómo buscar una coincidencia parcial de JSON en una matriz mediante ARRAY_CONTAINS.  
  
```  
SELECT  
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}, true) AS b1, 
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}) AS b2,
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "mangoes"}, true) AS b3 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{
  "b1": true,
  "b2": false,
  "b3": false
}] 
```  
  
####  <a name="bk_array_length"></a> ARRAY_LENGTH  
 Devuelve el número de elementos de la expresión de matriz especificada.  
  
 **Sintaxis**  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 **Argumentos**  
  
- `arr_expr`  
  
   Es cualquier expresión de matriz válida.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica.  
  
  **Ejemplos**  
  
  En el siguiente ejemplo se muestra cómo obtener la longitud de una matriz con ARRAY_LENGTH.  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"]) AS len  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{"len": 3}]  
```  
  
####  <a name="bk_array_slice"></a> ARRAY_SLICE  
 Devuelve parte de una expresión de matriz.
  
 **Sintaxis**  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argumentos**  
  
- `arr_expr`  
  
   Es cualquier expresión de matriz válida.  
  
- `num_expr`  
  
   Índice numérico basado en cero en el que comienza la matriz. Se pueden usar valores negativos para especificar el índice inicial en relación con el último elemento de la matriz; por ejemplo, -1 hace referencia al último elemento de la matriz.  

- `num_expr`  

   Número máximo de elementos en la matriz resultante.    

  **Tipos de valor devuelto**  
  
  Devuelve una expresión de matriz.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo obtener distintos segmentos de una matriz con ARRAY_SLICE.  
  
```  
SELECT
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1) AS s1,  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1) AS s2,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 1) AS s3,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 2) AS s4,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 0) AS s5,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1000) AS s6,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, -100) AS s7      
  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{  
           "s1": ["strawberries", "bananas"],   
           "s2": ["strawberries"],
           "s3": ["strawberries"],  
           "s4": ["strawberries", "bananas"], 
           "s5": [],
           "s6": ["strawberries", "bananas"],
           "s7": [] 
}]  
```  
## <a id="date-time-functions"></a>Funciones de fecha y hora

Las funciones escalares siguientes le permiten obtener la fecha y la hora UTC actuales de dos formas: una marca de tiempo numérica cuyo valor es la época de Unix en milisegundos, o una cadena que se ajusta al formato ISO 8601. 

|||
|-|-|
|[GETCURRENTDATETIME](#bk_get_current_date_time)|[GETCURRENTTIMESTAMP](#bk_get_current_timestamp)||

####  <a name="bk_get_current_date_time"></a> GETCURRENTDATETIME
 Devuelve la fecha y hora UTC actuales como una cadena ISO 8601.
  
 **Sintaxis**
  
```
GETCURRENTDATETIME ()
```
  
  **Tipos de valor devuelto**
  
  Devuelve el valor de cadena de fecha y hora UTC actuales ISO 8601. 

  Se expresa en el formato AAAA-MM-DDThh:mm:ss.sssZ donde:
  
  |||
  |-|-|
  |AAAA|año con cuatro dígitos|
  |MM|mes con dos dígitos (01 = enero, etc.)|
  |DD|día con dos dígitos del mes (de 01 a 31)|
  |T|indica el comienzo de los elementos de hora|
  |hh|hora con dos dígitos (de 00 a 23)|
  |MM|minutos con dos dígitos (de 00 a 59)|
  |ss|segundos con dos dígitos (de 00 a 59)|
  |.sss|tres dígitos de fracciones decimales de segundo|
  |Z|Indicador de UTC (hora universal coordinada)||
  
  Para más detalles sobre el formato ISO 8601, consulte [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601)

  **Comentarios:**

  GETCURRENTDATETIME es una función no determinista. 
  
  El resultado devuelto está en UTC (hora universal coordinada).

  **Ejemplos**  
  
  El ejemplo siguiente muestra cómo obtener la fecha y hora UTC actuales mediante la función integrada GetCurrentDateTime.
  
```  
SELECT GETCURRENTDATETIME() AS currentUtcDateTime
```  
  
 Este es un conjunto de resultados de ejemplo.
  
```  
[{
  "currentUtcDateTime": "2019-05-03T20:36:17.784Z"
}]  
```  

####  <a name="bk_get_current_timestamp"></a> GETCURRENTTIMESTAMP
 Devuelve el número de milisegundos que han transcurrido desde las 00:00:00 del jueves, 1 de enero de 1970. 
  
 **Sintaxis**  
  
```  
GETCURRENTTIMESTAMP ()  
```  
  
  **Tipos de valor devuelto**  
  
  Devuelve un valor numérico, el número actual de milisegundos que han transcurrido desde la época de Unix, es decir, el número de milisegundos que han transcurrido desde las 00:00:00 del jueves, 1 de enero de 1970.

  **Comentarios:**

  GETCURRENTTIMESTAMP es una función no determinista.
  
  El resultado devuelto está en UTC (hora universal coordinada).

  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo obtener la marca de tiempo actual mediante la función integrada GetCurrentTimestamp.
  
```  
SELECT GETCURRENTTIMESTAMP() AS currentUtcTimestamp
```  
  
 Este es un conjunto de resultados de ejemplo.
  
```  
[{
  "currentUtcTimestamp": 1556916469065
}]  
```

## <a id="spatial-functions"></a>Funciones espaciales

Cosmos DB admite las siguientes funciones integradas de Open Geospatial Consortium (OGC) para realizar consultas geoespaciales. Las siguientes funciones escalares realizan una operación en un valor de entrada de objeto espacial y devuelven un valor numérico o booleano.  
  
|||||
|-|-|-|-|
|[ST_DISTANCE](#bk_st_distance)|[ST_WITHIN](#bk_st_within)|[ST_INTERSECTS](#bk_st_intersects)|[ST_ISVALID](#bk_st_isvalid)|
|[ST_ISVALIDDETAILED](#bk_st_isvaliddetailed)||||
  
####  <a name="bk_st_distance"></a> ST_DISTANCE  
 Devuelve la distancia entre dos expresiones Point, Polygon o LineString de GeoJSON.  
  
 **Sintaxis**  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumentos**  
  
- `spatial_expr`  
  
   Es cualquier expresión válida de objeto de tipo Point, Polygon o LineString de GeoJSON.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión numérica con la distancia, que se expresa en metros para el sistema de referencia predeterminado.  
  
  **Ejemplos**  
  
  En el siguiente ejemplo se muestra cómo se devuelven todos los documentos de la familia en un radio de 30 km a partir de la ubicación especificada mediante la función integrada ST_DISTANCE. .  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{  
  "id": "WakefieldFamily"  
}]  
```  
  
####  <a name="bk_st_within"></a> ST_WITHIN  
 Devuelve una expresión condicional que indica si el objeto de GeoJSON (Point, Polygon o LineString) especificado en el primer argumento se encuentra en el objeto de GeoJSON (Point, Polygon o LineString) del segundo.  
  
 **Sintaxis**  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumentos**  
  
- `spatial_expr`  
  
   Es cualquier expresión válida de objeto de tipo Point, Polygon o LineString de GeoJSON.  
 
- `spatial_expr`  
  
   Es cualquier expresión válida de objeto de tipo Point, Polygon o LineString de GeoJSON.  
  
  **Tipos de valor devuelto**  
  
  Devuelve un valor booleano.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo buscar todos los documentos de la familia en un polígono con ST_WITHIN.  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{ "id": "WakefieldFamily" }]  
```  

####  <a name="bk_st_intersects"></a> ST_INTERSECTS  
 Devuelve una expresión condicional que indica si el objeto de GeoJSON (Point, Polygon o LineString) especificado en el primer argumento forma intersección con el objeto de GeoJSON (Point, Polygon o LineString) del segundo.  
  
 **Sintaxis**  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumentos**  
  
- `spatial_expr`  
  
   Es cualquier expresión válida de objeto de tipo Point, Polygon o LineString de GeoJSON.  
 
- `spatial_expr`  
  
   Es cualquier expresión válida de objeto de tipo Point, Polygon o LineString de GeoJSON.  
  
  **Tipos de valor devuelto**  
  
  Devuelve un valor booleano.  
  
  **Ejemplos**  
  
  En el siguiente ejemplo se muestra cómo buscar todas las áreas de intersección con el polígono indicado.  
  
```  
SELECT a.id
FROM Areas a
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{ "id": "IntersectingPolygon" }]  
```  
  
####  <a name="bk_st_isvalid"></a> ST_ISVALID  
 Devuelve un valor booleano que indica si la expresión de Point, Polygon o LineString de GeoJSON especificada es válida.  
  
 **Sintaxis**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Argumentos**
  
- `spatial_expr`  
  
   Es cualquier expresión válida de objeto de tipo Point, Polygon o LineString de GeoJSON.  
  
  **Tipos de valor devuelto**  
  
  Devuelve una expresión condicional.  
  
  **Ejemplos**  
  
  En el ejemplo siguiente se muestra cómo comprobar si un punto es válido con ST_VALID.  
  
  Por ejemplo, este punto tiene un valor de latitud que no está en el intervalo válido de valores [-90, 90], por lo que la consulta devuelve false.  
  
  Para los polígonos, la especificación de GeoJSON requiere que el último par de coordenadas indicado sea igual que el primero, para crear una forma cerrada. El orden de los puntos dentro de un polígono debe especificarse siguiendo el sentido contrario al de las agujas del reloj. Un polígono cuyos puntos se hayan especificado en el sentido de las agujas del reloj representa el inverso de la región dentro de él.  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] }) AS b 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{ "b": false }]  
```  
  
####  <a name="bk_st_isvaliddetailed"></a> ST_ISVALIDDETAILED  
 Devuelve un valor JSON que contiene un valor booleano si la expresión de Point, Polygon o LineString de GeoJSON especificada es válida; si no es válida, devuelve también el motivo como un valor de cadena.  
  
 **Sintaxis**  
  
```  
ST_ISVALIDDETAILED(<spatial_expr>)  
```  
  
 **Argumentos**  
  
- `spatial_expr`  
  
   Es cualquier expresión válida de objeto Point o Polygon de GeoJSON.  
  
  **Tipos de valor devuelto**  
  
  Devuelve un valor JSON que contiene un valor booleano si la expresión de punto o polígono GeoJSON especificada es válida; si no es válida, devuelve también el motivo en forma de valor de cadena.  
  
  **Ejemplos**  
  
  En el siguiente ejemplo se muestra cómo comprobar la validez (con detalles) mediante ST_ISVALIDDETAILED.  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
}) AS b 
```  
  
 El conjunto de resultados es el siguiente:  
  
```  
[{  
  "b": {
    "valid": false,
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a Azure Cosmos DB](introduction.md)
- [UDF](sql-query-udfs.md)
- [Agregados](sql-query-aggregates.md)