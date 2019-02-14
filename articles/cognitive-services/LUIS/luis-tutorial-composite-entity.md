---
title: Entidad compuesta
titleSuffix: Azure Cognitive Services
description: Agregue una entidad compuesta para agrupar los datos extraídos de varios tipos en una única entidad contenedora. Mediante la agrupación de los datos, la aplicación cliente puede extraer fácilmente los datos relacionados en diferentes tipos de datos.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 0d78c365b171ea80d208c447f4746fe80b965ef2
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55883311"
---
# <a name="tutorial-group-and-extract-related-data"></a>Tutorial: Agrupación y extracción de datos relacionados
En este tutorial, agregue una entidad compuesta para agrupar los datos extraídos de varios tipos en una única entidad contenedora. Mediante la agrupación de los datos, la aplicación cliente puede extraer fácilmente los datos relacionados en diferentes tipos de datos.

El propósito de la entidad compuesta es agrupar las entidades relacionadas en una entidad de categoría principal. La información existe como entidades independientes antes de que se cree una compuesta. Es similar a una entidad jerárquica, pero puede contener distintos tipos de entidades. 

La entidad compuesta es una buena opción para este tipo de datos, ya que los datos:

* Están relacionados entre sí. 
* Usan una variedad de tipos de entidad.
* Debe estar agrupados y procesados por la aplicación cliente como una unidad de información.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Importar la aplicación de ejemplo
> * Crear intención
> * Incorporación de entidad compuesta 
> * Train
> * Publicar
> * Obtener intenciones y entidades del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Importar la aplicación de ejemplo

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/build-app/tutorial_list.json) desde el tutorial para enumerar entidades.

2. Importe el archivo JSON en una aplicación nueva.

3. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `composite`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL.

## <a name="composite-entity"></a>Entidad compuesta

En esta aplicación, se define el nombre del departamento en la lista de entidades **Departamento** y se incluyen los sinónimos. 

La intención **TransferEmployeeToDepartment** tiene expresiones de ejemplo para solicitar que un empleado se transfiera a un nuevo departamento. 

Entre las expresiones de ejemplo de esta intención se incluyen las siguientes:

|Expresiones de ejemplo|
|--|
|move John W. Smith to the accounting department (mover a John W. Smith al departamento de contabilidad)|
|transfer Jill Jones from to R&D (transferir a Jill Jones a I+D)|
 
La solicitud de transferencia debe incluir el nombre del departamento y el nombre del empleado. 

## <a name="add-the-personname-prebuilt-entity-to-help-with-common-data-type-extraction"></a>Incorporación de la entidad PersonName creada previamente para ayudar con la extracción de tipos de datos habituales

LUIS proporciona varias entidades creadas previamente para la extracción de datos comunes. 

1. Seleccione **Build** (Compilación) en el menú de navegación superior y seleccione **Entities** (Entidades) en el menú de navegación de la izquierda.

1. Seleccione el botón **Manage prebuilt entities** (Administrar entidades creadas previamente).

1. Seleccione **[PersonName](luis-reference-prebuilt-person.md)** en la lista de entidades predeterminadas y, a continuación, seleccione **Done** (Listo).

    ![Captura de pantalla del cuadro de diálogo de la selección de número en entidades creadas previamente](./media/luis-tutorial-composite-entity/add-personname-prebuilt-entity.png)

    Esta entidad le servirá para agregar el reconocimiento de nombres a la aplicación cliente.

## <a name="create-composite-entity-from-example-utterances"></a>Creación de una entidad compuesta a partir de expresiones de ejemplo

1. Haga clic en **Intents** (Intenciones) en el panel de navegación izquierdo.

1. Seleccione **TransferEmployeeToDepartment** en la lista de intenciones.

1. En la primera expresión, seleccione la entidad personName, `John Jackson` y, a continuación, seleccione **Start wrapping composite entity** (Iniciar encapsulado de entidad compuesta) en la lista del menú emergente de la siguiente declaración:

    `place John Jackson in engineering`

1. A continuación, seleccione inmediatamente la última entidad, `engineering`, en la expresión. Se dibuja una barra verde debajo de las palabras seleccionadas que indica una entidad compuesta. En el menú emergente, escriba el nombre compuesto `TransferEmployeeInfo` y, a continuación, seleccione Intro. 

