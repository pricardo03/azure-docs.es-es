---
title: Entidad simple, lista de frases
titleSuffix: Azure Cognitive Services
description: En este tutorial se extraen datos aprendidos automáticamente de un nombre de empleo de una expresión con la entidad Simple. Para aumentar la precisión de extracción, agregue una lista de frases de términos específicos para la entidad simple.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 167dce168f9adfa59a96c09c62548c7c6e253b09
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211413"
---
# <a name="tutorial-extract-names-with-simple-entity-and-a-phrase-list"></a>Tutorial: Extracción de nombres con una lista de entidades simples y frases

En este tutorial se extraen datos aprendidos automáticamente de un nombre de trabajo de implementación de una expresión con la entidad **Simple**. Para aumentar la precisión de extracción, agregue una lista de frases de términos específicos para la entidad simple.

La entidad simple detecta un concepto de datos único contenido en palabras o frases.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Importar la aplicación de ejemplo
> * Agregar una entidad simple 
> * Agregar una lista de frases para aumentar la señal de las palabras
> * Train 
> * Publicar 
> * Obtener intenciones y entidades del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]


## <a name="simple-entity"></a>Entidad simple

En este tutorial se agrega una nueva entidad simple para extraer el nombre del trabajo. El propósito de la entidad simple en esta aplicación de LUIS es enseñar a LUIS qué es un nombre de trabajo y dónde se puede encontrar en una expresión. La parte de la expresión que es el nombre del trabajo puede cambiar de una expresión a otra en función de la longitud de la expresión y de la elección de las palabras. LUIS necesita ejemplos de nombres de trabajo en todas las intenciones que usan nombres de trabajo.  

La entidad simple es una buena opción para este tipo de datos cuando:

* Los datos son un concepto único.
* Los datos no tienen un formato correcto, como una expresión regular.
* Los datos no son comunes, como una entidad previamente creada de número de teléfono o de datos.
* Los datos no coinciden exactamente con una lista de palabras conocidas, como una entidad de lista.
* Los datos no contienen otros elementos de datos como una entidad compuesta o jerárquica.

Fíjese en las siguientes expresiones de un bot de chat:

|Expresión|Nombre del trabajo que se puede extraer|
|:--|:--|
|I want to apply for the new accounting job.|accounting|
|Submit my resume for the engineering position.|engineering|
|Fill out application for job 123456|123456|

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

Esta aplicación de LUIS tiene nombres de trabajos en varias intenciones. Mediante el etiquetado de estas palabras en los registros de todas las intenciones, LUIS aprende más acerca de qué es un nombre de trabajo y de dónde se encuentra en las expresiones.

Una vez que las entidades están marcadas en las expresiones de ejemplo, es importante agregar una lista de frases para aumentar la señal de la entidad simple. Una lista de frases **no** se usa como una coincidencia exacta y no es necesario que tenga todos los valores posibles que espera. 

## <a name="import-example-app"></a>Importar la aplicación de ejemplo

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/build-app/intentonly.json) desde el tutorial de intenciones.

2. Importe el archivo JSON en una aplicación nueva.

3. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `simple`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL.

## <a name="mark-entities-in-example-utterances-of-an-intent"></a>Marcado de entidades en expresiones de ejemplo de una intención

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. En la página **Intents** (Intenciones), seleccione la intención **ApplyForJob**. 

