---
title: 'Referencia de esquema del lenguaje de definición de flujo de trabajo: Azure Logic Apps | Microsoft Docs'
description: Escritura de definiciones de flujo de trabajo personalizadas para Azure Logic Apps con el lenguaje de definición de flujo de trabajo
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: reference
ms.date: 04/30/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 3c17ec2133e278b17475e4988e1e9766b1349ba4
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55734647"
---
# <a name="schema-reference-for-workflow-definition-language-in-azure-logic-apps"></a>Referencia de esquema del lenguaje de definición de flujo de trabajo en Azure Logic Apps

Cuando se crea un flujo de trabajo de aplicación lógica con [Azure Logic Apps](../logic-apps/logic-apps-overview.md), la definición subyacente del flujo de trabajo describe la lógica real que se ejecuta para la aplicación lógica. Esta descripción sigue una estructura que se define y valida mediante el esquema de lenguaje de definición de flujo de trabajo, que usa la [notación de objetos JavaScript (JSON)](https://www.json.org/).

## <a name="workflow-definition-structure"></a>Estructura de definición de flujo de trabajo

Una definición de flujo de trabajo tiene al menos un desencadenador que crea una instancia de la aplicación lógica, además de una o varias acciones que ejecuta dicha aplicación.

Esta es la estructura de alto nivel de una definición de flujo de trabajo:

```json
"definition": {
  "$schema": "<workflow-definition-language-schema-version>",
  "contentVersion": "<workflow-definition-version-number>",
  "parameters": { "<workflow-parameter-definitions>" },
  "triggers": { "<workflow-trigger-definitions>" },
  "actions": { "<workflow-action-definitions>" },
  "outputs": { "<workflow-output-definitions>" }
}
```

| Elemento | Obligatorio | DESCRIPCIÓN |
|---------|----------|-------------|
| definición | Sí | El elemento inicial de la definición de flujo de trabajo. |
| $schema | Solo cuando se hace referencia externa a una definición de flujo de trabajo. | La ubicación del archivo de esquema JSON que describe la versión del lenguaje de definición de flujo de trabajo, que puede encontrar aquí: <p>`https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json`</p> |
| contentVersion | Sin  | El número de versión de la definición de flujo de trabajo, que es "1.0.0.0" de forma predeterminada. Para ayudar a identificar y confirmar la definición correcta al implementar un flujo de trabajo, especifique el valor que usará. |
| parameters | Sin  | Las definiciones de uno o varios parámetros que pasan datos al flujo de trabajo. <p><p>Máximo de parámetros: 50 |
| Desencadenadores | Sin  | Las definiciones de uno o varios desencadenadores que crean una instancia del flujo de trabajo. Puede definir más de un desencadenador, pero solo con el lenguaje de definición de flujo de trabajo, no visualmente mediante el Diseñador de aplicaciones lógicas. <p><p>Máximo de desencadenadores: 10 |
| actions | Sin  | Las definiciones de una o varias acciones para ejecutar en el entorno de tiempo de ejecución del flujo de trabajo. <p><p>Máximo de acciones: 250 |
| outputs | Sin  | Las definiciones de las salidas que se devuelven de la ejecución de un flujo de trabajo. <p><p>Máximo de salidas: 10 |
||||

## <a name="parameters"></a>Parámetros

En la sección `parameters`, defina todos los parámetros de flujo de trabajo que usa su aplicación lógica en la implementación para aceptar entradas. Tanto las declaraciones de parámetros como los valores de parámetros son necesarios en la implementación. Antes de poder usar estos parámetros en otras secciones del flujo de trabajo, asegúrese de declarar todos los parámetros en las siguientes secciones. 

Esta es la estructura general de una definición de parámetro:

```json
"parameters": {
  "<parameter-name>": {
    "type": "<parameter-type>",
    "defaultValue": "<default-parameter-value>",
    "allowedValues": [ <array-with-permitted-parameter-values> ],
    "metadata": {
      "key": {
        "name": "<key-value>"
      }
    }
  }
},
```

| Elemento | Obligatorio | Type | DESCRIPCIÓN |
|---------|----------|------|-------------|
| Tipo | Sí | int, float, string, securestring, bool, array, JSON object, secureobject <p><p>**Nota**: Con todas las contraseñas, claves y secretos, use los tipos `securestring` y `secureobject` porque la operación `GET` no los devuelve. | El tipo del parámetro. |
| defaultValue | Sin  | Igual que `type`. | El valor de parámetro predeterminado cuando no se especifica ningún valor al crear una instancia del flujo de trabajo. |
| allowedValues | Sin  | Igual que `type`. | Una matriz con valores que puede aceptar el parámetro. |
| metadata | Sin  | Objeto JSON | Cualquier otro detalle del parámetro, por ejemplo, el nombre o una descripción legible de la aplicación lógica, o los datos de tiempo de diseño que usa Visual Studio u otras herramientas. |
||||

## <a name="triggers-and-actions"></a>Desencadenadores y acciones

En una definición de flujo de trabajo, las secciones `triggers` y `actions` definen las llamadas que se producen durante la ejecución del flujo de trabajo. Para más información sobre estas secciones y la sintaxis, consulte [Desencadenadores y acciones para flujos de trabajo](../logic-apps/logic-apps-workflow-actions-triggers.md).

## <a name="outputs"></a>Salidas

En la sección `outputs`, defina los datos que puede devolver el flujo de trabajo cuando termine de ejecutarse. Por ejemplo, para realizar el seguimiento de un estado o valor específico de cada ejecución, especifique que la salida del flujo de trabajo devuelva esos datos.

> [!NOTE]
> Al responder a las solicitudes entrantes de la API REST de un servicio, no use `outputs`. En su lugar, use el tipo de acción `Response`. Para más información, consulte [Desencadenadores y acciones para flujos de trabajo](../logic-apps/logic-apps-workflow-actions-triggers.md).

Esta es la estructura general de una definición de salida:

```json
"outputs": {
  "<key-name>": {
    "type": "<key-type>",
    "value": "<key-value>"
  }
}
```

| Elemento | Obligatorio | Type | DESCRIPCIÓN |
|---------|----------|------|-------------|
| <*key-name*> | Sí | string | El nombre de clave del valor devuelto de salida. |
| Tipo | Sí | int, float, string, securestring, bool, array, JSON object | El tipo del valor devuelto de salida. |
| value | Sí | Igual que `type`. | El valor devuelto de salida. |
|||||

Para obtener la salida de una ejecución de flujo de trabajo, revise el historial y los detalles de ejecución de la aplicación lógica en Azure Portal o use la [API REST de flujo de trabajo](https://docs.microsoft.com/rest/api/logic/workflows). También puede pasar la salida a sistemas externos, por ejemplo, a Power BI, para crear paneles.

<a name="expressions"></a>

## <a name="expressions"></a>Expresiones

Con JSON, puede tener valores literales que existen en tiempo de diseño, por ejemplo:

```json
"customerName": "Sophia Owen",
"rainbowColors": ["red", "orange", "yellow", "green", "blue", "indigo", "violet"],
"rainbowColorsCount": 7
```

También puede tener valores que no existen hasta el momento de ejecución. Para representar estos valores, puede usar *expresiones*, que se evalúan en tiempo de ejecución. Una expresión es una secuencia que puede contener una o varias [funciones](#functions), [operadores](#operators), variables, valores explícitos o constantes. En la definición de flujo de trabajo, se puede usar una expresión en cualquier lugar de un valor de cadena JSON agregando delante de la expresión el prefijo de signo de arroba (\@). Al evaluar una expresión que representa un valor JSON, se extrae el cuerpo de la expresión quitando el carácter \@ y siempre da como resultado otro valor JSON.

Por ejemplo, en el caso de la propiedad `customerName` anteriormente definida, puede obtener su valor mediante la función [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) en una expresión y asignar ese valor a la propiedad `accountName`:

```json
"customerName": "Sophia Owen",
"accountName": "@parameters('customerName')"
```

La *interpolación de cadena* también le permite usar varias expresiones dentro de cadenas que están encapsuladas mediante el carácter \@ y llaves ({}). Esta es la sintaxis:

```json
@{ "<expression1>", "<expression2>" }
```

El resultado siempre es una cadena, de forma que esta funcionalidad es similar a la función `concat()`, por ejemplo: 

```json
"customerName": "First name: @{parameters('firstName')} Last name: @{parameters('lastName')}"
```

Si tiene un literal de cadena que comienza por el carácter \@, coloque delante de carácter\@ otro carácter \@ como carácter de escape: \@\@

En estos ejemplos se muestra cómo se evalúan las expresiones:

| Valor JSON | Resultado |
|------------|--------|
| "Sophia Owen" | Devuelve estos caracteres: "Sophia Owen" |
| "array[1]" | Devuelve estos caracteres: "array [1]" |
| "\@\@" | Devuelve estos caracteres como una cadena de un carácter: "\@" |
| " \@" | Devuelve estos caracteres como una cadena de dos caracteres: "\@" |
|||

Para estos ejemplos, suponga que tiene que definir "myBirthMonth" es igual a "January" y "myAge" es igual al número 42:

```json
"myBirthMonth": "January",
"myAge": 42
```

Estos ejemplos muestran cómo se evalúan las expresiones siguientes:

| Expresión JSON | Resultado |
|-----------------|--------|
| "\@parameters('myBirthMonth')" | Devuelve esta cadena: "January" |
| "\@{parameters('myBirthMonth')}" | Devuelve esta cadena: "January" |
| "\@parameters('myAge')" | Devuelve este número: 42 |
| "\@{parameters('myAge')}" | Devuelve este número como una cadena: "42" |
| "My age is \@{parameters('myAge')}" | Devuelve esta cadena: "My age is 42" |
| "\@concat('My age is ', string(parameters('myAge')))" | Devuelve esta cadena: "My age is 42" |
| "My age is \@\@{parameters('myAge')}" | Devuelve esta cadena, que incluye la expresión: "My age is \@{parameters('myAge')}` |
|||

Al trabajar visualmente en el Diseñador de aplicaciones lógicas, puede crear expresiones mediante el generador de expresiones, por ejemplo:

![Diseñador de aplicaciones lógicas > Generador de expresiones](./media/logic-apps-workflow-definition-language/expression-builder.png)

Cuando haya terminado, la expresión aparece en la propiedad correspondiente en la definición de flujo de trabajo, por ejemplo, la propiedad `searchQuery` aquí:

```json
"Search_tweets": {
  "inputs": {
    "host": {
      "connection": {
        "name": "@parameters('$connections')['twitter']['connectionId']"
      }
    }
  },
  "method": "get",
  "path": "/searchtweets",
  "queries": {
    "maxResults": 20,
    "searchQuery": "Azure @{concat('firstName','', 'LastName')}"
  }
},
```

<a name="operators"></a>

## <a name="operators"></a>Operadores

En [expresiones](#expressions) y [funciones](#functions), los operadores realizan tareas específicas, como hacer referencia a una propiedad o un valor en una matriz.

| Operador | Tarea |
|----------|------|
| ' | Para usar un literal de cadena como entrada o en expresiones y funciones, encapsule la cadena solo con comillas sencillas, por ejemplo, `'<myString>'`. No use comillas dobles (""), ya que entran en conflicto con el formato JSON que rodea una expresión entera. Por ejemplo:  <p>**Sí**: length('Hello') </br>**No**: length("Hello") <p>Al pasar matrices o números, no es necesario encapsular los signos de puntuación. Por ejemplo:  <p>**Sí**: length([1, 2, 3]) </br>**No**: length("[1, 2, 3]") |
| [] | Para hacer referencia a un valor en una posición concreta (índice) de una matriz, use corchetes. Por ejemplo, para obtener el segundo elemento de una matriz: <p>`myArray[1]` |
| . | Para hacer referencia a una propiedad de un objeto, use el operador punto. Por ejemplo, para obtener la propiedad `name` de un objeto JSON `customer`: <p>`"@parameters('customer').name"` |
| ? | Para hacer referencia a propiedades nulas de un objeto sin un error de tiempo de ejecución, use el operador de signo de interrogación. Por ejemplo, para administrar salidas nulas desde un desencadenador, puede usar esta expresión: <p>`@coalesce(trigger().outputs?.body?.<someProperty>, '<property-default-value>')` |
|||

<a name="functions"></a>

## <a name="functions"></a>Functions

Algunas expresiones obtienen sus valores de las acciones en tiempo de ejecución que puede que no existan aún cuando comienza a ejecutarse una aplicación lógica. Para trabajar con estos valores o hacer referencia a ellos mediante expresiones, puede usar [*funciones*](../logic-apps/workflow-definition-language-functions-reference.md) proporcionadas por el lenguaje de definición de flujo de trabajo.

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre [Desencadenadores y acciones para flujos de trabajo](../logic-apps/logic-apps-workflow-actions-triggers.md).
* Aprenda sobre la creación y administración de aplicaciones lógicas mediante programación con la [API REST de flujo de trabajo](https://docs.microsoft.com/rest/api/logic/workflows).
