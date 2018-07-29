---
title: Tutorial para crear una aplicación de LUIS para obtener datos que coincidan con expresiones regulares en Azure | Microsoft Docs
description: En este tutorial, va a aprender a crear una aplicación de LUIS sencilla con intenciones y una entidad de expresión regular para extraer datos.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: diberry
ms.openlocfilehash: 99f796bf26df755ca938c3023057e2e9de1706a1
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39238342"
---
# <a name="tutorial-3-add-regular-expression-entity"></a>Tutorial: 3. Incorporación de entidades de expresiones regulares
En este tutorial, va a crear una aplicación que demuestra cómo extraer datos con formato de forma coherente desde una expresión con la entidad **Expresión regular**.


<!-- green checkmark -->
> [!div class="checklist"]
> * Comprender las entidades de expresión regular 
> * Usar una aplicación de LUIS para un dominio de recursos humanos (RRHH) con la intención FindForm
> * Agregar una entidad de expresión regular para extraer el número de formularios de la expresión
> * Entrenamiento y publicación de la aplicación
> * Consulta del punto de conexión de la aplicación para ver la respuesta JSON de LUIS

Para este artículo, necesita una cuenta gratuita de [LUIS](luis-reference-regions.md#luis-website) para crear la aplicación.

## <a name="before-you-begin"></a>Antes de empezar
Si no tiene la aplicación de Recursos humanos del tutorial de [entidades precompiladas](luis-tutorial-prebuilt-intents-entities.md), [importe](luis-how-to-start-new-app.md#import-new-app) el archivo JSON a una nueva aplicación en el sitio web [LUIS](luis-reference-regions.md#luis-website), desde el repositorio Github [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-prebuilts-HumanResources.json).

Si quiere conservar la aplicación original de Recursos humanos, clone la versión en la página [Configuración](luis-how-to-manage-versions.md#clone-a-version) y llámela `regex`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. 


## <a name="purpose-of-the-regular-expression-entity"></a>Propósito de la entidad de expresión regular
El propósito de una entidad es obtener datos importantes contenidos en la expresión. El uso de la aplicación de la entidad de expresión regular es extraer los números de formularios del Recursos humanos formateados. No es aprendizaje automático. 

Las expresiones de ejemplo sencillas incluyen:

```
Where is HRF-123456?
Who authored HRF-123234?
HRF-456098 is published in French?
```

Las versiones abreviadas o de argot de las expresiones incluyen:

```
HRF-456098
HRF-456098 date?
HRF-456098 title?
```
 
La entidad de expresión regular para que coincida con el número de formulario es `hrf-[0-9]{6}`. Esta expresión regular coincide con los caracteres literales `hrf -` pero omite variantes de mayúsculas y minúsculas y la referencia cultural. Coincide exactamente con dígitos de 0 a 9, para 6 dígitos exactamente.

HRF significa formulario de recursos humanos.

### <a name="tokenization-with-hyphens"></a>Tokenización con guiones
LUIS tokeniza la expresión cuando esta se agrega a una intención. La tokenización para estas expresiones agrega espacios antes y después del guion, `Where is HRF - 123456?` La expresión regular se aplica a la expresión en su forma sin formato, antes de que se tokenice. Debido a que se aplica al formulario _sin formato_, la expresión regular no tiene que tratar con límites de palabras. 


## <a name="add-findform-intent"></a>Adición de la intención FindForm

1. Asegúrese de que la aplicación de recursos humanos se encuentra en la sección **Build** (Crear) de LUIS. Para cambiar a esta sección, seleccione **Build** (Crear) en la barra de menús superior derecha. 

    [ ![Captura de pantalla de la aplicación de LUIS con Build (Crear) resaltado en la barra de navegación superior derecha](./media/luis-quickstart-intents-regex-entity/first-image.png)](./media/luis-quickstart-intents-regex-entity/first-image.png#lightbox)

2. Seleccione **Create new intent** (Crear nueva intención). 

    [ ![Captura de pantalla de la página Intents (Intenciones) con el botón Create new intent (Crear nueva intención) resaltado](./media/luis-quickstart-intents-regex-entity/create-new-intent-button.png) ](./media/luis-quickstart-intents-regex-entity/create-new-intent-button.png#lightbox)

3. Escriba `FindForm` en el cuadro de diálogo emergente y seleccione **Done** (Listo). 

    ![Captura de pantalla del cuadro de diálogo de creación de nueva intención con las utilidades en el cuadro de búsqueda](./media/luis-quickstart-intents-regex-entity/create-new-intent-ddl.png)

4. Agregue expresiones de ejemplo a la intención.

    |Expresiones de ejemplo|
    |--|
    |¿Cuál es la dirección URL de hrf-123456?|
    |¿Dónde está hrf-345678?|
    |¿Cuándo se ha actualizado hrf-456098?|
    |¿John Smith ha actualizado hrf 234639 la semana pasada?|
    |¿Cuántas versiones de hrf 345123 existen?|
    |¿Quién tiene que autorizar el formulario hrf-123456?|
    |¿Cuántas personas tienen que cerrar la sesión en hrf 345678?|
    |¿fecha de hrf 234123?|
    |¿autor de hrf 546234?|
    |¿título de hrf 456234?|

    [ ![Captura de pantalla de la página Intent (Intención) con nuevas expresiones resaltadas](./media/luis-quickstart-intents-regex-entity/findform-intent.png) ](./media/luis-quickstart-intents-regex-entity/findform-intent.png#lightbox)

    La aplicación tiene una entidad de número creada previamente agregada del tutorial anterior, por lo que cada número de formulario está etiquetado. Esto puede ser suficiente para la aplicación cliente, pero el número no se etiquetará con el tipo de número. La creación de una nueva entidad con un nombre apropiado permite a la aplicación cliente procesar la entidad apropiadamente cuando se devuelve desde LUIS.

## <a name="create-a-hrf-number-regular-expression-entity"></a>Creación de una entidad de expresión regular de número HRF 
Cree una entidad de expresión regular para decirle a LUIS qué es un formato de número HRF en los siguientes pasos:

1. Seleccione **Entities** (Entidades) en el panel izquierdo.

2. Seleccione **Create new entity** (Crear nueva entidad) en la página Entities (Entidades). 

    [![Captura de pantalla de la página Entities (Entidades) con el botón Create new entity (Crear nueva entidad) resaltado](./media/luis-quickstart-intents-regex-entity/create-new-entity-1.png)](./media/luis-quickstart-intents-regex-entity/create-new-entity-1.png#lightbox)

3. En el cuadro de diálogo emergente, introduzca el nombre de la nueva entidad `HRF-number`, seleccione **RegEx** como tipo de entidad, especifique `hrf-[0-9]{6}` como Regex y, después, seleccione **Done** (Listo).

    ![Captura de pantalla del cuadro de diálogo emergente que establece las propiedades de la nueva entidad](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

4. Seleccione **Intents** (Intenciones), después la intención **FindForm** para ver la expresión regular etiquetada en la expresiones. 

    [![Captura de pantalla de la expresión de etiqueta con la entidad existente y el patrón de la expresión regular](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    Debido a que la entidad no es una entidad que se aprende automáticamente, la etiqueta se aplica a las expresiones y se muestra en el sitio web de LUIS tan pronto como se crea.

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS
Una entidad de expresión regular no requiere entrenamiento pero las nuevas intenciones y expresiones sí que lo requieren. 

1. En la parte superior derecha del sitio web de LUIS, seleccione el botón **Train** (Entrenar).

    ![Imagen del botón de entrenamiento](./media/luis-quickstart-intents-regex-entity/train-button.png)

2. El entrenamiento se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

    ![Imagen de la barra de notificación con la confirmación de realización correcta](./media/luis-quickstart-intents-regex-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión
Para obtener una predicción de LUIS en un bot de chat u otra aplicación, tiene que publicar la aplicación. 

1. En la parte superior derecha del sitio web de LUIS, seleccione el botón **Publish** (Publicar). 

    ![Captura de pantalla de FindKnowledgeBase con el botón Publish (Publicar) de la barra de navegación superior resaltado](./media/luis-quickstart-intents-regex-entity/publish-button.png)

2. Seleccione el espacio de producción y el botón **Publish** (Publicar).

    ![Captura de pantalla de la página Publish (Publicar) con el botón de publicación en el espacio de producción resaltado](./media/luis-quickstart-intents-regex-entity/publish-to-production.png)

3. La publicación se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consulta del punto de conexión con una expresión diferente
1. En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. 

    ![Captura de pantalla de la página Publish (Publicar) con la dirección URL del punto de conexión resaltada](./media/luis-quickstart-intents-regex-entity/publish-select-endpoint.png)

2. Vaya al final de la dirección URL en la dirección y escriba `When were HRF-123456 and hrf-234567 published in the last year?`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que cualquiera de las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `FindForm` con los dos números de formulario `HRF-123456` y `hrf-234567`.

    ```
    {
      "query": "When were HRF-123456 and hrf-234567 published in the last year?",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9993477
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.9993477
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0206110049
        },
        {
          "intent": "GetJobInformation",
          "score": 0.00533067342
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.004215215
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00209096959
        },
        {
          "intent": "None",
          "score": 0.0017655947
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00109490135
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.0005704638
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.000525338168
        }
      ],
      "entities": [
        {
          "entity": "last year",
          "type": "builtin.datetimeV2.daterange",
          "startIndex": 53,
          "endIndex": 61,
          "resolution": {
            "values": [
              {
                "timex": "2017",
                "type": "daterange",
                "start": "2017-01-01",
                "end": "2018-01-01"
              }
            ]
          }
        },
        {
          "entity": "hrf-123456",
          "type": "HRF-number",
          "startIndex": 10,
          "endIndex": 19
        },
        {
          "entity": "hrf-234567",
          "type": "HRF-number",
          "startIndex": 25,
          "endIndex": 34
        },
        {
          "entity": "-123456",
          "type": "builtin.number",
          "startIndex": 13,
          "endIndex": 19,
          "resolution": {
            "value": "-123456"
          }
        },
        {
          "entity": "-234567",
          "type": "builtin.number",
          "startIndex": 28,
          "endIndex": 34,
          "resolution": {
            "value": "-234567"
          }
        }
      ]
    }
    ```

    Los números en la expresión se devuelven dos veces, una como la nueva entidad `hrf-number` y otra como una entidad creada previamente, `number`. Una expresión puede tener más de una entidad, y más de una del mismo tipo de entidad, como muestra este ejemplo. Mediante el uso de una entidad de expresión regular, LUIS extrae datos con nombre, lo que es más útil desde el punto de vista de programación para la aplicación cliente que recibe la respuesta de JSON.

## <a name="what-has-this-luis-app-accomplished"></a>¿Qué ha logrado esta aplicación de LUIS?
Esta aplicación ha identificado la intención y ha devuelto los datos extraídos. 

El bot de chat ahora tiene suficiente información para determinar la acción principal, `FindForm`, y qué números de formulario estaban en la búsqueda. 

## <a name="where-is-this-luis-data-used"></a>¿Dónde se utilizan estos datos de LUIS? 
LUIS ha terminado con esta solicitud. La aplicación que realiza la llamada, como un bot de chat, puede tomar el resultado topScoringIntent y los números de formulario y buscar una API de terceros. LUIS no hace ese trabajo. LUIS solo determina cuál es la intención del usuario y extrae datos sobre esa intención. 

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no sea necesaria, elimine la aplicación de LUIS. Seleccione **My apps** (Mis aplicaciones) en el menú superior izquierdo. Seleccione los puntos suspensivos (***...***) a la derecha del nombre de la aplicación en la lista de aplicaciones y, después, seleccione **Delete** (Eliminar). En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), haga clic en **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Obtenga información acerca de la entidad de la lista](luis-quickstart-intent-and-list-entity.md)

