---
title: 'Tutorial 1: Búsqueda de intenciones en la aplicación de LUIS personalizada'
titleSuffix: Azure Cognitive Services
description: Cree una aplicación personalizada que prediga la intención del usuario. Esta aplicación es el tipo más primitivo de aplicación de LUIS, dado que no extrae varios elementos de datos del texto de la expresión, como direcciones de correo electrónico o fechas.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 30c9f572d77caacbeecf5f15d74fd8517e9fa883
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52426866"
---
# <a name="tutorial-1-build-custom-app-to-determine-user-intentions"></a>Tutorial 1: Creación de una aplicación personalizada para determinar las intenciones del usuario

En este tutorial creará una aplicación de recursos humanos (RRHH) personalizada que predice la intención del usuario en función de la expresión (texto). Cuando termine, tendrá un punto de conexión LUIS en ejecución en la nube.

La finalidad de la aplicación es determinar la intención del texto de conversión en lenguaje natural. Estas intenciones se categorizan en **Intenciones**. Esta aplicación tiene algunas intenciones. La primera intención, **`GetJobInformation`**, identifica cuando un usuario quiere información sobre los trabajos disponibles dentro de una empresa. La segunda intención, **`None`**, se usa para cualquier expresión del usuario que esté fuera del _dominio_ (ámbito) de esta aplicación. Más adelante, una tercera intención, **`ApplyForJob`**, se agrega para cualquier expresión sobre la aplicación para un trabajo. Esta tercera intención es diferente de `GetJobInformation` porque ya se debe conocer la información del trabajo cuando alguien solicita dicho trabajo. Sin embargo, dependiendo de la elección de la palabra, determinar la intención puede ser complicado porque ambas son sobre un trabajo.

Después de que LUIS devuelva la respuesta JSON, LUIS termina con esta solicitud. LUIS no proporciona respuestas a expresiones de usuario, solo identifica el tipo de información que se solicita en lenguaje natural. 

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Creación de una nueva aplicación 
> * Creación de intenciones
> * Incorporación de expresiones de ejemplo
> * Entrenamiento de la aplicación
> * Publicación de aplicación
> * Obtención de la intención a partir del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="create-a-new-app"></a>Creación de una nueva aplicación

