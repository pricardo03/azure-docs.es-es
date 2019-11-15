---
title: 'Tutorial: Extracción de datos estructurados con una entidad de aprendizaje automático: LUIS'
titleSuffix: Azure Cognitive Services
description: Extraiga los datos estructurados de una expresión mediante la entidad de aprendizaje automático. Para aumentar la precisión de extracción, agregue subcomponentes con descriptores y restricciones.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 11/07/2019
ms.author: diberry
ms.openlocfilehash: 36b75f33b4fc9062d09fbc670a509594142f09bd
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73828260"
---
# <a name="tutorial-extract-structured-data-with-machine-learned-entities-in-language-understanding-luis"></a>Tutorial: Extracción de datos estructurados con entidades con aprendizaje automático en Language Understanding (LUIS)

En este tutorial, se extraen datos estructurados de una expresión mediante la entidad de aprendizaje automático. 

La entidad de aprendizaje automático admite el [concepto de descomposición de modelos](luis-concept-model.md#v3-authoring-model-decomposition), ya que proporciona entidades de subcomponentes con sus descriptores y restricciones. 

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Importar la aplicación de ejemplo
> * Agregar una entidad de aprendizaje automático 
> * Agregar un subcomponente
> * Agregar un descriptor del subcomponente
> * Agregar una restricción del subcomponente
> * Entrenamiento de la aplicación
> * Probar la aplicación
> * Publicación de aplicación
> * Obtener la predicción de la entidad a partir del punto de conexión

[!INCLUDE [LUIS Free account](includes/quickstart-tutorial-use-free-starter-key.md)]


## <a name="why-use-a-machine-learned-entity"></a>¿Por qué usar una entidad de aprendizaje automático?

En este tutorial se agrega una entidad de aprendizaje automático para extraer datos de una expresión. 

El propósito de una entidad es definir los datos que se van a extraer. Esto incluye asignar a los datos un nombre, un tipo (si es posible), cualquier resolución de los datos si existe ambigüedad y el texto exacto que compone los datos. 

Para definir la entidad, debe crear la entidad y, a continuación, etiquetar el texto que representa la entidad en la expresión de ejemplo. Estos ejemplos etiquetados enseñan a LUIS qué es la entidad y dónde se puede encontrar en una expresión. 

## <a name="entity-decomposability-is-important"></a>La descomponibilidad de la entidad es importante

La descomponibilidad de la entidad es importante tanto para la predicción de la intención como para la extracción de los datos. 

Comience con una entidad de aprendizaje automático, que es la entidad de inicio y de nivel superior para la extracción de datos. A continuación, descomponga la entidad en los elementos que necesita la aplicación cliente. 

Aunque es posible que no sepa qué detalle quiere que tenga la entidad al comenzar la aplicación, un procedimiento recomendado es empezar con una entidad de aprendizaje automático y, luego, descomponer los subcomponentes a medida que la aplicación crece.

En términos prácticos, creará una entidad de aprendizaje automático para representar una aplicación de pedido de pizzas. El pedido debe tener todos los elementos necesarios para cumplir el pedido. Para empezar, la entidad incluirá todo el texto relacionado con el pedido y, en concreto, extraerá el tamaño y la cantidad. 

Una expresión para `deliver one large cheese pizza` debe extraer toda la expresión como el pedido y, a continuación, extraer `1` y `large`. 

Se puede realizar una descomposición adicional, como los ingredientes o la corteza. Después de este tutorial, debería poder agregar estos subcomponentes a la entidad `Order` existente.

## <a name="import-example-json-to-begin-app"></a>Importación de un archivo .json de ejemplo para comenzar la aplicación

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/machine-learned-entity/pizza-intents-only.json).

