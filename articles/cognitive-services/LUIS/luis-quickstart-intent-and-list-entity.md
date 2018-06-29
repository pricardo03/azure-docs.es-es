---
title: 'Tutorial sobre la creación de una aplicación de LUIS para obtener texto exacto que coincida con datos enumerados: Azure | Microsoft Docs'
description: En este tutorial, aprenda a crear una aplicación sencilla de LUIS con intenciones y entidades de lista para extraer datos de esta guía de inicio rápido.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 33394dff1091f27c79c74d8648a90724ba8d6698
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264834"
---
# <a name="tutorial-create-app-using-a-list-entity"></a>Tutorial: Creación de una aplicación mediante una entidad de lista
En este tutorial, cree una aplicación que muestra cómo obtener datos que coinciden con una lista predefinida. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Descripción de las entidades de lista 
> * Creación de una nueva aplicación de LUIS para el dominio de bebida con intenciones de OrderDrinks
> * Adición de la intención _None_ y adición de expresiones de ejemplo
> * Agregar entidad de lista para extraer elementos de bebida de una expresión
> * Entrenamiento y publicación de la aplicación
> * Consulta del punto de conexión de la aplicación para ver la respuesta JSON de LUIS

Para este artículo, necesita una cuenta de [LUIS][LUIS] gratuita para crear la aplicación de LUIS.

## <a name="purpose-of-the-list-entity"></a>Propósito de la entidad de lista
Esta aplicación recibe pedidos de bebidas como `1 coke and 1 milk please` y devuelve datos como el tipo de bebida. Una entidad de **lista** de bebidas busca coincidencias de texto exactas y las devuelve. 

Una entidad de lista es una buena opción para este tipo de datos cuando los valores son un conjunto conocido. Los nombres de las bebidas pueden variar para incluir argot o abreviaturas, pero no suelen cambiar con frecuencia. 

