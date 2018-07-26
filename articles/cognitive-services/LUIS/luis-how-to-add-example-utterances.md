---
title: Adición de expresiones de ejemplo en las aplicaciones de LUIS | Microsoft Docs
titleSuffix: Azure
description: Obtenga información acerca de cómo agregar expresiones en las aplicaciones de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: cf6fc131a1ae2ba0a77bdbeda942c9f85aadd189
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39226294"
---
# <a name="add-example-utterances-and-label-with-entities"></a>Adición de expresiones de ejemplo y etiquetas con entidades

Las expresiones de ejemplo son ejemplos de texto de preguntas de los usuarios o de comandos. Para entrenar el servicio Language Understanding (LUIS), debe agregar [expresiones de ejemplo](luis-concept-utterance.md) a una [intención](luis-concept-intent.md).

En general, primero se agrega una expresión de ejemplo a una intención y luego se crean expresiones de etiquetas y entidades en la página de intención. Si prefiere crear primero las entidades, consulte [Agregar entidades](luis-how-to-add-entities.md).

## <a name="add-an-utterance"></a>Adición de una expresión
En una página de intención, escriba una expresión de ejemplo pertinente que espera de los usuarios, como `book 2 adult business tickets to Paris tomorrow on Air France`, en el cuadro de texto que hay debajo del nombre de intención y, luego, presione Entrar. 
 
>[!NOTE]
>LUIS convierte todas las expresiones a minúsculas.

![Captura de pantalla de la página de detalles de las intenciones, con la expresión resaltada](./media/luis-how-to-add-example-utterances/add-new-utterance-to-intent.png) 

Las expresiones se agregan a la lista de expresiones de la intención actual. 

