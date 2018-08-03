---
title: Incorporación de entidades en las aplicaciones de LUIS | Microsoft Docs
titleSuffix: Azure
description: Agregue entidades (datos clave del dominio de la aplicación) a las aplicaciones de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: e97f9a5391799849983bd98db5400e0a842627b7
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224133"
---
# <a name="manage-entities"></a>Administrar entidades
Después de identificar las [intenciones](luis-concept-intent.md) de la aplicación, deberá [etiquetar las expresiones de ejemplo](luis-concept-utterance.md) con [entidades](luis-concept-entity-types.md). Las entidades son las partes importantes de un comando o una pregunta y pueden ser esenciales para que la aplicación cliente lleve a cabo su tarea. 

Puede agregar, editar o eliminar entidades de la aplicación a través de la **lista de entidades** en la página **Entities** (Entidades). LUIS ofrece dos tipos principales de entidades: [entidades precompiladas](luis-reference-prebuilt-entities.md) y sus propias entidades personalizadas.

Las secciones siguientes solo están disponibles dentro de una aplicación de LUIS, desde la sección **Build** (Compilación). El vínculo **Build** (Compilación) está en la barra de navegación superior. Una vez dentro de la sección **Build** (Compilación), seleccione **Entities** (Entidades) en el menú de navegación de la izquierda. Una vez que se agrega una entidad a la aplicación, si la entidad es de aprendizaje automático, puede [etiquetar la entidad](luis-how-to-add-example-utterances.md) dentro de la expresión. Una vez que se entrena y publica la aplicación, puede recibir datos de la entidad [extraídos](luis-concept-data-extraction.md) de la predicción. 

## <a name="add-prebuilt-entity"></a>Incorporación de entidad precompilada
Las entidades precompiladas están definidas en el proyecto [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text). Las entidades precompiladas comunes que se agregan a una aplicación son *number* y *datetimeV2*. 

1. En la aplicación, en la sección **Build** (Compilación), haga clic en **Entities** (Entidades) en el panel de la izquierda.
 
2. En la página **Entities** (Entidades), seleccione **Manage prebuilt entities** (Administrar entidades precompiladas).

    ![Captura de pantalla de la incorporación de una entidad precompilada en la página de entidades](./media/add-entities/manage-prebuilt-entities-button.png)

