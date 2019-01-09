---
title: Predicción de las intenciones
titleSuffix: Azure Cognitive Services
description: En este tutorial, cree una aplicación personalizada que prediga la intención de un usuario. Esta aplicación es el tipo más primitivo de aplicación de LUIS, dado que no extrae varios elementos de datos del texto de la expresión, como direcciones de correo electrónico o fechas.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 20cd3931488f3d3cf4728b3022316b685da3277a
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2018
ms.locfileid: "53754275"
---
# <a name="tutorial-build-luis-app-to-determine-user-intentions"></a>Tutorial: Compilación de una aplicación LUIS para determinar las intenciones del usuario

En este tutorial creará una aplicación de recursos humanos (RRHH) personalizada que predice la intención del usuario en función de la expresión (texto). 

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Creación de una nueva aplicación 
> * Creación de intenciones
> * Incorporación de expresiones de ejemplo
> * Entrenamiento de la aplicación
> * Publicación de aplicación
> * Obtención de la intención a partir del punto de conexión


[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="user-intentions-as-intents"></a>Intenciones del usuario como intenciones

La finalidad de la aplicación es determinar la intención del texto de conversión en lenguaje natural: 

`Are there any new positions in the Seattle office?`

Estas intenciones se categorizan en **Intenciones**. 

Esta aplicación tiene algunas intenciones. 

|Intención|Propósito|
|--|--|
|ApplyForJob|Determinar si el usuario postula a un trabajo.|
|GetJobInformation|Determinar si el usuario busca información sobre trabajos en general o sobre un trabajo específico.|
|None|Determinar si el usuario pregunta algo que la aplicación no puede responder. Esta intención se proporciona como parte de la creación de la aplicación y no se puede eliminar. |

## <a name="create-a-new-app"></a>Creación de una nueva aplicación

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-intent-for-job-information"></a>Creación de una intención para información de trabajo

1. Haga clic en **Create new intent** (Crear intención). Escriba el nombre de la intención nueva `GetJobInformation`. Esta intención se predice cuando un usuario quiere información sobre las ofertas de trabajo de la empresa. 

    ![Captura de pantalla del cuadro de diálogo New Intent (Nueva intención) de Language Understanding (LUIS)](media/luis-quickstart-intents-only/create-intent.png "Captura de pantalla del cuadro de diálogo New Intent (Nueva intención) de Language Understanding (LUIS)")

1. Seleccione **Listo**.

2. Agregue varias expresiones de ejemplo a esta intención que espera que un usuario pida:

    | Expresiones de ejemplo|
    |--|
    |Any new jobs posted today? (¿Se publicó algún trabajo nuevo hoy?)|
    |Are there any new positions in the Seattle office? (¿Hay algún puesto nuevo en la oficina de Seattle?)|
    |Are there any remote worker or telecommute jobs open for engineers? (¿Hay alguna vacante de trabajo remoto o a distancia para ingenieros?)|
    |Is there any work in databases? (¿Hay algún trabajo con bases de datos?)|
    |I'm looking for a co-working situation in the tampa office. (Busco una posibilidad de trabajo compartido en la oficina de Tampa)|
    |Is there an internship in the san francisco office? (¿Hay alguna práctica profesional disponible en la oficina de San Francisco?)|
    |Is there any part-time work for people in college? (¿Hay algún trabajo a tiempo parcial para quienes estudian en la universidad?)|
    |Looking for a new situation with responsibilities in accounting (Busco una nueva situación con responsabilidades en contabilidad)|
    |Looking for a job in new york city for bilingual speakers. (Busco un trabajo en Nueva York para hablantes bilingües)|
    |Looking for a new situation with responsibilities in accounting. (Busco una nueva situación con responsabilidades en contabilidad)|
    |New jobs? (¿Trabajos nuevos?)|
    |Show me all the jobs for engineers that were added in the last 2 days. (Quiero ver todos los trabajos para ingenieros que se agregaron en los últimos 2 días).|
    |Today's job postings? (Quiero ver las ofertas de trabajo de hoy)|
    |What accounting positions are open in the london office? (¿Qué puestos contables hay disponibles en la oficina de Londres?)|
    |What positions are available for Senior Engineers? (¿Qué puestos hay disponibles para ingenieros de alto nivel?)|
    |Where is the job listings (¿Dónde están las listas de vacantes?)|

    [![Captura de pantalla de ingreso de expresiones nuevas para la intención MyStore](media/luis-quickstart-intents-only/utterance-getstoreinfo.png "Captura de pantalla de ingreso de expresiones nuevas para la intención MyStore")](media/luis-quickstart-intents-only/utterance-getstoreinfo.png#lightbox)

    Al proporcionar _expresiones de ejemplo_, está entrenando a LUIS sobre qué tipos de expresiones deben predecirse para esta intención. 

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]    

## <a name="add-example-utterances-to-the-none-intent"></a>Incorporación de expresiones de ejemplo a la intención None 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-before-testing-or-publishing"></a>Entrenamiento de la aplicación antes de las pruebas o la publicación

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-query-from-the-endpoint"></a>Publicación de la aplicación en la consulta desde el punto de conexión

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent-prediction-from-the-endpoint"></a>Obtención de la predicción de la intención desde el punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

1. Vaya al final de la dirección URL en la barra de direcciones y escriba `I'm looking for a job with Natural Language Processing`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que cualquiera de las expresiones de ejemplo. Es una buena prueba y debe devolver la intención `GetJobInformation` como la intención con la puntuación más alta. 

    ```JSON
    {
      "query": "I'm looking for a job with Natural Language Processing",
      "topScoringIntent": {
        "intent": "GetJobInformation",
        "score": 0.9923871
      },
      "intents": [
        {
          "intent": "GetJobInformation",
          "score": 0.9923871
        },
        {
          "intent": "None",
          "score": 0.007810574
        }
      ],
      "entities": []
    }
    ```

    El parámetro QueryString de `verbose=true` significa incluir **todas las intenciones** en los resultados de la consulta de la aplicación. La matriz de entidades está vacía porque esta aplicación no tiene actualmente ninguna entidad. 

    El resultado de JSON identifica la intención que tiene la puntuación más alta como propiedad **`topScoringIntent`**. Todas las puntuaciones están comprendidas entre 1 y 0, y la mejor está próxima a 1. 

## <a name="create-intent-for-job-applications"></a>Creación de intención para solicitudes de trabajo

Regrese al portal de LUIS y cree una nueva intención para determinar si la expresión del usuario trata de solicitar un trabajo.

1. Seleccione **Build** (Compilar) en el menú superior de la derecha para volver a la compilación de aplicaciones.

1. Seleccione **Intenciones** en el menú de la izquierda para ver la lista de intenciones.

1. Seleccione **Create new intent** (Crear nueva intención) y escriba el nombre `ApplyForJob`. 

    ![Cuadro de diálogo LUIS para crear una intención](./media/luis-quickstart-intents-only/create-applyforjob-intent.png)

1. Agregue varias expresiones a esta intención que espera que un usuario pida como, por ejemplo:

    | Expresiones de ejemplo|
    |--|
    |Fill out application for Job 123456 (Rellenar la postulación para el trabajo 123456)|
    |This is my c.v. for position 654234 (Este es mi currículum vítae para el puesto 654234)|
    |Here is my resume for the part-time receptionist post. (Este es mi currículum vítae para el puesto de recepcionista a tiempo parcial)|
    |I'm applying for the art desk job with this paperwork. (Con estos documentos postulo al trabajo de escritorio artístico)|
    |I'm applying for the summer college internship in Research and Development in San Diego. (Postulo a la práctica universitaria durante el verano en Investigación y desarrollo en San Diego)|
    |I'm requesting to submit my resume to the temporary position in the cafeteria. (Solicito enviar mi currículum vítae para el puesto temporal en la cafetería)|
    |I'm submitting my resume for the new Autocar team in Columbus, OH. (Envío mi currículum vítae para el nuevo equipo Autocar en Columbus, Ohio)|
    |I want to apply for the new accounting job. (Quiero postular al nuevo trabajo de contabilidad)|
    |Job 456789 accounting internship paperwork is here. (Esta es la documentación para la práctica profesional de contabilidad del trabajo 456789)|
    |Job 567890 and my paperwork (Trabajo 567890 y mi documentación)|
    |My papers for the tulsa accounting internship are attached. (Adjunto mi documentación para la práctica profesional de contabilidad en Tulsa)|
    |My paperwork for the holiday delivery position. (Mi documentación para el puesto de reparto durante las vacaciones)|
    |Please send my resume for the new accounting job in seattle. (Envíe mi currículum vítae para el nuevo trabajo contable en Seattle)|
    |Submit resume for engineering position (Enviar el currículum vítae para el puesto de ingeniero)|
    |This is my c.v. for post 234123 in Tampa. (Este es mi currículum vítae para el puesto 234123 en Tampa).|

    [![Captura de pantalla del ingreso de expresiones nuevas para la intención ApplyForJob](media/luis-quickstart-intents-only/utterance-applyforjob.png "Captura de pantalla del ingreso de expresiones nuevas para la intención ApplyForJob")](media/luis-quickstart-intents-only/utterance-applyforjob.png#lightbox)

    La intención etiquetada aparece en rojo porque actualmente LUIS no está seguro de que la intención esté correcta. Entrenar la aplicación indica a LUIS que las expresiones están en la intención correcta. 

## <a name="train-again"></a>Volver a entrenar

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-again"></a>Volver a publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent-prediction-again"></a>Volver a obtener la predicción de la intención

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. En la nueva ventana del explorador, escriba `Can I submit my resume for job 235986` al final de la dirección URL. 

    ```json
    {
      "query": "Can I submit my resume for job 235986",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.9634406
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.9634406
        },
        {
          "intent": "GetJobInformation",
          "score": 0.0171300638
        },
        {
          "intent": "None",
          "score": 0.00670867041
        }
      ],
      "entities": []
    }
    ```

    Los resultados incluyen la nueva intención **ApplyForJob**, así como las intenciones existentes. 

## <a name="client-application-next-steps"></a>Pasos siguientes de la aplicación cliente

Después de que LUIS devuelva la respuesta JSON, LUIS termina con esta solicitud. LUIS no proporciona respuestas a expresiones de usuario, solo identifica el tipo de información que se solicita en lenguaje natural. La aplicación cliente permite el seguimiento de la conversación, como una instancia de Azure Bot. 

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Información relacionada

* [Tipos de entidades](luis-concept-entity-types.md)
* [Cómo se realiza el entrenamiento](luis-how-to-train.md)
* [Publicación](luis-how-to-publish-app.md)
* [Prueba de la aplicación en el portal de LUIS](luis-interactive-test.md)
* [Azure Bot](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)


## <a name="next-steps"></a>Pasos siguientes

En este tutorial se creó la aplicación de recursos humanos (RRHH), se crearon dos intenciones, se agregaron expresiones de ejemplo a cada intención, se agregaron expresiones de ejemplo a la intención None (Ninguna), se entrenó, se publicó y se probó en el punto de conexión. Estos son los pasos básicos de la creación de un modelo de LUIS. 

Continúe con esta aplicación y [agregue una lista sencilla de entidades y frases](luis-quickstart-primary-and-secondary-data.md).

> [!div class="nextstepaction"]
> [Add prebuilt intents and entities to this app](luis-tutorial-prebuilt-intents-entities.md) (Incorporación de entidades e intenciones creadas previamente a esta aplicación)