1. En la expresión, `I want to apply for the new accounting job`, seleccione `accounting`, escriba `Job` en el campo superior del menú emergente y, después, seleccione **Create new entity** (Crear nueva entidad) en el menú emergente. 

    [![Captura de pantalla de LUIS con la intención "ApplyForJob" donde se resaltan los pasos para crear una entidad](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png "Screenshot of LUIS with 'ApplyForJob' intent with create entity steps highlighted")](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png#lightbox)

1. En la ventana emergente, compruebe el nombre y tipo de la entidad y seleccione **Done** (Listo).

    ![Crear cuadro de diálogo modal emergente de la entidad simple con el nombre Job y el tipo Simple](media/luis-quickstart-primary-and-secondary-data/hr-create-simple-entity-popup.png)

1. En las expresiones restantes, marque las palabras relacionadas con el trabajo con la entidad **Job** (Trabajo) seleccionando la palabra o frase y, después, seleccionando **Job** (Trabajo) en el menú emergente. 

    [![Captura de pantalla de LUIS donde se resalta el etiquetado de la entidad de trabajo](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png "etiquetado resaltado de entidad de trabajo de captura de pantalla de LUIS")](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png#lightbox)


## <a name="add-more-example-utterances-and-mark-entity"></a>Adición de más expresiones de ejemplo y marcado de entidades

Las entidades simples necesitan muchos ejemplos para tener un grado alto de confianza en la predicción. 
 
1. Agregue más expresiones y marque las palabras o frases de trabajo como entidad de **Job**. 

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

## <a name="mark-job-entity-in-other-intents"></a>Marcado de entidades de trabajo en otras intenciones

1. Seleccione **Intents** (Intenciones) en el menú izquierdo.

1. Seleccione **GetJobInformation** en la lista de intenciones. 

1. Etiquete los trabajos en las expresiones del ejemplo.

    Si hay más expresiones de ejemplo en una intención que en otra, esa intención tiene una mayor probabilidad de ser la de más alta predicción. 

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Entrenamiento de la aplicación para probar los cambios en la intención 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Publicación de la aplicación para que se pueda consultar al modelo entrenado desde el punto de conexión

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Obtención de intención y predicción de entidad desde el punto de conexión 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vaya al final de la dirección URL en la dirección y escriba `Here is my c.v. for the engineering job`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que las expresiones etiquetadas, por lo que es una buena prueba y debería devolver las expresiones `ApplyForJob`.

    ```json
    {
      "query": "Here is my c.v. for the engineering job",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.98052007
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.98052007
        },
        {
          "intent": "GetJobInformation",
          "score": 0.03424581
        },
        {
          "intent": "None",
          "score": 0.0015820954
        }
      ],
      "entities": [
        {
          "entity": "engineering",
          "type": "Job",
          "startIndex": 24,
          "endIndex": 34,
          "score": 0.668959737
        }
      ]
    }
    ```
    
    LUIS encontró la intención correcta, **ApplyForJob**, y extrajo la entidad correcta, **Job** (Trabajo), con un valor de `engineering`.


## <a name="names-are-tricky"></a>Los nombres son complicados
La aplicación de LUIS ha encontrado la intención correcta con una confianza alta y ha extraído el nombre del trabajo, pero los nombres son complicados. Pruebe la expresión `This is the lead welder paperwork`.  

En el siguiente JSON, LUIS responde con la intención correcta, `ApplyForJob`, pero no ha extraído el nombre de trabajo `lead welder`. 

```json
{
  "query": "This is the lead welder paperwork",
  "topScoringIntent": {
    "intent": "ApplyForJob",
    "score": 0.860295951
  },
  "intents": [
    {
      "intent": "ApplyForJob",
      "score": 0.860295951
    },
    {
      "intent": "GetJobInformation",
      "score": 0.07265678
    },
    {
      "intent": "None",
      "score": 0.00482481951
    }
  ],
  "entities": []
}
```

Dado que un nombre puede ser cualquier cosa, LUIS predice las entidades con mayor precisión si tiene una lista de frases de palabras que aumentar la señal.

## <a name="to-boost-signal-of-the-job-related-words-add-a-phrase-list-of-job-related-words"></a>Para aumentar la señal de las palabras relacionadas con trabajos, agregue una lista con frases de palabras relacionadas con trabajos.

Abra el archivo [jobs-phrase-list.csv](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/job-phrase-list.csv) del repositorio de GitHub de ejemplos de Azure. La lista tiene más de mil palabras y frases de trabajo. Examine las palabras de trabajo de la lista que tienen algún sentido para usted. Si sus palabras o frases no están en la lista, agréguelas.

1. En la sección **Build** (Crear) de la aplicación de LUIS, seleccione **Phrase lists** (Listas de frases) en el menú **Improve app performance** (Mejorar el rendimiento de la aplicación).

1. Seleccione **Create new phrase list** (Crear lista de frases). 

1. Asigne un nombre a la nueva lista de frases `JobNames` y copie la lista del archivo jobs-phrase-list.csv en el cuadro de texto **Values**. Seleccione ENTRAR. 

    [![Captura de pantalla donde se muestra el cuadro de diálogo para crear una nueva lista de frases](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png "Screenshot of create new phrase list dialog pop-up")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png#lightbox)

    Si desea agregar más palabras a la lista de frases, examine **Related Values** (Valores relacionados) y agregue todas aquellas que considere pertinentes. 

1. Seleccione **Save** (Guardar) para activar la lista de frases.

    [![Captura de pantalla donde se muestra el cuadro de diálogo para crear una lista de frases que contiene palabras en el cuadro de valores de la lista de frases](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png "Screenshot of create new phrase list dialog pop-up with words in phrase list values box")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png#lightbox)

1. Vuelva a [entrenar](#train) y [publicar](#publish) la aplicación para que use la lista de frases.

1. Vuelva a realizar una consulta en el punto de conexión con la misma expresión: `This is the lead welder paperwork.`

    La respuesta de JSON incluye la entidad extraída:

    ```json
      {
      "query": "This is the lead welder paperwork.",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.983076453
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.983076453
        },
        {
          "intent": "GetJobInformation",
          "score": 0.0120766377
        },
        {
          "intent": "None",
          "score": 0.00248388131
        }
      ],
      "entities": [
        {
          "entity": "lead welder",
          "type": "Job",
          "startIndex": 12,
          "endIndex": 22,
          "score": 0.8373154
        }
      ]
    }
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Información relacionada

* [Tutorial sobre intenciones sin entidades](luis-quickstart-intents-only.md)
* Información conceptual sobre [entidades simples](luis-concept-entity-types.md)
* Información conceptual sobre [listas de frases](luis-concept-feature.md)
* [Cómo se realiza el entrenamiento](luis-how-to-train.md)
* [Publicación](luis-how-to-publish-app.md)
* [Prueba de la aplicación el portal de LUIS](luis-interactive-test.md)


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, la aplicación de recursos humanos utiliza una entidad simple aprendida automáticamente para encontrar nombres de trabajo en expresiones. Dado que los nombres de trabajo pueden tener una gran variedad de palabras o frases, la aplicación necesitaba una lista de frases para aumentar las palabras de los nombres de trabajo. 

> [!div class="nextstepaction"]
> [Incorporación de una entidad keyphrase precompilada](luis-quickstart-intent-and-key-phrase.md)