## <a name="ignoring-words-and-punctuation"></a>Omisión de palabras y puntuación
Si quiere omitir palabras específicas o signos de puntuación en la expresión de ejemplo, use un [patrón](luis-concept-patterns.md#pattern-syntax) con la sintaxis _ignore_. 

## <a name="add-simple-entity-label"></a>Adición de una etiqueta de entidad simple
En el siguiente procedimiento, se crearán y etiquetarán entidades personalizadas dentro de la expresión siguiente en la página de intención:

```
book me 2 adult business tickets to Paris tomorrow on Air France
```

1. Seleccione a "Air France" en la expresión para etiquetarlo como una entidad simple.

    > [!NOTE]
    > Al seleccionar las palabras que quiere etiquetar como entidades:
    > * Para una sola palabra, simplemente selecciónela. 
    > * Para un conjunto de dos o más palabras, seleccione el principio y el final del conjunto.

2. En el cuadro desplegable de la entidad que aparece, puede seleccionar una entidad existente o agregar una nueva entidad. Para agregar una nueva entidad, escriba el nombre en el cuadro de texto y luego seleccione **Crear nueva entidad**. 
 
    ![Captura de pantalla de la página de detalles de la intención, con la opción de etiquetado de la entidad simple resaltada](./media/luis-how-to-add-example-utterances/create-airline-simple-entity.png)

3. En el cuadro de diálogo emergente **What type of entity do you want to create?** (Qué tipo de entidad quiere crear?), compruebe el nombre de entidad y seleccione el tipo de entidad simple y, a continuación, seleccione **Hecho**.

    ![Imagen del cuadro de diálogo de confirmación](./media/luis-how-to-add-example-utterances/create-simple-airline-entity.png)

    Consulte [Extracción de datos](luis-concept-data-extraction.md#simple-entity-data) para obtener más información acerca de cómo extraer la entidad simple a partir de la respuesta a la consulta JSON del punto de conexión. Pruebe el [inicio rápido](luis-quickstart-primary-and-secondary-data.md) de la entidad simple para obtener más información sobre cómo usar una entidad simple.


## <a name="add-list-entity-and-label"></a>Adición de la etiqueta y la entidad de la lista
Las entidades de la lista representan un conjunto fijo y cerrado (coincidencias de texto exacto) de palabras relacionadas en el sistema. 

Para una entidad de la lista de bebidas, puede tener dos valores normalizados: agua y refresco. Cada nombre normalizado tiene sinónimos. Para el agua, los sinónimos son H20, con gas y natural. Para el refresco, los sinónimos son frutas, cola y jengibre. No es necesario conocer todos los valores cuando se crea la entidad. Puede agregar más después de revisar las expresiones reales del usuario con los sinónimos.

|Nombre normalizado|Sinónimos|
|--|--|
|Agua|H20, con gas, natural|
|Refresco|Frutas, cola, jengibre|

Al crear una nueva entidad de la lista a partir de la página de intención, se realizan dos cosas que pueden no ser evidentes. En primer lugar, se crea una nueva lista al agregar el primer elemento de lista. En segundo lugar, el primer elemento de lista se nombra con la palabra o frase seleccionada de la expresión. Aunque es posible cambiar estos valores más adelante desde la página de entidad, puede ser más rápido seleccionar una expresión que tenga la palabra que quiere para el nombre del elemento de lista.

Por ejemplo, si quisiera crear una lista de tipos de bebida y seleccionara la palabra `h2o` de la expresión para crear la entidad, la lista tendría un elemento, cuyo nombre sería H20. Si quisiera incluir un nombre más genérico, debería elegir una expresión que use el nombre más genérico. 

1. En la expresión, seleccione la palabra que es el primer elemento en la lista y, luego, escriba el nombre de la lista en el cuadro de texto. Por último, seleccione **Crear nueva entidad**.   

    ![Captura de pantalla de la página de detalles de intención, con la opción Crear nueva entidad resaltada](./media/luis-how-to-add-example-utterances/create-drink-list-entity.png)

2. En el cuadro de diálogo **What type of entity do you want to create?** (¿Qué tipo de entidad quiere crear?), agregue sinónimos de este elemento de lista. Para el elemento de agua en una lista de bebidas, agregue `h20`, `perrier` y `waters`, y seleccione **Hecho**. Tenga en cuenta que se ha agregado "aguas" porque los sinónimos de la lista coinciden en el nivel de token. En la configuración regional inglesa, ese nivel se encuentra en el nivel de la palabra, por tanto, "aguas" no coincidiría con "agua" a menos que esté en la lista. 

    ![Captura de pantalla del cuadro de diálogo What type of entity do you want to create (Qué tipo de entidad quiere crear)](./media/luis-how-to-add-example-utterances/drink-list-ddl.png)

    Esta lista de bebidas tiene un único tipo de bebida: agua. Puede agregar más tipos de bebidas etiquetando otras expresiones o mediante la edición de la entidad en **Entidades** en el panel de navegación izquierdo. La [edición](luis-how-to-add-entities.md#add-list-entities) de las entidades le proporciona las opciones de escribir elementos adicionales con sinónimos correspondientes o [importar](luis-how-to-add-entities.md#import-list-entity-values) una lista. 

    Consulte [Extracción de datos](luis-concept-data-extraction.md#list-entity-data) para obtener más información acerca de cómo extraer entidades de la lista de la respuesta a la consulta JSON del punto de conexión. Pruebe el [inicio rápido](luis-quickstart-intent-and-list-entity.md) para obtener más información acerca de cómo usar una entidad de la lista.

## <a name="add-synonyms-to-the-list-entity"></a>Adición de sinónimos a la entidad de lista 
Para agregar un sinónimo a la entidad de la lista, seleccione la palabra o frase en la expresión. Si tiene una entidad de lista de bebidas y quiere agregar `agua` como sinónimo de agua, siga los pasos:

En la expresión, seleccione la palabra sinónima, como `aqua` para agua, luego seleccione el nombre de entidad de la lista en la lista desplegable, como **Bebida**, a continuación, seleccione **Set as synonym** (Establecer como sinónimo) y, por último, seleccione el elemento de lista sinónimo, como **agua**.

![Captura de pantalla de la página de detalles de intención, con la opción Create a new synonym (Crear nuevo sinónimo) resaltada](./media/luis-how-to-add-example-utterances/set-agua-as-synonym.png)

## <a name="create-new-item-for-list-entity"></a>Creación de un nuevo elemento para la entidad de la lista
Para crear un nuevo elemento para una entidad de la lista existente, seleccione la palabra o frase en la expresión. Si tiene una lista de bebidas y quiere agregar `tea` como un nuevo elemento, siga los pasos:

En la expresión, seleccione la palabra para el nuevo elemento de lista, como `tea`, a continuación, seleccione el nombre de entidad de la lista en la lista desplegable, como **Bebida**, y seleccione **Create a new synonym** (Crear un nuevo sinónimo). 

![Captura de pantalla para agregar un nuevo elemento de lista](./media/luis-how-to-add-example-utterances/list-entity-create-new-item.png)

Ahora la palabra se resalta de color azul. Si mantiene el puntero sobre la palabra, una etiqueta muestra el nombre del elemento de lista, como té.

![Captura de pantalla de etiqueta de nuevo elemento de lista](./media/luis-how-to-add-example-utterances/list-entity-item-name-tag.png)

## <a name="wrap-entities-in-composite-label"></a>Encapsulado de entidades en la etiqueta compuesta
Se crean entidades compuestas a partir de **Entidades**. No se puede crear una entidad de compuesta a partir de la página de intención. Una vez que se crea la entidad compuesta, puede ajustar las entidades en una expresión en la página de intención. 

Suponiendo que se tiene la expresión `book 2 tickets from Seattle to Cairo`, una expresión compuesta puede devolver la información de la entidad del recuento de incidencias (2) y las ubicaciones de origen (Seattle) y destino (Cairo) en una entidad primaria única. 

Siga estos [pasos](luis-how-to-add-entities.md#add-prebuilt-entity) para agregar la entidad precompilada de **número**. Después de crear la entidad, el `2` en la expresión es azul, lo que indica que es una entidad etiquetada. LUIS etiqueta las entidades precompiladas. No se puede agregar ni quitar la etiqueta de entidad precompilada desde una expresión única. Solo pueden agregar o quitar todas las etiquetas predeterminadas si se agrega o quita la entidad precompilada de la aplicación.

Siga estos [pasos](#add-hierarchical-entity-and-label) para crear una entidad jerárquica de **Location**. Etiquete las ubicaciones de origen y destino en la expresión de ejemplo. 

Antes de encapsular las entidades en una entidad compuesta, asegúrese de que todas las entidades secundarias estén resaltadas en azul, lo que significa que se han etiquetado en la expresión.

1. Para encapsular las entidades individuales en una composición, seleccione la primera entidad etiquetada en la expresión para la entidad compuesta. En la expresión de ejemplo, `book 2 tickets from Seattle to Cairo`, la primera entidad es el número 2. Aparece una lista de desplegable que muestra las opciones de esta selección.

    ![Captura de pantalla del número seleccionado y las opciones de lista desplegable resaltadas](./media/luis-how-to-add-example-utterances/wrap-1.png)

2. Seleccione **Wrap in composite entity** (Encapsular la entidad compuesta) en la lista desplegable. 

    ![Captura de pantalla de opciones de lista desplegable para encapsular la entidad compuesta con la opción Wrap in composite entity (Encapsular la entidad compuesta) resaltada.](./media/luis-how-to-add-example-utterances/wrap-2.png)

3. Seleccione la última palabra de la entidad compuesta. En la expresión de este ejemplo, seleccione "Location::Destination" (que representa el Cairo). La línea verde ahora está debajo de todas las palabras, incluidas las palabras que no son de la entidad, de la expresión que forman la composición.

    ![Captura de pantalla de la página de intención BookFlight con número resaltado](./media/luis-how-to-add-example-utterances/wrap-composite.png)

4. Seleccione el nombre de la entidad compuesta de la lista desplegable. En este ejemplo, sería **TicketOrder**.

    ![Captura de pantalla de encapsulado de palabras con entidad compuesta, con el nombre de entidad compuesta resaltado en la lista desplegable](./media/luis-how-to-add-example-utterances/wrap-4.png)

    Cuando se encapsulan las entidades correctamente, aparece una línea verde debajo de toda la frase.

    ![Captura de pantalla de la expresión con la entidad compuesta resaltada](./media/luis-how-to-add-example-utterances/wrap-5.png)

    Consulte [Data Extraction](luis-concept-data-extraction.md#composite-entity-data) (Extracción de datos) para obtener más información acerca de cómo extraer la entidad compuesta a partir de la respuesta a la consulta JSON del punto de conexión. Pruebe el [tutorial](luis-tutorial-composite-entity.md) de la entidad compuesta para obtener más información sobre cómo usar una entidad de compuesta.

## <a name="add-hierarchical-entity-and-label"></a>Adición de la etiqueta y la entidad jerárquica
Una entidad jerárquica es una categoría de entidades relacionadas conceptualmente y aprendidas contextualmente. En el ejemplo siguiente, la entidad contiene ubicaciones de origen y destino. 

En la expresión `Book 2 tickets from Seattle to Cairo`, Seattle es la ubicación de origen y el Cairo es la ubicación de destino. Cada ubicación es contextualmente diferente y se aprende a partir del orden de palabras y elección de palabras en la expresión.

1. En la página de intención, en la expresión, seleccione "Seattle", a continuación, escriba el nombre de entidad "Location" y, finalmente, seleccione **Crear nueva entidad**.

    ![Captura de pantalla del cuadro de diálogo Create Hierarchical Entity Labeling "Crear etiquetado de entidad jerárquico"](./media/luis-how-to-add-example-utterances/create-hier-ent-1.png)

2. En el cuadro de diálogo emergente, seleccione jerárquico en **Tipo de entidad**, a continuación, agregue `Origin` y `Destination` como elementos secundarios y, por último, seleccione **Hecho**.

    ![Captura de pantalla de la página de detalles de intención, con la entidad ToLocation resaltada](./media/luis-how-to-add-example-utterances/create-location-hierarchical-entity.png)

3. La palabra en la expresión se etiqueta con la entidad jerárquica primaria. Debe asignar la palabra a una entidad secundaria. Vuelva a la expresión en la página de intención. Seleccione la palabra, a continuación, en la lista desplegable, elija el nombre de la entidad que ha creado y siga el menú a la derecha para elegir la entidad secundaria correcta.

    ![Captura de pantalla de la página de detalles de intención, con la entidad ToLocation resaltada](./media/luis-how-to-add-example-utterances/label-tolocation.png)

    >[!CAUTION]
    >Los nombres de entidad secundaria deben ser únicos en todas las entidades en una sola aplicación. Dos entidades jerárquicas diferentes no pueden contener entidades secundarias con el mismo nombre. 

    Consulte [Data Extraction](luis-concept-data-extraction.md#hierarchical-entity-data) (Extracción de datos) para obtener más información acerca de cómo extraer la entidad jerárquica a partir de la respuesta a la consulta JSON del punto de conexión. Pruebe el [inicio rápido](luis-quickstart-intent-and-hier-entity.md) de la entidad jerárquica para obtener más información sobre cómo usar una entidad jerárquica.


## <a name="remove-entity-labels-from-utterances"></a>Eliminación de etiquetas de entidad de las expresiones
Puede quitar las etiquetas de entidad de aprendizaje automático de una expresión en la página de intención. Si la entidad no procede del aprendizaje automático, no se puede quitar de una expresión. Si tiene que quitar una entidad que no es de aprendizaje automático de la expresión, debe eliminar la entidad de toda la aplicación. 

Para quitar una etiqueta de entidad de una expresión de aprendizaje automático, seleccione la entidad en la expresión. A continuación, seleccione **Quitar etiqueta** en el cuadro desplegable de la entidad que aparece.

![Captura de pantalla de la página de detalles de intención, con la opción Quitar etiqueta resaltada](./media/luis-how-to-add-example-utterances/remove-label.png) 

## <a name="add-prebuilt-entity-label"></a>Adición de etiqueta de entidad precompilada
Si agrega las entidades precompiladas a su aplicación de LUIS, no necesitará expresiones de etiqueta con estas entidades. Para obtener más información sobre entidades precompiladas y cómo agregarlas, consulte la sección [Add entities](luis-how-to-add-entities.md#add-prebuilt-entity) (Agregar entidades).

## <a name="add-regular-expression-entity-label"></a>Adición de la etiqueta de la entidad de expresiones regulares
Si agrega las entidades de expresiones regulares a su aplicación de LUIS, no necesitará expresiones de etiqueta con estas entidades. Para obtener más información sobre entidades de expresiones regulares y cómo agregarlas, consulte la sección [Add entities](luis-how-to-add-entities.md#add-regular-expression-entities) (Agregar entidades).

## <a name="create-a-pattern-from-an-utterance"></a>Creación de un patrón a partir de una expresión
Consulte [Add pattern from existing utterance on intent or entity page](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page) (Adición de un patrón a partir de una de expresión existente en la página de intención o entidad).

## <a name="add-patternany-entity-label"></a>Adición de la etiqueta de entidad pattern.any
Si agrega las entidades pattern.any a su aplicación de LUIS, no puede etiquetar expresiones con estas entidades. Solo son válidas en los patrones. Para obtener más información sobre las entidades pattern-any y cómo agregarlas, consulte la sección [Agregar entidades](luis-how-to-add-entities.md#add-patternany-entities).

<!--
Fix this - moved to luis-how-to-add-intents.md - how ?

## Search in utterances
## Prediction discrepancy errors
## Filter by intent prediction discrepancy errors
## Filter by entity type
## Switch to token view
## Delete utterances
## Edit an utterance
## Reassign utterances

-->
## <a name="train-your-app-after-changing-model-with-utterances"></a>Entrenamiento de la aplicación después de cambiar el modelo con expresiones
Después de agregar, editar o quitar expresiones, [entrene](luis-how-to-train.md) y [publique](luis-how-to-publish-app.md) la aplicación para que los cambios se apliquen a las consultas de punto de conexión. 

## <a name="next-steps"></a>Pasos siguientes

Después de etiquetar expresiones en sus intenciones, puede crear una [entidad compuesta](luis-how-to-add-entities.md).
