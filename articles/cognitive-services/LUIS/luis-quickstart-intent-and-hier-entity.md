---
title: 'Tutorial para crear una aplicación de LUIS que obtenga datos de ubicación: Azure | Microsoft Docs'
description: En este tutorial, va a aprender a crear una aplicación de LUIS sencilla con intenciones y una entidad jerárquica para extraer datos.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: v-geberr
ms.openlocfilehash: 2547407126943161ba604fa2f5e80b9186cae57e
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266505"
---
# <a name="tutorial-create-app-that-uses-hierarchical-entity"></a>Tutorial: Creación de una aplicación que utiliza una entidad jerárquica
En este tutorial, creará una aplicación que muestra cómo buscar datos relacionados en función del contexto. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Descripción de entidades jerárquicas y elementos secundarios aprendidos por el contexto 
> * Creación de una nueva aplicación de LUIS para el dominio de los viajes con la intención Bookflight
> * Adición de la intención _None_ y adición de expresiones de ejemplo
> * Agregar la entidad jerárquica de ubicación con los elementos secundarios de origen y destino
> * Entrenamiento y publicación de la aplicación
> * Consulta del punto de conexión de la aplicación para ver la respuesta JSON de LUIS que incluye elementos secundarios jerárquicos 

Para este artículo, necesita una cuenta de [LUIS][LUIS] gratuita para crear la aplicación de LUIS.

## <a name="purpose-of-the-app-with-this-entity"></a>Propósito de la aplicación con esta entidad
Esta aplicación determina si un usuario desea reservar un vuelo. Usa la entidad jerárquica para determinar las ubicaciones y las ciudades de origen y destino en el texto del usuario. 

La entidad jerárquica es una buena opción para este tipo de datos debido a las dos partes de datos:

* Son ubicaciones que normalmente se expresan como ciudades o códigos de aeropuerto.
* Normalmente se puede elegir una sola palabra para determinar cuál es el origen y cuál es el destino. Estas palabras se incluyen: a, dirigido hacia, desde o partiendo.
* Ambas ubicaciones suelen estar en la misma expresión. 

El propósito de la entidad **jerárquica** es encontrar datos relacionados en la expresión en función del contexto. Considere la siguiente expresión:

