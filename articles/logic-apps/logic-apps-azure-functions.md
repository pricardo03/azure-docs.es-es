---
title: Agregar y ejecutar código en Azure Logic Apps con Azure Functions
description: Agregar y ejecutar código en Azure Logic Apps con Azure Functions
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.topic: article
ms.date: 08/20/2018
ms.reviewer: klam, LADocs
ms.openlocfilehash: 9b304f2d4d2e498701be5977decf202cb0fa995b
ms.sourcegitcommit: d73c46af1465c7fd879b5a97ddc45c38ec3f5c0d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65922069"
---
# <a name="add-and-run-code-by-using-azure-functions-in-azure-logic-apps"></a>Agregar y ejecutar código con Azure Functions en Azure Logic Apps

Cuando desea ejecutar código que realiza un trabajo específico en las aplicaciones lógicas, puede crear sus propias funciones con [Azure Functions](../azure-functions/functions-overview.md). Este servicio le permite crear Node.js, C#, y F# código por lo que no tiene que crear una aplicación completa o la infraestructura para ejecutar el código. También puede [llamar a aplicaciones lógicas desde Azure Functions](#call-logic-app).
Azure Functions proporciona informática sin servidor en la nube y es útil para realizar tareas, como las siguientes:

* Extender el comportamiento de la aplicación lógica con funciones en Node.js o C#.
* Realizar cálculos en el flujo de trabajo de las aplicaciones lógicas.
* Aplicar formato avanzado o campos de proceso a las aplicaciones lógicas.

