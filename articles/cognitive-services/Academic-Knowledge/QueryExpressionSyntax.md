---
title: Sintaxis de expresión de consulta en Academic Knowledge API | Microsoft Docs
description: Aprenda a utilizar la sintaxis de expresión de consulta de Academic Knowledge API para Microsoft Cognitive Services.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: 6ec338fff09954e2f14066ce2b83bc1228794af8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380047"
---
# <a name="query-expression-syntax"></a>Sintaxis de expresión de consulta

Hemos visto que la respuesta a una solicitud de **interpretación** incluye una expresión de consulta. La gramática que interpretaba la consulta del usuario ha creado una expresión de consulta para cada interpretación. Se puede utilizar una expresión de consulta para emitir una solicitud de **evaluación** a fin de recuperar los resultados de la búsqueda de entidad.

También puede crear sus propias expresiones de consulta y usarlas en una solicitud de **evaluación**. Esto puede ser útil si está creando su propia interfaz de usuario que elabora una expresión de consulta en respuesta a las acciones del usuario. Para ello, debe conocer la sintaxis de expresiones de consulta.  

Cada atributo de entidad que se puede incluir en una expresión de consulta tiene un tipo de datos específico y un conjunto de posibles operadores de consulta. El conjunto de atributos de entidad y los operadores admitidos para cada atributo se especifican en [Atributos de entidad](EntityAttributes.md). Una consulta de valor único requiere el atributo para admitir la operación *Equals*. Una consulta de prefijo requiere el atributo para admitir la operación *StartsWith*. Las consultas de rango numérico requieren el atributo para admitir la operación *IsBetween*.

Algunos de los datos de entidad se almacenan como atributos compuestos, como se indica mediante un punto '.' en el nombre del atributo. Por ejemplo, la información de autor/afiliación se representa como un atributo compuesto. Contiene 4 componentes: AuN, AuId, AfN, AfId. Estos componentes son partes independientes de datos que forman un valor de atributo de entidad única.


**Atributo de cadena: valor único** (incluye coincidencias con sinónimos)  
Ti = 'indexación por análisis semántico latente'  
Composite(AA.AuN='sue dumais')

**Atributo de cadena: valor único exacto** (coincide solo con valores canónicos)  
Ti = 'indexación por análisis semántico latente'  
Composite(AA.AuN=='susan t dumais')
     
**Atributo de cadena: valor de prefijo**   
Ti = 'indexación por análisis semán'...  
Composite(AA.AuN='sue du'...)

**Atributo numérico: valor único**  
Y=2010
 
**Atributo numérico: valor de intervalo**  
Y>2005  
Y>=2005  
Y<2010  
Y<=2010  
Y=\[2010, 2012\) (incluye solo el valor de límite izquierdo: 2010, 2011)  
Y=\[2010, 2012\] (incluye ambos valores de límite: 2010, 2011, 2012)
 
**Atributo numérico: valor de prefijo**  
Y='19'... (cualquier valor numérico que empiece por 19) 
 
**Atributo de fecha: valor único**  
D='2010-02-04'

**Atributo de fecha: valor de intervalo**  
D>'2010-02-03'  
D=['2010-02-03','2010-02-05']

**Consultas And/Or:**  
And(Y=1985, Ti='sistemas electrónicos sin orden')  
Or(Ti='sistemas electrónicos sin orden,' Ti='principios de tolerancia a errores y práctica')  
And(Or(Y=1985,Y=2008), Ti=''sistemas electrónicos sin orden')
 
**Consultas compuestas:**  
Para consultar los componentes de un atributo compuesto, debe incluir la parte de la expresión de consulta que hace referencia al atributo compuesto en la función Composite(). 

Por ejemplo, para consultar artículos por nombre de autor, utilice la siguiente consulta:
```
Composite(AA.AuN='mike smith')
```
<br>Para consultar artículos por un autor concreto mientras el autor estaba en una institución determinada, utilice la siguiente consulta:
```
Composite(And(AA.AuN='mike smith',AA.AfN='harvard university'))
```
<br>La función Composite() une las dos partes del atributo compuesto. Esto significa que solo se obtienen los artículos en los que uno de los autores sea "Mike Smith" mientras estaba en Harvard. 

Para consultar artículos por un autor concreto en afiliaciones con (otros) autores de una institución determinada, utilice la siguiente consulta:
```
And(Composite(AA.AuN='mike smith'),Composite(AA.AfN='harvard university'))
```
<br>En esta versión, como Composite() se aplica al autor y a la afiliación de forma individual antes de And(), obtenemos todos los artículos en los que uno de los autores sea "Mike Smith" y una de las afiliaciones de los autores sea "Harvard". Esto parece similar a la consulta del ejemplo anterior, pero no es lo mismo.

En general, considere el siguiente ejemplo: tenemos un atributo compuesto C que tiene dos componentes A y B. Una entidad puede tener varios valores para C. Estas son nuestras entidades:
```
E1: C={A=1, B=1}  C={A=1,B=2}  C={A=2,B=3}
E2: C={A=1, B=3}  C={A=3,B=2}
```

<br>La consulta 
```
Composite(And(C.A=1, C.B=2))
```

<br>devuelve solo las entidades que tienen un valor para C, donde el componente C.A es 1 y el componente C.B es 2. Solo E1 coincide con esta consulta.

La consulta 
```
And(Composite(C.A=1), Composite(C.B=2))
```
<br>coincide con las entidades que tienen un valor para C donde C.A es 1 y que también tienen un valor para C donde C.B es 2. E1 y E2 coinciden con esta consulta.

Tenga en cuenta lo siguiente:  
- No puede hacer referencia a una parte de un atributo compuesto fuera de una función Composite().
- No puede hacer referencia a partes de dos atributos compuestos diferentes dentro de la misma función Composite().
- No puede hacer referencia a un atributo que no forme parte de un atributo compuesto dentro de una función Composite().