1. En el [portal de LUIS en versión preliminar](https://preview.luis.ai), en la página **Mis aplicaciones**, seleccione **Importar** y, a continuación, **Import as JSON** (Importar como JSON). Busque el archivo JSON guardado en el paso anterior. No es necesario cambiar el nombre de la aplicación. Seleccione **Listo**

1. Desde la sección **Administrar**, en la pestaña **Versiones**, seleccione la versión, luego seleccione **Clonar** para clonar la versión y asígnele el nombre `mach-learn`. A continuación, seleccione **Listo** para finalizar el proceso de clonación. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL.

    > [!TIP] 
    > La clonación es un procedimiento recomendado antes de modificar la aplicación. Cuando haya terminado con una versión, exporte una versión como archivo .json o .lu, y compruébelo en el control de código fuente.

1. Seleccione **Compilar** después **Intenciones** para ver los principales bloques de creación de una aplicación de LUIS: las intenciones.

    ![Cambie de la página Versiones a la página Intenciones.](media/tutorial-machine-learned-entity/new-version-imported-app.png)

## <a name="label-text-as-entities-in-example-utterances"></a>Etiquetado de texto como entidades en expresiones de ejemplo

Para extraer detalles sobre un pedido de pizza, cree un nivel superior, una entidad `Order` de aprendizaje automático.

1. En la página **Intenciones**, seleccione la intención **OrderPizza**. 

1. En la lista de expresiones de ejemplo, seleccione la expresión siguiente. 

    |Expresión de pedido de ejemplo|
    |--|
    |`pickup a cheddar cheese pizza large with extra anchovies`|

    Comience a seleccionar justo antes del texto situado más a la izquierda de `pickup` (n.º 1) y, después, vaya hasta el texto de más a la derecha, `anchovies` (n.º 2, esto finaliza el proceso de etiquetado). Aparece un menú emergente. En el cuadro emergente, escriba el nombre de la entidad como `Order` (n.º 3). A continuación, seleccione `Order - Create new entity` de la lista (n.º 4).

    ![Etiquetar el comienzo y el final del texto para el pedido completo](media/tutorial-machine-learned-entity/mark-complete-order.png)

    > [!NOTE]
    > Una entidad no siempre será toda la expresión. En este caso concreto, `pickup` indica cómo se va a recibir el pedido, por lo que debe formar parte de la entidad etiquetada para el pedido. 

1. En el cuadro **Choose an entity type** (Elegir un tipo de entidad), seleccione **Add Structure** (Agregar estructura) y, a continuación, seleccione **Siguiente**. La estructura es necesaria para permitir subcomponentes, como el tamaño y la cantidad.

    ![Agregar estructura a la entidad](media/tutorial-machine-learned-entity/add-structure-to-entity.png)

1. En el cuadro **Create a machine learned entity** (Crear una entidad de aprendizaje automático), en el cuadro **Estructura**, agregue `Size` presione Entrar. 
1. Para agregar un **descriptor**, seleccione `+` en el área **Descriptors for Size** (Descriptores de tamaño) y, a continuación, seleccione **Crear lista de frases**.

1. En el cuadro **Create new phrase list descriptor** (Crear descriptor de la lista de frases), escriba el nombre `SizeDescriptor` escriba los valores `small`, `medium`y `large`. Cuando se rellene el cuadro **Sugerencias**, seleccione `extra large` y `xl`. Seleccione **Listo** para crear la lista de frases. 

    Este descriptor de la lista de frases ayuda al subcomponente `Size` a buscar palabras relacionadas con el tamaño al proporcionarle una palabra de ejemplo. Esta lista no necesita incluir cada palabra de tamaño, pero debe incluir palabras que se espera que indiquen tamaño. 

    ![Crear un descriptor para el subcomponente size](media/tutorial-machine-learned-entity/size-entity-size-descriptor-phrase-list.png)

1. En la ventana **Create a machine learned entity** (Crear una entidad de aprendizaje automático), seleccione **Crear** para terminar de crear el subcomponente `Size`.  

    Se crea la entidad `Order` con un componente `Size`, pero solo se ha aplicado la entidad `Order` a la expresión. Debe etiquetar el texto de la entidad `Size` en la expresión de ejemplo. 

1. En la misma expresión de ejemplo, etiquete el subcomponente **Size** de `large` seleccionando la palabra y seleccionando a continuación la entidad **Size** de la lista desplegable. 

    ![Etiquete la entidad size para el texto en la expresión.](media/tutorial-machine-learned-entity/mark-and-create-size-entity.png)

    La línea es sólida debajo del texto porque la etiqueta y la predicción coinciden porque el texto se etiquetó explícitamente.

1. Etiquete la entidad `Order` en las expresiones restantes junto con la entidad size. Los corchetes en el texto indican la entidad `Order` etiquetada y la entidad `Size` en su interior.

    |Expresiones de pedido de ejemplo|
    |--|
    |`can i get [a pepperoni pizza and a can of coke] please`|
    |`can i get [a [small] pizza with onions peppers and olives]`|
    |`[delivery for a [small] pepperoni pizza]`|
    |`i need [2 [large] cheese pizzas 6 [large] pepperoni pizzas and 1 [large] supreme pizza]`|

    ![Crear la entidad y los subcomponentes en todas las expresiones de ejemplo restantes.](media/tutorial-machine-learned-entity/entity-subentity-labeled-not-trained.png)

    > [!CAUTION]
    > ¿Cómo se tratan los datos implícitos, como la letra `a`, que implican una única pizza? ¿O la falta de `pickup` y `delivery` para indicar dónde se espera la pizza? ¿O la falta de un tamaño para indicar el tamaño predeterminado o pequeño o grande? Considere la posibilidad de tratar los datos implícitos como parte de las reglas de negocio en la aplicación cliente. 

1. Para entrenar la aplicación, seleccione **Entrenar**. El entrenamiento aplica los cambios, como las nuevas entidades y las expresiones etiquetadas, al modelo activo.

1. Después del entrenamiento, agregue una nueva expresión de ejemplo para comprender el grado en que LUIS entiende la entidad de aprendizaje automático. 

    |Expresión de pedido de ejemplo|
    |--|
    |`pickup XL meat lovers pizza`|

    La entidad superior general, `Order`, está etiquetada, y el subcomponente `Size` también está etiquetado con líneas punteadas. Se trata de una predicción correcta. 

    ![Nueva expresión de ejemplo predicha con la entidad](media/tutorial-machine-learned-entity/new-example-utterance-predicted-with-entity.png)

    El vínculo punteado indica la predicción. 

1. Para cambiar la predicción por una entidad etiquetada, seleccione la fila y, a continuación, seleccione **Confirm entity predictions** (Confirmar predicciones de entidad).

    ![Para aceptar la predicción, seleccione Confirm entity prediction.](media/tutorial-machine-learned-entity/confirm-entity-prediction-for-new-example-utterance.png)

    En este momento, la entidad de aprendizaje automático funciona porque puede encontrar la entidad dentro de una nueva expresión de ejemplo. Cuando agregue expresiones de ejemplo, si la entidad no se predice correctamente, etiquete la entidad y los subcomponentes. Si la entidad se predice correctamente, asegúrese de confirmar las predicciones. 

## <a name="add-prebuilt-number-to-app-to-help-extract-data"></a>Adición del número predefinido a la aplicación para extraer datos

La información del pedido también debe incluir el número de elementos del pedido, como el número de pizzas. Para extraer estos datos, es necesario agregar un nuevo subcomponente de aprendizaje automático a `Order`, y ese componente necesita una restricción de un número predefinido. Al restringir la entidad a un número predefinido, la entidad buscará y extraerá números si el texto es una cifra o, `2`, o texto, `two`.

Comience agregando a la aplicación el número predefinido. 

1. Seleccione **Entidades** en el menú de la izquierda y, a continuación, seleccione **+ Agregar entidad predefinida**. 

1. En el cuadro **Agregar entidades predefinidas**, busque y seleccione **número** y, a continuación, seleccione **Listo**. 

    ![Incorporación de entidad precompilada](media/tutorial-machine-learned-entity/add-prebuilt-entity-as-constraint-to-quantity-subcomponent.png)

    La entidad predefinida se ha agregado a la aplicación, pero aún no es una restricción. 

## <a name="create-subcomponent-entity-with-constraint-to-help-extract-data"></a>Creación de una entidad con subcomponente con restricción para extraer datos

La entidad `Order` debe tener un subcomponente `Quantity` para determinar cuánto de un elemento hay en el pedido. La cantidad se debe restringir a un número para que la aplicación cliente pueda usar inmediatamente los datos extraídos. 

Una restricción se aplica como una coincidencia de texto, ya sea con coincidencia exacta (como una entidad de lista) o a través de expresiones regulares (como una entidad de expresión regular o una entidad predefinida). 

1. Seleccione **Entidades** y seleccione la entidad `Order`. 
1. Seleccione **+ Agregar componente**, a continuación, escriba el nombre `Quantity` y presione Entrar para agregar la nueva entidad a la aplicación.
1. Después de la notificación correcta, seleccione el subcomponente `Quantity` y seleccione el lápiz de Restricción.
1. En la lista desplegable, seleccione el número predefinido. 

    ![Cree una entidad quantity con un número predefinido como restricción.](media/tutorial-machine-learned-entity/create-constraint-from-prebuilt-number.png)

    La entidad con la restricción se ha creado, pero aún no se aplica a las expresiones de ejemplo.

    > [!NOTE]
    > Un subcomponente se puede anidar dentro de un subcomponente hasta 5 niveles. Aunque esto no se muestra en este artículo, está disponible en el portal y la API.  

## <a name="label-example-utterance-with-subcomponent-for-quantity-to-teach-luis-about-the-entity"></a>Etiquetado de expresiones de ejemplo con el subcomponente de quantity para enseñar a LUIS sobre la entidad

1. Seleccione **Intenciones** desde el panel de navegación de la izquierda y, a continuación, seleccione **OrderPizza**. Los tres números de las siguientes expresiones se han etiquetado, pero se muestran visualmente debajo de la línea de entidad `Order`. Este nivel inferior significa que se han encontrado las entidades, pero que no se consideran aparte de la entidad `Order`.

    ![El número predefinido se ha encontrado, pero no se considera aparte de la entidad Order todavía.](media/tutorial-machine-learned-entity/prebuilt-number-not-part-of-order-entity.png)

1. Para etiquetar los números con la entidad `Quantity`, seleccione `2` en la expresión de ejemplo y seleccione `Quantity` de la lista. Etiquete el `6` y el `1` en la misma expresión de ejemplo.

    ![Etiquete el texto con la entidad quantity.](media/tutorial-machine-learned-entity/mark-example-utterance-with-quantity-entity.png)  

## <a name="train-the-app-to-apply-the-entity-changes-to-the-app"></a>Entrenamiento de la aplicación para aplicar los cambios de la entidad a la aplicación

Seleccione **Entrenar** para entrenar la aplicación con estas nuevas expresiones.

![Entrene la aplicación y revise las expresiones de ejemplo.](media/tutorial-machine-learned-entity/trained-example-utterances.png)

En este momento, el pedido tiene algunos detalles que se pueden extraer (tamaño, cantidad y texto total del pedido). Hay más refinamientos de la entidad `Order`, como los ingredientes de la pizza, el tipo de corteza y los pedidos secundarios. Cada uno debe crearse como subcomponentes de la entidad `Order`. 

## <a name="test-the-app-to-validate-the-changes"></a>Prueba de la aplicación para validar los cambios

Pruebe la aplicación mediante el panel **Prueba** interactivo. Este proceso permite especificar una nueva expresión y, a continuación, ver los resultados de la predicción para ver el funcionamiento de la aplicación activa y entrenada. La predicción de intención debe ser bastante segura (por encima del 70 %), y la extracción de entidades debe recoger al menos la entidad `Order`. Puede que falten los detalles de la entidad Order porque 5 expresiones no bastan para controlar cada caso.

1. Seleccione **Test** (Prueba) en la barra de navegación superior.
1. Escriba la expresión `deliver a medium veggie pizza` y presione Entrar. El modelo activo predijo la intención correcta con más del 70 % de confianza. 

    ![Escriba una nueva expresión para probar la intención.](media/tutorial-machine-learned-entity/interactive-test-panel-with-first-utterance.png)

1. Seleccione **Inspeccionar** para ver las predicciones de la entidad.

    ![Vea las predicciones de la entidad en el panel de prueba interactivo.](media/tutorial-machine-learned-entity/interactive-test-panel-with-first-utterance-and-entity-predictions.png)

    El tamaño se identificó correctamente. Recuerde que las expresiones de ejemplo en la intención `OrderPizza` no tiene un ejemplo de `medium` como tamaño, sino que usa un descriptor de una lista de frases de `SizeDescriptor` que incluye medium.

    La cantidad no se predice correctamente. Para solucionarlo, puede agregar más expresiones de ejemplo usando esa palabra para indicar la cantidad y etiquetar esa palabra como entidad `Quantity`. 

## <a name="publish-the-app-to-access-it-from-the-http-endpoint"></a>Publicación de la aplicación para tener acceso a ella desde el punto de conexión HTTP

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-and-entity-prediction--from-http-endpoint"></a>Obtención de la intención y la predicción de la entidad desde el punto de conexión HTTP

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Vaya al final de la URL en la dirección y escriba la misma consulta que especificó en el panel de prueba interactivo. 

    `deliver a medium veggie pizza`

    El último parámetro de la cadena de consulta es `query`, la expresión **query**. 

    ```json
    {
        "query": "deliver a medium veggie pizza",
        "prediction": {
            "topIntent": "OrderPizza",
            "intents": {
                "OrderPizza": {
                    "score": 0.7812769
                },
                "None": {
                    "score": 0.0314020254
                },
                "Confirm": {
                    "score": 0.009299271
                },
                "Greeting": {
                    "score": 0.007551549
                }
            },
            "entities": {
                "Order": [
                    {
                        "Size": [
                            "medium"
                        ],
                        "$instance": {
                            "Size": [
                                {
                                    "type": "Size",
                                    "text": "medium",
                                    "startIndex": 10,
                                    "length": 6,
                                    "score": 0.9955588,
                                    "modelTypeId": 1,
                                    "modelType": "Entity Extractor",
                                    "recognitionSources": [
                                        "model"
                                    ]
                                }
                            ]
                        }
                    }
                ],
                "$instance": {
                    "Order": [
                        {
                            "type": "Order",
                            "text": "a medium veggie pizza",
                            "startIndex": 8,
                            "length": 21,
                            "score": 0.7983857,
                            "modelTypeId": 1,
                            "modelType": "Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }    
    ```
    

[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>Información relacionada

* [Tutorial: Intenciones](luis-quickstart-intents-only.md)
* [Concepto: Entidades](luis-concept-entity-types.md) información conceptual
* [Concepto: Características](luis-concept-feature.md) información conceptual
* [Cómo se realiza el entrenamiento](luis-how-to-train.md)
* [Publicación](luis-how-to-publish-app.md)
* [Prueba de la aplicación el portal de LUIS](luis-interactive-test.md)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, la aplicación usa una entidad de aprendizaje automático para encontrar la intención de la expresión de un usuario y extraer los detalles de esa expresión. El uso de la entidad de aprendizaje automático permite descomponer los detalles de la entidad.  

> [!div class="nextstepaction"]
> [Incorporación de una entidad keyphrase precompilada](luis-quickstart-intent-and-key-phrase.md)
