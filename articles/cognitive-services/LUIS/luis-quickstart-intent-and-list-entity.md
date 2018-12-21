---
title: Coincidencia de texto exacta
titleSuffix: Azure Cognitive Services
description: Obtenga datos que coincidan con una lista de elementos predefinida. Cada elemento de la lista puede tener sinónimos que también coincidan exactamente.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 5706e0b124bb9ceaf1abf7228faf088dc4e510ce
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53096696"
---
# <a name="tutorial-4-extract-exact-text-matches"></a>Tutorial 4: Extracción de coincidencias exactas de texto
En este tutorial, aprenderá cómo obtener datos que coincidan con una lista de elementos predefinida. Cada elemento de la lista puede incluir una lista de sinónimos. Para la aplicación de recursos humanos, un empleado puede identificarse mediante varios datos clave, como nombre, correo electrónico, número de teléfono y número de identificación fiscal federal de Estados Unidos. 

La aplicación de recursos humanos debe determinar qué empleado se mueve de un edificio a otro. Para obtener una expresión sobre el desplazamiento de un empleado, LUIS determina la intención y extrae el empleado para que la aplicación cliente pueda crear una orden estándar para el desplazamiento del empleado.

Esta aplicación usa una entidad de lista para extraer el empleado. La referencia al empleado se puede realizar por su nombre, número de teléfono, correo electrónico o número de la seguridad social federal de Estados Unidos. 

Una entidad de lista es una buena opción para este tipo de datos cuando:

* Los valores de los datos son un conjunto conocido.
* El conjunto no excede los [límites](luis-boundaries.md) máximos de LUIS para este tipo de entidad.
* El texto de la expresión es una coincidencia exacta con un sinónimo o el nombre canónico. 

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Usar la aplicación del tutorial existente
> * Incorporación de la intención MoveEmployee
> * Incorporación de entidad de lista 
> * Train 
> * Publicar
> * Obtener intenciones y entidades del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Usar una aplicación existente
Continúe con la aplicación creada en el último tutorial, denominada **HumanResources**. 

Si no tiene la aplicación HumanResources del tutorial anterior, siga estos pasos:

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-regex-HumanResources.json).

2. Importe el archivo JSON en una aplicación nueva.

3. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `list`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL. 


## <a name="moveemployee-intent"></a>Intención MoveEmployee

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

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

    Recuerde que número y datetimeV2 se agregaron en un tutorial anterior y que se etiquetarán automáticamente cuando se encuentren en cualquier expresión de ejemplo.

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="employee-list-entity"></a>Entidad de lista de empleados
Ahora que la intención **MoveEmployee** tiene expresiones de ejemplo, LUIS debe comprender qué es un empleado. 

El nombre principal _canónico_ de cada elemento es el número de empleado. Para este dominio, los ejemplos de los sinónimos de cada nombre canónico son: 

|Propósito del sinónimo|Valor del sinónimo|
|--|--|
|NOMBRE|John W. Smith|
|Dirección de correo electrónico|john.w.smith@mycompany.com|
|Extensión del teléfono|x12345|
|Número de teléfono móvil personal|425-555-1212|
|Número del seguro social federal en EE. UU.|123-45-6789|


1. Seleccione **Entities** (Entidades) en el panel izquierdo.

2. Haga clic en **Crear nueva entidad**.

3. En el cuadro de diálogo emergente de la entidad, escriba `Employee` para el nombre de la entidad y **List** (Lista) para el tipo de entidad. Seleccione **Listo**.  

    [![Captura de pantalla del cuadro de diálogo emergente para crear una entidad nueva](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png "Captura de pantalla del cuadro de diálogo emergente para crear una entidad nueva")](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png#lightbox)

4. En la página de la entidad Employee, escriba `Employee-24612` como el nuevo valor.

    [![Captura de pantalla de ingreso de valor](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png "Captura de pantalla de ingreso de valor")](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png#lightbox)

5. Para los sinónimos, agregue los siguientes valores:

    |Propósito del sinónimo|Valor del sinónimo|
    |--|--|
    |NOMBRE|John W. Smith|
    |Dirección de correo electrónico|john.w.smith@mycompany.com|
    |Extensión del teléfono|x12345|
    |Número de teléfono móvil personal|425-555-1212|
    |Número del seguro social federal en EE. UU.|123-45-6789|

    [![Captura de pantalla de ingreso de sinónimos](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png "Captura de pantalla de ingreso de sinónimos")](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png#lightbox)

6. Escriba `Employee-45612` como un nuevo valor.

7. Para los sinónimos, agregue los siguientes valores:

    |Propósito del sinónimo|Valor del sinónimo|
    |--|--|
    |NOMBRE|Jill Jones|
    |Dirección de correo electrónico|jill-jones@mycompany.com|
    |Extensión del teléfono|x23456|
    |Número de teléfono móvil personal|425-555-0000|
    |Número del seguro social federal en EE. UU.|234-56-7891|

## <a name="train"></a>Train

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obtención de intenciones y entidades del punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Vaya al final de la dirección URL en la dirección y escriba `shift 123-45-6789 from Z-1242 to T-54672`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `MoveEmployee` con `Employee` extraído.

  ```json
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

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes
Este tutorial se creó una nueva intención, se agregaron expresiones de ejemplo y luego se creó una entidad de lista para extraer las coincidencias exactas del texto de las expresiones. Después del aprendizaje y de la publicación de la aplicación, una consulta al punto de conexión identificó la intención y devolvió los datos extraídos.

> [!div class="nextstepaction"]
> [Incorporación de una entidad jerárquica a la aplicación](luis-quickstart-intent-and-hier-entity.md)

