---
title: 'Tutorial 6: Extraer datos compuestos con una entidad compuesta de LUIS'
titleSuffix: Azure Cognitive Services
description: Agregue una entidad compuesta para agrupar los datos extraídos de varios tipos en una única entidad contenedora. Mediante la agrupación de los datos, la aplicación cliente puede extraer fácilmente los datos relacionados en diferentes tipos de datos.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 8f7edecf1abd1f01a2f40f1420a6a85224271239
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52423508"
---
# <a name="tutorial-6-group-and-extract-related-data"></a>Tutorial 6: Agrupación y extracción de datos relacionados
En este tutorial, agregue una entidad compuesta para agrupar los datos extraídos de varios tipos en una única entidad contenedora. Mediante la agrupación de los datos, la aplicación cliente puede extraer fácilmente los datos relacionados en diferentes tipos de datos.

El propósito de la entidad compuesta es agrupar las entidades relacionadas en una entidad de categoría principal. La información existe como entidades independientes antes de que se cree una compuesta. Es similar a la entidad jerárquica, pero puede contener distintos tipos de entidades. 

La entidad compuesta es una buena opción para este tipo de datos, ya que los datos:

* Están relacionados entre sí. 
* Usan una variedad de tipos de entidad.
* Debe estar agrupados y procesados por la aplicación cliente como una unidad de información.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Usar la aplicación del tutorial existente
> * Incorporación de entidad compuesta 
> * Train
> * Publicar
> * Obtener intenciones y entidades del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Usar una aplicación existente
Continúe con la aplicación creada en el último tutorial, denominada **HumanResources**. 

Si no tiene la aplicación HumanResources del tutorial anterior, siga estos pasos:

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-hier-HumanResources.json).

2. Importe el archivo JSON en una aplicación nueva.

3. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `composite`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL.


## <a name="composite-entity"></a>Entidad compuesta
Cree una entidad compuesta cuando se puedan agrupar las entidades independientes de forma lógica; esta agrupación lógica es útil para la aplicación cliente. 

En esta aplicación, el nombre del empleado se define en la entidad de lista **Employee** (Empleado) e incluye sinónimos del nombre, la dirección de correo electrónico, la extensión de teléfono de la empresa, el número de teléfono móvil y EE. UU. Número de identificación fiscal 

La intención **MoveEmployee** tiene expresiones de ejemplo para solicitar el traslado de un empleado de un edificio y oficina a otro. Los nombres de los edificios son alfabéticos: "A", "B", etc., mientras que las oficinas son numéricas: "1234", "13245". 

Entre las expresiones de ejemplo de la intención **MoveEmployee** se incluyen las siguientes:

|Expresiones de ejemplo|
|--|
|Move John W . Smith to a-2345|
|shift x12345 to h-1234 tomorrow|
 
La solicitud de traslado debe incluir el empleado (con cualquier sinónimo) y la ubicación final del edificio y la oficina. La solicitud también puede incluir la oficina de origen, además de una fecha en que se debe realizar el traslado. 

Los datos extraídos del punto de conexión deben contener esta información y devolverla en la entidad compuesta `RequestEmployeeMove`:

```JSON
"compositeEntities": [
  {
    "parentType": "RequestEmployeeMove",
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
]
```

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. En la página **Intents** (Intenciones), seleccione la intención **MoveEmployee**. 

