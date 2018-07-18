---
title: 'Creación de una entidad compuesta para extraer datos complejos: Azure | Microsoft Docs'
description: Obtenga información sobre cómo crear una entidad compuesta en la aplicación de LUIS para extraer distintos tipos de datos de entidad.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 03/28/2018
ms.author: v-geberr
ms.openlocfilehash: cb581ee60dea2b0810332933455a03a8b68e16ea
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264392"
---
# <a name="use-composite-entity-to-extract-complex-data"></a>Uso de entidades compuestas para extraer datos complejos
Esta aplicación simple tiene dos [intenciones](luis-concept-intent.md) y varias entidades. Su propósito es reservar vuelos como "1 billete de Seattle a El Cairo el viernes" y devolver todos los detalles de la reserva como un solo tipo de datos. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
* Agregar las entidades creadas previamente datetimeV2 y number
* Crear una entidad compuesta
* Consultar LUIS y recibir datos de entidad compuestos

## <a name="before-you-begin"></a>Antes de empezar
* La aplicación de LUIS del **[inicio rápido jerárquico](luis-tutorial-composite-entity.md)**. 

> [!Tip]
> Si aún no tiene una suscripción, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/).

## <a name="composite-entity-is-a-logical-grouping"></a>Una entidad compuesta es una agrupación lógica 
El propósito de la entidad es encontrar y categorizar partes del texto en una expresión. Una entidad [compuesta](luis-concept-entity-types.md) se compone de otros tipos de entidades que se extraen del contexto. Para esta aplicación de viajes que hace reservas de vuelos, hay varios datos como, por ejemplo, fechas, ubicaciones y número de plazas. 

La información existe como entidades independientes antes de que se cree una compuesta. Cree una entidad compuesta cuando se puedan agrupar las entidades independientes de forma lógica; esta agrupación lógica es útil para el bot de chat u otras aplicaciones que consumen LUIS. 

Entre las expresiones sencillas de ejemplo de los usuarios se pueden incluir:

```
Book a flight to London for next Monday
2 tickets from Dallas to Dublin this weekend
Reserve a seat from New York to Paris on the first of April
```
 
La entidad compuesta coincide con el número de plazas, lugar de origen y de destino, y fecha. 

