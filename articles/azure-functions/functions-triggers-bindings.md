---
title: Desencadenadores y enlaces en Azure Functions
description: Aprenda a utilizar desencadenadores y enlaces para conectarse a la función de Azure a eventos en línea y servicios en la nube.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: 3865f748a9ca2fe09660d6454542d64f73a8e3c1
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58889845"
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions

En este artículo obtendrá información sobre los conceptos de alto nivel que rodean los enlaces y desencadenadores de functions.

Los desencadenadores son lo que ocasionar una función que se ejecuta. Un desencadenador define cómo se invoca una función y una función debe tener exactamente un desencadenador. Los desencadenadores tienen datos asociados, que a menudo se proporcionan como la carga de la función. 

Enlace a una función es una manera de conectarse mediante declaración otro recurso a la función; los enlaces pueden estarlo como *enlaces de entrada*, *enlaces de salida*, o ambos. Datos de los enlaces se proporcionan a la función como parámetros.

Puede mezclar y coincidir con enlaces diferentes para satisfacer sus necesidades. Los enlaces son opcionales y una función puede tener uno o varios de entrada o enlaces de salida.

Desencadenadores y enlaces permiten evitar codificar acceso a otros servicios. La función recibe los datos (por ejemplo, el contenido de un mensaje de cola) en parámetros de función. El usuario envía datos (por ejemplo, para crear un mensaje de la cola) mediante el valor devuelto de la función. 

Considere los siguientes ejemplos de cómo puede implementar las distintas funciones.

| Escenario de ejemplo | Desencadenador | Enlace de entrada | Enlace de salida |
|-------------|---------|---------------|----------------|
| Llega un nuevo mensaje de cola que ejecuta una función para escribir a otra cola. | Queue<sup>*</sup> | *None* | Queue<sup>*</sup> |
|Un trabajo programado lee el contenido de almacenamiento de blobs y crea un nuevo documento de Cosmos DB. | Temporizador | Blob Storage | Cosmos DB |
|La cuadrícula de eventos se utiliza para leer una imagen en Blob Storage y un documento de Cosmos DB para enviar un correo electrónico. | Event Grid | BLOB Storage y Cosmos DB | SendGrid |
| Un webhook que usa Microsoft Graph para actualizar una hoja de Excel. | HTTP | *None* | Microsoft Graph |

<sup>\*</sup> Representa diferentes colas

Estos ejemplos no pretende ser exhaustivo, pero sirven para ilustrar cómo se pueden utilizar desencadenadores y enlaces juntos.

###  <a name="trigger-and-binding-definitions"></a>Definiciones de desencadenadores y enlaces

Desencadenadores y enlaces se definen de forma diferente según el enfoque de desarrollo.

| Plataforma | Desencadenadores y enlaces se configuran por... |
|-------------|--------------------------------------------|
| C#biblioteca de clases | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;decoración de métodos y parámetros con C# atributos |
| Todos los demás (incluido Azure portal) | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Actualizando [function.json](./functions-reference.md) ([esquema](http://json.schemastore.org/function)) |

El portal proporciona una interfaz de usuario para esta configuración, pero puede editar el archivo directamente abriendo el **editor avanzado** disponible a través de la **integrar** pestaña de la función.

En. NET, el tipo de parámetro define el tipo de datos para datos de entrada. Por ejemplo, use `string` para enlazar con el texto de un desencadenador de cola, una matriz de bytes que se lee como binaria y un tipo personalizado para deserializar un objeto.

Para los lenguajes que se escriben dinámicamente, como JavaScript, use la propiedad `dataType` del archivo *function.json*. Por ejemplo, para leer el contenido de una solicitud HTTP en formato binario, establezca `dataType` en `binary`:

```json
{
    "dataType": "binary",
    "type": "httpTrigger",
    "name": "req",
    "direction": "in"
}
```

Otras opciones para `dataType` son `stream` y `string`.

## <a name="binding-direction"></a>Dirección de los enlaces

Todos los desencadenadores y enlaces tienen una propiedad `direction` en el archivo [function.json](./functions-reference.md):

- En el caso de los desencadenadores, esta propiedad siempre aparece como `in`
- Los enlaces de entrada y de salida usan `in` y `out`
- Algunos enlaces admiten la dirección especial `inout`. Si usas `inout`, solo el **editor avanzado** está disponible a través de la **integrar** ficha en el portal.

Cuando se usan [atributos en una biblioteca de clases](functions-dotnet-class-library.md) para configurar los desencadenadores y los enlaces, la dirección se proporciona en un constructor de atributos o se deduce del tipo de parámetro.

## <a name="supported-bindings"></a>Enlaces admitidos

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Para información sobre qué enlaces están en versión preliminar o aprobados para su uso en producción, consulte los [lenguajes admitidos](supported-languages.md).

## <a name="resources"></a>Recursos
- [Patrones y expresiones de enlace](./functions-bindings-expressions-patterns.md)
- [Utilice el valor devuelto de función de Azure](./functions-bindings-return-value.md)
- [Cómo registrar una expresión de enlace](./functions-bindings-register.md)
- Prueba:
  - [Estrategias para probar el código en Azure Functions](functions-test-a-function.md)
  - [Ejecución manual de una función no desencadenada por HTTP](functions-manually-run-non-http.md)
- [Control de errores de enlace](./functions-bindings-errors.md)

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Registrar las extensiones de enlace de Azure Functions](./functions-bindings-register.md)