3. Seleccione el icono de lupa de la barra de herramientas para filtrar la lista de expresiones. 

    [![](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png "Captura de pantalla de LUIS en la intención \"MoveEmployee\" con el botón de lupa resaltado")](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png#lightbox)

4. Escriba `tomorrow` en el cuadro de texto de filtro para buscar la expresión `shift x12345 to h-1234 tomorrow`.

    [![](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png "Captura de pantalla de LUIS en la intención \"MoveEmployee\" con el filtro de \"tomorrow\" resaltado")](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png#lightbox)

    Otro método consiste en filtrar la entidad por datetimeV2, mediante la selección de **Entity filters** (Filtros de entidad) y seleccione **datetimeV2** en la lista. 

5. Seleccione la primera entidad, `Employee`, y seleccione **Ajustar en la entidad compuesta** en la lista del menú emergente. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-1.png "Captura de pantalla de LUIS en la intención \"MoveEmployee\" con la opción de seleccionar la primera entidad en la composición resaltada")](media/luis-tutorial-composite-entity/hr-create-entity-1.png#lightbox)


6. A continuación, seleccione inmediatamente la última entidad, `datetimeV2`, en la expresión. Se dibuja una barra verde debajo de las palabras seleccionadas que indica una entidad compuesta. En el menú emergente, escriba el nombre compuesto `RequestEmployeeMove` y, a continuación, seleccione Intro. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-2.png "Captura de pantalla de LUIS en la intención \"MoveEmployee\" con la opción de seleccionar la última entidad en la composición y la creación de entidad resaltadas")](media/luis-tutorial-composite-entity/hr-create-entity-2.png#lightbox)

7. En **What type of entity do you want to create?** (¿Qué tipo de entidad quiere crear), casi todos los campos necesarios están en la lista. Solo falta la ubicación de origen. Seleccione **Add a child entity** (Agregar una entidad secundaria), seleccione **Locations::Origin** en la lista de las entidades existentes y seleccione **Done** (Listo). 

    Observe que la entidad creada previamente, number, se ha agregado a la entidad compuesta. Si puede hacer que una entidad creada previamente aparezca entre los tokens de inicio y finalización de una entidad compuesta, la entidad compuesta debe contener estas entidades creadas previamente. Si no se incluyen las entidades creadas previamente, la entidad compuesta no se predice correctamente, pero cada elemento individual, sí.

    ![Captura de pantalla de LUIS en la intención "MoveEmployee" con la opción de agregar otra entidad en la ventana emergente](media/luis-tutorial-composite-entity/hr-create-entity-ddl.png)

8. Seleccione el icono de lupa de la barra de herramientas para quitar el filtro. 

9. Elimine la palabra `tomorrow` del filtro para poder ver todas las expresiones de ejemplo de nuevo. 

## <a name="label-example-utterances-with-composite-entity"></a>Etiquetado de expresiones de ejemplo con una entidad compuesta


1. En cada expresión de ejemplo, seleccione la entidad que se encuentra más a la izquierda que debería estar en la composición. A continuación, seleccione **Wrap in composite entity** (Ajustar en la entidad compuesta).

    [![](media/luis-tutorial-composite-entity/hr-label-entity-1.png "Captura de pantalla de LUIS en la intención \"MoveEmployee\" con la opción de seleccionar la primera unidad en la composición resaltada")](media/luis-tutorial-composite-entity/hr-label-entity-1.png#lightbox)

2. Seleccione la última palabra en la entidad compuesta y luego seleccione **RequestEmployeeMove** en el menú emergente. 

    [![](media/luis-tutorial-composite-entity/hr-label-entity-2.png "Captura de pantalla de LUIS en la intención \"MoveEmployee\" con la opción de seleccionar la última entidad en la composición resaltada")](media/luis-tutorial-composite-entity/hr-label-entity-2.png#lightbox)

3. Compruebe que todas las expresiones de la intención están etiquetadas con la entidad compuesta. 

    [![](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png "Captura de pantalla de LUIS en la intención \"MoveEmployee\" con todas las expresiones etiquetadas")](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png#lightbox)

## <a name="train"></a>Train

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obtención de intenciones y entidades del punto de conexión 

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
          "type": "RequestEmployeeMove",
          "startIndex": 5,
          "endIndex": 54,
          "score": 0.4027723
        }
      ],
      "compositeEntities": [
        {
          "parentType": "RequestEmployeeMove",
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
      ]
    }
    ```

  Esta expresión devuelve una matriz de entidades compuestas. Cada entidad tiene un tipo y un valor. Para encontrar más precisión para cada entidad secundaria, use la combinación de tipo y valor del elemento de matriz compuesta para buscar el elemento correspondiente en la matriz de entidades.  

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se ha creado una entidad compuesta para encapsular las entidades existentes. Esto permite que la aplicación cliente busque un grupo de datos relacionados en diferentes tipos de datos para continuar la conversación. Una aplicación cliente para esta aplicación de recursos humanos puede preguntar el día y la hora en que el traslado debe empezar y terminar. También puede preguntar sobre otros aspectos logísticos del traslado, como un teléfono físico. 

> [!div class="nextstepaction"] 
> [Más información sobre cómo agregar una entidad simple con una lista de frases](luis-quickstart-primary-and-secondary-data.md)  
