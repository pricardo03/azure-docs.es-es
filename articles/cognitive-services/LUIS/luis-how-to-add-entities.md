---
title: agregar entidades
titleSuffix: Language Understanding - Azure Cognitive Services
description: Cree entidades para extraer datos clave de las expresiones de usuario de las aplicaciones de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 241e89ac7fa78184e7c55f9e8065e1534cea9143
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65148727"
---
# <a name="create-entities-without-utterances"></a>Creación de entidades sin expresiones

La entidad representa una palabra o frase dentro de la expresión que quiere extraer. Una entidad representa una clase que incluye una colección de objetos parecidos (lugares, cosas, personas, eventos o conceptos). Las entidades describen información relevante para la intención y a veces son esenciales para que la aplicación lleve a cabo la tarea. Puede crear entidades cuando agregue una expresión a una intención o aparte de agregar (antes o después) una expresión a una intención.

Puede agregar, editar o eliminar entidades de la aplicación de LUIS a través de la **lista de entidades** en la página **Entidades**. LUIS ofrece dos tipos principales de entidades: [entidades precompiladas](luis-reference-prebuilt-entities.md) y [entidades del usuario personalizadas](luis-concept-entity-types.md#types-of-entities).

Una vez que se crea una entidad de aprendizaje automático, debe marcar esa entidad en todas las expresiones de ejemplo de todas las intenciones en las que se encuentra.

<a name="add-prebuilt-entity"></a>

## <a name="add-a-prebuilt-entity-to-your-app"></a>Adición de una entidad precompilada a la aplicación

Las entidades precompiladas comunes que se agregan a una aplicación son *number* y *datetimeV2*. 

1. En la aplicación, en la sección **Compilar**, seleccione **Entidades** en el panel de la izquierda.
 
1. En la página **Entidades**, seleccione **Add prebuilt entities** (Agregar entidades precompiladas).

1. En el cuadro de diálogo **Add prebuilt entities** (Agregar entidades precompiladas), seleccione las entidades precompiladas **number** y **datetimeV2**. A continuación, seleccione **Done** (Listo).

    ![Captura de pantalla del cuadro de diálogo para agregar entidad precompilada](./media/add-entities/list-of-prebuilt-entities.png)

<a name="add-simple-entities"></a>

## <a name="add-simple-entities-for-single-concepts"></a>Adición de entidades sencillas para conceptos únicos

Una entidad simple describe un concepto único. Utilice el procedimiento siguiente para crear una entidad que extraiga los nombres de departamento de la empresa como *Recursos humanos* u *Operaciones*.   

1. En la aplicación, seleccione la sección **Build** (Compilar) y, después, seleccione **Entities** (Entidades) en el panel de la izquierda y seleccione **Create new entity** (Crear nueva entidad).

1. En el cuadro de diálogo emergente, escriba `Location` en el cuadro **Entity name** (Nombre de la entidad), seleccione **Simple** en la lista **Entity type** (Tipo de entidad) y seleccione **Done** (Listo).

    Una vez creada esta entidad, vaya a todas las intenciones que tienen expresiones de ejemplo que contienen la entidad. Seleccione el texto de la expresión de ejemplo y marque el texto como la entidad. 

    Habitualmente se usa una [lista de frases](luis-concept-feature.md) para mejorar la señal de las entidades sencillas.

<a name="add-regular-expression-entities"></a>

## <a name="add-regular-expression-entities-for-highly-structured-concepts"></a>Adición de entidades de expresión regular para conceptos muy estructurados

Una entidad de expresión regular se usa para extraer datos de la expresión en función de una expresión regular que se proporciona. 

1. En la aplicación, seleccione **Entities** (Entidades) en el menú de navegación de la izquierda y seleccione **Create new entity** (Crear nueva entidad).

1. En el cuadro de diálogo emergente, escriba `Human resources form name` en el cuadro **Entity name** (Nombre de la entidad), seleccione **Regular expression** (Expresión regular) en la lista **Entity type** (Tipo de entidad), escriba la expresión regular `hrf-[0-9]{6}` y, después, haga clic en **Done** (Listo). 

    Esta expresión regular coincide con los caracteres literales `hrf-`, luego 6 dígitos para representar un número de formulario para un formulario de recursos humanos.

<a name="add-composite-entities"></a>

## <a name="add-composite-entities-to-group-into-a-parent-child-relationship"></a>Adición de entidades compuestas para agrupar en una relación de elementos primarios y secundarios

Puede definir las relaciones entre entidades de distintos tipos si crea una entidad compuesta. En el ejemplo siguiente, la entidad contiene una expresión regular y una entidad precompilada de name.  

En la expresión `Send hrf-123456 to John Smith`, el texto `hrf-123456` coincide con una [expresión regular](#add-regular-expression-entities) de recursos humanos y `John Smith` se extrae con la entidad precompilada personName. Cada entidad forma parte de una entidad principal de mayor tamaño. 

1. En la aplicación, seleccione **Entities** (Entidades) desde el panel de navegación de la izquierda de la sección **Build** (Compilar) y luego seleccione **Add prebuilt entity** (Agrear entidad precompilada).

1. Agregue la entidad precompilada **PersonName**. Para ver instrucciones, consulte [Incorporación de entidades precompiladas](#add-prebuilt-entity). 

1. Seleccione **Entities** (Entidades) en el menú de navegación de la izquierda y seleccione **Create new entity** (Crear nueva entidad).

1. En el cuadro de diálogo emergente, escriba `SendHrForm` en el cuadro **Entity name** (Nombre de la entidad) y seleccione **Composite** (Compuesta) en la lista **Entity type** (Tipo de entidad).

1. Seleccione **Add Child** (Agregar elemento secundario) para agregar un nuevo elemento secundario.

1. En **Child #1** (Elemento secundario n.°1), seleccione el elemento **number** de la entidad en la lista.

1. En **Child #2** (Elemento secundario n.° 2), seleccione la entidad **Human resources form name** (Nombre del formulario de recursos humanos) en la lista. 

1. Seleccione **Listo**.

<a name="add-pattern-any-entities"></a>

## <a name="add-patternany-entities-to-capture-free-form-entities"></a>Adición de entidades Pattern.any para capturar entidades de forma libre

Las entidades [Pattern.Any](luis-concept-entity-types.md) solo son válidas en los [patrones](luis-how-to-model-intent-pattern.md), no en las intenciones. Este tipo de entidad ayuda a LUIS a buscar el final de las entidades de longitud variable y la elección de palabras. Dado que esta entidad se usa en un patrón, LUIS sabe dónde está el final de la entidad en la plantilla de expresión.

Si una aplicación tiene una intención `FindHumanResourcesForm`, el título del formulario extraído puede interferir con la predicción de la intención. Con el fin de aclarar las palabras que están en el título del formulario, use una entidad Pattern.any dentro de un patrón. La predicción de LUIS empieza con la expresión. En primer lugar, la expresión se comprueba y se buscan coincidencias con las entidades y, cuando se encuentran, se comprueba y hace coincidir el patrón. 

En la expresión `Where is Request relocation from employee new to the company on the server?`, el título del formulario es complicado porque no resulta evidente en términos contextuales dónde finaliza el título y dónde empieza el resto de la expresión. Los títulos pueden tener cualquier orden de palabras, incluida una sola palabra, frases complejas con signos de puntuación y una ordenación de palabras sin sentido. Un patrón permite crear una entidad donde se puede extraer la entidad completa y exacta. Una vez que se encuentra el título, se predice la intención `FindHumanResourcesForm` porque esa es la intención del patrón.

1. En la sección **Build** (Compilar), seleccione **Entities** (Entidades) en el panel de la izquierda y seleccione **Create new entity** (Crear nueva entidad).

1. En el cuadro de diálogo **Add Entity** (Agregar entidad), escriba `HumanResourcesFormTitle` en el cuadro **Entity name** (Nombre de entidad) y seleccione **Pattern.any** como **Entity type**. (Tipo de entidad).

    Para usar la entidad pattern.any, agregue un patrón en la página **Patterns** (Patrones), en la sección **Mejorar el rendimiento de las aplicaciones** con la sintaxis de llave correcta, como `Where is **{HumanResourcesFormTitle}** on the server?`.

    Si descubre que el patrón, cuando incluye una entidad Pattern.any, extrae entidades de forma incorrecta, use una [lista explícita](luis-concept-patterns.md#explicit-lists) para corregir este problema. 

<a name="add-a-role-to-pattern-based-entity"></a>

## <a name="add-a-role-to-distinguish-different-contexts"></a>Adición de un rol para distinguir diferentes contextos

Un rol es un subtipo con nombre basado en el contexto. Está disponible en todas las entidades, incluidas las entidades precompiladas y las entidades que no son de aprendizaje automático. 

La sintaxis de un rol es **`{Entityname:Rolename}`** , donde el nombre de la entidad va seguido por dos puntos y, luego, el nombre del rol. Por ejemplo, `Move {personName} from {LocationUsingRoles:Origin} to {LocationUsingRoles:Destination}`.

1. En la sección **Build** (Compilar), seleccione **Entities** (Entidades) en el panel de la izquierda.

1. Haga clic en **Crear nueva entidad**. Escriba el nombre de `LocationUsingRoles`. Seleccione el tipo **Simple** y seleccione **Done** (Listo). 

1. Seleccione **Entities** (Entidades) en el panel de la izquierda y luego seleccione la nueva entidad **LocationUsingRoles** creada en el paso anterior.

1. En el cuadro de texto **Role name** (Nombre del rol), escriba el nombre del rol `Origin` y presione ENTRAR. Agregue un segundo nombre de rol de `Destination`. 

    ![Captura de pantalla de la incorporación del rol de origen a la entidad de ubicación](./media/add-entities/roles-enter-role-name-text.png)

<a name="add-list-entities"></a>

## <a name="add-list-entities-for-exact-matches"></a>Adición de entidades de lista para coincidencias exactas

Las entidades de la lista representan un conjunto fijo y cerrado de palabras relacionadas. 

Para una aplicación de recursos humanos, puede tener una lista de todos los departamentos junto con los sinónimos de los mismos. No es necesario conocer todos los valores cuando se crea la entidad. Puede agregar más después de revisar las expresiones reales del usuario con los sinónimos.

1. En la sección **Build** (Compilar), seleccione **Entities** (Entidades) en el panel de la izquierda y seleccione **Create new entity** (Crear nueva entidad).

1. En el cuadro de diálogo **Add Entity** (Agregar entidad), escriba `Department` en el cuadro **Entity name** (Nombre de la entidad) y seleccione **List** (Lista) como **Entity type** (Tipo de entidad). Seleccione **Listo**.
  
1. La página de entidad de lista permite agregar nombres normalizados. En el cuadro de texto **Values** (Valores), escriba un nombre de departamento para ver la lista, como `HumanResources`, y presione ENTRAR en el teclado. 

1. A la derecha del valor normalizado, escriba sinónimos. Presione la tecla ENTRAR después escribir cada elemento.

1. Si quiere más elementos normalizados para la lista, seleccione **Recommend** (Recomendar) para ver las opciones del [diccionario de semántica](luis-glossary.md#semantic-dictionary).

    ![Captura de pantalla de la selección de características recomendadas para ver las opciones](./media/add-entities/hr-list-2.png)


1. Seleccione un elemento en la lista recomendada para agregarlo como un valor normalizado o seleccione **Add all** (Agregar todo) para agregar todos los elementos. 
    Puede importar los valores en una entidad de lista existente con el formato JSON siguiente:

    ```JSON
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

<a name="change-entity-type"></a>

## <a name="do-not-change-entity-type"></a>No cambiar el tipo de entidad

LUIS no permite cambiar el tipo de la entidad porque no sabe qué agregar o quitar para construir esa entidad. A fin de cambiar el tipo, se recomienda crear una entidad del tipo correcto con un nombre ligeramente distinto. Una vez creada la entidad, en cada expresión debe quitar el nombre de entidad con etiqueta anterior y agregar el nuevo nombre de entidad. Una vez que se vuelven a etiquetar las expresiones, elimine la entidad anterior. 

<a name="create-a-pattern-from-an-utterance"></a>

## <a name="create-a-pattern-from-an-example-utterance"></a>Creación de un patrón a partir de una expresión de ejemplo

Consulte [Add pattern from existing utterance on intent or entity page](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page) (Adición de un patrón a partir de una de expresión existente en la página de intención o entidad).

## <a name="train-your-app-after-changing-model-with-entities"></a>Entrenamiento de la aplicación después de cambiar el modelo con entidades

Después de agregar, editar o quitar entidades, [entrene](luis-how-to-train.md) y [publique](luis-how-to-publish-app.md) la aplicación para que los cambios se apliquen a las consultas de punto de conexión. 

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las entidades preconfiguradas, consulte el proyecto [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text). 

Para información sobre cómo la entidad aparece en la respuesta de la consulta de punto JSON, [Extracción de datos](luis-concept-data-extraction.md)

Ahora que agregó las intenciones, las expresiones y las entidades, tendrá una aplicación de LUIS básica. Obtenga información sobre cómo [entrenar](luis-how-to-train.md), [probar](luis-interactive-test.md) y [publicar](luis-how-to-publish-app.md) la aplicación.
 