```JSON
1 ticket from Seattle to Cairo`
```

La expresión tiene dos ubicaciones especificadas. Una es la ciudad de origen, Seattle, y la otra es la de destino, El Cairo. Estas ciudades son importantes para reservar un vuelo. Aunque se pueden encontrar mediante el uso de entidades simples, están relacionados entre sí y con frecuencia se encontrarán en la misma expresión. Por consiguiente, tiene sentido que estén agrupadas como elementos secundarios de una entidad jerárquica, **"Location"**. 

Al ser entidades que se aprendido automáticamente, la aplicación necesita expresiones de ejemplo con las ciudades de origen y destino etiquetadas. Esto le indica a LUIS en qué lugar de las expresiones se encuentran las entidades, su longitud y las palabras que tienen alrededor. 

## <a name="app-intents"></a>Intenciones de aplicación
Las intenciones son categorías de lo que quiere el usuario. Esta aplicación tiene dos intenciones: BookFlight y None. La intención [None](luis-concept-intent.md#none-intent-is-fallback-for-app) existe a propósito, e indica cualquier cosa que no esté en la aplicación.  

## <a name="hierarchical-entity-is-contextually-learned"></a>Las entidades jerárquicas se aprenden por el contexto 
El propósito de la entidad es encontrar y categorizar partes del texto en una expresión. Una entidad [jerárquica](luis-concept-entity-types.md) es una entidad principal-secundaria que se basa en el contexto del uso. Una persona puede determinar las ciudades de origen y destino en una expresión basada en el uso de `to` y `from`. Estos son un ejemplo de uso contextual.  

Para esta aplicación de viaje, LUIS extrae las ubicaciones de origen y destino de forma que se pueda crear y rellenar una reserva estándar. LUIS permite que las expresiones tengan variantes, abreviaturas y argot. 

Entre las expresiones sencillas de ejemplo de los usuarios se pueden incluir:

```
Book a flight to London for next Monday
2 tickets from Dallas to Dublin this weekend
Researve a seat from New York to Paris on the first of April
```

Las versiones abreviadas o de argot de las expresiones incluyen:

```
LHR tomorrow
SEA to NYC next Monday
LA to MCO spring break
```
 
La entidad jerárquica coincide con la ubicación de origen y destino. Aunque solo exista un elemento secundario (origen o destino) de una entidad jerárquica, se extraerá. No es preciso encontrar todos los elementos secundarios si solo se va a extraer uno, o varios. 

## <a name="what-luis-does"></a>Cuál es la función de LUIS
LUIS identifica, [extrae](luis-concept-data-extraction.md#list-entity-data) y devuelve el resultado en formato JSON desde el [punto de conexión](https://aka.ms/luis-endpoint-apis) las intenciones y entidades de la expresión. La aplicación que realiza la llamada, o bot de chat, toma esa respuesta en formato JSON y completa la solicitud de la forma en la que la aplicación o bot de chat esté diseñada para hacerlo. 

## <a name="create-a-new-app"></a>Creación de una nueva aplicación
1. Inicie sesión en el sitio web de [LUIS][LUIS]. Asegúrese de que inicia sesión en la [región][LUIS-regions] en la que necesita publicar los puntos de conexión de LUIS.

2. En el sitio web de [LUIS][LUIS], seleccione **Create new app** (Crear aplicación).  

    [![](media/luis-quickstart-intent-and-hier-entity/app-list.png "Captura de pantalla de la página de lista de aplicaciones")](media/luis-quickstart-intent-and-hier-entity/app-list.png#lightbox)

3. En el cuadro de diálogo emergente, escriba el nombre `MyTravelApp`. 

    [![](media/luis-quickstart-intent-and-hier-entity/create-new-app.png "Captura de pantalla del cuadro de diálogo emergente Create new app (Crear aplicación)")](media/luis-quickstart-intent-and-hier-entity/create-new-app.png#lightbox)

4. Cuando el proceso finalice, la aplicación mostrará la página **Intents** (Intenciones) con la intención **None**. 

    [![](media/luis-quickstart-intent-and-hier-entity/intents-page-none-only.png "Captura de pantalla de la lista Intents (Intenciones) con la intención None")](media/luis-quickstart-intent-and-hier-entity/intents-page-none-only.png#lightbox)

## <a name="create-a-new-intent"></a>Creación de una nueva intención

1. En la página **Intents** (Intenciones), seleccione **Create new intent** (Crear intención). 

    [![](media/luis-quickstart-intent-and-hier-entity/create-new-intent-button.png "Captura de pantalla de la lista Intents (Intenciones) con el botón Create new intent (Crear nueva intención) resaltado ")](media/luis-quickstart-intent-and-hier-entity/create-new-intent-button.png#lightbox)

2. Escriba el nombre de la intención nueva `BookFlight`. Esta intención se debe seleccionar cada vez que un usuario quiera reservar un vuelo.

    Al crear una intención, se crea la categoría principal de información que desea identificar. Dar un nombre a la categoría permite que otras aplicaciones que usen los resultados de consulta de LUIS puedan utilizar ese nombre de categoría para encontrar una respuesta apropiada o tomar las medidas adecuadas. LUIS no responde a estas preguntas, solo identifica el tipo de información que se solicita en lenguaje natural. 

    [![](media/luis-quickstart-intent-and-hier-entity/create-new-intent.png "Captura de pantalla del cuadro de diálogo emergente Create new intent (Crear intención)")](media/luis-quickstart-intent-and-hier-entity/create-new-intent.png#lightbox)

3. Agregue varias expresiones a la intención `BookFlight` que espera que un usuario pida como, por ejemplo:

    | Expresiones de ejemplo|
    |--|
    |Reservar dos vuelos de Seattle a El Cairo para el lunes siguiente|
    |Reservar un billete a Londres para mañana|
    |Programar cuatro asientos de París a Londres para el 1 de abril|

    [![](media/luis-quickstart-intent-and-hier-entity/enter-utterances-on-intent.png "Captura de pantalla de escritura de expresiones en la página de la intención BookFlight")](media/luis-quickstart-intent-and-hier-entity/enter-utterances-on-intent.png#lightbox)

## <a name="add-utterances-to-none-intent"></a>Adición de expresiones a la intención None

La aplicación de LUIS no tiene actualmente expresiones en la intención **None**. Es necesario que tenga expresiones que no desea que la aplicación conteste, por ello debe incluir expresiones en la intención **None**. No la deje en blanco. 

1. Seleccione **Intents** (Intenciones) en el panel izquierdo. 

    [![](media/luis-quickstart-intent-and-hier-entity/select-intents-from-bookflight-intent.png "Captura de pantalla de la página de la intención BookFlight con el botón Intents (Intenciones) resaltado")](media/luis-quickstart-intent-and-hier-entity/select-intents-from-bookflight-intent.png#lightbox)

2. Seleccione la intención **None**. Agregue tres expresiones que el usuario podría solicitar pero que no son pertinentes para la aplicación:

    | Expresiones de ejemplo|
    |--|
    |Cancelar|
    |Adiós|
    |¿Qué está pasando?|

## <a name="when-the-utterance-is-predicted-for-the-none-intent"></a>Cuando se predice la expresión de la intención None
En la aplicación que realiza la llamada de LUIS (por ejemplo, un bot de chat), cuando LUIS devuelve la intención **None** para una expresión, el bot puede preguntar al usuario si desea finalizar la conversación. El bot también puede dar más instrucciones para que continúe la conversación si el usuario no desea terminarla. 

Las entidades funcionan en la intención **None**. Si la intención que tiene la puntuación más alta es **None** pero se ha extraído una entidad con significado para el bot de chat, este puede continuar con una pregunta que permita concretar la intención del usuario. 

## <a name="create-a-location-entity-from-the-intent-page"></a>Creación de una entidad de ubicación en la página Intents (Intenciones)
Ahora que las dos intenciones incluyen expresiones, es necesario que LUIS comprenda qué es una ubicación. Vuelva a la intención `BookFlight` y etiquete (marque) el nombre de la ciudad de una expresión mediante los siguientes pasos:

1. Vuelva a la intención `BookFlight` seleccionando **Intents** (Intenciones) en el panel izquierdo.

2. Seleccione `BookFlight` en la lista de intenciones.

3. En la expresión, `Book 2 flights from Seattle to Cairo next Monday`, seleccione la palabra `Seattle`. Aparece un menú desplegable con un cuadro de texto en la parte superior para crear una nueva entidad. Escriba el nombre de entidad `Location` en el cuadro de texto y, a continuación, seleccione **Create new entity** (Crear nueva entidad) en el menú desplegable. 

    [![](media/luis-quickstart-intent-and-hier-entity/label-seattle-in-utterance.png "Captura de pantalla de la página de la intención BookFlight en la que se crea una entidad a partir del texto seleccionado")](media/luis-quickstart-intent-and-hier-entity/label-seattle-in-utterance.png#lightbox)

4. En la ventana emergente, seleccione el tipo de entidad **Hierarchical** con `Origin` y `Destination` como entidades secundarias. Seleccione **Listo**.

    [![](media/luis-quickstart-intent-and-hier-entity/hier-entity-ddl.png "Captura de pantalla del cuadro de diálogo emergente de creación de entidad para la nueva entidad Location (Ubicación)")](media/luis-quickstart-intent-and-hier-entity/hier-entity-ddl.png#lightbox)

    La etiqueta de `Seattle` está marcada como `Location` porque LUIS no sabe si el término era el origen, el destino o ninguno de ellos. Seleccione `Seattle`, luego seleccione la ubicación y, finalmente, vaya el menú de la derecha y seleccione `Origin`.

5. Una vez que se ha creado la entidad y etiquetado una expresión, etiquete las restantes ciudades, para lo que debe seleccionar el nombre de la ciudad, seleccionar Location (Ubicación) e ir al menú de la derecha para seleccionar `Origin` o `Destination`.

    [![](media/luis-quickstart-intent-and-hier-entity/label-destination-in-utterance.png "Captura de pantalla de entidad de Bookflight con el texto de expresión seleccionado para la selección de entidad")](media/luis-quickstart-intent-and-hier-entity/label-destination-in-utterance.png#lightbox)

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS
LUIS no conoce los cambios en las intenciones y entidades (el modelo) hasta que se le entrena. 

1. En la parte superior derecha del sitio web de LUIS, seleccione el botón **Train** (Entrenar).

    ![Entrenamiento de la aplicación](./media/luis-quickstart-intent-and-hier-entity/train-button.png)

2. El entrenamiento se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

    ![El entrenamiento se realizó correctamente](./media/luis-quickstart-intent-and-hier-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión
Para obtener una predicción de LUIS en un bot de chat u otra aplicación, tiene que publicar la aplicación. 

1. En la parte superior derecha del sitio web de LUIS, seleccione el botón **Publish** (Publicar). 

    [![](media/luis-quickstart-intent-and-hier-entity/publish.png "Captura de pantalla de la página de la intención Bookflight con el botón Publish (Publicar) resaltado")](media/luis-quickstart-intent-and-hier-entity/publish.png#lightbox)

2. Seleccione el espacio de producción y el botón **Publish** (Publicar).

    [![](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png "Captura de pantalla de la página Publish (Publicar) con el botón de publicación en el espacio de producción resaltado")](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png#lightbox)

3. La publicación se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consulta del punto de conexión con una expresión diferente
1. En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. 

    [![](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png "Captura de pantalla de la página Publish (Publicar) con la dirección URL del punto de conexión resaltada")](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png#lightbox)

2. Vaya al final de la dirección URL en la dirección y escriba `1 ticket to Portland on Friday`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que cualquiera de las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `BookFlight` con la entidad jerárquica extraída.

```
{
  "query": "1 ticket to Portland on Friday",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.9998226
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 0.9998226
    },
    {
      "intent": "None",
      "score": 0.221926212
    }
  ],
  "entities": [
    {
      "entity": "portland",
      "type": "Location::Destination",
      "startIndex": 12,
      "endIndex": 19,
      "score": 0.564448953
    }
  ]
}
```

## <a name="what-has-this-luis-app-accomplished"></a>¿Qué ha logrado esta aplicación de LUIS?
Esta aplicación, con solo dos intenciones y una entidad jerárquica, ha identificado una intención de consulta en lenguaje natural y ha devuelto los datos extraídos. 

El bot de chat ahora tiene suficiente información para determinar la acción principal, `BookFlight`, y la información de ubicación encontrada en la expresión. 

## <a name="where-is-this-luis-data-used"></a>¿Dónde se utilizan estos datos de LUIS? 
LUIS ha terminado con esta solicitud. La aplicación que realiza la llamada, como un bot de chat, puede tomar el resultado de topScoringIntent y los datos de la entidad para realizar el siguiente paso. LUIS no realiza este trabajo de programación para el bot o la aplicación que realiza la llamada. LUIS solo determina cuál es la intención del usuario. 

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no sea necesaria, elimine la aplicación de LUIS. Para ello, seleccione el menú de tres puntos (...) a la derecha del nombre de la aplicación en la lista de aplicaciones y seleccione **Delete** (Eliminar). En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), seleccione **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Aprenda cómo agregar una entidad de lista](luis-quickstart-intent-and-list-entity.md) 

Agregue el **número** de la [entidad creada previamente](luis-how-to-add-entities.md#add-prebuilt-entity) para extraer el número. 

Agregue **datetimeV2** de la [entidad creada previamente](luis-how-to-add-entities.md#add-prebuilt-entity) para extraer la información de fecha.


<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