Para ejecutar fragmentos de código sin necesidad de crear las funciones de Azure, obtenga información sobre cómo [adición y ejecución de código en línea](../logic-apps/logic-apps-add-run-inline-code.md).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Una aplicación de función de Azure, que es un contenedor para las funciones de Azure, incluida la del usuario. Si no tiene una aplicación de función, [cree primero la aplicación de función](../azure-functions/functions-create-first-azure-function.md). Puede hacerlo [de manera independiente de la aplicación lógica](#create-function-external) o [dentro de ella](#create-function-designer) en el Diseñador de aplicación lógica.

  Las aplicaciones de función y las funciones nuevas y existentes tienen los mismos requisitos de funcionamiento con las aplicaciones lógicas:

  * La aplicación de función debe tener la misma suscripción de Azure que la aplicación lógica.

  * La función usa un desencadenador de HTTP, por ejemplo, la plantilla de función de **desencadenador de HTTP** para **JavaScript** o **C#**. 

    Esta plantilla puede aceptar contenido que tenga el tipo `application/json` de la aplicación lógica. 
    Cuando se agrega una función de Azure a la aplicación lógica, el Diseñador de aplicaciones lógicas muestra las funciones personalizadas creadas a partir de esta plantilla dentro de la suscripción de Azure. 

  * La función no usa rutas personalizadas a menos que haya especificado una [definición de OpenAPI](../azure-functions/functions-openapi-definition.md), lo que antes se conocía como [archivo Swagger](https://swagger.io/). 
  
  * Si ha especificado una definición de OpenAPI para la función, el Diseñador de aplicaciones lógicas le ofrece una experiencia más completa para trabajar con parámetros de función. Para que la aplicación lógica pueda buscar funciones con definiciones de OpenAPI y acceder a estas, [siga estos pasos para configurar la aplicación de función](#function-swagger).

* La aplicación lógica en la que quiere agregar la función, incluido un [desencadenador](../logic-apps/logic-apps-overview.md#logic-app-concepts) como primer paso de la aplicación lógica 

  Antes de poder agregar acciones que puedan ejecutar funciones, la aplicación lógica se debe iniciar con un desencadenador.

  Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md) e [Inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

> [!NOTE]
> Integración de Logic apps con Functions no funciona cuando se habilitan las ranuras (versión preliminar).

<a name="create-function-external"></a>

## <a name="create-functions-outside-logic-apps"></a>Creación de funciones independientes de las aplicaciones lógicas

En [Azure Portal](https://portal.azure.com), cree la aplicación de función de Azure, que debe tener la misma suscripción de Azure que la aplicación lógica, y, después, la función de Azure.
Si no está familiarizado con la creación de funciones de Azure, aprenda a [crear su primera función en Azure Portal](../azure-functions/functions-create-first-azure-function.md), pero tenga en cuenta estos requisitos para crear funciones de Azure que pueda llamar desde aplicaciones lógicas:

* Asegúrese de seleccionar la plantilla de función de **desencadenador de HTTP** para **JavaScript** o **C#**.

  ![Desencadenador de HTTP: JavaScript o C#](./media/logic-apps-azure-functions/http-trigger-function.png)

<a name="function-swagger"></a>

* De manera opcional, si para la función [genera una definición de API](../azure-functions/functions-openapi-definition.md), que antes se conocía como [archivo Swagger](https://swagger.io/), obtendrá una experiencia más enriquecida al trabajar con los parámetros de la función en el Diseñador de aplicación lógica. Para configurar la aplicación de función de manera que la aplicación lógica pueda buscar y usar funciones con descripciones de Swagger, siga estos pasos:

  1. Asegúrese de que la aplicación de función se está ejecutando.

  2. En la aplicación de función, siga estos pasos para configurar el [uso compartido de recursos entre orígenes (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) de manera que se permitan todos los orígenes:

     1. En la lista **Instancias de Function App**, seleccione la aplicación de función > **Características de la plataforma** > **CORS**.

        ![Seleccione la aplicación de función > "Características de la plataforma" > "CORS"](./media/logic-apps-azure-functions/function-platform-features-cors.png)

     2. En **CORS**, agregue el carácter comodín `*`, quite los demás orígenes de la lista y elija **Guardar**.

        ![Establecimiento de "CORS * en el carácter comodín "*"](./media/logic-apps-azure-functions/function-platform-features-cors-origins.png)

### <a name="access-property-values-inside-http-requests"></a>Acceso a los valor de propiedad de las solicitudes HTTP

Las funciones de Webhook pueden aceptar solicitudes HTTP como entradas y pasarlas a otras funciones. Por ejemplo, aunque Logic Apps tiene [funciones que convierten los valores de fecha y hora](../logic-apps/workflow-definition-language-functions-reference.md), este ejemplo básico de función de JavaScript muestra cómo acceder a una propiedad de un objeto de solicitud que se ha pasado a la función y realizar operaciones en ese valor de propiedad. Para acceder a las propiedades dentro de los objetos, en este ejemplo se usa el [operador punto (.)](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Property_accessors): 

```javascript
function convertToDateString(request, response){
   var data = request.body;
   response = {
      body: data.date.ToDateString();
   }
}
```

Esto es lo que ocurre dentro de esta función:

1. La función crea una variable `data` y asigna el objeto `body` dentro del objeto `request` a esa variable. La función utiliza el operador punto (.) para hacer referencia al objeto `body` dentro del objeto `request`: 

   ```javascript
   var data = request.body;
   ```

2. La función ahora puede acceder a la propiedad `date` a través de la variable `data` y convertir ese valor de propiedad del tipo DateTime al tipo DateString mediante una llamada a la función `ToDateString()`. La función también devuelve el resultado a través de la propiedad `body` en la respuesta de la función: 

   ```javascript
   body: data.date.ToDateString();
   ```

Ahora que ha creado la función de Azure, siga los pasos para saber cómo [agregar funciones a las aplicaciones lógicas](#add-function-logic-app).

<a name="create-function-designer"></a>

## <a name="create-functions-inside-logic-apps"></a>Creación de funciones en aplicaciones lógicas

Para poder crear una función de Azure desde la aplicación lógica en el Diseñador de aplicación lógica primero debe tener una aplicación de función de Azure, que es un contenedor para las funciones. Si no tiene una aplicación de función, cree primero la aplicación de función. Consulte [Creación de su primera función en Azure Portal](../azure-functions/functions-create-first-azure-function.md). 

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica. 

2. Para crear y agregar la función, siga los pasos que se apliquen a su escenario:

   * En el último paso del flujo de trabajo de la aplicación lógica, elija **Nuevo paso**.

   * Entre los pasos existentes del flujo de trabajo de la aplicación lógica, mueva el mouse sobre la flecha, elija el signo más (+) y luego seleccione **Agregar una acción**.

3. En el cuadro de búsqueda, escriba "azure functions" como filtro.
En la lista de acciones, seleccione esta acción: **Elegir una función de Azure - Azure Functions** 

   ![Búsqueda de "Azure Functions"](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. En la lista de aplicaciones de función, seleccione la que corresponda. Cuando se abra la lista de acciones, seleccione esta acción: **Funciones de Azure: crear una nueva función**

   ![Selección de la aplicación de función](./media/logic-apps-azure-functions/select-function-app-create-function.png)

5. En el editor de la definición de función, defínala:

   1. En el cuadro **Nombre de la función**, proporcione un nombre para la función. 

   2. En el cuadro **Código**, agregue el código a la plantilla, incluida la respuesta y la carga que desea que se devuelva a la aplicación lógica cuando la función termine de ejecutarse. 

      ![Definición de la función](./media/logic-apps-azure-functions/function-definition.png)

      En el código de la plantilla, el objeto *`context`*  hace referencia al mensaje que envía la aplicación lógica mediante el campo **Cuerpo de la solicitud** en un paso posterior. 
      Para acceder a las propiedades del objeto `context` desde dentro de la función, use esta sintaxis: 

      `context.body.<property-name>`

      Por ejemplo, para hacer referencia a la propiedad `content` dentro del objeto `context`, use esta sintaxis: 

      `context.body.content`

      El código de plantilla también incluye una variable `input`, que almacena el valor del parámetro `data` de modo que la función puede realizar operaciones sobre ese valor. 
      Dentro de las funciones de JavaScript, la variable `data` es también un acceso directo de `context.body`.

      > [!NOTE]
      > Aquí, la propiedad `body` se aplica al objeto `context` y no es la misma que el token **Body** de la salida de una acción, que también se podría pasar a la función. 
 
   3. Cuando termine, seleccione **Crear**.

6. En el cuadro **Cuerpo de la solicitud**, proporcione la entrada de su función, que debe tener el formato de un objeto de notación de objetos JavaScript (JSON). 

   Esta entrada es el *objeto de contexto* o el mensaje que envía la aplicación lógica a la función. Al hacer clic en el campo **Cuerpo de la solicitud**, aparece la lista de contenido dinámico, por lo que puede seleccionar tokens para las salidas de los pasos anteriores. En este ejemplo se especifica que la carga de contexto contiene una propiedad denominada `content` que tiene el valor del token **From** del desencadenador de correo electrónico:

   ![Ejemplo de "cuerpo de la solicitud": carga del objeto de contexto](./media/logic-apps-azure-functions/function-request-body-example.png)

   Aquí, el objeto de contexto no se convierte en cadena, por lo que el contenido del objeto se agrega directamente a la carga de JSON. Sin embargo, cuando el objeto de contexto no sea un token JSON que pasa una cadena, un objeto JSON o una matriz JSON, recibirá un error. Por tanto, si en este ejemplo se usa en su lugar el token **Received Time**, puede convertir el objeto de token en una cadena agregando comillas dobles:  

   ![Conversión del objeto a cadena](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

7. Para especificar otros detalles como el método de uso, los encabezados de solicitud o parámetros de consulta, elija **Mostrar opciones avanzadas**.

<a name="add-function-logic-app"></a>

## <a name="add-existing-functions-to-logic-apps"></a>Incorporación de funciones existentes a aplicaciones lógicas

Para llamar a funciones de Azure existentes desde las aplicaciones lógicas, puede agregar funciones de Azure como cualquier otra acción en el Diseñador de aplicación lógica. 

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica. 

2. En el paso para agregar la función, elija **Nuevo paso** > **Agregar una acción**. 

3. En el cuadro de búsqueda, escriba "azure functions" como filtro.
En la lista de acciones, seleccione esta acción: **Elegir una función de Azure - Azure Functions** 

   ![Búsqueda de "Azure Functions"](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. En la lista de aplicaciones de función, seleccione la que corresponda. Cuando aparezca la lista de funciones, seleccione la que corresponda. 

   ![Selección de la aplicación de función y la función de Azure](./media/logic-apps-azure-functions/select-function-app-existing-function.png)

   Para las funciones con definiciones de API (descripciones de Swagger) y que [se configuran de manera que la aplicación lógica pueda buscarlas y acceder a ellas](#function-swagger), puede seleccionar **Acciones de Swagger**:

   ![Selección de la aplicación de función, "Acciones de Swagger" y la función de Azure](./media/logic-apps-azure-functions/select-function-app-existing-function-swagger.png)

5. En el cuadro **Cuerpo de la solicitud**, proporcione la entrada de su función, que debe tener el formato de un objeto de notación de objetos JavaScript (JSON). 

   Esta entrada es el *objeto de contexto* o el mensaje que envía la aplicación lógica a la función. Al hacer clic en el campo **Cuerpo de la solicitud**, aparece la lista de contenido dinámico, por lo que puede seleccionar tokens para las salidas de los pasos anteriores. En este ejemplo se especifica que la carga de contexto contiene una propiedad denominada `content` que tiene el valor del token **From** del desencadenador de correo electrónico:

   ![Ejemplo de "cuerpo de la solicitud": carga del objeto de contexto](./media/logic-apps-azure-functions/function-request-body-example.png)

   Aquí, el objeto de contexto no se convierte en cadena, por lo que el contenido del objeto se agrega directamente a la carga de JSON. Sin embargo, cuando el objeto de contexto no sea un token JSON que pasa una cadena, un objeto JSON o una matriz JSON, recibirá un error. Por tanto, si en este ejemplo se usa en su lugar el token **Received Time**, puede convertir el objeto de token en una cadena agregando comillas dobles: 

   ![Conversión del objeto a cadena](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

6. Para especificar otros detalles como el método de uso, los encabezados de solicitud o parámetros de consulta, elija **Mostrar opciones avanzadas**.

<a name="call-logic-app"></a>

## <a name="call-logic-apps-from-functions"></a>Llamada a las aplicaciones lógicas desde una función

Cuando quiera desencadenar una aplicación lógica desde dentro de una función de Azure, la aplicación lógica debe comenzar con un desencadenador que proporcione un punto de conexión al que se pueda llamar. Por ejemplo, puede iniciar la aplicación lógica con el desencadenador **HTTP**, **Request**, **Azure Queues** o **Event Grid**. Desde la función, envíe una solicitud HTTP POST a la dirección URL del desencadenador e incluya la carga que quiere que procese esa aplicación lógica. Para obtener más información, consulte [Llamada, desencadenamiento o anidación de aplicaciones lógicas](../logic-apps/logic-apps-http-endpoint.md). 

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre los [conectores de Logic Apps](../connectors/apis-list.md)
