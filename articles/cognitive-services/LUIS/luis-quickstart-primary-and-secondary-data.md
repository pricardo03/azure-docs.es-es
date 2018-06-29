---
title: 'Tutorial para crear una aplicación de LUIS que extraiga datos: Azure | Microsoft Docs'
description: En este tutorial, va a aprender a crear una aplicación de LUIS sencilla con intenciones y una entidad simple para extraer datos que se han aprendido automáticamente.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 03/29/2018
ms.author: v-geberr
ms.openlocfilehash: 1e8647e34da3d34946a4f6ac298017f6d4c99de6
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265367"
---
# <a name="tutorial-create-app-that-uses-simple-entity"></a>Tutorial: Creación de una aplicación que utiliza la entidad Simple
En este tutorial se crea una aplicación que muestra cómo se extraen datos aprendidos automáticamente de una expresión con la entidad **Simple**.

<!-- green checkmark -->
> [!div class="checklist"]
> * Información acerca de las entidades Simple 
> * Creación de una nueva aplicación de LUIS para el dominio de comunicación con la intención SendMessage
> * Adición de la intención _None_ y adición de expresiones de ejemplo
> * Adición de una entidad Simple para extraer el contenido del mensaje de una expresión
> * Entrenamiento y publicación de la aplicación
> * Consulta del punto de conexión de la aplicación para ver la respuesta JSON de LUIS

Para este artículo, necesita una cuenta de [LUIS][LUIS] gratuita para crear la aplicación de LUIS.

## <a name="purpose-of-the-app"></a>Propósito de la aplicación
Esta aplicación muestra cómo extraer datos de una expresión. Fíjese en las siguientes expresiones de un bot de chat:

```JSON
Send a message telling them to stop
```

La intención es enviar un mensaje. Los datos importantes de la expresión son el propio mensaje, `telling them to stop`.  

## <a name="purpose-of-the-simple-entity"></a>Propósito de la entidad simple
El propósito de la entidad simple es enseñar a LUIS qué es un mensaje y dónde se puede encontrar en una expresión. La parte de la expresión que es el mensaje puede cambiar de una expresión a otra en función de la longitud de la expresión y de la elección de las palabras. LUIS necesita ejemplos de mensajes en cualquier expresión de todas las intenciones.  

En esta aplicación simple, el mensaje estará al final de la expresión. 

## <a name="create-a-new-app"></a>Creación de una nueva aplicación
1. Inicie sesión en el sitio web de [LUIS][LUIS]. Asegúrese de que inicia sesión en la región en la que necesita publicar los puntos de conexión de LUIS.

2. En el sitio web de [LUIS][LUIS], seleccione **Create new app** (Crear aplicación).  

    ![Lista de aplicaciones de LUIS](./media/luis-quickstart-primary-and-secondary-data/app-list.png)

3. En el cuadro de diálogo emergente, escriba el nombre `MyCommunicator`. 

    ![Lista de aplicaciones de LUIS](./media/luis-quickstart-primary-and-secondary-data/create-new-app-dialog.png)

