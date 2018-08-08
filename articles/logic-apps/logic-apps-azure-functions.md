---
title: Incorporación y ejecución de código personalizado en Azure Logic Apps con Azure Functions | Microsoft Docs
description: Aprenda a agregar y ejecutar fragmentos de código personalizados en Azure Logic Apps con Azure Functions
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.date: 07/25/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 20ad738541554279ff9fd6dd6babe90a38676c00
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263197"
---
# <a name="add-and-run-custom-code-snippets-in-azure-logic-apps-with-azure-functions"></a>Incorporación y ejecución de fragmentos de código personalizados en Azure Logic Apps con Azure Functions

Cuando desee crear y ejecutar solo el código necesario para corregir un problema específico en las aplicaciones lógicas, puede crear sus propias funciones mediante [Azure Functions](../azure-functions/functions-overview.md). Este servicio proporciona la funcionalidad para crear y ejecutar fragmentos de código personalizados escritos con C# o Node.js en las aplicaciones lógicas sin necesidad de preocuparse por crear una aplicación completa o la infraestructura para ejecutar el código. Azure Functions proporciona informática sin servidor en la nube y es útil para realizar tareas, como las siguientes:

* Extender el comportamiento de la aplicación lógica con las funciones compatibles con C# o Node.js.
* Realizar cálculos en el flujo de trabajo de las aplicaciones lógicas.
* Aplicar formato avanzado o campos de proceso a las aplicaciones lógicas.

