---
title: 'Tutorial para crear una aplicación de LUIS que extraiga datos: Azure | Microsoft Docs'
description: En este tutorial, va a aprender a crear una aplicación de LUIS sencilla con intenciones y una entidad simple para extraer datos que se han aprendido automáticamente.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: v-geberr
ms.openlocfilehash: a4bf63b7a2fbbb26b8c121f5360aea0a5ca8a687
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952392"
---
# <a name="tutorial-7-add-simple-entity-and-phrase-list"></a>Tutorial: 7. Incorporación de entidad simple y lista de frases
En este tutorial se crea una aplicación que muestra cómo se extraen datos aprendidos automáticamente de una expresión con la entidad **Simple**.

<!-- green checkmark -->
> [!div class="checklist"]
> * Información acerca de las entidades Simple 
> * Creación de una nueva aplicación de LUIS para el dominio de recursos humanos (RR. HH.) 
> * Creación de una entidad simple para extraer trabajos de una aplicación
> * Entrenamiento y publicación de la aplicación
> * Consulta del punto de conexión de la aplicación para ver la respuesta JSON de LUIS
> * Adición de una lista de frases para aumentar la señal de las palabras del trabajo
> * Entrenamiento, publicación de la aplicación y nueva consulta a punto de conexión

Para este artículo, necesita una cuenta gratuita de [LUIS](luis-reference-regions.md#luis-website) para crear la aplicación.

## <a name="before-you-begin"></a>Antes de empezar
Si no tiene la aplicación de recursos humanos del tutorial de [entidades compuestas](luis-tutorial-composite-entity.md), [importe](luis-how-to-start-new-app.md#import-new-app) el archivo JSON en una nueva aplicación en el sitio web de [LUIS](luis-reference-regions.md#luis-website). La aplicación que se va a importar se encuentra en el repositorio de GitHub [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-composite-HumanResources.json).

Si desea conservar la aplicación original de Recursos humanos, clone la versión en la página [Configuración](luis-how-to-manage-versions.md#clone-a-version) y llámela `simple`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original.  

## <a name="purpose-of-the-app"></a>Propósito de la aplicación
Esta aplicación muestra cómo extraer datos de una expresión. Fíjese en las siguientes expresiones de un bot de chat:

|Expresión|Nombre del trabajo que se puede extraer|
|:--|:--|
|I want to apply for the new accounting job.|accounting|
|Please submit my resume for the engineering position.|engineering|
|Fill out application for job 123456|123456|

En este tutorial se agrega una nueva entidad para extraer el nombre del trabajo. 

## <a name="purpose-of-the-simple-entity"></a>Propósito de la entidad simple
El propósito de la entidad simple en esta aplicación de LUIS es enseñar a LUIS qué es un nombre de trabajo y dónde se puede encontrar en una expresión. La parte de la expresión que es el trabajo puede cambiar de una expresión a otra en función de la longitud de la expresión y de la elección de las palabras. LUIS necesita ejemplos de trabajos en cualquier expresión de todas las intenciones.  

El nombre del trabajo es difícil de determinar porque un nombre puede ser un sustantivo, un verbo o una frase de varias palabras. Por ejemplo: 

|Trabajos|
|--|
|producto|
|software engineer|
|senior software engineer|
|engineering team lead |
|air traffic controller|
|motor vehicle operator|
|ambulance driver|
|tender|
|extruder|
|millwright|

Esta aplicación de LUIS tiene nombres de trabajos en varias intenciones. Mediante el etiquetado de estas palabras en los registros de todas las intenciones, LUIS aprende más acerca de qué es un trabajo y de dónde se encuentra en las expresiones.

## <a name="create-job-simple-entity"></a>Creación de una entidad simple de trabajo

1. Asegúrese de que la aplicación de recursos humanos se encuentra en la sección **Build** (Crear) de LUIS. Para cambiar a esta sección, seleccione **Build** (Crear) en la barra de menús superior derecha. 

    [ ![Captura de pantalla de una aplicación de LUIS con Build (Crear) resaltado en la barra de navegación superior derecha](./media/luis-quickstart-primary-and-secondary-data/hr-first-image.png)](./media/luis-quickstart-primary-and-secondary-data/hr-first-image.png#lightbox)

2. En la página **Intents** (Intenciones), seleccione la intención **ApplyForJob**. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-select-applyforjob.png "Captura de pantalla de LUIS con la intención \"ApplyForJob\" resaltada")](media/luis-quickstart-primary-and-secondary-data/hr-select-applyforjob.png#lightbox)

3. En la expresión, `I want to apply for the new accounting job`, seleccione `accounting`, escriba `Job` en el campo superior del menú emergente y, después, seleccione **Create new entity** (Crear nueva entidad) en el menú emergente. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png "Captura de pantalla de LUIS con la intención \"ApplyForJob\" con los pasos para crear una entidad resaltados")](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png#lightbox)

4. En la ventana emergente, compruebe el nombre y tipo de la entidad y seleccione **Done** (Listo).

    ![Crear cuadro de diálogo modal emergente de la entidad simple con el nombre Job y el tipo Simple](media/luis-quickstart-primary-and-secondary-data/hr-create-simple-entity-popup.png)

5. En la expresión, `Submit resume for engineering position`, etiquete la palabra `engineering` como entidad de Job. Seleccione la palabra `engineering` y después, seleccione **Job** en el menú emergente. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png "Captura de pantalla de la entidad de trabajo de etiquetado de LUIS resaltada")](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png#lightbox)

    Todas las expresiones se etiquetan pero cinco expresiones no son suficientes para enseñar a LUIS las palabras y frases relacionadas con el trabajo. Los trabajos que utilizan el valor numérico no necesitan más ejemplos, ya que usa una entidad de expresión regular. Sin embargo, los trabajos que son palabras o frases necesitan al menos quince ejemplos más. 

6. Agregue más expresiones y marque las palabras o frases de trabajo como entidad de **Job**. Los tipos de trabajo son de empleo en general en un servicio de empleo. Si deseara trabajos relacionados con un sector específico, las palabras de trabajo deberían reflejarlo. 

    |Expresión|Entidad de Job|
    |:--|:--|
    |I'm applying for the Program Manager desk in R&D|Program Manager|
    |Here is my line cook application.|line cook|
    |My resume for camp counselor is attached.|camp counselor|
    |This is my c.v. for administrative assistant.|administrative assistant|
    |I want to apply for the management job in sales.|management, sales|
    |This is my resume for the new accounting position.|accounting|
    |My application for barback is included.|barback|
    |I'm submitting my application for roofer and framer.|roofer, framer|
    |My c.v. for bus driver is here.|bus driver|
    |I'm a registered nurse. Here is my resume.|registered nurse|
    |I would like to submit my paperwork for the teaching position I saw in the paper.|teaching|
    |This is my c.v. for the stocker post in fruits and vegetables.|stocker|
    |Apply for tile work.|Icono|
    |Attached resume for landscape architect.|landscape architect|
    |My curriculum vitae for professor of biology is enclosed.|professor of biology|
    |I would like to apply for the position in photography.|photography|git 

## <a name="label-entity-in-example-utterances-for-getjobinformation-intent"></a>Etiquetado de entidad en expresiones de ejemplo de la intención GetJobInformation
1. Seleccione **Intents** (Intenciones) en el menú izquierdo.

2. Seleccione **GetJobInformation** en la lista de intenciones. 

3. Etiquete los trabajos en las expresiones del ejemplo:

    |Expresión|Entidad de Job|
    |:--|:--|
    |Is there any work in databases?|bases de datos|
    |Looking for a new situation with responsibilities in accounting (Busco una nueva situación con responsabilidades en contabilidad)|accounting|
    |What positions are available for senior engineers?|senior engineers|

    Hay otras expresiones en el ejemplo, pero no contienen palabras de trabajo.

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

Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. Vaya al final de la dirección URL en la dirección y escriba `Here is my c.v. for the programmer job`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que las expresiones etiquetadas, por lo que es una buena prueba y debería devolver las expresiones `ApplyForJob`.

```JSON
{
  "query": "Here is my c.v. for the programmer job",
  "topScoringIntent": {
    "intent": "ApplyForJob",
    "score": 0.9826467
  },
  "intents": [
    {
      "intent": "ApplyForJob",
      "score": 0.9826467
    },
    {
      "intent": "GetJobInformation",
      "score": 0.0218927357
    },
    {
      "intent": "MoveEmployee",
      "score": 0.007849265
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00349470088
    },
    {
      "intent": "Utilities.Confirm",
      "score": 0.00348804821
    },
    {
      "intent": "None",
      "score": 0.00319909188
    },
    {
      "intent": "FindForm",
      "score": 0.00222647213
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00211193133
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00172086991
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.00138010911
    }
  ],
  "entities": [
    {
      "entity": "programmer",
      "type": "Job",
      "startIndex": 24,
      "endIndex": 33,
      "score": 0.5230502
    }
  ]
}
```

## <a name="names-are-tricky"></a>Los nombres son complicados
La aplicación de LUIS ha encontrado la intención correcta con una confianza alta y ha extraído el nombre del trabajo, pero los nombres son complicados. Pruebe la expresión `This is the lead welder paperwork`.  

En el siguiente JSON, LUIS responde con la intención correcta, `ApplyForJob`, pero no ha extraído el nombre de trabajo `lead welder`. 

```JSON
{
  "query": "This is the lead welder paperwork.",
  "topScoringIntent": {
    "intent": "ApplyForJob",
    "score": 0.468558252
  },
  "intents": [
    {
      "intent": "ApplyForJob",
      "score": 0.468558252
    },
    {
      "intent": "GetJobInformation",
      "score": 0.0102701457
    },
    {
      "intent": "MoveEmployee",
      "score": 0.009442534
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00639619166
    },
    {
      "intent": "None",
      "score": 0.005859333
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.005087704
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00315379258
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00259344373
    },
    {
      "intent": "FindForm",
      "score": 0.00193389168
    },
    {
      "intent": "Utilities.Confirm",
      "score": 0.000420796918
    }
  ],
  "entities": []
}
```

Dado que un nombre puede ser cualquier cosa, LUIS predice las entidades con mayor precisión si tiene una lista de frases de palabras que aumentar la señal.

## <a name="to-boost-signal-add-jobs-phrase-list"></a>Para aumentar la señal, agregue la lista de frases de trabajos
Abra el archivo [jobs-phrase-list.csv](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/job-phrase-list.csv) del repositorio de Github LUIS-Samples. La lista tiene más de mil palabras y frases de trabajo. Examine las palabras de trabajo de la lista que tienen algún sentido para usted. Si sus palabras o frases no están en la lista, agréguelas.

1. En la sección **Build** (Crear) de la aplicación de LUIS, seleccione **Phrase lists** (Listas de frases) en el menú **Improve app performance** (Mejorar el rendimiento de la aplicación).

    [![](media/luis-quickstart-primary-and-secondary-data/hr-select-phrase-list-left-nav.png "Captura de pantalla con el botón de navegación izquierdo de Phrases lists (Listas de frases) resaltado")](media/luis-quickstart-primary-and-secondary-data/hr-select-phrase-list-left-nav.png#lightbox)

2. Seleccione **Create new phrase list** (Crear lista de frases). 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-new-phrase-list.png "Captura de pantalla con el botón Create new phrase list (Crear lista de frases) resaltado")](media/luis-quickstart-primary-and-secondary-data/hr-create-new-phrase-list.png#lightbox)

3. Asigne un nombre a la nueva lista de frases `Jobs` y copie la lista del archivo jobs-phrase-list.csv en el cuadro de texto **Values**. Seleccione ENTRAR. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png "Captura de pantalla del elemento emergente del cuadro de diálogo Create new phrase list (Crear lista de frases)")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png#lightbox)

    Si desea agregar más palabras a la lista de frases, examine **Related Values** (Valores relacionados) y agregue todas aquellas que considere pertinentes. 