1. Inicie sesión en el portal de LUIS con la dirección URL [https://www.luis.ai](https://www.luis.ai). 

2. Seleccione **Create new app** (Crear nueva aplicación).  

    [![](media/luis-quickstart-intents-only/app-list.png "Captura de pantalla de la página My Apps (Mis aplicaciones) de Language Understanding (LUIS)")](media/luis-quickstart-intents-only/app-list.png#lightbox)

3. En el cuadro de diálogo emergente, escriba el nombre `HumanResources` y mantenga la referencia cultural predeterminada, **English** (Inglés). Deje en blanco la descripción.

    ![Nueva aplicación de LUIS](./media/luis-quickstart-intents-only/create-app.png)

    A continuación, la aplicación mostrará la página **Intents** (Intenciones) con la intención **None** (Ninguna).

## <a name="getjobinformation-intent"></a>Intención GetJobInformation

1. Haga clic en **Create new intent** (Crear intención). Escriba el nombre de la intención nueva `GetJobInformation`. Esta intención se predice cada vez que un usuario quiere información sobre las ofertas de trabajo de la empresa.

    ![](media/luis-quickstart-intents-only/create-intent.png "Captura de pantalla del cuadro de diálogo New intent (Nueva intención) de Language Understanding (LUIS)")

2. Al proporcionar _expresiones de ejemplo_, está entrenando a LUIS sobre qué tipos de expresiones deben predecirse para esta intención. Agregue varias expresiones de ejemplo a esta intención que espera que un usuario pida como, por ejemplo:

    | Expresiones de ejemplo|
    |--|
    |Any new jobs posted today? (¿Se publicó algún trabajo nuevo hoy?)|
    |What positions are available for Senior Engineers? (¿Qué puestos hay disponibles para ingenieros de alto nivel?)|
    |Is there any work in databases? (¿Hay algún trabajo con bases de datos?)|
    |Looking for a new situation with responsibilities in accounting (Busco una nueva situación con responsabilidades en contabilidad)|
    |Where is the job listings (¿Dónde están las listas de vacantes?)|
    |New jobs? (¿Trabajos nuevos?)|
    |Are there any new positions in the Seattle office? (¿Hay algún puesto nuevo en la oficina de Seattle?)|

    [![](media/luis-quickstart-intents-only/utterance-getstoreinfo.png "Captura de pantalla del ingreso de expresiones nuevas para la intención MyStore")](media/luis-quickstart-intents-only/utterance-getstoreinfo.png#lightbox)

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]    


## <a name="none-intent"></a>Intención None 
La aplicación cliente debe saber si una expresión está fuera del dominio del asunto de la aplicación. Si LUIS devuelve la intención **None** (Ninguna) para una expresión, la aplicación cliente puede preguntar al usuario si desea finalizar la conversación. La aplicación cliente también puede dar más instrucciones para que continúe la conversación si el usuario no desea terminarla. 

Estas expresiones de ejemplo, fuera del dominio del asunto, se agrupan en la intención **None** (Ninguna). No la deje en blanco. 

1. Seleccione **Intents** (Intenciones) en el panel izquierdo.

2. Seleccione la intención **None**. Agregue tres expresiones que el usuario podría solicitar pero que no son pertinentes para la aplicación de recursos humanos. Si la aplicación está relacionada con los puestos de trabajo, algunas expresiones del tipo **None** (Ninguna) que puede usar son las siguientes:

    | Expresiones de ejemplo|
    |--|
    |Barking dogs are annoying (Los perros que ladran son molestos)|
    |Order a pizza for me (Pídanme una pizza)|
    |Penguins in the ocean (Pingüinos en el océano)|


## <a name="train"></a>Train 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent"></a>Obtener intención

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vaya al final de la dirección URL en la barra de direcciones y escriba `I'm looking for a job with Natural Language Processing`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que cualquiera de las expresiones de ejemplo. Es una buena prueba y debe devolver la intención `GetJobInformation` como la intención con la puntuación más alta. 

    ```JSON
    {
      "query": "I'm looking for a job with Natural Language Processing",
      "topScoringIntent": {
        "intent": "GetJobInformation",
        "score": 0.8965092
      },
      "intents": [
        {
          "intent": "GetJobInformation",
          "score": 0.8965092
        },
        {
          "intent": "None",
          "score": 0.147104025
        }
      ],
      "entities": []
    }
    ```

    Los resultados incluyen **todas las intenciones** de la aplicación, actualmente dos. La matriz de entidades está vacía porque esta aplicación no tiene actualmente ninguna entidad. 

    El resultado de JSON identifica la intención que tiene la puntuación más alta como propiedad **`topScoringIntent`**. Todas las puntuaciones están comprendidas entre 1 y 0, y la mejor está próxima a 1. 

## <a name="applyforjob-intent"></a>Intención ApplyForJob
Regrese al sitio web de LUIS y cree una nueva intención para determinar si la expresión del usuario trata de solicitar un trabajo.

1. Seleccione **Build** (Compilar) en el menú superior de la derecha para volver a la compilación de aplicaciones.

2. Seleccione **Intents** (Intenciones) en el menú izquierdo.

3. Seleccione **Create new intent** (Crear nueva intención) y escriba el nombre `ApplyForJob`. 

    ![Cuadro de diálogo LUIS para crear una intención](./media/luis-quickstart-intents-only/create-applyforjob-intent.png)

4. Agregue varias expresiones a esta intención que espera que un usuario pida como, por ejemplo:

    | Expresiones de ejemplo|
    |--|
    |I want to apply for the new accounting job. (Quiero postular al nuevo trabajo de contabilidad)|
    |Fill out application for Job 123456 (Rellenar la postulación para el trabajo 123456)|
    |Submit resume for engineering position (Enviar el currículum vítae para el puesto de ingeniero)|
    |This is my c.v. for position 654234 (Este es mi currículum vítae para el puesto 654234)|
    |Job 567890 and my paperwork (Trabajo 567890 y mi documentación)|

    [![](media/luis-quickstart-intents-only/utterance-applyforjob.png "Captura de pantalla del ingreso de expresiones nuevas para la intención ApplyForJob")](media/luis-quickstart-intents-only/utterance-applyforjob.png#lightbox)

    La intención etiquetada aparece en rojo porque actualmente LUIS no está seguro de que la intención esté correcta. Entrenar la aplicación indica a LUIS que las expresiones están en la intención correcta. 

## <a name="train-again"></a>Volver a entrenar

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-again"></a>Volver a publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent-again"></a>Volver a obtener intención

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. En la nueva ventana del explorador, escriba `Can I submit my resume for job 235986` al final de la dirección URL. 

    ```JSON
    {
      "query": "Can I submit my resume for job 235986",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.9166808
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.9166808
        },
        {
          "intent": "GetJobInformation",
          "score": 0.07162977
        },
        {
          "intent": "None",
          "score": 0.0262826588
        }
      ],
      "entities": []
    }
    ```

    Los resultados incluyen la nueva intención **ApplyForJob**, así como las intenciones existentes. 

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se creó la aplicación de recursos humanos (RRHH), se crearon dos intenciones, se agregaron expresiones de ejemplo a cada intención, se agregaron expresiones de ejemplo a la intención None (Ninguna), se entrenó, se publicó y se probó en el punto de conexión. Estos son los pasos básicos de la creación de un modelo de LUIS. 

> [!div class="nextstepaction"]
> [Add prebuilt intents and entities to this app](luis-tutorial-prebuilt-intents-entities.md) (Incorporación de entidades e intenciones creadas previamente a esta aplicación)