3. En el cuadro de diálogo **Add or remove prebuilt entities** (Agregar o quitar entidades precompiladas), seleccione las entidades precompiladas **number** y **datetimeV2**. A continuación, seleccione **Done** (Listo).

    ![Captura de pantalla del cuadro de diálogo para agregar entidad precompilada](./media/add-entities/list-of-prebuilt-entities.png)

    Consulte [Extracción de datos](luis-concept-data-extraction.md#prebuilt-entity-data) para más información sobre cómo extraer la entidad precompilada a partir de la respuesta a la consulta JSON del punto de conexión.

## <a name="add-simple-entities"></a>Incorporación de entidades simples
Una entidad simple es una entidad genérica que describe un concepto único. 

1. En la aplicación, en la sección **Build** (Compilación), haga clic en **Entities** en el panel de la izquierda y seleccione **Create new entity** (Crear nueva entidad).

    ![Captura de pantalla de la página Entities (Entidades) con el botón Create new entity (Crear nueva entidad) resaltado](./media/add-entities/create-new-entity-button.png)

2. En el cuadro de diálogo emergente, escriba `Airline` en el cuadro **Entity name** (Nombre de la entidad), seleccione **Simple** en la lista **Entity type** (Tipo de entidad) y seleccione **Done** (Listo).

    ![Captura de pantalla del cuadro de diálogo para crear una entidad simple Airline (Línea aérea)](./media/add-entities/create-simple-airline-entity.png)

    Vea [Extracción de datos](luis-concept-data-extraction.md#simple-entity-data) para obtener más información sobre cómo extraer la entidad simple a partir de la respuesta a la consulta JSON del punto de conexión. Pruebe el [inicio rápido](luis-quickstart-primary-and-secondary-data.md) de la entidad simple para obtener más información sobre cómo usar una entidad simple.

## <a name="add-regular-expression-entities"></a>Incorporación de entidades de expresión regular
Una entidad de expresión regular se usa para extraer datos de la expresión en función de una expresión regular que se proporciona. 

1. En la aplicación, seleccione **Entities** (Entidades) en el menú de navegación de la izquierda y seleccione **Create new entity** (Crear nueva entidad).

2. En el cuadro de diálogo emergente, escriba `AirFrance Flight` en el cuadro **Entity name** (Nombre de la entidad), seleccione **Regular expression** (Expresión regular) en la lista **Entity type** (Tipo de entidad), escriba la expresión regular `AFR[0-9]{3,4}` y seleccione **Done** (Listo). 

    Esta expresión regular AirFrance Flight (Vuelo de AirFrance) espera tres caracteres, literalmente `AFR`, y luego 3 o 4 dígitos. Los dígitos pueden ser cualquier número entre 0 y 9. La expresión regular coincide con números de vuelos de AirFrance como: "AFR101", "ARF1302" y "AFR5006". Consulte [Extracción de datos](luis-concept-data-extraction.md) para más información sobre cómo extraer la entidad a partir de la respuesta a la consulta JSON del punto de conexión.

    ![Imagen del cuadro de diálogo para crear una entidad de la expresión regular](./media/add-entities/regex-entity-create-dialog.png)

    Consulte [Extracción de datos](luis-concept-data-extraction.md#regular-expression-entity-data) para más información sobre cómo extraer una entidad de expresión regular de la respuesta a la consulta JSON del punto de conexión. Pruebe el [inicio rápido](luis-quickstart-intents-regex-entity.md) de la entidad de expresión regular para más información sobre cómo usar una entidad de la expresión regular.

## <a name="add-hierarchical-entities"></a>Incorporación de entidades jerárquicas
Una entidad jerárquica es una categoría de entidades relacionadas conceptualmente y aprendidas contextualmente. En el ejemplo siguiente, la entidad contiene ubicaciones de origen y destino. 

En la expresión `Book 2 tickets from Seattle to Cairo`, Seattle es la ubicación de origen y el Cairo es la ubicación de destino. Cada ubicación es contextualmente diferente y se aprende a partir del orden de palabras y elección de palabras en la expresión.

Complete estos pasos para agregar entidades jerárquicas: 

1. En la aplicación, seleccione **Entities** (Entidades) en el menú de navegación de la izquierda y seleccione **Create new entity** (Crear nueva entidad).

2. En el cuadro de diálogo emergente, escriba `Location` en el cuadro **Entity name** (Nombre de la entidad) y seleccione **Hierarchical** (Jerárquica) en la lista **Entity type** (Tipo de entidad).

    ![Incorporación de entidad jerárquica](./media/add-entities/hier-location-entity-creation.png)

3. Seleccione **Add Child** (Agregar elemento secundario) y escriba "Origen" en el cuadro **Child #1** (Elemento secundario n.°1). 

4. Seleccione **Add Child** (Agregar elemento secundario) y escriba "Destino" en el cuadro **Child #2** (Elemento secundario n.°2). Seleccione **Listo**.
5. 
    >[!NOTE]
    >Para eliminar a un elemento secundario, seleccione el icono de la papelera que está junto a él.

    >[!CAUTION]
    >Los nombres de entidad secundaria deben ser únicos en todas las entidades en una sola aplicación. Dos entidades jerárquicas diferentes no pueden contener entidades secundarias con el mismo nombre. 

    Consulte [Data Extraction](luis-concept-data-extraction.md#hierarchical-entity-data) (Extracción de datos) para obtener más información acerca de cómo extraer la entidad jerárquica a partir de la respuesta a la consulta JSON del punto de conexión. Pruebe el [inicio rápido](luis-quickstart-intent-and-hier-entity.md) de la entidad jerárquica para obtener más información sobre cómo usar una entidad jerárquica.

## <a name="add-composite-entities"></a>Incorporación de entidades compuestas
Puede definir las relaciones entre varias entidades existentes mediante la creación de una entidad compuesta. En el ejemplo siguiente, la entidad contiene recuento de incidencias y ubicaciones de origen y destino. 

En la expresión `Book 2 tickets from Seattle to Cairo`, el número 2 coincide con una entidad precompilada, Seattle es la ubicación de origen y El Cairo es la ubicación de destino. Cada entidad forma parte de una entidad principal más grande una vez creada la entidad compuesta.

1. En la aplicación, agregue el **número** de la entidad precompilada. Para instrucciones, consulte [Incorporación de entidades precompiladas](#add-prebuilt-entity). 

2. Agregue la entidad jerárquica `Location`, incluidos los subtipos: `origin`, `destination`. Para más instrucciones, consulte [Incorporación de entidades jerárquicas](#add-hierarchical-entities). 

3. Seleccione **Entities** (Entidades) en el menú de navegación de la izquierda y seleccione **Create new entity** (Crear nueva entidad).

4. En el cuadro de diálogo emergente, escriba `TicketsOrder` en el cuadro **Entity name** (Nombre de la entidad) y seleccione **Composite** (Compuesta) en la lista **Entity type** (Tipo de entidad).

5. Seleccione **Add Child** (Agregar elemento secundario) para agregar un nuevo elemento secundario.

6. En **Child #1** (Elemento secundario n.°1), seleccione el elemento **number** de la entidad en la lista.

7. En **Child #2** (Elemento secundario n.°2), seleccione el elemento **Location::Origin**de la entidad en la lista. 

8. En **Child #3** (Elemento secundario n.°3), seleccione el elemento **Location::Destination** de la entidad en la lista. 

9. Seleccione **Listo**.

    ![Imagen del cuadro de diálogo para crear una entidad compuesta](./media/add-entities/ticketsorder-composite-entity.png)

    >[!NOTE]
    >Para eliminar a un elemento secundario, seleccione el botón de la papelera que está junto a él.

    Vea [Extracción de datos](luis-concept-data-extraction.md#composite-entity-data) para obtener más información sobre cómo extraer la entidad compuesta a partir de la respuesta a la consulta JSON del punto de conexión. Pruebe el [tutorial](luis-tutorial-composite-entity.md) de entidades compuestas para obtener más información sobre cómo usar una entidad de compuesta.


## <a name="add-patternany-entities"></a>Incorporación de las entidades Pattern.any
Las entidades [Pattern.Any](luis-concept-entity-types.md) solo son válidas en los [patrones](luis-how-to-model-intent-pattern.md). Esta entidad ayuda a LUIS a buscar el final de las entidades de longitud variable y la elección de palabras. Dado que esta entidad se usa en un patrón, LUIS sabe dónde está el final de la entidad en la expresión.

Si una aplicación tiene una intención `FindBookInfo`, el título del libro puede interferir con la predicción de la intención. Con el fin de aclarar las palabras que están en el título del libro, use una entidad Pattern.any dentro de un patrón. La predicción de LUIS empieza con la expresión. En primer lugar, la expresión se comprueba y se buscan coincidencias con las entidades y, cuando se encuentran, se comprueba y hace coincidir el patrón. 

En la expresión `Who wrote the book Ask and when was it published?`, el título del libro, Ask, es complicado porque no resulta evidente en términos contextuales dónde finaliza el título y dónde empieza el resto de la expresión. Los títulos de libros pueden tener cualquier orden de palabras, incluida una sola palabra, frases complejas con signos de puntuación y una ordenación de palabras sin sentido. Un patrón permite crear una entidad donde se puede extraer la entidad completa y exacta. Una vez que se encuentra el título del libro, se predice la intención `FindBookInfo` porque esa es la intención del modelo.

1. En la aplicación, en la sección **Build** (Compilación), haga clic en **Entities** en el panel de la izquierda y seleccione **Create new entity** (Crear nueva entidad).

2. En el cuadro de diálogo **Add Entity** (Agregar entidad), escriba `BookTitle` en el cuadro **Entity name** (Nombre de la entidad) y seleccione **Pattern.any** como **Entity type** (Tipo de entidad).
 
    ![Captura de pantalla de creación de una entidad pattern.any](./media/add-entities/create-pattern-any-entity.png)

    Para usar la entidad pattern.any, agregue un patrón en la página **Patterns** (Patrones) (en la sección **Mejorar el rendimiento de las aplicaciones**) con la sintaxis de llave correcta, como "¿Quién es el autor de **{BookTitle}**?".

    Consulte [Extracción de datos](luis-concept-data-extraction.md#patternany-entity-data) para más información sobre cómo extraer la entidad Pattern.any de la respuesta a la consulta JSON del punto de conexión. Pruebe el tutorial sobre [patrones](luis-tutorial-pattern.md) para más información sobre cómo usar una entidad Pattern.any.

Si descubre que el patrón, cuando incluye una entidad Pattern.any, extrae entidades de forma incorrecta, use una [lista explícita](luis-concept-patterns.md#explicit-lists) para corregir este problema. 

## <a name="add-role-to-pattern-based-entity"></a>Incorporación de un rol a una entidad basada en patrón
Un rol es un subtipo con nombre de una entidad basada en el contexto. Es comparable a una entidad [jerárquica](#add-hierarchical-entities), pero los roles solo se usan en [patrones](luis-how-to-model-intent-pattern.md). 

Por ejemplo, un billete de plano tiene una *ciudad de origen* y una *ciudad de destino*, pero ambas son ciudades. LUIS determina que ambas son ciudades y puede determinar la ciudad de origen y la ciudad de destino en función del contexto del orden de las palabras y la elección de palabras. 

La sintaxis de un rol es **{Entity name:Role name}**, donde después del nombre de la entidad aparecen dos puntos y, luego, el nombre del rol. Por ejemplo, "Reservar un billete de {Location:Origin} a {Location:Destination}".

1. En la aplicación, en la sección **Build** (Compilación), seleccione **Entities** (Entidades) en el panel de la izquierda.

2. Haga clic en **Crear nueva entidad**. Escriba el nombre de `Location`. Seleccione el tipo **Simple** y seleccione **Done** (Listo).

3. Seleccione **Entities** (Entidades) en el panel de la izquierda y luego seleccione la nueva **Location** (Ubicación) de la entidad que se creó en el paso 2.

4. En el cuadro de texto **Role name** (Nombre del rol), escriba el nombre del rol `Origin` y presione ENTRAR. Agregue un segundo nombre de rol de `Destination`. Por ejemplo, un viaje en avión puede tener una ciudad de origen y destino. Los dos roles son "Origen" y "Destino".

    ![Captura de pantalla de la incorporación del rol de origen a la entidad de ubicación](./media/add-entities/roles-enter-role-name-text.png)

    Consulte [Extracción de datos](luis-concept-data-extraction.md) para más información sobre cómo extraer los roles a partir de la respuesta a la consulta JSON del punto de conexión. Pruebe el tutorial sobre patrones para más información sobre cómo usar una entidad Pattern.any.

## <a name="add-list-entities"></a>Incorporación de entidades de lista
Las entidades de la lista representan un conjunto fijo y cerrado de palabras relacionadas en el sistema. 

Para una entidad de la lista de bebidas, puede tener dos valores normalizados: agua y refresco. Cada nombre normalizado tiene sinónimos. Para el agua, los sinónimos son H20, con gas y natural. Para el refresco, los sinónimos son frutas, cola y jengibre. No es necesario conocer todos los valores cuando se crea la entidad. Puede agregar más después de revisar las expresiones reales del usuario con los sinónimos.

|Nombre normalizado|Sinónimos|
|--|--|
|Agua|H20, con gas, natural|
|Refresco|Frutas, cola, jengibre|

1. En la aplicación, en la sección **Build** (Compilación), haga clic en **Entities** en el panel de la izquierda y seleccione **Create new entity** (Crear nueva entidad).

2. En el cuadro de diálogo **Add Entity** (Agregar entidad), escriba `Drinks` en el cuadro **Entity name** (Nombre de la entidad) y seleccione **List** (Lista) como **Entity type** (Tipo de entidad). Seleccione **Listo**.
 
    ![Imagen del cuadro de diálogo de creación de la entidad de lista Drinks](./media/add-entities/menu-list-dialog.png)
  
3.  La página de entidad de lista permite agregar nombres normalizados. En el cuadro de texto **Values** (Valores), escriba un elemento para la lista, como `Water` para las lista de bebidas y presione la tecla ENTRAR. 

    ![Captura de pantalla de la entidad de lista Drinks con el valor normalizado Water (Agua) en el cuadro de texto](./media/add-entities/entity-list-normalized-name.png)

4. A la derecha del valor normalizado **water** (agua), escriba los sinónimos `h20`, `flat`, y `gas`. Presione la tecla ENTRAR después escribir cada elemento.

    ![Captura de pantalla de la entidad de lista Drinks con elementos sinónimos para agua resaltados](./media/add-entities/menu-list-synonyms.png)

5. Si quiere más elementos normalizados para la lista, seleccione **Recommend** (Recomendar) para ver las opciones del [diccionario de semántica](luis-glossary.md#semantic-dictionary).

    ![Captura de pantalla de la entidad de lista Drinks con los elementos recomendados resaltados](./media/add-entities/entity-list-recommended-list.png)

6. Seleccione un elemento en la lista recomendada para agregarlo como un valor normalizado o seleccione **Add all** (Agregar todo) para agregar todos los elementos. 

    ![Captura de pantalla de la entidad de lista Drinks con el elemento recomendado para cerveza y el botón Add all (Agregar todo) resaltados](./media/add-entities/list-entity-add-suggestions.png)

    Vea [Extracción de datos](luis-concept-data-extraction.md#list-entity-data) para obtener más información sobre cómo extraer entidades de la lista de la respuesta a la consulta JSON del punto de conexión. Pruebe el [inicio rápido](luis-quickstart-intent-and-list-entity.md) para obtener más información sobre cómo usar una entidad de la lista.

## <a name="import-list-entity-values"></a>Importación de valores de entidad de lista
Puede importar valores a una entidad de la lista existente.

 1. En la página de entidad de lista, seleccione **Import Lists** (Importar listas).

 2. En el cuadro de diálogo **Import New Entries** (Importar entradas nuevas), seleccione **Choose File** (Elegir archivo) y seleccione el archivo JSON que incluye la lista.

    ![Captura de pantalla del cuadro de diálogo emergente para importar valores de la entidad de lista](./media/add-entities/menu-list-import-json-dialog-with-file.png)

    >[!NOTE]
    >LUIS solo importa los archivos con la extensión ".json".

 3. Para información sobre la sintaxis de lista compatible con JSON, seleccione **Learn about supported list syntax** (Información sobre la sintaxis de lista compatible) para expandir el cuadro de diálogo y mostrar un ejemplo de la sintaxis permitida. Para contraer el cuadro de diálogo y ocultar la sintaxis, vuelva a seleccionar el título del vínculo.

 4. Seleccione **Listo**.

    En el código con formato JSON siguiente se muestra un ejemplo de JSON válido para una entidad de lista **Colors** (Colores):

    ```
    [
        {
            "canonicalForm": "Blue",
            "list": [
                "navy",
                "royal",
                "baby"
            ]
        },
        {
            "canonicalForm": "Green",
            "list": [
                "kelly",
                "forest",
                "avacado"
            ]
        }
    ]  
    ```

## <a name="edit-entity-name"></a>Edición del nombre de la entidad
1. En la página de lista de **entidades**, seleccione la entidad en la lista. Esta acción lo lleva a la página **Entity** (Entidad).

2. En la página **Entity** (Entidad), puede editar el nombre de la entidad mediante la selección del icono de edición que se encuentra junto al nombre de la entidad. El tipo de entidad no se puede editar. 

## <a name="delete-entity"></a>Eliminación de la entidad

En la página **Entity** (Entidad), seleccione el botón **Delete Entity** (Eliminar entidad). Después, seleccione **Aceptar** en el mensaje de confirmación para confirmar la eliminación.
 
![Captura de pantalla de la página de la entidad de ubicación con el botón de eliminación de entidad resaltado](./media/add-entities/entity-delete.png)

>[!NOTE]
>* Cuando se elimina una entidad jerárquica, se eliminan todas las entidades secundarias.
>* Cuando se elimina una entidad compuesta, solo se elimina esto último y se interrumpe la relación compuesta, pero no se eliminan las entidades que la conforman.

## <a name="changing-entity-type"></a>Cambio del tipo de entidad
LUIS no permite cambiar el tipo de la entidad porque no sabe qué agregar o quitar para construir esa entidad. A fin de cambiar el tipo, se recomienda crear una entidad del tipo correcto con un nombre ligeramente distinto. Una vez creada la entidad, en cada expresión debe quitar el nombre de entidad con etiqueta anterior y agregar el nuevo nombre de entidad. Una vez que se vuelven a etiquetar las expresiones, elimine la entidad anterior. 

## <a name="create-a-pattern-from-an-utterance"></a>Creación de un patrón a partir de una expresión
Consulte [Add pattern from existing utterance on intent or entity page](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page) (Adición de un patrón a partir de una de expresión existente en la página de intención o entidad).

## <a name="search-utterances"></a>Búsqueda de expresiones
Puede usar el icono de lupa que aparece en la barra de herramientas para buscar y filtrar expresiones. 

## <a name="train-your-app-after-changing-model-with-entities"></a>Entrenamiento de la aplicación después de cambiar el modelo con entidades
Después de agregar, editar o quitar entidades, [entrene](luis-how-to-train.md) y [publique](luis-how-to-publish-app.md) la aplicación para que los cambios se apliquen a las consultas de punto de conexión. 

## <a name="next-steps"></a>Pasos siguientes
Ahora que agregó las intenciones, las expresiones y las entidades, tendrá una aplicación de LUIS básica. Obtenga información sobre cómo [entrenar](luis-how-to-train.md), [probar](luis-interactive-test.md) y [publicar](luis-how-to-publish-app.md) la aplicación.
 