4. Seleccione **Save** (Guardar) para activar la lista de frases.

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png "Captura de pantalla del elemento emergente del cuadro de diálogo Create new phrase list (Crear lista de frases) con palabras en el cuadro Phrase list values (Valores de lista de frase)")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png#lightbox)

5. Vuelva a [entrenar](#train-the-luis-app) y [publicar](#publish-the-app-to-get-the-endpoint-URL) la aplicación para que use la lista de frases.

6. Vuelva a realizar una consulta en el punto de conexión con la misma expresión: `This is the lead welder paperwork.`

    La respuesta de JSON incluye la entidad extraída:

    ```JSON
    {
        "query": "This is the lead welder paperwork.",
        "topScoringIntent": {
            "intent": "ApplyForJob",
            "score": 0.920025647
        },
        "intents": [
            {
            "intent": "ApplyForJob",
            "score": 0.920025647
            },
            {
            "intent": "GetJobInformation",
            "score": 0.003800706
            },
            {
            "intent": "Utilities.StartOver",
            "score": 0.00299335527
            },
            {
            "intent": "MoveEmployee",
            "score": 0.0027167045
            },
            {
            "intent": "None",
            "score": 0.00259556063
            },
            {
            "intent": "FindForm",
            "score": 0.00224019377
            },
            {
            "intent": "Utilities.Stop",
            "score": 0.00200693542
            },
            {
            "intent": "Utilities.Cancel",
            "score": 0.00195913855
            },
            {
            "intent": "Utilities.Help",
            "score": 0.00162656687
            },
            {
            "intent": "Utilities.Confirm",
            "score": 0.0002851904
            }
        ],
        "entities": [
            {
            "entity": "lead welder",
            "type": "Job",
            "startIndex": 12,
            "endIndex": 22,
            "score": 0.8295959
            }
        ]
    }
    ```

## <a name="phrase-lists"></a>Listas de frases
Al agregar la lista de frases se ha aumentado la señal de las palabras de la lista, pero **no** se utiliza como coincidencia exacta. La lista de frases tiene varios trabajos con la primera palabra de `lead` y también tiene el trabajo `welder`, pero no tiene el trabajo `lead welder`. Es posible que esta lista de frases de trabajo no esté completa. Como [examina expresiones de punto de conexión](luis-how-to-review-endoint-utt.md) con regularidad y encuentra otras palabras de trabajo, agréguelas a la lista de frases. A continuación, vuelva a entrenar y a publicar la aplicación.

## <a name="what-has-this-luis-app-accomplished"></a>¿Qué ha logrado esta aplicación de LUIS?
Esta aplicación, con una entidad simple y una lista de frases de palabras, ha identificado una intención de consulta en lenguaje natural y ha devuelto los datos del trabajo. 

El bot de chat ya tiene suficiente información para determinar la acción principal de aplicar un trabajo y un parámetro de dicha acción, a qué trabajo se hace referencia. 

## <a name="where-is-this-luis-data-used"></a>¿Dónde se utilizan estos datos de LUIS? 
LUIS ha terminado con esta solicitud. La aplicación que realiza la llamada, como un bot de chat, puede tomar el resultado de topScoringIntent y los datos de la entidad para usar una API de terceros para enviar la información del trabajo a un representante del departamento de recursos humanos. Si hay otras opciones de programación para el bot o la aplicación que realiza la llamada, LUIS no realiza ese trabajo. LUIS solo determina cuál es la intención del usuario. 

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no sea necesaria, elimine la aplicación de LUIS. Seleccione **My apps** (Mis aplicaciones) en la parte superior, menú de la izquierda. Seleccione los puntos suspensivos (***...***) a la derecha del nombre de la aplicación en la lista de aplicaciones y, después, seleccione **Delete** (Eliminar). En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), seleccione **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Incorporación de una entidad keyphrase precompilada](luis-quickstart-intent-and-key-phrase.md)