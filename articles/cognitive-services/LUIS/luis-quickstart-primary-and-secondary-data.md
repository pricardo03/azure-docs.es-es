---
title: 'Tutorial: 7: Entidad simple con la lista de frases de LUIS'
titleSuffix: Azure Cognitive Services
description: Extraer datos aprendidos automáticamente de una expresión
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: f3e931344d2d2294c03756d630c688df1e5da9a8
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52425266"
---
# <a name="tutorial-7-extract-names-with-simple-entity-and-phrase-list"></a>Tutorial: 7: Extracción de nombres con la lista de entidades y frases simple

En este tutorial se extraen datos aprendidos automáticamente de un nombre de trabajo de implementación de una expresión con la entidad **Simple**. Para aumentar la precisión de extracción, agregue una lista de frases de términos específicos para la entidad simple.

En este tutorial se agrega una nueva entidad simple para extraer el nombre del trabajo. El propósito de la entidad simple en esta aplicación de LUIS es enseñar a LUIS qué es un nombre de trabajo y dónde se puede encontrar en una expresión. La parte de la expresión que es el nombre del trabajo puede cambiar de una expresión a otra en función de la longitud de la expresión y de la elección de las palabras. LUIS necesita ejemplos de nombres de trabajo en todas las intenciones que usan nombres de trabajo.  

La entidad simple es una buena opción para este tipo de datos cuando:

* Los datos son un concepto único.
* Los datos no tienen un formato correcto, como una expresión regular.
* Los datos no son comunes, como una entidad previamente creada de número de teléfono o de datos.
* Los datos no coinciden exactamente con una lista de palabras conocidas, como una entidad de lista.
* Los datos no contienen otros elementos de datos como una entidad compuesta o jerárquica.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Usar la aplicación del tutorial existente
> * Creación de una entidad simple para extraer trabajos de una aplicación
> * Adición de una lista de frases para aumentar la señal de las palabras del trabajo
> * Train 
> * Publicar 
> * Obtener intenciones y entidades del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Usar una aplicación existente

Continúe con la aplicación creada en el último tutorial, denominada **HumanResources**. 

Si no tiene la aplicación HumanResources del tutorial anterior, siga estos pasos:

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-composite-HumanResources.json).

2. Importe el archivo JSON en una aplicación nueva.

3. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `simple`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL.

## <a name="simple-entity"></a>Entidad simple
La entidad simple detecta un concepto de datos único contenido en palabras o frases.

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

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. En la página **Intents** (Intenciones), seleccione la intención **ApplyForJob**. 

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

## <a name="label-entity-in-example-utterances"></a>Entidad de etiqueta en las expresiones de ejemplo

El etiquetado, o _marcado_, de la entidad muestra a LUIS donde se encuentra la entidad en las expresiones de ejemplo.

1. Seleccione **Intents** (Intenciones) en el menú izquierdo.

2. Seleccione **GetJobInformation** en la lista de intenciones. 

3. Etiquete los trabajos en las expresiones del ejemplo:

    |Expresión|Entidad de Job|
    |:--|:--|
    |Is there any work in databases?|bases de datos|
    |Looking for a new situation with responsibilities in accounting (Busco una nueva situación con responsabilidades en contabilidad)|accounting|
    |What positions are available for senior engineers?|senior engineers|

    Hay otras expresiones en el ejemplo, pero no contienen palabras de trabajo.

## <a name="train"></a>Train

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obtención de intenciones y entidades del punto de conexión 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vaya al final de la dirección URL en la dirección y escriba `Here is my c.v. for the programmer job`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que las expresiones etiquetadas, por lo que es una buena prueba y debería devolver las expresiones `ApplyForJob`.

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
    
    LUIS encontró la intención correcta, **ApplyForJob**, y extrajo la entidad correcta, **Job** (Trabajo), con un valor de `programmer`.


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

## <a name="to-boost-signal-add-phrase-list"></a>Para aumentar la señal, agregue la lista de frases

Abra el archivo [jobs-phrase-list.csv](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/job-phrase-list.csv) del repositorio de Github LUIS-Samples. La lista tiene más de mil palabras y frases de trabajo. Examine las palabras de trabajo de la lista que tienen algún sentido para usted. Si sus palabras o frases no están en la lista, agréguelas.

1. En la sección **Build** (Crear) de la aplicación de LUIS, seleccione **Phrase lists** (Listas de frases) en el menú **Improve app performance** (Mejorar el rendimiento de la aplicación).

2. Seleccione **Create new phrase list** (Crear lista de frases). 

3. Asigne un nombre a la nueva lista de frases `Job` y copie la lista del archivo jobs-phrase-list.csv en el cuadro de texto **Values**. Seleccione ENTRAR. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png "Captura de pantalla del elemento emergente del cuadro de diálogo Create new phrase list (Crear lista de frases)")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png#lightbox)

    Si desea agregar más palabras a la lista de frases, examine **Related Values** (Valores relacionados) y agregue todas aquellas que considere pertinentes. 

4. Seleccione **Save** (Guardar) para activar la lista de frases.

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png "Captura de pantalla del elemento emergente del cuadro de diálogo Create new phrase list (Crear lista de frases) con palabras en el cuadro Phrase list values (Valores de lista de frase)")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png#lightbox)

5. Vuelva a [entrenar](#train) y [publicar](#publish) la aplicación para que use la lista de frases.

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

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, la aplicación de recursos humanos utiliza una entidad simple aprendida automáticamente para encontrar nombres de trabajo en expresiones. Dado que los nombres de trabajo pueden tener una gran variedad de palabras o frases, la aplicación necesitaba una lista de frases para aumentar las palabras de los nombres de trabajo. 

> [!div class="nextstepaction"]
> [Incorporación de una entidad keyphrase precompilada](luis-quickstart-intent-and-key-phrase.md)