## <a name="what-luis-does"></a>Cuál es la función de LUIS
LUIS identifica, [extrae](luis-concept-data-extraction.md#list-entity-data) y devuelve el resultado en formato JSON desde el [punto de conexión](https://aka.ms/luis-endpoint-apis) las intenciones y entidades de la expresión. La aplicación que realiza la llamada, o bot de chat, toma esa respuesta en formato JSON y completa la solicitud de la forma en la que la aplicación o bot de chat esté diseñada para hacerlo. 

## <a name="add-prebuilt-entities-number-and-datetimev2"></a>Agregar las entidades creadas previamente number y datetimeV2
1. Seleccione la aplicación `MyTravelApp` de la lista de aplicaciones del sitio web de [LUIS][LUIS].

2. Cuando se abra la aplicación, seleccione el vínculo de navegación **Entidades** de la izquierda.

    ![Hacer clic en el botón Entidades](./media/luis-tutorial-composite-entity/intents-page-select-entities.png)    

3. Haga clic en **Manage prebuilt entities** (Administrar entidades predefinidas).

    ![Hacer clic en el botón Entidades](./media/luis-tutorial-composite-entity/manage-prebuilt-entities-button.png)

4. En el cuadro emergente, seleccione **number** y **datetimeV2**.

    ![Hacer clic en el botón Entidades](./media/luis-tutorial-composite-entity/prebuilt-entity-ddl.png)

5. Para que se extraigan las nuevas entidades, haga clic en **Entrenar** en la barra de navegación superior.

    ![Haga clic en el botón Entrenar](./media/luis-tutorial-composite-entity/train.png)

## <a name="use-existing-intent-to-create-composite-entity"></a>Use la intención existente para crear la entidad compuesta
1. Haga clic en **Intents** (Intenciones) en el panel de navegación izquierdo. 

    ![Seleccione la página Intents (Intenciones)](./media/luis-tutorial-composite-entity/intents-from-entities-page.png)

2. Seleccione `BookFlight` en la lista **Intents** (Intenciones).  

    ![Seleccione la intención BookFlight de la lista](./media/luis-tutorial-composite-entity/intent-page-with-prebuilt-entities-labeled.png)

    Las entidades number y datetimeV2 creadas previamente están etiquetadas en las expresiones.

3. Para la expresión `book 2 flights from seattle to cairo next monday`, seleccione la entidad `number` de color azul, después, seleccione **Wrap in composite entity** (Ajustar en la entidad compuesta) en la lista. Una línea verde bajo las palabras sigue el cursor mientras se mueve a la derecha, lo que indica una entidad compuesta. Después, desplácese hacia la derecha para seleccionar la última entidad creada previamente `datetimeV2`, luego escriba `FlightReservation` en el cuadro de texto de la ventana emergente y, por último, haga clic en **Create new composite** (Crear nueva composición). 

    ![Crear entidad compuesta en la página de intenciones](./media/luis-tutorial-composite-entity/create-new-composite.png)

4. Un cuadro de diálogo emergente aparece y le permite comprobar los elementos secundarios de la entidad compuesta. Seleccione **Listo**.

    ![Crear entidad compuesta en la página de intenciones](./media/luis-tutorial-composite-entity/validate-composite-entity.png)

## <a name="wrap-the-entities-in-the-composite-entity"></a>Ajustar las entidades en la entidad compuesta
Una vez que se ha creado la entidad compuesta, etiquete las expresiones restantes de la entidad compuesta. Para ajustar una frase como una entidad compuesta, debe seleccionar la palabra de más a la izquierda, luego seleccione **Ajustar en la entidad compuesta** en la lista que se muestra, después seleccione la palabra de más a la derecha y, por último, seleccione la entidad compuesta con nombre `FlightReservation`. Se trata de una serie de selecciones rápidas y sencillas, que se dividen en los pasos siguientes:

1. En la expresión `schedule 4 seats from paris to london for april 1`, seleccione 4 como el número de entidad creada previamente.

    ![Seleccionar la palabra más a la izquierda](./media/luis-tutorial-composite-entity/wrap-composite-step-1.png)

2. Seleccione **Ajustar en la entidad compuesta** en la lista que se muestra.

    ![Seleccione ajustar en la lista](./media/luis-tutorial-composite-entity/wrap-composite-step-2.png)

3. Seleccione la palabra más a la derecha. Una línea de color verde aparece debajo de la frase, lo que indica que es una entidad compuesta.

    ![Seleccione la palabra más a la derecha](./media/luis-tutorial-composite-entity/wrap-composite-step-3.png)

4. Seleccione de la lista el nombre de composición `FlightReservation`.

    ![Seleccione la entidad compuesta con nombre](./media/luis-tutorial-composite-entity/wrap-composite-step-4.png)

    Para la última expresión, ajuste `London` y `tomorrow` en la entidad compuesta, siguiendo las mismas instrucciones. 

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS
LUIS no conoce los cambios en las intenciones y entidades (el modelo) hasta que se le entrena. 

1. En la parte superior derecha del sitio web de LUIS, haga clic en el botón **Entrenar**.

    ![Entrenamiento de la aplicación](./media/luis-tutorial-composite-entity/train-button.png)

2. El entrenamiento se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

    ![El entrenamiento se realizó correctamente](./media/luis-tutorial-composite-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión
Para obtener una predicción de LUIS en un bot de chat u otra aplicación, tiene que publicar la aplicación. 

1. En la parte superior derecha del sitio web de LUIS, haga clic en el botón **Publish** (Publicar). 

2. Seleccione el espacio de producción y haga clic en el botón **Publicar**.

    ![publicación de la aplicación](./media/luis-tutorial-composite-entity/publish-to-production.png)

3. La publicación se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consulta del punto de conexión con una expresión diferente
1. En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. 

    ![Seleccionar la dirección URL del punto de conexión](./media/luis-tutorial-composite-entity/publish-select-endpoint.png)

2. Vaya al final de la dirección URL en la dirección y escriba `reserve 3 seats from London to Cairo on Sunday`. El último parámetro de la cadena de consulta es `q`, la expresión query. Esta expresión no es la misma que cualquiera de las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `BookFlight` con la entidad jerárquica extraída.

```
{
  "query": "reserve 3 seats from London to Cairo on Sunday",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.999999046
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 0.999999046
    },
    {
      "intent": "None",
      "score": 0.227036044
    }
  ],
  "entities": [
    {
      "entity": "sunday",
      "type": "builtin.datetimeV2.date",
      "startIndex": 40,
      "endIndex": 45,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-WXX-7",
            "type": "date",
            "value": "2018-03-25"
          },
          {
            "timex": "XXXX-WXX-7",
            "type": "date",
            "value": "2018-04-01"
          }
        ]
      }
    },
    {
      "entity": "3 seats from london to cairo on sunday",
      "type": "flightreservation",
      "startIndex": 8,
      "endIndex": 45,
      "score": 0.6892485
    },
    {
      "entity": "cairo",
      "type": "Location::Destination",
      "startIndex": 31,
      "endIndex": 35,
      "score": 0.557570755
    },
    {
      "entity": "london",
      "type": "Location::Origin",
      "startIndex": 21,
      "endIndex": 26,
      "score": 0.8933808
    },
    {
      "entity": "3",
      "type": "builtin.number",
      "startIndex": 8,
      "endIndex": 8,
      "resolution": {
        "value": "3"
      }
    }
  ],
  "compositeEntities": [
    {
      "parentType": "flightreservation",
      "value": "3 seats from london to cairo on sunday",
      "children": [
        {
          "type": "builtin.datetimeV2.date",
          "value": "sunday"
        },
        {
          "type": "Location::Destination",
          "value": "cairo"
        },
        {
          "type": "builtin.number",
          "value": "3"
        },
        {
          "type": "Location::Origin",
          "value": "london"
        }
      ]
    }
  ]
}
```

Esta expresión devuelve una matriz de entidades compuestas que incluye el objeto **flightreservation** con los datos extraídos.  

## <a name="what-has-this-luis-app-accomplished"></a>¿Qué ha logrado esta aplicación de LUIS?
Esta aplicación, con solo dos intenciones y una entidad compuesta, ha identificado una intención de consulta en lenguaje natural y ha devuelto los datos extraídos. 

El bot de chat ahora tiene suficiente información para determinar la acción principal, `BookFlight`, y la información de reserva encontrada en la expresión. 

## <a name="where-is-this-luis-data-used"></a>¿Dónde se usan estos datos de LUIS? 
LUIS ha terminado con esta solicitud. La aplicación que realiza la llamada, como un bot de chat, puede tomar el resultado de topScoringIntent y los datos de la entidad para realizar el siguiente paso. LUIS no realiza este trabajo de programación para el bot o la aplicación que realiza la llamada. LUIS solo determina cuál es la intención del usuario. 

## <a name="next-steps"></a>Pasos siguientes

[Más información sobre las entidades](luis-concept-entity-types.md). 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