También puede [llamar a aplicaciones lógicas desde Azure Functions](#call-logic-app).

## <a name="prerequisites"></a>Requisitos previos

Para poder seguir los pasos de este artículo, estos son los elementos que necesita:

* Si aún no tiene ninguna suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>. 

* La aplicación lógica en la que desea agregar la función.

  Si nunca trabajó con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps](../logic-apps/logic-apps-overview.md) y el artículo sobre [Inicio rápido: creación de su primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Un [desencadenador](../logic-apps/logic-apps-overview.md#logic-app-concepts) que será el primer paso de la aplicación lógica. 

  Antes de poder agregar acciones para ejecutar funciones, debe iniciar la aplicación lógica con un desencadenador.

* Una aplicación de función de Azure, que es un contenedor para las funciones de Azure, incluida la del usuario. Si no tiene una aplicación de función, debe [crear primero la aplicación de función](../azure-functions/functions-create-first-azure-function.md). Puede hacerlo [de manera independiente de la aplicación lógica](#create-function-external) o [dentro de ella](#create-function-designer) en el Diseñador de aplicación lógica.

  Las aplicaciones de función y las funciones de Azure nuevas y existentes tienen los mismos requisitos de trabajo con las aplicaciones lógicas:

  * La aplicación de función debe pertenecer a la misma suscripción de Azure que la aplicación lógica.

  * La función debe usar la plantilla de función de **Webhook genérico** para **JavaScript** o **C#**. Esta plantilla puede aceptar el contenido de tipo `application/json` de la aplicación lógica. Estas plantillas también ayudan al Diseñador de aplicación lógica a buscar y mostrar las funciones personalizadas que cree con estas plantillas al agregarlas a las aplicaciones lógicas.

  * Compruebe que la propiedad **Mode** de la plantilla de función se encuentre en **Webhook** y **Webhook type**, en **Generic JSON**.

    1. Inicie sesión en el <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.
    2. En el menú principal de Azure, seleccione **Instancias de Function App**. 
    3. En la lista de **Instancias de Function App**, seleccione la aplicación de función, expanda la función y seleccione **Integrar**. 
    4. Compruebe que la propiedad **Mode** de la plantilla se encuentre en **Webhook** y **Webhook type**, en **Generic JSON**. 

  * Si la función tiene una [definición de API](../azure-functions/functions-openapi-definition.md), que antes se conocía como [archivo Swagger](http://swagger.io/), el Diseñador de aplicación lógica ofrece una experiencia más enriquecida para trabajar con los parámetros de la función. 
  Para que la aplicación lógica pueda buscar funciones con descripciones de Swagger y acceder a ellas, [siga estos pasos para configurar la aplicación de función](#function-swagger).

<a name="create-function-external"></a>

## <a name="create-functions-outside-logic-apps"></a>Creación de funciones independientes de las aplicaciones lógicas

En <a href="https://portal.azure.com" target="_blank">Azure Portal</a>, cree la aplicación de función de Azure, que debe tener la misma suscripción de Azure que la aplicación lógica, y, después, la función de Azure. Si no está familiarizado con Azure Functions, aprenda a [crear la primera función en Azure Portal](../azure-functions/functions-create-first-azure-function.md), pero anote estos requisitos para crear funciones de Azure que pueda agregar y llamar desde las aplicaciones lógicas.

* Asegúrese de seleccionar la plantilla de función de **Webhook genérico** para **JavaScript** o **C#**.

  ![Webhook genérico: JavaScript o C#](./media/logic-apps-azure-functions/generic-webhook.png)

* Después de crear la función de Azure, compruebe que las propiedades **Mode** y **Webhook type** estén establecidas correctamente.

  1. En la lista de **Instancias de Function App**, expanda la función y seleccione **Integrar**. 

  2. Compruebe que la propiedad **Mode** de la plantilla se encuentre en **Webhook** y **Webhook type**, en **Generic JSON**. 

     ![Propiedades de "Integrar" de la plantilla de función](./media/logic-apps-azure-functions/function-integrate-properties.png)

<a name="function-swagger"></a>

* De manera opcional, si para la función [genera una definición de API](../azure-functions/functions-openapi-definition.md), que antes se conocía como [archivo Swagger](http://swagger.io/), obtendrá una experiencia más enriquecida al trabajar con los parámetros de la función en el Diseñador de aplicación lógica. Para configurar la aplicación de función de manera que la aplicación lógica pueda buscar funciones con descripciones de Swagger y acceder a ellas:

  * Asegúrese de que la aplicación de función se está ejecutando.

  * En la aplicación de función, configure el [intercambio de recursos de origen cruzado (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) de manera que se permitan todos los orígenes:

    1. En la lista **Instancias de Function App**, seleccione la aplicación de función > **Características de la plataforma** > **CORS**.

       ![Seleccione la aplicación de función > "Características de la plataforma" > "CORS"](./media/logic-apps-azure-functions/function-platform-features-cors.png)

    2. En **CORS**, agregue el carácter comodín `*`, quite los demás orígenes de la lista y elija **Guardar**.

       ![Seleccione la aplicación de función > "Características de la plataforma" > "CORS"](./media/logic-apps-azure-functions/function-platform-features-cors-origins.png)

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

1. En <a href="https://portal.azure.com" target="_blank">Azure Portal</a>, abra la aplicación lógica en Diseñador de aplicación lógica. 

2. En el paso para crear y agregar la función, elija **Nuevo paso** > **Agregar una acción**. 

3. En el cuadro de búsqueda, escriba "azure functions" como filtro.
De la lista de acciones, seleccione esta acción: **Elegir una función de Azure: Azure Functions**. 

   ![Búsqueda de "Azure Functions"](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. En la lista de aplicaciones de función, seleccione la que corresponda. Una vez abierta la lista de acciones, seleccione esta: **Azure Functions: Crear nueva función**.

   ![Selección de la aplicación de función](./media/logic-apps-azure-functions/select-function-app-create-function.png)

5. En el editor de la definición de función, defínala:

   1. En el cuadro **Nombre de la función**, proporcione un nombre para la función. 

   2. En el cuadro **Código**, agregue el código de función a la plantilla, incluida la respuesta y la carga que desea que se devuelva a la aplicación lógica cuando la función termine de ejecutarse. 
   El objeto de contexto del código de la plantilla describe el mensaje y el contenido que la aplicación lógica pasará a la función, por ejemplo:

      ![Definición de la función](./media/logic-apps-azure-functions/function-definition.png)

      En la función puede hacer referencia a las propiedades del objeto de contexto mediante esta sintaxis:

      ```text
      context.<token-name>.<property-name>
      ```
      En este ejemplo se podría utilizar la siguiente sintaxis:

      ```text
      context.body.content
      ```

   3. Cuando termine, seleccione **Crear**.

6. En el cuadro **Cuerpo de la solicitud**, especifique el objeto de contexto que se pasará como entrada de la función, que debe tener formato JavaScript Object Notation (JSON). Al hacer clic en el cuadro **Cuerpo de la solicitud** se abre la lista de contenido dinámica para que pueda seleccionar tokens para las propiedades disponibles de los pasos anteriores. 

   En este ejemplo se pasa el objeto del token **Cuerpo** del desencadenador de correo electrónico:  

   ![Ejemplo de "cuerpo de la solicitud": carga del objeto de contexto](./media/logic-apps-azure-functions/function-request-body-example.png)

   Según el contenido del objeto de contexto, el Diseñador de aplicación lógica genera una plantilla de función que podrá editar insertada. 
   Logic Apps también crea variables basadas en el objeto de contexto de entrada.

   En este ejemplo, el objeto de contexto no se convierte en cadena, por lo que el contenido se agrega directamente a la carga de JSON. 
   Sin embargo, si el objeto no es un token de JSON (una cadena, un objeto JSON o una matriz JSON) obtendrá un error. 
   Para convertir el objeto de contexto a cadena, agregue marcas de comillas dobles, por ejemplo:

   ![Conversión del objeto a cadena](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

7. Para especificar otros detalles como el método de uso, los encabezados de solicitud o parámetros de consulta, elija **Mostrar opciones avanzadas**.

<a name="add-function-logic-app"></a>

## <a name="add-existing-functions-to-logic-apps"></a>Incorporación de funciones existentes a aplicaciones lógicas

Para llamar a funciones de Azure existentes desde las aplicaciones lógicas, puede agregar funciones de Azure como cualquier otra acción en el Diseñador de aplicación lógica. 

1. En <a href="https://portal.azure.com" target="_blank">Azure Portal</a>, abra la aplicación lógica en Diseñador de aplicación lógica. 

2. En el paso para agregar la función, elija **Nuevo paso** > **Agregar una acción**. 

3. En el cuadro de búsqueda, escriba "azure functions" como filtro.
De la lista de acciones, seleccione esta acción: **Elegir una función de Azure: Azure Functions**. 

   ![Búsqueda de "Azure Functions"](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. En la lista de aplicaciones de función, seleccione la que corresponda. Cuando aparezca la lista de funciones, seleccione la que corresponda. 

   ![Selección de la aplicación de función y la función de Azure](./media/logic-apps-azure-functions/select-function-app-existing-function.png)

   Para las funciones con definiciones de API (descripciones de Swagger) y que [se configuran de manera que la aplicación lógica pueda buscarlas y acceder a ellas](#function-swagger), puede seleccionar **Acciones de Swagger**:

   ![Selección de la aplicación de función, "Acciones de Swagger" y la función de Azure](./media/logic-apps-azure-functions/select-function-app-existing-function-swagger.png)

5. En el cuadro **Cuerpo de la solicitud**, especifique el objeto de contexto que se pasará como entrada de la función, que debe tener formato JavaScript Object Notation (JSON). Este objeto de contexto describe el mensaje y el contenido que la aplicación lógica envía a la función. 

   Al hacer clic en el cuadro **Cuerpo de la solicitud** se abre la lista de contenido dinámica para que pueda seleccionar tokens para las propiedades disponibles de los pasos anteriores. 
   En este ejemplo se pasa el objeto del token **Cuerpo** del desencadenador de correo electrónico:

   ![Ejemplo de "cuerpo de la solicitud": carga del objeto de contexto](./media/logic-apps-azure-functions/function-request-body-example.png)

   En este ejemplo, el objeto de contexto no se convierte en cadena, por lo que el contenido se agrega directamente a la carga de JSON. 
   Sin embargo, si el objeto no es un token de JSON (una cadena, un objeto JSON o una matriz JSON) obtendrá un error. 
   Para convertir el objeto de contexto a cadena, agregue marcas de comillas dobles, por ejemplo:

   ![Conversión del objeto a cadena](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

6. Para especificar otros detalles como el método de uso, los encabezados de solicitud o parámetros de consulta, elija **Mostrar opciones avanzadas**.

<a name="call-logic-app"></a>

## <a name="call-logic-apps-from-functions"></a>Llamada a las aplicaciones lógicas desde una función

Para desencadenar una aplicación lógica desde una función de Azure, esa aplicación lógica debe tener un punto de conexión al que se pueda llamar o, más específicamente, un desencadenador **Request**. A continuación, desde la función, envíe una solicitud HTTP POST a la dirección URL del desencadenador **Request** e incluya la carga que desea que esa aplicación lógica procese. Para obtener más información, consulte [Llamada, desencadenamiento o anidación de aplicaciones lógicas](../logic-apps/logic-apps-http-endpoint.md). 

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre los [conectores de Logic Apps](../connectors/apis-list.md)
