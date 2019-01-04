---
title: Incorporación de expresiones de ejemplo
titleSuffix: Language Understanding - Azure Cognitive Services
description: Las expresiones de ejemplo son ejemplos de texto de preguntas de los usuarios o de comandos. Para entrenar el servicio Language Understanding (LUIS), debe agregar expresiones de ejemplo a una intención.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 33c941f84952faca1961bb65687b4098b837a2fd
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53139181"
---
# <a name="add-an-entity-to-example-utterances"></a>Incorporación de una entidad a expresiones de ejemplo 

Las expresiones de ejemplo son ejemplos de texto de preguntas de los usuarios o de comandos. Para entrenar el servicio Language Understanding (LUIS), debe agregar [expresiones de ejemplo](luis-concept-utterance.md) a una [intención](luis-concept-intent.md).

En general, primero se agrega una expresión de ejemplo a una intención y luego se crean expresiones de etiquetas y entidades en la página de intención. Si prefiere crear primero las entidades, consulte [Agregar entidades](luis-how-to-add-entities.md).

## <a name="marking-entities-in-example-utterances"></a>Marcado de entidades en expresiones de ejemplo

Cuando se selecciona texto en expresión de ejemplo para marcarlo para una entidad, aparece un menú emergente en contexto. Use este menú para crear o seleccionar una entidad. 

En la expresión de ejemplo no se pueden etiquetar determinados tipos de entidad, como las precompiladas y las entidades de expresión regular, ya que se etiquetan automáticamente. 

## <a name="add-a-simple-entity"></a>Incorporación de una entidad sencilla

En el siguiente procedimiento se crea y se etiqueta una entidad personalizada dentro de la siguiente expresión en la página de la intención:

```text
Does John Smith work in Seattle?
```

1. Seleccione a `Seattle` en la expresión para etiquetarlo como entidad sencilla.

    [![Captura de pantalla de la selección de texto en una expresión como entidad sencilla](./media/luis-how-to-add-example-utterances/hr-create-simple-1.png)](./media/luis-how-to-add-example-utterances/hr-create-simple-1.png)

    > [!NOTE]
    > Al seleccionar palabras para etiquetarlas como entidades:
    > * Para una sola palabra, simplemente selecciónela. 
    > * Para un conjunto de dos o más palabras, seleccione el principio y el final del conjunto.

1. En el cuadro desplegable de la entidad que aparece, puede seleccionar una entidad existente o agregar una nueva entidad. Para agregar una nueva entidad, escriba el nombre en el cuadro de texto y luego seleccione **Crear nueva entidad**. 

    ![Captura de pantalla de la especificación del nombre de la entidad](./media/luis-how-to-add-example-utterances/hr-create-simple-2.png)

1. En el cuadro de diálogo emergente **What type of entity do you want to create?** (¿Qué tipo de entidad desea crear?), verifique el nombre de entidad, y seleccione el tipo de entidad **Simple** (Sencilla) y **Done** (Listo).

    Habitualmente se usa una [lista de frases](luis-concept-feature.md) para mejorar la señal de las entidades sencillas.

## <a name="add-a-list-entity"></a>Incorporación de una entidad de lista

Las entidades de la lista representan un conjunto fijo y cerrado (coincidencias de texto exacto) de palabras relacionadas en el sistema. 

Para la lista de departamento de una empresa, puede tener valores normalizados: `Accounting` y `Human Resources`. Cada nombre normalizado tiene sinónimos. Para un departamento, estos sinónimos pueden incluir cualquier acrónimo, número o jerga de departamento. No es necesario conocer todos los valores cuando se crea la entidad. Puede agregar más después de revisar las expresiones reales del usuario con los sinónimos.

1. En la lista de expresiones de ejemplo, para una expresión específica, seleccione la palabra o frase que desea incluir en la nueva lista. A continuación, escriba el nombre de la lista en el cuadro de texto superior y seleccione **Create new entity**(Crear nueva entidad).   

    ![Captura de pantalla de la especificación del nombre de la entidad de lista](./media/luis-how-to-add-example-utterances/hr-create-list-1.png)