4. Cuando el proceso finalice, la aplicación mostrará la página **Intents** (Intenciones) con la intención **None**. 

    [![](media/luis-quickstart-primary-and-secondary-data/intents-list.png "Captura de pantalla de la página Intents (Intenciones) de LUIS con la intención None")](media/luis-quickstart-primary-and-secondary-data/intents-list.png#lightbox)

## <a name="create-a-new-intent"></a>Creación de una nueva intención

1. En la página **Intents** (Intenciones), seleccione **Create new intent** (Crear intención). 

    [![](media/luis-quickstart-primary-and-secondary-data/create-new-intent-button.png "Captura de pantalla de LUIS con el botón \"Create new intent\" (Crear intención) resaltado")](media/luis-quickstart-primary-and-secondary-data/create-new-intent-button.png#lightbox)

2. Escriba el nombre de la intención nueva `SendMessage`. Esta intención se debe seleccionar cada vez que un usuario quiera enviar un mensaje.

    Al crear una intención, se crea la categoría principal de información que desea identificar. Dar un nombre a la categoría permite que otras aplicaciones que usen los resultados de consulta de LUIS puedan utilizar ese nombre de categoría para encontrar una respuesta apropiada o tomar las medidas adecuadas. LUIS no responde a estas preguntas, solo identifica el tipo de información que se solicita en lenguaje natural. 

    ![Escribir el nombre de intención SendMessage](./media/luis-quickstart-primary-and-secondary-data/create-new-intent-popup-dialog.png)

3. Agregue siete expresiones a la intención `SendMessage` que espera que un usuario pida, por ejemplo:

    | Expresiones de ejemplo|
    |--|
    |Responder con "Me ha llegado el mensaje, tendré la respuesta mañana"|
    |Enviar el mensaje "¿Cuándo estará en casa?"|
    |Enviar por texto "Estoy ocupado"|
    |Indicar que es preciso que se realice hoy|
    |Mensaje instantáneo que dice "Estoy conduciendo, responderé después"|
    |Crear un mensaje para David que diga "¿Cuándo fue qué?"|
    |decir hola a greg|

    [![](media/luis-quickstart-primary-and-secondary-data/enter-utterances-on-intent-page.png "Captura de pantalla de LUIS con las expresiones especificadas")](media/luis-quickstart-primary-and-secondary-data/enter-utterances-on-intent-page.png#lightbox)

## <a name="add-utterances-to-none-intent"></a>Adición de expresiones a la intención None

La aplicación de LUIS no tiene actualmente expresiones en la intención **None**. Es necesario que tenga expresiones que no desea que la aplicación conteste, por ello debe incluir expresiones en la intención **None**. No la deje en blanco. 
    
1. Seleccione **Intents** (Intenciones) en el panel izquierdo. 

    [![](media/luis-quickstart-primary-and-secondary-data/select-intent-link.png "Captura de pantalla de LUIS con el botón \"Intents\" (Intenciones) resaltado")](media/luis-quickstart-primary-and-secondary-data/select-intent-link.png#lightbox)

2. Seleccione la intención **None**. 

    [![](media/luis-quickstart-primary-and-secondary-data/select-none-intent.png "Captura de pantalla de la selección de la intención None")](media/luis-quickstart-primary-and-secondary-data/select-none-intent.png#lightbox)

3. Agregue tres expresiones que el usuario podría solicitar pero que no son pertinentes para la aplicación. Estas son algunas expresiones **None** buenas:

    | Expresiones de ejemplo|
    |--|
    |Cancelar|
    |Adiós|
    |¿Qué está pasando?|
    
    En la aplicación que realiza la llamada de LUIS, por ejemplo, un bot de chat, si LUIS devuelve la intención **None** para una expresión, el bot puede preguntar al usuario si desea finalizar la conversación. El bot también puede dar más instrucciones para que continúe la conversación si el usuario no desea terminarla. 

    [![](media/luis-quickstart-primary-and-secondary-data/utterances-for-none-intent.png "Captura de pantalla de LUIS con las expresiones para la intención None")](media/luis-quickstart-primary-and-secondary-data/utterances-for-none-intent.png#lightbox)

## <a name="create-a-simple-entity-to-extract-message"></a>Creación de una entidad Simple para extraer un mensaje 
1. Seleccione **Intents** (Intenciones) en el menú izquierdo.

    ![Seleccionar el vínculo Intents (Intenciones)](./media/luis-quickstart-primary-and-secondary-data/select-intents-from-none-intent.png)

2. Seleccione `SendMessage` en la lista de intenciones.

    ![Seleccionar la intención SendMessage](./media/luis-quickstart-primary-and-secondary-data/select-sendmessage-intent.png)

3. En la expresión, `Reply with I got your message, I will have the answer tomorrow`, seleccione la primera palabra del cuerpo del mensaje, `I`, y la última palabra del cuerpo del mensaje, `tomorrow`. Todas estas palabras se seleccionan para el mensaje y aparece un menú desplegable con un cuadro de texto en la parte superior.

    [![](media/luis-quickstart-primary-and-secondary-data/select-words-in-utterance.png "Captura de pantalla de selección de palabras en la expresión para el mensaje")](media/luis-quickstart-primary-and-secondary-data/select-words-in-utterance.png#lightbox)

4. Escriba el nombre de la entidad `Message` en el cuadro de texto.

    [![](media/luis-quickstart-primary-and-secondary-data/enter-entity-name-in-box.png "Captura de pantalla de especificación del nombre de la entidad en el cuadro")](media/luis-quickstart-primary-and-secondary-data/enter-entity-name-in-box.png#lightbox)

5. Seleccione **Create new entity** (Crear intención) en el menú desplegable. El propósito de la entidad es extraer el texto que forma el cuerpo del mensaje. En esta aplicación de LUIS, el mensaje de texto está al final de la expresión, pero tanto la expresión como el mensaje pueden tener cualquier longitud. 

    [![](media/luis-quickstart-primary-and-secondary-data/create-message-entity.png "Captura de pantalla de creación de una entidad a partir de una expresión")](media/luis-quickstart-primary-and-secondary-data/create-message-entity.png#lightbox)

6. En la ventana emergente, el tipo de entidad predeterminada es **Simple** y el nombre de la entidad es `Message`. Conserve esta configuración y seleccione **Done** (Listo).

    ![Comprobar tipo de entidad](./media/luis-quickstart-primary-and-secondary-data/entity-type.png)

7. Una vez que haya creado la entidad y etiquetado una expresión, etiquete el resto de expresiones con dicha entidad. Seleccione una expresión y, después, seleccione la primera y última palabra de un mensaje. En el menú desplegable, seleccione la entidad, `Message`. El mensaje ya está etiquetado en la entidad. Siga etiquetando las frases de todos los mensajes en las restantes expresiones.

    [![](media/luis-quickstart-primary-and-secondary-data/all-labeled-utterances.png "Captura de pantalla de todas las expresiones con mensajes etiquetadas")](media/luis-quickstart-primary-and-secondary-data/all-labeled-utterances.png#lightbox)

    La vista predeterminada de las expresiones es **Entities view** (Vista de entidades). Seleccione el control **Entities view** (Vista de entidades) que está encima de las expresiones. La **vista Tokens** muestra el texto de la expresión. 

    [![](media/luis-quickstart-primary-and-secondary-data/tokens-view-of-utterances.png "Captura de pantalla de las expresiones en la vista de Tokens")](media/luis-quickstart-primary-and-secondary-data/tokens-view-of-utterances.png#lightbox)

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS
LUIS no conoce los cambios en las intenciones y entidades (el modelo) hasta que se le entrena. 

1. En la parte superior derecha del sitio web de LUIS, seleccione el botón **Train** (Entrenar).

    ![Seleccionar el botón Train (Entrenar)](./media/luis-quickstart-primary-and-secondary-data/train-button.png)

2. El entrenamiento se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

    ![Notificación de entrenamiento correcto](./media/luis-quickstart-primary-and-secondary-data/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión
Para obtener una predicción de LUIS en un bot de chat u otra aplicación, tiene que publicar la aplicación. 

1. En la parte superior derecha del sitio web de LUIS, seleccione el botón **Publish** (Publicar). 

2. Seleccione el espacio de producción y el botón **Publish** (Publicar).

    [![](media/luis-quickstart-primary-and-secondary-data/publish-to-production.png "Captura de pantalla de la página Publish (Publicar) con el botón de publicación en el espacio de producción resaltado")](media/luis-quickstart-primary-and-secondary-data/publish-to-production.png#lightbox)

3. La publicación se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consulta del punto de conexión con una expresión diferente
En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. 

[![](media/luis-quickstart-primary-and-secondary-data/publish-select-endpoint.png "Captura de pantalla de la página Publish (Publicar) con el punto de conexión resaltado")](media/luis-quickstart-primary-and-secondary-data/publish-select-endpoint.png#lightbox)

Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. Vaya al final de la dirección URL en la dirección y escriba `text I'm driving and will be 30 minutes late to the meeting`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que las expresiones etiquetadas, por lo que es una buena prueba y debería devolver las expresiones `SendMessage`.

```
{
  "query": "text I'm driving and will be 30 minutes late to the meeting",
  "topScoringIntent": {
    "intent": "SendMessage",
    "score": 0.987501
  },
  "intents": [
    {
      "intent": "SendMessage",
      "score": 0.987501
    },
    {
      "intent": "None",
      "score": 0.111048922
    }
  ],
  "entities": [
    {
      "entity": "i ' m driving and will be 30 minutes late to the meeting",
      "type": "Message",
      "startIndex": 5,
      "endIndex": 58,
      "score": 0.162995353
    }
  ]
}
```

## <a name="what-has-this-luis-app-accomplished"></a>¿Qué ha logrado esta aplicación de LUIS?
Esta aplicación, con solo dos intenciones y una entidad, ha identificado una intención de consulta en lenguaje natural y ha devuelto los datos del mensaje. 

El resultado de JSON identifica la puntuación más alta `SendMessage` con una puntuación de 0,987501. Todas las puntuaciones están comprendidas entre 1 y 0, y la mejor está próxima a 1. La `None` puntuación de la intención es 0,111048922, mucho más cerca de cero. 

Los datos del mensaje tienen un tipo, `Message`, así como un valor, `i ' m driving and will be 30 minutes late to the meeting`. 

El bot de chat ya tiene suficiente información para determinar la acción principal, `SendMessage`, y un parámetro de dicha acción, el texto del mensaje. 

## <a name="where-is-this-luis-data-used"></a>¿Dónde se utilizan estos datos de LUIS? 
LUIS ha terminado con esta solicitud. La aplicación que realiza la llamada, como un bot de chat, puede tomar el resultado de topScoringIntent y los datos de la entidad para enviar el mensaje a través de una API de terceros. Si hay otras opciones de programación para el bot o la aplicación que realiza la llamada, LUIS no realiza ese trabajo. LUIS solo determina cuál es la intención del usuario. 

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no sea necesaria, elimine la aplicación de LUIS. Para ello, seleccione el menú de tres puntos (...) a la derecha del nombre de la aplicación en la lista de aplicaciones y seleccione **Delete** (Eliminar). En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), seleccione **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aprender a agregar una entidad jerárquica](luis-quickstart-intent-and-hier-entity.md)


<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
