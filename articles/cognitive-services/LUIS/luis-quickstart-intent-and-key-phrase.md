---
title: 'Tutorial para crear una aplicación de LUIS que devuelve frases clave: Azure | Microsoft Docs'
description: En este tutorial, aprenderá cómo agregar y devolver la entidad keyPhrase a su aplicación de LUIS para analizar los temas principales de las expresiones.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 12c306b5199da5862302c28d1690b81c6e1edb0e
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264622"
---
# <a name="tutorial-create-app-that-returns-keyphrases-entity-data-found-in-utterances"></a>Tutorial: Creación una aplicación que devuelve datos de entidad de KeyPhrases que se encuentran en expresiones
En este tutorial, va a crear una aplicación que muestra cómo los temas principales de expresiones.

<!-- green checkmark -->
> [!div class="checklist"]
> * Descripción de las entidades keyPhrase 
> * Creación de una nueva aplicación de LUIS para el dominio de recursos humanos
> * Adición de la intención _None_ y adición de expresiones de ejemplo
> * Adición de la entidad keyPhrase para extraer contenido de una expresión
> * Entrenamiento y publicación de la aplicación
> * Consulta del punto de conexión de la aplicación para ver la respuesta JSON de LUIS

Para este artículo, puede usar la cuenta de [LUIS][LUIS] gratuita para crear la aplicación de LUIS.

## <a name="keyphrase-entity-extraction"></a>Extracción de entidades keyPhrase
La entidad predefinida **keyPhrase** proporciona los temas principales. Esta entidad devuelve el tema principal de la expresión.

Las expresiones siguientes muestran ejemplos de frases clave:

|Expresión|Valores de la entidad KeyPhrase|
|--|--|
|¿Se ofrecerá el próximo año un nuevo plan médico con una franquicia menor?|"franquicia menor"<br>"nuevo plan médico"<br>"año"|
|¿El plan médico con franquicia alta incluye tratamiento de la visión?|"plan médico con franquicia alta"<br>"tratamiento de la visión"|

Su bot de chat puede considerar estos valores, además de todas las demás entidades extraídas, a la hora de decidir el siguiente paso de la conversación.

## <a name="download-sample-app"></a>Descargar aplicación de ejemplo
Descargue la aplicación [Human Resources](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/HumanResources.json) y guárdela en un archivo con la extensión *.json. Esta aplicación de ejemplo reconoce expresiones pertinentes para prestaciones de los empleados, organigramas y recursos físicos.

## <a name="create-a-new-app"></a>Creación de una nueva aplicación
1. Inicie sesión en el sitio web de [LUIS][LUIS]. Asegúrese de que inicia sesión en la [región][LUIS-regions] en la que necesita publicar los puntos de conexión de LUIS.