1. En el cuadro de diálogo emergente **What type of entity do you want to create?** (¿Qué tipo de entidad desea crear?), asígnele un nombre a la entidad y seleccione **List** (Lista) como tipo. Agregue sinónimos de este elemento de lista y seleccione **Done** (Listo). 

    ![Captura de pantalla de la especificación de sinónimos de la entidad de lista](./media/luis-how-to-add-example-utterances/hr-create-list-2.png)

    Puede agregar más elementos de lista o más sinónimos de los elementos al etiquetar otras expresiones o al editar la entidad en **Entities** (Entidades) en el panel de navegación izquierdo. La [edición](luis-how-to-add-entities.md#add-list-entities) de las entidades proporciona la opción de escribir elementos adicionales con los sinónimos correspondientes o de importar una lista. 

## <a name="add-composite-entity"></a>Incorporación de entidad compuesta

Las entidades compuestas se crean a partir **entidades** existentes en una entidad primaria. 

Con la expresión `Does John Smith work in Seattle?`, una expresión compuesta puede devolver información de entidad del nombre del empleado y la ubicación en un objeto primario sencillo. 

El nombre del empleado, John Smith, es una entidad [personName](luis-reference-prebuilt-person.md) precompilada. La ubicación, Seattle, es una entidad personalizada sencilla. Una vez creadas y etiquetadas las dos entidades en una expresión de ejemplo, se pueden encapsular en una entidad compuesta. 

1. Para encapsular las entidades individuales en una composición, seleccione la **primera** entidad etiquetada (en la izquierda) en la expresión de la entidad compuesta. Aparece una lista de desplegable que muestra las opciones de esta selección.

1. Seleccione **Wrap in composite entity** (Encapsular la entidad compuesta) en la lista desplegable. 

    ![Captura de pantalla de la selección "Wrap in composite entity" (Encapsular la entidad compuesta)](./media/luis-how-to-add-example-utterances/hr-create-composite-1.png)

1. Seleccione la última palabra de la entidad compuesta (en la derecha). Observe que una línea verde sigue a la entidad compuesta.

1. Escriba el nombre de la entidad compuesta en la lista desplegable.

    ![Captura de pantalla de la especificación del nombre de la entidad compuesta en la lista desplegable](./media/luis-how-to-add-example-utterances/hr-create-composite-2.png)

    Cuando se encapsulan las entidades correctamente, aparece una línea verde debajo de toda la frase.

1. Valide los detalles de la entidad compuesta en el cuadro de diálogo emergente **What type of entity do you want to create?** (¿Qué tipo de entidad desea crear?) y seleccione **Done** (Listo).

    ![Captura de pantalla del elemento emergente de los detalles de la entidad](./media/luis-how-to-add-example-utterances/hr-create-composite-3.png)

1. La entidad compuesta aparece con las entidades individuales resaltadas en azul y la entidad compuesta subrayada en verde. 

    ![Captura de pantalla de la página de detalles de Intents (Intenciones) con la entidad compuesta](./media/luis-how-to-add-example-utterances/hr-create-composite-4.png)

## <a name="add-hierarchical-entity"></a>Incorporación de entidad jerárquica

Una entidad jerárquica es una categoría de entidades relacionadas conceptualmente y aprendidas contextualmente. En el ejemplo siguiente, la entidad contiene ubicaciones de origen y destino. 

En la expresión `Move John Smith from Seattle to Cairo`, Seattle es la ubicación de origen y el Cairo es la ubicación de destino. Cada ubicación es contextualmente diferente y se aprende a partir del orden de palabras y elección de palabras en la expresión.

1. En la página de la intención, en la expresión, seleccione `Seattle`, escriba el nombre de entidad `Location` y seleccione Entrar en el teclado.

    ![Captura de pantalla del cuadro de diálogo Create Hierarchical Entity Labeling "Crear etiquetado de entidad jerárquico"](./media/luis-how-to-add-example-utterances/hr-hier-1.png)

1. En el cuadro de diálogo emergente **What type of entity do you want to create?** (¿Qué tipo de entidad desea crear?), seleccione _Hierarchical_ (Jerárquico) como **tipo de entidad** y agregue `Origin` y `Destination` como elementos secundarios, después, seleccione **Done** (Listo).

    ![Captura de pantalla de la página de detalles de intención, con la entidad ToLocation resaltada](./media/luis-how-to-add-example-utterances/create-location-hierarchical-entity.png)

1. La palabra en la expresión se etiqueta con la entidad jerárquica primaria. Debe asignar la palabra a una entidad secundaria. Vuelva a la expresión en la página de detalles de la intención. Seleccione la palabra, a continuación, en la lista desplegable, elija el nombre de la entidad que ha creado y siga el menú a la derecha para elegir la entidad secundaria correcta.

    ![Captura de pantalla de la página de detalles de intenciones, donde deberá asignar la palabra a una entidad secundaria](./media/luis-how-to-add-example-utterances/hr-hier-3.png)

    >[!CAUTION]
    >Los nombres de entidad secundaria deben ser únicos en todas las entidades en una sola aplicación. Dos entidades jerárquicas diferentes no pueden contener entidades secundarias con el mismo nombre. 

## <a name="entity-status-predictions"></a>Predicciones de estado de entidad

Cuando indica una nueva expresión en el portal de LUIS, la expresión puede tener errores de predicción de entidad. El error de predicción es una diferencia entre cómo una entidad se etiqueta en comparación con cómo LUIS ha predicho la entidad. 

Esta diferencia se representa visualmente en el portal de LUIS con un subrayado rojo en la expresión. El subrayado rojo puede aparecer entre corchetes de la entidad o fuera de los corchetes. 

![Captura de pantalla de la discrepancia de predicción del estado de entidad](./media/luis-how-to-add-example-utterances/entity-prediction-error.png)

Seleccione las palabras que se hayan subrayado en rojo en la expresión. 

El cuadro de la entidad muestra el **Estado de entidad** con un signo de exclamación rojo si hay una discrepancia de predicción. Para ver el Estado de entidad con información sobre la diferencia entre las entidades etiquetadas y previstas, seleccione **Estado de entidad** y seleccione el elemento a la derecha.

![Captura de pantalla de la selección del elemento correcto para corregir la discrepancia en la predicción](./media/luis-how-to-add-example-utterances/entity-status.png)

La línea roja puede aparecer en cualquiera de los momentos siguientes:

   * Cuando se declara una expresión, pero antes de que se etiquete la entidad
   * Cuando se aplica la etiqueta de la entidad
   * Cuando se quita la etiqueta de la entidad
   * Cuando se predice más de una etiqueta de entidad para ese texto 

Las siguientes soluciones ayudan a resolver la discrepancia de predicción de la entidad:

|Entidad|Indicador visual|Predicción|Solución|
|--|--|--|--|
|Expresión declarada, la entidad no está etiquetada aún.|subrayado rojo|La predicción es correcta.|Etiquete la entidad con el valor predicho.|
|Texto sin etiquetar|subrayado rojo|Predicción incorrecta|Las expresiones actuales con esta entidad incorrecta tienen que revisarse en todas las intenciones. Las expresiones actuales han enseñado mal a LUIS que este texto es la entidad predicha.
|Texto etiquetado correctamente|resaltado azul de la entidad, subrayado rojo|Predicción incorrecta|Proporcione más expresiones con la entidad etiquetada correctamente en una variedad de lugares y usos. Las expresiones actuales no son suficientes para enseñar a LUIS que se trata de la entidad o aparecen entidades similares en el mismo contexto. La entidad similar debe combinarse en una única entidad de modo que LUIS no se confunda. Otra solución es agregar una lista de frases para aumentar la significación de las palabras. |
|Texto etiquetado incorrectamente|resaltado azul de la entidad, subrayado rojo|Predicción correcta| Proporcione más expresiones con la entidad etiquetada correctamente en una variedad de lugares y usos. 

## <a name="other-actions"></a>Otras acciones

Puede realizar acciones en las expresiones de ejemplo como grupo selecto o como elemento individual. Los grupos de expresiones de ejemplo selectos cambian el menú contextual sobre la lista. Los elementos individuales pueden usar tanto el menú contextual sobre la lista como los puntos suspensivos contextuales al final de cada fila de expresión. 

### <a name="remove-entity-labels-from-utterances"></a>Eliminación de etiquetas de entidad de las expresiones

Puede quitar las etiquetas de entidad de aprendizaje automático de una expresión en la página de intención. Si la entidad no procede del aprendizaje automático, no se puede quitar de una expresión. Si tiene que quitar una entidad que no es de aprendizaje automático de la expresión, debe eliminar la entidad de toda la aplicación. 

Para quitar una etiqueta de entidad de una expresión de aprendizaje automático, seleccione la entidad en la expresión. A continuación, seleccione **Quitar etiqueta** en el cuadro desplegable de la entidad que aparece.

![Captura de pantalla de la página de detalles de intención, con la opción Quitar etiqueta resaltada](./media/luis-how-to-add-example-utterances/remove-label.png) 

### <a name="add-prebuilt-entity-label"></a>Adición de etiqueta de entidad precompilada

Al agregar las entidades precompiladas a la aplicación de LUIS, no necesitará etiquetar expresiones con estas entidades. Para obtener más información sobre entidades precompiladas y cómo agregarlas, consulte la sección [Add entities](luis-how-to-add-entities.md#add-prebuilt-entity) (Agregar entidades).

### <a name="add-regular-expression-entity-label"></a>Adición de la etiqueta de la entidad de expresiones regulares

Al agregar las entidades de expresiones regulares a la aplicación de LUIS, no necesitará etiquetar expresiones con estas entidades. Para obtener más información sobre entidades de expresiones regulares y cómo agregarlas, consulte la sección [Add entities](luis-how-to-add-entities.md#add-regular-expression-entities) (Agregar entidades).


### <a name="create-a-pattern-from-an-utterance"></a>Creación de un patrón a partir de una expresión

Consulte [Add pattern from existing utterance on intent or entity page](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page) (Adición de un patrón a partir de una de expresión existente en la página de intención o entidad).


### <a name="add-patternany-entity"></a>Incorporación de la entidad pattern.any

Si agrega las entidades pattern.any a su aplicación de LUIS, no puede etiquetar expresiones con estas entidades. Solo son válidas en los patrones. Para obtener más información sobre las entidades pattern-any y cómo agregarlas, consulte la sección [Agregar entidades](luis-how-to-add-entities.md#add-patternany-entities).

## <a name="train-your-app-after-changing-model-with-utterances"></a>Entrenamiento de la aplicación después de cambiar el modelo con expresiones

Después de agregar, editar o quitar expresiones, [entrene](luis-how-to-train.md) y [publique](luis-how-to-publish-app.md) la aplicación para que los cambios se apliquen a las consultas de punto de conexión. 

## <a name="next-steps"></a>Pasos siguientes

Después de etiquetar expresiones en sus intenciones, puede crear una [entidad compuesta](luis-how-to-add-entities.md).
