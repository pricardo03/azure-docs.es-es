---
title: 'Tutorial sobre la creación de una entidad compuesta para extraer datos complejos: Azure | Microsoft Docs'
description: Obtenga información sobre cómo crear una entidad compuesta en la aplicación de LUIS para extraer distintos tipos de datos de entidad.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 17a8110624975d8053ad69c5bf30477e6d715ee8
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159833"
---
# <a name="tutorial-6-add-composite-entity"></a>Tutorial: 6. Incorporación de entidad compuesta 
En este tutorial, agregue una entidad compuesta para agrupar los datos extraídos en una entidad contenedora.

En este tutorial, aprenderá a:

<!-- green checkmark -->
> [!div class="checklist"]
> * Entender las entidades compuestas 
> * Agregar una entidad compuesta para extraer datos
> * Entrenamiento y publicación de la aplicación
> * Consulta del punto de conexión de la aplicación para ver la respuesta JSON de LUIS

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Antes de empezar
Si no tiene la aplicación de recursos humanos del tutorial de [entidades jerárquicas](luis-quickstart-intent-and-hier-entity.md), [importe](luis-how-to-start-new-app.md#import-new-app) el archivo JSON en una nueva aplicación en el sitio web de [LUIS](luis-reference-regions.md#luis-website). La aplicación que se va a importar se encuentra en el repositorio de GitHub [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-hier-HumanResources.json).

Si desea conservar la aplicación original de Recursos humanos, clone la versión en la página [Configuración](luis-how-to-manage-versions.md#clone-a-version) y llámela `composite`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original.  

## <a name="composite-entity-is-a-logical-grouping"></a>Una entidad compuesta es una agrupación lógica 
El propósito de la entidad compuesta es agrupar las entidades relacionadas en una entidad de categoría principal. La información existe como entidades independientes antes de que se cree una compuesta. Es similar a la entidad jerárquica, pero puede contener más tipos de entidades. 

 Cree una entidad compuesta cuando se puedan agrupar las entidades independientes de forma lógica; esta agrupación lógica es útil para la aplicación cliente. 

En esta aplicación, el nombre del empleado se define en la entidad de lista **Employee** (Empleado) e incluye sinónimos del nombre, la dirección de correo electrónico, la extensión de teléfono de la empresa, el número de teléfono móvil y EE. UU. Número de identificación fiscal 

La intención **MoveEmployee** tiene expresiones de ejemplo para solicitar el traslado de un empleado de un edificio y oficina a otro. Los nombres de los edificios son alfabéticos: "A", "B", etc., mientras que las oficinas son numéricas: "1234", "13245". 

Entre las expresiones de ejemplo de la intención **MoveEmployee** se incluyen las siguientes:

|Expresiones de ejemplo|
|--|
|Move John W . Smith to a-2345|
|shift x12345 to h-1234 tomorrow|
 
La solicitud de traslado debe incluir al menos el empleado (con cualquier sinónimo) y la ubicación final del edificio y la oficina. La solicitud también puede incluir la oficina de origen, además de una fecha en que se debe realizar el traslado. 

Los datos extraídos del punto de conexión deben contener esta información y devolverla en una entidad compuesta `RequestEmployeeMove`. 

## <a name="create-composite-entity"></a>Creación de una entidad compuesta
1. Asegúrese de que la aplicación de recursos humanos se encuentra en la sección **Build** (Crear) de LUIS. Para cambiar a esta sección, seleccione **Build** (Crear) en la barra de menús superior derecha. 

2. En la página **Intents** (Intenciones), seleccione la intención **MoveEmployee**. 

3. Seleccione el icono de lupa de la barra de herramientas para filtrar la lista de expresiones. 

    [![](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png "Captura de pantalla de LUIS en la intención \"MoveEmployee\" con el botón de lupa resaltado")](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png#lightbox)

4. Escriba `tomorrow` en el cuadro de texto de filtro para buscar la expresión `shift x12345 to h-1234 tomorrow`.

    [![](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png "Captura de pantalla de LUIS en la intención \"MoveEmployee\" con el filtro de \"tomorrow\" resaltado")](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png#lightbox)

    Otro método consiste en filtrar la entidad por datetimeV2, mediante la selección de **Entity filters** (Filtros de entidad) y seleccione **datetimeV2** en la lista. 

5. Seleccione la primera entidad, `Employee`, y seleccione **Ajustar en la entidad compuesta** en la lista del menú emergente. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-1.png "Captura de pantalla de LUIS en la intención \"MoveEmployee\" con la opción de seleccionar la primera entidad en la composición resaltada")](media/luis-tutorial-composite-entity/hr-create-entity-1.png#lightbox)


6. A continuación, seleccione inmediatamente la última entidad, `datetimeV2`, en la expresión. Se dibuja una barra verde debajo de las palabras seleccionadas que indica una entidad compuesta. En el menú emergente, escriba el nombre compuesto `RequestEmployeeMove` y seleccione **Create new composite** (Crear nueva composición) en el menú emergente. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-2.png "Captura de pantalla de LUIS en la intención \"MoveEmployee\" con la opción de seleccionar la última entidad en la composición y la creación de entidad resaltadas")](media/luis-tutorial-composite-entity/hr-create-entity-2.png#lightbox)

7. En **What type of entity do you want to create?** (¿Qué tipo de entidad quiere crear), casi todos los campos necesarios están en la lista. Solo falta la ubicación de origen. Seleccione **Add a child entity** (Agregar una entidad secundaria), seleccione **Locations::Origin** en la lista de las entidades existentes y seleccione **Done** (Listo). 

    ![Captura de pantalla de LUIS en la intención "MoveEmployee" con la opción de agregar otra entidad en la ventana emergente](media/luis-tutorial-composite-entity/hr-create-entity-ddl.png)

8. Seleccione el icono de lupa de la barra de herramientas para quitar el filtro. 

## <a name="label-example-utterances-with-composite-entity"></a>Etiquetado de expresiones de ejemplo con una entidad compuesta
1. En cada expresión de ejemplo, seleccione la entidad que se encuentra más a la izquierda que debería estar en la composición. A continuación, seleccione **Wrap in composite entity** (Ajustar en la entidad compuesta).

    [![](media/luis-tutorial-composite-entity/hr-label-entity-1.png "Captura de pantalla de LUIS en la intención \"MoveEmployee\" con la opción de seleccionar la primera unidad en la composición resaltada")](media/luis-tutorial-composite-entity/hr-label-entity-1.png#lightbox)

2. Seleccione la última palabra en la entidad compuesta y luego seleccione **RequestEmployeeMove** en el menú emergente. 

    [![](media/luis-tutorial-composite-entity/hr-label-entity-2.png "Captura de pantalla de LUIS en la intención \"MoveEmployee\" con la opción de seleccionar la última entidad en la composición resaltada")](media/luis-tutorial-composite-entity/hr-label-entity-2.png#lightbox)

3. Compruebe que todas las expresiones de la intención están etiquetadas con la entidad compuesta. 

    [![](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png "Captura de pantalla de LUIS en la intención \"MoveEmployee\" con todas las expresiones etiquetadas")](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png#lightbox)

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint"></a>Consulta del punto de conexión 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vaya al final de la dirección URL en la dirección y escriba `Move Jill Jones from a-1234 to z-2345 on March 3 2 p.m.`. El último parámetro de la cadena de consulta es `q`, la expresión query. 

    Como esta prueba se realiza para comprobar que la entidad compuesta se extrae correctamente, puede incluir una expresión de ejemplo existente o una expresión nueva. En una prueba buena se incluyen todas las entidades secundarias de la entidad compuesta.

    ```JSON
    {
      "query": "Move Jill Jones from a-1234 to z-2345 on March 3  2 p.m",
      "topScoringIntent": {
        "intent": "MoveEmployee",
        "score": 0.9959525
      },
      "intents": [
        {
          "intent": "MoveEmployee",
          "score": 0.9959525
        },
        {
          "intent": "GetJobInformation",
          "score": 0.009858314
        },
        {
          "intent": "ApplyForJob",
          "score": 0.00728598563
        },
        {
          "intent": "FindForm",
          "score": 0.0058053555
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.005371796
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00266987388
        },
        {
          "intent": "None",
          "score": 0.00123299169
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.00116407464
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00102653319
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.0006628214
        }
      ],
      "entities": [
        {
          "entity": "march 3 2 p.m",
          "type": "builtin.datetimeV2.datetime",
          "startIndex": 41,
          "endIndex": 54,
          "resolution": {
            "values": [
              {
                "timex": "XXXX-03-03T14",
                "type": "datetime",
                "value": "2018-03-03 14:00:00"
              },
              {
                "timex": "XXXX-03-03T14",
                "type": "datetime",
                "value": "2019-03-03 14:00:00"
              }
            ]
          }
        },
        {
          "entity": "jill jones",
          "type": "Employee",
          "startIndex": 5,
          "endIndex": 14,
          "resolution": {
            "values": [
              "Employee-45612"
            ]
          }
        },
        {
          "entity": "z - 2345",
          "type": "Locations::Destination",
          "startIndex": 31,
          "endIndex": 36,
          "score": 0.9690751
        },
        {
          "entity": "a - 1234",
          "type": "Locations::Origin",
          "startIndex": 21,
          "endIndex": 26,
          "score": 0.9713137
        },
        {
          "entity": "-1234",
          "type": "builtin.number",
          "startIndex": 22,
          "endIndex": 26,
          "resolution": {
            "value": "-1234"
          }
        },
        {
          "entity": "-2345",
          "type": "builtin.number",
          "startIndex": 32,
          "endIndex": 36,
          "resolution": {
            "value": "-2345"
          }
        },
        {
          "entity": "3",
          "type": "builtin.number",
          "startIndex": 47,
          "endIndex": 47,
          "resolution": {
            "value": "3"
          }
        },
        {
          "entity": "2",
          "type": "builtin.number",
          "startIndex": 50,
          "endIndex": 50,
          "resolution": {
            "value": "2"
          }
        },
        {
          "entity": "jill jones from a - 1234 to z - 2345 on march 3 2 p . m",
          "type": "requestemployeemove",
          "startIndex": 5,
          "endIndex": 54,
          "score": 0.4027723
        }
      ],
      "compositeEntities": [
        {
          "parentType": "requestemployeemove",
          "value": "jill jones from a - 1234 to z - 2345 on march 3 2 p . m",
          "children": [
            {
              "type": "builtin.datetimeV2.datetime",
              "value": "march 3 2 p.m"
            },
            {
              "type": "Locations::Destination",
              "value": "z - 2345"
            },
            {
              "type": "Employee",
              "value": "jill jones"
            },
            {
              "type": "Locations::Origin",
              "value": "a - 1234"
            }
          ]
        }
      ],
      "sentimentAnalysis": {
        "label": "neutral",
        "score": 0.5
      }
    }
    ```

  Esta expresión devuelve una matriz de entidades compuestas. Cada entidad tiene un tipo y un valor. Para encontrar más precisión para cada entidad secundaria, use la combinación de tipo y valor del elemento de matriz compuesta para buscar el elemento correspondiente en la matriz de entidades.  

## <a name="what-has-this-luis-app-accomplished"></a>¿Qué ha logrado esta aplicación de LUIS?
Esta aplicación identificó una intención de consulta de lenguaje natural y devolvió los datos extraídos como un grupo con nombre. 

El bot de chat ahora tiene información suficiente para determinar la acción principal y los detalles relacionados en la expresión. 

## <a name="where-is-this-luis-data-used"></a>¿Dónde se usan estos datos de LUIS? 
LUIS ha terminado con esta solicitud. La aplicación que realiza la llamada, como un bot de chat, puede tomar el resultado de topScoringIntent y los datos de la entidad para realizar el siguiente paso. LUIS no realiza este trabajo de programación para el bot o la aplicación que realiza la llamada. LUIS solo determina cuál es la intención del usuario. 

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Más información sobre cómo agregar una entidad simple con una lista de frases](luis-quickstart-primary-and-secondary-data.md)  