1. En **What type of entity do you want to create?** (¿Qué tipo de entidad quiere crear?), todos los campos necesarios están en la lista: `personName` y `Department`. Seleccione **Listo**. 

    Observe que la entidad creada previamente, personName, se ha agregado a la entidad compuesta. Si puede hacer que una entidad creada previamente aparezca entre los tokens de inicio y finalización de una entidad compuesta, la entidad compuesta debe contener estas entidades creadas previamente. Si no se incluyen las entidades creadas previamente, la entidad compuesta no se predice correctamente, pero cada elemento individual, sí.

## <a name="label-example-utterances-with-composite-entity"></a>Etiquetado de expresiones de ejemplo con una entidad compuesta


1. En cada expresión de ejemplo, seleccione la entidad que se encuentra más a la izquierda que debería estar en la composición. A continuación, seleccione **Wrap in composite entity** (Ajustar en la entidad compuesta).

1. Seleccione la última palabra en la entidad compuesta y luego seleccione **TransferEmployeeInfo** en el menú emergente. 

1. Compruebe que todas las expresiones de la intención están etiquetadas con la entidad compuesta. 

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Entrenamiento de la aplicación para probar los cambios en la intención 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Publicación de la aplicación para que se pueda consultar al modelo entrenado desde el punto de conexión

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Obtención de intención y predicción de entidad desde el punto de conexión 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vaya al final de la dirección URL en la dirección y escriba `Move Jill Jones to DevOps`. El último parámetro de la cadena de consulta es `q`, la expresión query. 

    Como esta prueba se realiza para comprobar que la entidad compuesta se extrae correctamente, puede incluir una expresión de ejemplo existente o una expresión nueva. En una prueba buena se incluyen todas las entidades secundarias de la entidad compuesta.

    ```json
    {
      "query": "Move Jill Jones to DevOps",
      "topScoringIntent": {
        "intent": "TransferEmployeeToDepartment",
        "score": 0.9882747
      },
      "intents": [
        {
          "intent": "TransferEmployeeToDepartment",
          "score": 0.9882747
        },
        {
          "intent": "None",
          "score": 0.00925369747
        }
      ],
      "entities": [
        {
          "entity": "jill jones",
          "type": "builtin.personName",
          "startIndex": 5,
          "endIndex": 14
        },
        {
          "entity": "devops",
          "type": "Department",
          "startIndex": 19,
          "endIndex": 24,
          "resolution": {
            "values": [
              "Development Operations"
            ]
          }
        },
        {
          "entity": "jill jones to devops",
          "type": "TransferEmployeeInfo",
          "startIndex": 5,
          "endIndex": 24,
          "score": 0.9607566
        }
      ],
      "compositeEntities": [
        {
          "parentType": "TransferEmployeeInfo",
          "value": "jill jones to devops",
          "children": [
            {
              "type": "builtin.personName",
              "value": "jill jones"
            },
            {
              "type": "Department",
              "value": "devops"
            }
          ]
        }
      ]
    }
    ```

  Esta expresión devuelve una matriz de entidades compuestas. Cada entidad tiene un tipo y un valor. Para encontrar más precisión para cada entidad secundaria, use la combinación de tipo y valor del elemento de matriz compuesta para buscar el elemento correspondiente en la matriz de entidades.  

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Información relacionada

* [Tutorial para enumerar intenciones](luis-quickstart-intents-only.md)
* Información conceptual sobre [entidades compuestas](luis-concept-entity-types.md)
* [Cómo se realiza el entrenamiento](luis-how-to-train.md)
* [Publicación](luis-how-to-publish-app.md)
* [Prueba de la aplicación el portal de LUIS](luis-interactive-test.md)


## <a name="next-steps"></a>Pasos siguientes

En este tutorial se ha creado una entidad compuesta para encapsular las entidades existentes. Esto permite que la aplicación cliente busque un grupo de datos relacionados en diferentes tipos de datos para continuar la conversación. Una aplicación cliente para esta aplicación de recursos humanos puede preguntar el día y la hora en que el traslado debe empezar y terminar. También puede preguntar sobre otros aspectos logísticos del traslado, como un teléfono físico. 

> [!div class="nextstepaction"] 
> [Más información sobre cómo agregar una entidad simple con una lista de frases](luis-quickstart-primary-and-secondary-data.md)  
