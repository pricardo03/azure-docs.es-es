---
title: 'Sintaxis de búsqueda lambda: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre la sintaxis de búsqueda lambda que puede usar en Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 284f1d90f043e2634e143508e2ab0e98cd309f46
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902695"
---
# <a name="lambda-search-syntax"></a>Sintaxis de búsqueda lambda

Cada cadena de consulta de búsqueda *lambda* describe un patrón de grafo. Una consulta debe tener al menos un nodo de inicio que especifique el nodo del grafo desde el que se puede iniciar el recorrido. Para especificar un nodo de inicio, llame al método *MAG.StartFrom()* y pase los identificadores de uno o varios nodos o un objeto de consulta que especifique las restricciones de búsqueda. El método *StartFrom()* tiene tres sobrecargas. Todas ellas aceptan dos argumentos, siendo el segundo opcional. El primer argumento puede ser un entero largo, una colección enumerable de enteros largos o una cadena que representa un objeto JSON, con la misma semántica que en la búsqueda *json*:
```
StartFrom(long cellid, IEnumerable<string> select = null)
StartFrom(IEnumerable<long> cellid, IEnumerable<string> select = null)
StartFrom(string queryObject, IEnumerable<string> select = null)
```

El proceso de escribir una consulta de búsqueda lambda es desplazarse de un nodo a otro. Para especificar el tipo de borde por el que desplazarse, use *FollowEdge()* y pase los tipos de borde deseados. *FollowEdge()* acepta un número arbitrario de argumentos de cadena:
```
FollowEdge(params string[] edgeTypes)
```
> [!NOTE]
> Si no es importante el tipo de borde que se va a seguir, basta con omitir *FollowEdge()* entre dos nodos: la consulta se desplazará por todos los bordes posibles entre estos dos nodos.

Se pueden especificar acciones de recorrido para realizar en un nodo mediante *VisitNode()*; por ejemplo, si detenerse en este nodo y devolver la ruta de acceso actual como resultado, o seguir explorando el grafo.  El tipo de enumeración *Action* (Acción) define dos tipos de acciones: *Action.Return* y *Action.Continue*. Se puede pasar un valor de enumeración así directamente a *VisitNode()*, o combinarlos con bit a bit o un operador "&". Cuando se combinan dos acciones, se pueden aceptar ambas. Nota: No use bit a bit o el operador "|" en las acciones. Si lo hace, la consulta terminará sin devolver nada. Al omitir *VisitNode()* ente dos llamadas *FollowEdge()*, la consulta explorará el grafo incondicionalmente tras la llegada a un nodo.

```
VisitNode(Action action, IEnumerable<string> select = null)
```

Para *VisitNode()*, también se puede pasar una expresión lambda de tipo *Expression\<Func\<INode, Action\>\>*, que acepta un elemento *INode* y devuelve una acción de recorrido:

```
VisitNode(Expression<Func<INode, Action>> action, IEnumerable<string> select = null)
```

## <a name="inode"></a>*INode* 

*INode* proporciona interfaces de acceso a datos de *solo lectura* y funciones auxiliares integradas en un nodo. 

### <a name="basic-data-access-interfaces"></a>Interfaces básicas de acceso a datos

##### <a name="long-cellid"></a>long CellID

El identificador de 64 bits del nodo. 

##### <a name="t-getfieldtstring-fieldname"></a>T GetField\<T\>(string fieldName)

Obtiene el valor de la propiedad especificada. *T* es el tipo deseado con el que se supone que se va a interpretar el campo. Si el tipo deseado no se puede convertir implícitamente desde el tipo de campo, se intentará la conversión automática del tipo. Nota: Cuando la propiedad es multivalor, *GetField\<cadena\>*  hará que la lista se serialice en una cadena Json ["val1", "val2",...]. Si la propiedad no existe, se produce una excepción y se anula la exploración de grafo actual.

##### <a name="bool-containsfieldstring-fieldname"></a>bool ContainsField(string fieldName)

Indica si existe un campo con el nombre especificado en el nodo actual.

##### <a name="string-getstring-fieldname"></a>string get(string fieldName)

Funciona igual que *GetField\<cadena\>(fieldName)*. Sin embargo, no inicia excepciones cuando el campo no se encuentra, sino que, en su lugar, devuelve una cadena vacía ("").

##### <a name="bool-hasstring-fieldname"></a>bool has(string fieldName)

Indica si la propiedad especificada existe en el nodo actual. Igual que *ContainsField(fieldName)*.

##### <a name="bool-hasstring-fieldname-string-value"></a>bool has(string fieldName, string value)

Indica si la propiedad tiene el valor especificado. 

##### <a name="int-countstring-fieldname"></a>int count(string fieldname)

Obtenga el recuento de valores de una propiedad determinada. Cuando la propiedad no existe, se devuelve 0.

### <a name="built-in-helper-functions"></a>Funciones auxiliares integradas

##### <a name="action-returnifbool-condition"></a>Action return_if(bool condition)

Devuelve *Action.Return* si la condición es *true*. Si la condición es *false* y esta expresión no está combinada con otras acciones por un bit a bit y un operador, se anula la exploración del grafo.

##### <a name="action-continueifbool-condition"></a>Action continue_if(bool condition)

Devuelve *Action.Continue* si la condición es *true*. Si la condición es *false* y esta expresión no está combinada con otras acciones por un bit a bit y un operador, se anula la exploración del grafo.

##### <a name="bool-dicedouble-p"></a>bool dice(double p)

Genera un número aleatorio que es mayor o igual que 0,0 y menor que 1,0. Esta función devuelve *true* solo si el número es menor o igual que *p*.

En comparación con la búsqueda *json*, la búsqueda *lambda* es más expresiva: las expresiones lambda de C# pueden usarse directamente para especificar patrones de consulta. Estos son dos ejemplos.

```
MAG.StartFrom(@"{
    type  : ""ConferenceSeries"",
    match : {
        FullName : ""graph""
    }
}", new List<string>{ "FullName", "ShortName" })
.FollowEdge("ConferenceInstanceIDs")
.VisitNode(v => v.return_if(v.GetField<DateTime>("StartDate").ToString().Contains("2014")),
        new List<string>{ "FullName", "StartDate" })
```

```
MAG.StartFrom(@"{
    type  : ""Affiliation"",
    match : {
        Name : ""microsoft""
    }
}").FollowEdge("PaperIDs")
.VisitNode(v => v.return_if(v.get("NormalizedTitle").Contains("graph") || v.GetField<int>("CitationCount") > 100),
        new List<string>{ "OriginalTitle", "CitationCount" })
```