## <a name="app-intents"></a>Intenciones de aplicación
Las intenciones son categorías de lo que quiere el usuario. Esta aplicación tiene dos intenciones: OrderDrink y None. La intención [None](luis-concept-intent.md#none-intent-is-fallback-for-app) existe a propósito, e indica cualquier cosa que no esté en la aplicación.  

## <a name="list-entity-is-an-exact-text-match"></a>La entidad de lista es una coincidencia de texto exacta
El propósito de la entidad es encontrar y categorizar partes del texto en una expresión. Una entidad de [lista](luis-concept-entity-types.md) permite una coincidencia exacta de palabras o frases.  

En esta aplicación de bebidas, LUIS extrae el pedido de bebidas de tal manera que se puede crear y rellenar un pedido estándar. LUIS permite que las expresiones tengan variantes, abreviaturas y argot. 

Entre las expresiones sencillas de ejemplo de los usuarios se pueden incluir:

```
2 glasses of milk
3 bottles of water
2 cokes
```

Las versiones abreviadas o de argot de las expresiones incluyen:

```
5 milk
3 h2o
1 pop
```
 
La entidad de lista hace coincidir `h2o` con agua y `pop` con refrescos.  

## <a name="what-luis-does"></a>Cuál es la función de LUIS
LUIS identifica, [extrae](luis-concept-data-extraction.md#list-entity-data) y devuelve el resultado en formato JSON desde el [punto de conexión](https://aka.ms/luis-endpoint-apis) las intenciones y entidades de la expresión. La aplicación que realiza la llamada, o bot de chat, toma esa respuesta en formato JSON y completa la solicitud de la forma en la que la aplicación o bot de chat esté diseñada para hacerlo. 

## <a name="create-a-new-app"></a>Creación de una nueva aplicación
1. Inicie sesión en el sitio web de [LUIS][LUIS]. Asegúrese de que inicia sesión en la [región][LUIS-regions] en la que necesita publicar los puntos de conexión de LUIS.

2. En el sitio web de [LUIS][LUIS], seleccione **Create new app** (Crear aplicación).  

    ![Crear nueva aplicación](./media/luis-quickstart-intent-and-list-entity/app-list.png)

3. En el cuadro de diálogo emergente, escriba el nombre `MyDrinklist`. 

    ![Asigne a la aplicación el nombre MyDrinkList.](./media/luis-quickstart-intent-and-list-entity/create-app-dialog.png)

4. Cuando el proceso finalice, la aplicación mostrará la página **Intents** (Intenciones) con la intención **None**. 

    [![](media/luis-quickstart-intent-and-list-entity/intents-page-none-only.png "Captura de pantalla de la página Intenciones")](media/luis-quickstart-intent-and-list-entity/intents-page-none-only.png#lightbox)

## <a name="create-a-new-intent"></a>Creación de una nueva intención

1. En la página **Intents** (Intenciones), seleccione **Create new intent** (Crear intención). 

    [![](media/luis-quickstart-intent-and-list-entity/create-new-intent.png "Captura de pantalla de la página Intents (Intenciones) con el botón Create new intent (Crear nueva intención) resaltado")](media/luis-quickstart-intent-and-list-entity/create-new-intent.png#lightbox)

2. Escriba el nombre de la intención nueva `OrderDrinks`. Esta intención se debe seleccionar siempre que un usuario quiera pedir una bebida.

    Al crear una intención, se crea la categoría principal de información que desea identificar. Dar un nombre a la categoría permite que otras aplicaciones que usen los resultados de consulta de LUIS puedan utilizar ese nombre de categoría para encontrar una respuesta apropiada o tomar las medidas adecuadas. LUIS no responde a estas preguntas, solo identifica el tipo de información que se solicita en lenguaje natural. 

    [![](media/luis-quickstart-intent-and-list-entity/intent-create-dialog-order-drinks.png "Captura de pantalla de creación de la nueva intención OrderDrings")](media/luis-quickstart-intent-and-list-entity/intent-create-dialog-order-drinks.png#lightbox)

3. Agregue varias expresiones a la intención `OrderDrinks` que espera que un usuario pida como, por ejemplo:

    | Expresiones de ejemplo|
    |--|
    |Envíenos 2 coca-colas y una botella de agua a mi habitación|
    |2 botellas de Perrier con un toque de limón|
    |h20|

    [![](media/luis-quickstart-intent-and-list-entity/intent-order-drinks-utterance.png "Captura de pantalla de escritura de la expresión en la página de la intención OrderDrinks")](media/luis-quickstart-intent-and-list-entity/intent-order-drinks-utterance.png#lightbox)

## <a name="add-utterances-to-none-intent"></a>Adición de expresiones a la intención None

La aplicación de LUIS no tiene actualmente expresiones en la intención **None**. Es necesario que tenga expresiones que no desea que la aplicación conteste, por ello debe incluir expresiones en la intención **None**. No la deje en blanco. 

1. Seleccione **Intents** (Intenciones) en el panel izquierdo. 

    [![](media/luis-quickstart-intent-and-list-entity/left-panel-intents.png "Captura de pantalla de selección de vínculo Intents (Intenciones) en el panel izquierdo")](media/luis-quickstart-intent-and-list-entity/left-panel-intents.png#lightbox)

2. Seleccione la intención **None**. Agregue tres expresiones que el usuario podría solicitar pero que no son pertinentes para la aplicación:

    | Expresiones de ejemplo|
    |--|
    |Cancelar|
    |Adiós|
    |¿Qué está pasando?|

## <a name="when-the-utterance-is-predicted-for-the-none-intent"></a>Cuando se predice la expresión de la intención None
En la aplicación que realiza la llamada de LUIS (por ejemplo, un bot de chat), cuando LUIS devuelve la intención **None** para una expresión, el bot puede preguntar al usuario si desea finalizar la conversación. El bot también puede dar más instrucciones para que continúe la conversación si el usuario no desea terminarla. 

Las entidades funcionan en la intención **None**. Si la intención que tiene la puntuación más alta es **None** pero se ha extraído una entidad con significado para el bot de chat, este puede continuar con una pregunta que permita concretar la intención del usuario. 

## <a name="create-a-menu-entity-from-the-intent-page"></a>Creación de una entidad de menú en la página Intents (Intenciones)
Ahora que las dos intenciones incluyen expresiones, es necesario que LUIS comprenda qué es una bebida. Vuelva a la intención `OrderDrinks` y etiquete (marque) las bebidas de una expresión mediante los siguientes pasos:

1. Vuelva a la intención `OrderDrinks` seleccionando **Intents** (Intenciones) en el panel izquierdo.

2. Seleccione `OrderDrinks` en la lista de intenciones.

3. En la expresión, `Please send 2 cokes and a bottle of water to my room`, seleccione la palabra `water`. Aparece un menú desplegable con un cuadro de texto en la parte superior para crear una nueva entidad. Escriba el nombre de entidad `Drink` en el cuadro de texto y, a continuación, seleccione **Create new entity** (Crear nueva entidad) en el menú desplegable. 

    [![](media/luis-quickstart-intent-and-list-entity/intent-label-h2o-in-utterance.png "Captura de pantalla de creación de una nueva entidad mediante la selección de palabras en una expresión")](media/luis-quickstart-intent-and-list-entity/intent-label-h2o-in-utterance.png#lightbox)

4. En la ventana emergente, seleccione el tipo de entidad **Lista**. Agregue el sinónimo `h20`. Seleccione la tecla ENTRAR después de cada sinónimo. No agregue `perrier` a la lista de sinónimos. Ese valor se agregará en el siguiente paso como ejemplo. Seleccione **Listo**.

    [![](media/luis-quickstart-intent-and-list-entity/create-list-ddl.png "Captura de pantalla de configuración de una nueva entidad")](media/luis-quickstart-intent-and-list-entity/create-list-ddl.png#lightbox)

5. Ahora que se ha creado la entidad, etiquete los demás sinónimos de agua seleccionando el sinónimo de agua y, posteriormente, seleccionando `Drink` en la lista desplegable. Siga el menú que aparece a la derecha y, a continuación, seleccione `Set as synonym` y, después, seleccione `water`.

    [![](media/luis-quickstart-intent-and-list-entity/intent-label-perriers.png "Captura de pantalla de etiquetado de la expresión con una entidad existente")](media/luis-quickstart-intent-and-list-entity/intent-label-perriers.png#lightbox)

## <a name="modify-the-list-entity-from-the-entity-page"></a>Modificación de la entidad de lista desde la página de entidades
La entidad de la lista de bebidas se crea pero no contiene muchos productos y sinónimos. Si conoce algunos términos, abreviaturas o argot, es más rápido rellenar en la lista de la página **Entity** (Entidad). 

1. Seleccione **Entities** (Entidades) en el panel izquierdo.

    [![](media/luis-quickstart-intent-and-list-entity/intent-select-entities.png "Captura de pantalla de selección de entidades en el panel izquierdo")](media/luis-quickstart-intent-and-list-entity/intent-select-entities.png#lightbox)

2. Seleccione `Drink` de la lista de entidades.

    [![](media/luis-quickstart-intent-and-list-entity/entities-select-drink-entity.png "Captura de pantalla de selección de la entidad de bebida en la lista de entidades")](media/luis-quickstart-intent-and-list-entity/entities-select-drink-entity.png#lightbox)

3. En el cuadro de texto, escriba `Soda pop` y, a continuación, seleccione ENTRAR. Se trata de un término que se aplica ampliamente a las bebidas con gas. Cada referencia cultural tiene un apodo o término de argot que hace referencia a este tipo de bebida.

    [![](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-canonical-name.png "Captura de pantalla de escritura de nombre canónico")](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-canonical-name.png#lightbox)

4. En la misma fila que `Soda pop`, escriba sinónimos como: 

    ```
    coke
    cokes
    coca-cola
    coca-colas
    ```

    Los sinónimos pueden incluir frases, puntuación, posesivos y plurales. Puesto que la entidad de lista es una coincidencia de texto exacta (excepto por el uso de mayúsculas y minúsculas), los sinónimos deben incluir todas las variantes. Puede expandir la lista a medida que aprende más variantes de los registros de consulta o revisar las coincidencias del punto de conexión. 

    Este artículo solo incluye unos pocos sinónimos para no alargar excesivamente el ejemplo. Una aplicación de LUIS en el nivel de producción debería incluir muchos sinónimos y debería revisarse y expandirse de forma regular. 

    [![](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-synonyms.png "Captura de pantalla de incorporación de sinónimos")](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-synonyms.png#lightbox)

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS
LUIS no conoce los cambios en las intenciones y entidades (el modelo) hasta que se le entrena. 

1. En la parte superior derecha del sitio web de LUIS, seleccione el botón **Train** (Entrenar).

    ![Entrenamiento de la aplicación](./media/luis-quickstart-intent-and-list-entity/train-button.png)

2. El entrenamiento se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

    ![El entrenamiento se realizó correctamente](./media/luis-quickstart-intent-and-list-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión
Para obtener una predicción de LUIS en un bot de chat u otra aplicación, tiene que publicar la aplicación. 

1. En la parte superior derecha del sitio web de LUIS, seleccione el botón **Publish** (Publicar). 

    [![](media/luis-quickstart-intent-and-list-entity/publish.png "Captura de pantalla de selección del botón Publish (Publicar)")](media/luis-quickstart-intent-and-list-entity/publish.png#lightbox)

2. Seleccione el espacio de producción y el botón **Publish** (Publicar). 

    [![](media/luis-quickstart-intent-and-list-entity/publish-to-production.png "Captura de pantalla de selección del botón Publish to production slot (Publicar en el espacio de producción)")](media/luis-quickstart-intent-and-list-entity/publish-to-production.png#lightbox)

3. La publicación se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consulta del punto de conexión con una expresión diferente
1. En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. 

    [![](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png "Captura de pantalla de dirección URL del punto de conexión en la página Publish (Publicar)")](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png#lightbox)

2. Vaya al final de la dirección URL en la dirección y escriba `2 cokes and 3 waters`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que cualquiera de las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `OrderDrinks` con los dos tipos de bebida `cokes` y `waters`.

```
{
  "query": "2 cokes and 3 waters",
  "topScoringIntent": {
    "intent": "OrderDrinks",
    "score": 0.999998569
  },
  "intents": [
    {
      "intent": "OrderDrinks",
      "score": 0.999998569
    },
    {
      "intent": "None",
      "score": 0.23884207
    }
  ],
  "entities": [
    {
      "entity": "cokes",
      "type": "Drink",
      "startIndex": 2,
      "endIndex": 6,
      "resolution": {
        "values": [
          "Soda pop"
        ]
      }
    },
    {
      "entity": "waters",
      "type": "Drink",
      "startIndex": 14,
      "endIndex": 19,
      "resolution": {
        "values": [
          "h20"
        ]
      }
    }
  ]
}
```

## <a name="where-is-the-natural-language-processing-in-the-list-entity"></a>¿Dónde está el procesamiento de lenguaje natural en la entidad List? 
Dado que la entidad List es una coincidencia de texto exacta, no se basa en el procesamiento de lenguaje natural (o aprendizaje automático). LUIS usa el procesamiento de lenguaje natural (o aprendizaje automático) para seleccionar la intención de puntuación más alta correcta. Además, una expresión puede ser una combinación de más de una entidad o incluso de más de un tipo de entidad. Cada expresión se procesa para todas las entidades de la aplicación, incluidas las entidades de procesamiento de lenguaje natural (o de aprendizaje automático) como la entidad **Simple**.

## <a name="what-has-this-luis-app-accomplished"></a>¿Qué ha logrado esta aplicación de LUIS?
Esta aplicación, con solo dos intenciones y una entidad de lista, ha identificado una intención de consulta en lenguaje natural y ha devuelto los datos extraídos. 

El bot de chat ya tiene suficiente información para determinar la acción principal, `OrderDrinks`, y qué tipos de bebidas se han pedido de la entidad de lista de bebidas. 

## <a name="where-is-this-luis-data-used"></a>¿Dónde se utilizan estos datos de LUIS? 
LUIS ha terminado con esta solicitud. La aplicación que realiza la llamada, como un bot de chat, puede tomar el resultado de topScoringIntent y los datos de la entidad para realizar el siguiente paso. LUIS no realiza este trabajo de programación para el bot o la aplicación que realiza la llamada. LUIS solo determina cuál es la intención del usuario. 

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no sea necesaria, elimine la aplicación de LUIS. Para ello, seleccione el menú de tres puntos (...) a la derecha del nombre de la aplicación en la lista de aplicaciones y seleccione **Delete** (Eliminar). En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), seleccione **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aprenda a agregar una entidad de expresión regular](luis-quickstart-intents-regex-entity.md)

Agregue el **número** de la [entidad creada previamente](luis-how-to-add-entities.md#add-prebuilt-entity) para extraer el número. 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
