---
title: 'Tutorial de uso de patrones para mejorar las predicciones de LUIS: Azure | Microsoft Docs'
titleSuffix: Cognitive Services
description: En este tutorial, use patrones para intenciones a fin de mejorar las predicciones de intenciones y entidades de LUIS.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 07/20/2018
ms.author: diberry
ms.openlocfilehash: 9ad1d9e1543c3d9a74025fb23bd1767478b53b4b
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39238461"
---
# <a name="tutorial-improve-app-with-patterns"></a>Tutorial: Mejorar la aplicación con patrones

En este tutorial, use patrones para mejorar la predicción de intenciones y entidades.  

> [!div class="checklist"]
* Cómo identificar que un patrón ayudaría a la aplicación
* Cómo crear un patrón 
* Cómo comprobar las mejoras de la predicción de patrones

Para este artículo, necesita una cuenta gratuita de [LUIS](luis-reference-regions.md) para crear la aplicación.

## <a name="before-you-begin"></a>Antes de empezar
Si no tiene la aplicación de recursos humanos del tutorial sobre [pruebas por lotes](luis-tutorial-batch-testing.md), [importe](luis-how-to-start-new-app.md#import-new-app) el archivo JSON a una nueva aplicación en el sitio web de [LUIS](luis-reference-regions.md#luis-website). La aplicación que se va a importar se encuentra en el repositorio de GitHub [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-batchtest-HumanResources.json).

Si quiere conservar la aplicación original de Recursos humanos, clone la versión en la página [Configuración](luis-how-to-manage-versions.md#clone-a-version) y llámela `patterns`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. 

## <a name="patterns-teach-luis-common-utterances-with-fewer-examples"></a>Los patrones enseñan a LUIS expresiones comunes con menos ejemplos
Dada la naturaleza del dominio de recursos humanos, hay algunas formas habituales de preguntar por relaciones de empleados en las organizaciones. Por ejemplo: 

```
Who does Jill Jones report to?
Who reports to Jill Jones? 
```

Estas expresiones son demasiado cercanas como para determinar la singularidad contextual de cada una de ellas sin proporcionar muchos ejemplos de expresiones. Al agregar un patrón para una intención, LUIS aprende patrones de expresión comunes para una intención sin proporcionar muchos ejemplos de expresiones. 

Entre las expresiones de plantilla de ejemplo de esta intención se incluyen las siguientes:

```
Who does {Employee} report to?
Who reports to {Employee}? 
```

El patrón se proporciona por medio de un ejemplo de expresión de plantilla, que incluye la sintaxis para identificar las entidades y el texto que se puede pasar por alto. Un patrón es una combinación de una coincidencia de expresión regular y el aprendizaje automático.  El ejemplo de expresión de plantilla junto con las expresiones de intención permiten que LUIS comprenda mejor qué expresiones se ajustan a la intención.

Para que un patrón coincida con una expresión, las entidades dentro de la expresión primero tienen que coincidir con las entidades de la expresión de plantilla. Sin embargo, la plantilla no ayuda a predecir las entidades, solo las intenciones. 

**Si bien los patrones permiten proporcionar menos expresiones de ejemplo, si no se detectan las entidades, el patrón no coincide.**

Recuerde que los empleados se crearon en el [tutorial de la entidad de lista](luis-quickstart-intent-and-list-entity.md).

## <a name="create-new-intents-and-their-utterances"></a>Creación de intenciones nuevas y sus expresiones
Agregue dos intenciones nuevas: `OrgChart-Manager` y `OrgChart-Reports`. Una vez que LUIS devuelve una predicción a la aplicación cliente, el nombre de la intención se puede usar como un nombre de función en la aplicación cliente y la entidad Employee (Empleado) podría usarse como un parámetro para esa función.

```
OrgChart-Manager(employee){
    ///
}
```

1. Asegúrese de que la aplicación de recursos humanos se encuentra en la sección **Build** (Crear) de LUIS. Para cambiar a esta sección, seleccione **Build** (Crear) en la barra de menús superior derecha. 

2. En la página **Intents** (Intenciones), seleccione **Create new intent** (Crear intención). 

3. Escriba `OrgChart-Manager` en el cuadro de diálogo emergente y seleccione **Done** (Listo).

    ![Creación de una ventana emergente de mensaje nuevo](media/luis-tutorial-pattern/hr-create-new-intent-popup.png)

4. Agregue expresiones de ejemplo a la intención.

    |Expresiones de ejemplo|
    |--|
    |¿De quién es subordinado John W. Smith?|
    |¿De quién depende John W. Smith?|
    |¿Quién es el gerente de John W. Smith?|
    |¿De quién depende directamente de Jill Jones?|
    |¿Quién es el supervisor de Jill Jones?|

    [![](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png "Captura de pantalla de LUIS con la opción de agregar expresiones nuevas a la intención")](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png#lightbox)

    No se preocupe si la entidad keyPhrase está etiquetada las expresiones de la intención en lugar de la entidad employee. Ambas se predicen correctamente en el panel de prueba y en el punto de conexión. 

5. Seleccione **Intents** (Intenciones) en el panel de navegación izquierdo.

6. Haga clic en **Create new intent** (Crear intención). 

7. Escriba `OrgChart-Reports` en el cuadro de diálogo emergente y seleccione **Done** (Listo).

8. Agregue expresiones de ejemplo a la intención.

    |Expresiones de ejemplo|
    |--|
    |¿Quiénes son los subordinados de John W. Smith?|
    |¿Quién depende de John W. Smith?|
    |¿De quién es supervisor John W. Smith?|
    |¿Quiénes dependen directamente de Jill Jones?|
    |¿De quién es supervisora Jill Jones?|

## <a name="caution-about-example-utterance-quantity"></a>Precaución sobre la cantidad de expresiones de ejemplo
La cantidad de expresiones de ejemplo en estas intenciones no es suficiente para entrenar correctamente a LUIS. En una aplicación real, cada intención debe tener un mínimo de 15 expresiones con una variedad de elecciones de palabras y de duraciones de expresiones. Estas pocas expresiones se seleccionan específicamente para resaltar los patrones. 

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS
La intención y las expresiones nuevas requieren entrenamiento. 

1. En la parte superior derecha del sitio web de LUIS, haga clic en el botón **Entrenar**.

    ![Imagen del botón de entrenamiento](./media/luis-tutorial-pattern/hr-train-button.png)

2. El entrenamiento se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

    ![Imagen de la barra de notificación con la confirmación de realización correcta](./media/luis-tutorial-pattern/hr-trained-inline.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión
Para obtener una predicción de LUIS en un bot de chat u otra aplicación, tiene que publicar la aplicación. 

1. En la parte superior derecha del sitio web de LUIS, haga clic en el botón **Publish** (Publicar). 

2. Seleccione el espacio de producción y haga clic en el botón **Publicar**.

    [ ![Captura de pantalla de la página Publish (Publicar) con el botón de publicación en el espacio de producción resaltado](./media/luis-tutorial-pattern/hr-publish-to-production.png)](./media/luis-tutorial-pattern/hr-publish-to-production.png#lightbox)

3. La publicación se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consulta del punto de conexión con una expresión diferente
1. En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. 

    [ ![Captura de pantalla de la página Publish (Publicar) con la dirección URL del punto de conexión resaltada](./media/luis-tutorial-pattern/hr-publish-select-endpoint.png)](./media/luis-tutorial-pattern/hr-publish-select-endpoint.png#lightbox)


2. Vaya al final de la dirección URL en la dirección y escriba `Who is the boss of Jill Jones?`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. 

    ```JSON
    {
        "query": "who is the boss of jill jones?",
        "topScoringIntent": {
            "intent": "OrgChart-Manager",
            "score": 0.353984952
        },
        "intents": [
            {
                "intent": "OrgChart-Manager",
                "score": 0.353984952
            },
            {
                "intent": "OrgChart-Reports",
                "score": 0.214128986
            },
            {
                "intent": "EmployeeFeedback",
                "score": 0.08434003
            },
            {
                "intent": "MoveEmployee",
                "score": 0.019131
            },
            {
                "intent": "GetJobInformation",
                "score": 0.004819009
            },
            {
                "intent": "Utilities.Confirm",
                "score": 0.0043958663
            },
            {
                "intent": "Utilities.StartOver",
                "score": 0.00312064588
            },
            {
                "intent": "Utilities.Cancel",
                "score": 0.002265454
            },
            {
                "intent": "Utilities.Help",
                "score": 0.00133465114
            },
            {
                "intent": "None",
                "score": 0.0011388344
            },
            {
                "intent": "Utilities.Stop",
                "score": 0.00111166481
            },
            {
                "intent": "FindForm",
                "score": 0.0008900076
            },
            {
                "intent": "ApplyForJob",
                "score": 0.0007836131
            }
        ],
        "entities": [
            {
                "entity": "jill jones",
                "type": "Employee",
                "startIndex": 19,
                "endIndex": 28,
                "resolution": {
                    "values": [
                        "Employee-45612"
                    ]
                }
            },
            {
                "entity": "boss of jill jones",
                "type": "builtin.keyPhrase",
                "startIndex": 11,
                "endIndex": 28
            }
        ]
    }
    ```

¿Se realizó correctamente esta consulta? Para este ciclo de entrenamiento, sí. Las puntuaciones de las dos intenciones principales son similares. Como el entrenamiento de LUIS no es exactamente igual cada vez, hay un poco de variación, que estas dos puntuaciones se podrían invertir en el próximo ciclo de entrenamiento. El resultado es que se podría devolver la intención incorrecta. 

Use los patrones para que la puntuación de la intención correcta sea considerablemente más alta en porcentaje y esté más lejos de la puntuación más alta siguiente. 

## <a name="add-the-template-utterances"></a>Agregar las expresiones de plantilla

1. Seleccione **Build** (Compilación) en el menú superior.

2. En el panel de navegación izquierdo, debajo de **Improve app performance** (Mejorar el rendimiento de la aplicación), seleccione **Patrones**.

3. Seleccione la intención **OrgChart-Managert** y, después, escriba las siguientes expresiones de plantilla, una de cada vez, y presione ENTRAR después de cada expresión de plantilla:

    |Expresiones de plantilla|
    |:--|
    |[¿]De quién es subordinado {Employee}[?]|
    |[¿]Quién depende de {Employee}[?]|
    |[¿]Quién es el administrador de {Employee}[?]|
    |[¿]De quién depende directamente {Employee}[?]|
    |[¿]Quién es el supervisor de {Employee}[?]|
    |[¿]Quién es el jefe de {Employee}[?]|

    La sintaxis `{Employee}` marca la ubicación de la entidad dentro de la expresión de plantilla, así como qué entidad. 
    
    Las entidades con roles usan sintaxis que incluye el nombre de rol y se describen en un [tutorial independiente para los roles](luis-tutorial-pattern-roles.md). 

    La sintaxis opcional, `[]`, marca las palabras o los signos de puntuación que son opcionales. LUIS coincide con la expresión y omite el texto opcional entre corchetes.

    Si escribe la expresión de plantilla, LUIS lo ayuda a rellenar la entidad al escribir la llave de apertura, `{`.

    [ ![Captura de pantalla del ingreso de expresiones de plantilla para la intención](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png)](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png#lightbox)



4. Seleccione la intención **OrgChart-Reports** y, después, escriba las siguientes expresiones de plantilla, una de cada vez, y presione ENTRAR después de cada expresión de plantilla:

    |Expresiones de plantilla|
    |:--|
    |[¿]Quiénes son los subordinados de {Employee}[?]|
    |[¿]Quién depende de {Employee}[?]|
    |[¿]De quién es supervisor {Employee}[?]|
    |[¿]Quiénes dependen directamente de {Employee}[?]|
    |[¿]De quién es supervisora {Employee}[?]|
    |[¿]Quién es el jefe de {Employee}[?]|

## <a name="query-endpoint-when-patterns-are-used"></a>Consulta de punto de conexión cuando se usan patrones

1. Vuelva a entrenar y publicar la aplicación.

2. En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. 

3. Vaya al final de la dirección URL en la dirección y escriba `Who is the boss of Jill Jones?` como la expresión. El último parámetro de la cadena de consulta es `q`, la expresión **query**. 

    ```JSON
    {
        "query": "who is the boss of jill jones?",
        "topScoringIntent": {
            "intent": "OrgChart-Manager",
            "score": 0.9999989
        },
        "intents": [
            {
                "intent": "OrgChart-Manager",
                "score": 0.9999989
            },
            {
                "intent": "OrgChart-Reports",
                "score": 7.616303E-05
            },
            {
                "intent": "EmployeeFeedback",
                "score": 7.84204349E-06
            },
            {
                "intent": "GetJobInformation",
                "score": 1.20674213E-06
            },
            {
                "intent": "MoveEmployee",
                "score": 7.91245157E-07
            },
            {
                "intent": "None",
                "score": 3.875E-09
            },
            {
                "intent": "Utilities.StartOver",
                "score": 1.49E-09
            },
            {
                "intent": "Utilities.Confirm",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Help",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Stop",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Cancel",
                "score": 1.225E-09
            },
            {
                "intent": "FindForm",
                "score": 1.123077E-09
            },
            {
                "intent": "ApplyForJob",
                "score": 5.625E-10
            }
        ],
        "entities": [
            {
                "entity": "jill jones",
                "type": "Employee",
                "startIndex": 19,
                "endIndex": 28,
                "resolution": {
                    "values": [
                        "Employee-45612"
                    ]
                },
                "role": ""
            },
            {
                "entity": "boss of jill jones",
                "type": "builtin.keyPhrase",
                "startIndex": 11,
                "endIndex": 28
            }
        ]
    }
    ```

La predicción de la intención ahora es considerablemente más alta. 

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no sea necesaria, elimine la aplicación de LUIS. Para hacerlo, seleccione los puntos suspensivos (***...***) a la derecha del nombre de la aplicación en la lista de aplicaciones y, después, seleccione **Delete** (Eliminar). En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), haga clic en **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Información sobre cómo usar roles con un patrón](luis-tutorial-pattern-roles.md)