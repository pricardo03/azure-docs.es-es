---
title: 'Tutorial sobre la creación de una aplicación de LUIS para obtener texto exacto que coincida con datos enumerados: Azure | Microsoft Docs'
description: En este tutorial, aprenda a crear una aplicación sencilla de LUIS con intenciones y entidades de lista para extraer datos de esta guía de inicio rápido.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: afad3fe725fddd0748cc206517a7274815cf1653
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495271"
---
# <a name="tutorial-4-add-list-entity"></a>Tutorial: 4. Incorporación de entidad de lista
En este tutorial, cree una aplicación que muestra cómo obtener datos que coinciden con una lista predefinida. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Descripción de las entidades de lista 
> * Creación de una nueva aplicación de LUIS para el dominio de recursos humanos (RR. HH.) con la intención MoveEmployee
> * Incorporación de una entidad de lista para extraer al empleado desde la expresión
> * Entrenamiento y publicación de la aplicación
> * Consulta del punto de conexión de la aplicación para ver la respuesta JSON de LUIS

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Antes de empezar
Si no tiene la aplicación de recursos humanos del tutorial de [entidades regex](luis-quickstart-intents-regex-entity.md), [importe](luis-how-to-start-new-app.md#import-new-app) el archivo JSON en una nueva aplicación en el sitio web de [LUIS](luis-reference-regions.md#luis-website). La aplicación que se va a importar se encuentra en el repositorio de GitHub [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-regex-HumanResources.json).

Si desea conservar la aplicación original de Recursos humanos, clone la versión en la página [Configuración](luis-how-to-manage-versions.md#clone-a-version) y llámela `list`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. 

## <a name="purpose-of-the-list-entity"></a>Propósito de la entidad de lista
Esta aplicación predice expresiones sobre el traslado de un empleado de un edificio a otro. Esta aplicación usa una entidad de la lista para extraer un empleado. Al empleado se le puede referir por su nombre, número de teléfono, correo electrónico o número del seguro social federal de Estados Unidos. 

Una entidad de lista puede contener muchos elementos con sinónimos para cada elemento. Para una empresa pequeña o mediana, la entidad de lista se utiliza para extraer la información del empleado. 

El nombre canónico de cada posición es el número de empleado. Para este dominio, los ejemplos de los sinónimos son: 

|Propósito del sinónimo|Valor del sinónimo|
|--|--|
|NOMBRE|John W. Smith|
|Dirección de correo electrónico|john.w.smith@mycompany.com|
|Extensión del teléfono|x12345|
|Número de teléfono móvil personal|425-555-1212|
|Número del seguro social federal en EE. UU.|123-45-6789|

Una entidad de lista es una buena opción para este tipo de datos cuando:

* Los valores de los datos son un conjunto conocido.
* El conjunto no excede los [límites](luis-boundaries.md) máximos de LUIS para este tipo de entidad.
* El texto de la expresión es una coincidencia exacta con un sinónimo. 

LUIS extrae al empleado de tal manera que la aplicación cliente puede crear un orden estándar para desplazar al empleado.
<!--
## Example utterances
Simple example utterances for a `MoveEmployee` inent:

```
move John W. Smith from B-1234 to H-4452
mv john.w.smith@mycompany from office b-1234 to office h-4452

```
-->

## <a name="add-moveemployee-intent"></a>Incorporación de la intención MoveEmployee

1. Asegúrese de que la aplicación de recursos humanos se encuentra en la sección **Build** (Crear) de LUIS. Para cambiar a esta sección, seleccione **Build** (Crear) en la barra de menús superior derecha. 

2. Haga clic en **Create new intent** (Crear intención). 

3. Escriba `MoveEmployee` en el cuadro de diálogo emergente y seleccione **Done** (Listo). 

    ![Captura de pantalla del cuadro de diálogo Create new intent (Crear nueva intención)](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-ddl.png)

4. Agregue expresiones de ejemplo a la intención.

    |Expresiones de ejemplo|
    |--|
    |move John W. Smith from B-1234 to H-4452|
    |mv john.w.smith@mycompany.com from office b-1234 to office h-4452|
    |shift x12345 to h-1234 tomorrow|
    |place 425-555-1212 in HH-2345|
    |move 123-45-6789 from A-4321 to J-23456|
    |mv Jill Jones from D-2345 to J-23456|
    |shift jill-jones@mycompany.com to M-12345|
    |x23456 to M-12345|
    |425-555-0000 to h-4452|
    |234-56-7891 to hh-2345|

    [ ![Captura de pantalla de la página Intent (Intención) con nuevas expresiones resaltadas](./media/luis-quickstart-intent-and-list-entity/hr-enter-utterances.png) ](./media/luis-quickstart-intent-and-list-entity/hr-enter-utterances.png#lightbox)

## <a name="create-an-employee-list-entity"></a>Creación de una entidad de lista de empleados
Ahora que la intención **MoveEmployee** tiene expresiones, LUIS debe comprender qué es un empleado. 

1. Seleccione **Entities** (Entidades) en el panel izquierdo.

2. Haga clic en **Crear nueva entidad**.

3. En el cuadro de diálogo emergente de la entidad, escriba `Employee` para el nombre de la entidad y **List** (Lista) para el tipo de entidad. Seleccione **Listo**.  

    [![](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png "Captura de pantalla de creación del cuadro de diálogo emergente de creación de una nueva entidad")](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png#lightbox)

4. En la página de la entidad Employee, escriba `Employee-24612` como el nuevo valor.

    [![](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png "Captura de pantalla de escritura de un valor")](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png#lightbox)

5. Para los sinónimos, agregue los siguientes valores:

    |Propósito del sinónimo|Valor del sinónimo|
    |--|--|
    |NOMBRE|John W. Smith|
    |Dirección de correo electrónico|john.w.smith@mycompany.com|
    |Extensión del teléfono|x12345|
    |Número de teléfono móvil personal|425-555-1212|
    |Número del seguro social federal en EE. UU.|123-45-6789|

    [![](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png "Captura de pantalla de incorporación de sinónimos")](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png#lightbox)

6. Escriba `Employee-45612` como un nuevo valor.

7. Para los sinónimos, agregue los siguientes valores:

    |Propósito del sinónimo|Valor del sinónimo|
    |--|--|
    |NOMBRE|Jill Jones|
    |Dirección de correo electrónico|jill-jones@mycompany.com|
    |Extensión del teléfono|x23456|
    |Número de teléfono móvil personal|425-555-0000|
    |Número del seguro social federal en EE. UU.|234-56-7891|

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consulta del punto de conexión con una expresión diferente

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Vaya al final de la dirección URL en la dirección y escriba `shift 123-45-6789 from Z-1242 to T-54672`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `MoveEmployee` con `Employee` extraído.

  ```JSON
  {
    "query": "shift 123-45-6789 from Z-1242 to T-54672",
    "topScoringIntent": {
      "intent": "MoveEmployee",
      "score": 0.9882801
    },
    "intents": [
      {
        "intent": "MoveEmployee",
        "score": 0.9882801
      },
      {
        "intent": "FindForm",
        "score": 0.016044287
      },
      {
        "intent": "GetJobInformation",
        "score": 0.007611245
      },
      {
        "intent": "ApplyForJob",
        "score": 0.007063288
      },
      {
        "intent": "Utilities.StartOver",
        "score": 0.00684710965
      },
      {
        "intent": "None",
        "score": 0.00304174074
      },
      {
        "intent": "Utilities.Help",
        "score": 0.002981
      },
      {
        "intent": "Utilities.Confirm",
        "score": 0.00212222221
      },
      {
        "intent": "Utilities.Cancel",
        "score": 0.00191026414
      },
      {
        "intent": "Utilities.Stop",
        "score": 0.0007461446
      }
    ],
    "entities": [
      {
        "entity": "123 - 45 - 6789",
        "type": "Employee",
        "startIndex": 6,
        "endIndex": 16,
        "resolution": {
          "values": [
            "Employee-24612"
          ]
        }
      },
      {
        "entity": "123",
        "type": "builtin.number",
        "startIndex": 6,
        "endIndex": 8,
        "resolution": {
          "value": "123"
        }
      },
      {
        "entity": "45",
        "type": "builtin.number",
        "startIndex": 10,
        "endIndex": 11,
        "resolution": {
          "value": "45"
        }
      },
      {
        "entity": "6789",
        "type": "builtin.number",
        "startIndex": 13,
        "endIndex": 16,
        "resolution": {
          "value": "6789"
        }
      },
      {
        "entity": "-1242",
        "type": "builtin.number",
        "startIndex": 24,
        "endIndex": 28,
        "resolution": {
          "value": "-1242"
        }
      },
      {
        "entity": "-54672",
        "type": "builtin.number",
        "startIndex": 34,
        "endIndex": 39,
        "resolution": {
          "value": "-54672"
        }
      }
    ]
  }
  ```

  Se encontró el empleado y se devolvió como tipo `Employee` con un valor de resolución de `Employee-24612`.

## <a name="where-is-the-natural-language-processing-in-the-list-entity"></a>¿Dónde está el procesamiento de lenguaje natural en la entidad List? 
Dado que la entidad List es una coincidencia de texto exacta, no se basa en el procesamiento de lenguaje natural (o aprendizaje automático). LUIS usa el procesamiento de lenguaje natural (o aprendizaje automático) para seleccionar la intención de puntuación más alta correcta. Además, una expresión puede ser una combinación de más de una entidad o incluso de más de un tipo de entidad. Cada expresión se procesa para todas las entidades de la aplicación, incluidas las entidades de procesamiento de lenguaje natural (o de aprendizaje automático).

## <a name="what-has-this-luis-app-accomplished"></a>¿Qué ha logrado esta aplicación de LUIS?
Esta aplicación, con una entidad de lista, extrajo el empleado correcto. 

El bot de chat ahora tiene suficiente información para determinar la acción principal, `MoveEmployee`, y qué empleados mover. 

## <a name="where-is-this-luis-data-used"></a>¿Dónde se usan estos datos de LUIS? 
LUIS ha terminado con esta solicitud. La aplicación que realiza la llamada, como un bot de chat, puede tomar el resultado de topScoringIntent y los datos de la entidad para realizar el siguiente paso. LUIS no realiza este trabajo de programación para el bot o la aplicación que realiza la llamada. LUIS solo determina cuál es la intención del usuario. 

## <a name="clean-up-resources"></a>Limpieza de recursos

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Incorporación de una entidad jerárquica a la aplicación](luis-quickstart-intent-and-hier-entity.md)

