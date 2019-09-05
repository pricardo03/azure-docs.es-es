---
title: 'Tutorial: Coincidencia de texto exacta: LUIS'
titleSuffix: Azure Cognitive Services
description: Obtenga datos que coincidan con una lista de elementos predefinida. Cada elemento de la lista puede tener sinónimos que también coincidan exactamente.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 09/03/2019
ms.author: diberry
ms.openlocfilehash: 25882f2aaa5d32dfb594d5f8fdb9cdc46f2ad958
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70307610"
---
# <a name="tutorial-get-exact-text-matched-data-from-an-utterance"></a>Tutorial: Obtener datos coincidentes con el texto exacto de una expresión

En este tutorial, aprenderá cómo obtener datos de entidad que coincidan con una lista de elementos predefinida. 

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Creación de una aplicación
> * Agregar intención
> * Incorporación de entidad de lista 
> * Train 
> * Publicar
> * Obtener intenciones y entidades del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="what-is-a-list-entity"></a>¿Qué es una entidad de lista?

Una entidad de la lista es una coincidencia de texto exacta con las palabras de la expresión. 

Cada elemento de la lista puede incluir una lista de sinónimos. Para la aplicación de recursos humanos, un departamento de la empresa puede identificarse mediante varios datos clave, como el nombre oficial, las siglas comunes y los códigos del departamento de facturación. 

La aplicación de recursos humanos debe determinar a qué departamento se transfiere un empleado. 

Una entidad de lista es una buena opción para este tipo de datos cuando:

* Los valores de los datos son un conjunto conocido.
* El conjunto no excede los [límites](luis-boundaries.md) máximos de LUIS para este tipo de entidad.
* El texto de la expresión es una coincidencia exacta con un sinónimo o el nombre canónico. LUIS no usa la lista más allá de las coincidencias exactas de texto. La lematización, los plurales y otras variaciones no se resuelven solo con una entidad de lista. Para administrar las variaciones, considere el uso de un [patrón](luis-concept-patterns.md#syntax-to-mark-optional-text-in-a-template-utterance) con la sintaxis de texto opcional. 

## <a name="create-a-new-app"></a>Creación de una nueva aplicación

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-an-intent-to-transfer-employees-to-a-different-department"></a>Creación de una intención para transferir empleados a un departamento diferente

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Haga clic en **Create new intent** (Crear intención). 

3. Escriba `TransferEmployeeToDepartment` en el cuadro de diálogo emergente y seleccione **Done** (Listo). 

    ![Captura de pantalla del cuadro de diálogo Create new intent (Crear nueva intención)](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-ddl.png)

4. Agregue expresiones de ejemplo a la intención.

    |Expresiones de ejemplo|
    |--|
    |move John W. Smith to the accounting department (mover a John W. Smith al departamento de contabilidad)|
    |transfer Jill Jones from to R&D (transferir a Jill Jones a I+D)|
    |Dept 1234 has a new member named Bill Bradstreet (El dep. 1234 tiene un nuevo miembro llamado Bill Bradstreet)|
    |Place John Jackson in Engineering (Colocar a John Jackson en Ingeniería) |
    |move Debra Doughtery to Inside Sales (mover a Debra Doughtery a Ventas internas)|
    |mv Jill Jones to IT (mover a Jill Jones a Informática)|
    |Shift Alice Anderson to DevOps (Cambiar a Alice Anderson a DevOps)|
    |Carl Chamerlin to Finance (Carl Chamerlin a Finanzas)|
    |Steve Standish to 1234 (Steve Standish a 1234)|
    |Tanner Thompson to 3456 (Tanner Thompson a 3456)|

    [![Captura de pantalla de intención con expresiones de ejemplo](media/luis-quickstart-intent-and-list-entity/intent-transfer-employee-to-department.png "Screenshot of intent with example utterances")](media/luis-quickstart-intent-and-list-entity/intent-transfer-employee-to-department.png#lightbox)

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="department-list-entity"></a>Entidad de lista de departamentos

Ahora que la intención **TransferEmployeeToDepartment** tiene expresiones de ejemplo, LUIS debe comprender qué es un departamento. 

El nombre principal _canónico_ de cada elemento es el nombre de departamento. Los ejemplos de los sinónimos de cada nombre canónico son: 

|Nombre canónico|Sinónimos|
|--|--|
|Control|acct<br>accting<br>3456|
|Development Operations|DevOps<br>4949|
|Engineering|eng<br>enging<br>4567|
|Finance|fin<br>2020|
|Information Technology|IT<br>2323|
|Inside Sales|isale<br>insale<br>1414|
|Research and Development|R&D<br>1234|

1. Seleccione **Entities** (Entidades) en el panel izquierdo.

1. Haga clic en **Crear nueva entidad**.

1. En el cuadro de diálogo emergente de la entidad, escriba `Department` para el nombre de la entidad y **List** (Lista) para el tipo de entidad. Seleccione **Listo**.  

    [![Captura de pantalla del cuadro de diálogo emergente para crear una entidad nueva](media/luis-quickstart-intent-and-list-entity/create-new-list-entity-named-department.png "Captura de pantalla del cuadro de diálogo emergente para crear una entidad nueva")](media/luis-quickstart-intent-and-list-entity/create-new-list-entity-named-department.png#lightbox)

1. En la página de la entidad Department, escriba `Accounting` como el nuevo valor.

1. Para Synonyms (Sinónimos), agregue los sinónimos de la tabla anterior.

1. Continúe agregando todos los nombres canónicos y sus sinónimos. 

## <a name="add-example-utterances-to-the-none-intent"></a>Adición de expresiones de ejemplo a la intención None 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Entrenamiento de la aplicación para probar los cambios en la intención 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Publicación de la aplicación para que se pueda consultar al modelo entrenado desde el punto de conexión

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Obtención de intención y predicción de entidad desde el punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

1. Vaya al final de la dirección URL en la dirección y escriba `shift Joe Smith to IT`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `TransferEmployeeToDepartment` con `Department` extraído.

   ```json
    {
      "query": "shift Joe Smith to IT",
      "topScoringIntent": {
        "intent": "TransferEmployeeToDepartment",
        "score": 0.9775754
      },
      "intents": [
        {
          "intent": "TransferEmployeeToDepartment",
          "score": 0.9775754
        },
        {
          "intent": "None",
          "score": 0.0154493852
        }
      ],
      "entities": [
        {
          "entity": "it",
          "type": "Department",
          "startIndex": 19,
          "endIndex": 20,
          "resolution": {
            "values": [
              "Information Technology"
            ]
          }
        }
      ]
    }
   ```

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Información relacionada

* Información conceptual sobre [entidades de lista](luis-concept-entity-types.md#list-entity)
* [Entrenamiento de la versión activa de la aplicación de LUIS](luis-how-to-train.md)
* [Publicación](luis-how-to-publish-app.md)
* [Prueba de la aplicación el portal de LUIS](luis-interactive-test.md)


## <a name="next-steps"></a>Pasos siguientes
Este tutorial se creó una nueva intención, se agregaron expresiones de ejemplo y luego se creó una entidad de lista para extraer las coincidencias exactas del texto de las expresiones. Después del aprendizaje y de la publicación de la aplicación, una consulta al punto de conexión identificó la intención y devolvió los datos extraídos.

Continúe con esta aplicación, [agregando una entidad compuesta](luis-tutorial-composite-entity.md).

> [!div class="nextstepaction"]
> [Incorporación de una entidad precompilada con un rol a la aplicación](tutorial-entity-roles.md)