2. En el sitio web de [LUIS][LUIS], seleccione **Import new app** (Importar nueva aplicación) para importar la aplicación Human Resources que descargó en la sección anterior. 

    [![](media/luis-quickstart-intent-and-key-phrase/app-list.png "Captura de pantalla de la página de lista de aplicaciones")](media/luis-quickstart-intent-and-key-phrase/app-list.png#lightbox)

3. En el cuadro de diálogo **Import new app** (Importar nueva aplicación), asigne a la aplicación el nombre `Human Resources with Key Phrase entity`. 

    ![Imagen del cuadro de diálogo Create new app (Crear nueva aplicación)](./media/luis-quickstart-intent-and-key-phrase/import-new-app-inline.png)

    Cuando se complete el proceso de creación de la aplicación, LUIS mostrará la lista de intenciones.

    [![](media/luis-quickstart-intent-and-key-phrase/intents-list.png "Captura de pantalla de la página de lista de intenciones")](media/luis-quickstart-intent-and-key-phrase/intents-list.png#lightbox)

## <a name="add-keyphrase-entity"></a>Adición de la entidad KeyPhrase 
Agregue la entidad predefinida keyPhrase para extraer los temas principales de las expresiones.

1. Seleccione **Entities** (Entidades) en el menú izquierdo.

    [ ![Captura de pantalla de entidades resaltado en la barra de navegación izquierda de la sección Build (Compilar)](./media/luis-quickstart-intent-and-key-phrase/select-entities.png)](./media/luis-quickstart-intent-and-key-phrase/select-entities.png#lightbox)

2. Seleccione **Manage prebuilt entities** (Administrar entidades predefinidas).

    [ ![Captura de pantalla del cuadro de diálogo emergente con la lista de entidades](./media/luis-quickstart-intent-and-key-phrase/manage-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/manage-prebuilt-entities.png#lightbox)

3. En el cuadro de diálogo emergente, seleccione **keyphrase** y, a continuación, seleccione **Done** (Listo). 

    [ ![Captura de pantalla del cuadro de diálogo emergente con la lista de entidades](./media/luis-quickstart-intent-and-key-phrase/add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/add-or-remove-prebuilt-entities.png#lightbox)

    <!-- TBD: asking Carol
    You won't see these entities labeled in utterances on the intents pages. 
    -->

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS
LUIS no sabe nada acerca de este cambio en el modelo hasta que se entrena. 

1. En la parte superior derecha del sitio web de LUIS, seleccione el botón **Train** (Entrenar).

    ![Captura de pantalla del botón Train (Entrenar) resaltado](./media/luis-quickstart-intent-and-key-phrase/train-button-expanded.png)

2. El entrenamiento se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

    ![Captura de pantalla de la barra de notificaciones de entrenamiento correcto ](./media/luis-quickstart-intent-and-key-phrase/trained-inline.png)

## <a name="publish-app-to-endpoint"></a>Publicación de la aplicación en un punto de conexión

1. Seleccione **Publish** (Publicar) en el panel de navegación superior derecho.

    ![Captura de pantalla de la página Entity (Entidad) con el botón Publish (Publicar) expandido ](./media/luis-quickstart-intent-and-key-phrase/publish-expanded.png)

2. Seleccione el espacio de producción y el botón **Publish** (Publicar).

    [![](media/luis-quickstart-intent-and-key-phrase/publish-to-production-inline.png "Captura de pantalla de la página Publish (Publicar) con el botón de publicación en el espacio de producción resaltado")](media/luis-quickstart-intent-and-key-phrase/publish-to-production-expanded.png#lightbox)

3. La publicación se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

## <a name="query-the-endpoint-with-an-utterance"></a>Consulta del punto de conexión con una expresión

1. En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. 

    ![Captura de pantalla de la página Publish (Publicar) con la dirección URL del punto de conexión resaltada](media/luis-quickstart-intent-and-key-phrase/endpoint-url-inline.png )

2. Vaya al final de la dirección URL en la dirección y escriba `Is there a new medical plan with a lower deductible offered next year?`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. 

```
{
  "query": "Is there a new medical plan with a lower deductible offered next year?",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.216838628
  },
  "entities": [
    {
      "entity": "lower deductible",
      "type": "builtin.keyPhrase",
      "startIndex": 35,
      "endIndex": 50
    },
    {
      "entity": "new medical plan",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 26
    },
    {
      "entity": "year",
      "type": "builtin.keyPhrase",
      "startIndex": 65,
      "endIndex": 68
    }
  ]
}
```

## <a name="what-has-this-luis-app-accomplished"></a>¿Qué ha logrado esta aplicación de LUIS?
Esta aplicación con detección de entidades habilitada ha identificado una intención de consulta en lenguaje natural y ha devuelto los datos extraídos, incluido el tema principal. 

Su bot de chat ya tiene ahora suficiente información para determinar el siguiente paso de la conversación. 

## <a name="where-is-this-luis-data-used"></a>¿Dónde se utilizan estos datos de LUIS? 
LUIS ha terminado con esta solicitud. La aplicación que realiza la llamada, como un bot de chat, puede tomar el resultado de topScoringIntent y los datos de keyPhrase de la expresión para realizar el siguiente paso. LUIS no realiza este trabajo de programación para el bot o la aplicación que realiza la llamada. LUIS solo determina cuál es la intención del usuario. 

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no sea necesaria, elimine la aplicación de LUIS. Para ello, seleccione el menú de tres puntos (...) a la derecha del nombre de la aplicación en la lista de aplicaciones y seleccione **Delete** (Eliminar). En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), seleccione **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una aplicación que devuelve opiniones junto con la predicción de intenciones](luis-quickstart-intent-and-sentiment-analysis.md)

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
