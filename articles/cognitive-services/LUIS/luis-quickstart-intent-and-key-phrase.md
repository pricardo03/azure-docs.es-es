---
title: 'Tutorial para crear una aplicación de LUIS que devuelve frases clave: Azure | Microsoft Docs'
description: En este tutorial, aprenderá cómo agregar y devolver la entidad keyPhrase a su aplicación de LUIS para analizar los temas principales de las expresiones.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/27/2018
ms.author: v-geberr
ms.openlocfilehash: ccefb4c2890d74978f340778cfab7cad979c9802
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929566"
---
# <a name="tutorial-8-add-keyphrase-entity"></a>Tutorial: 8. Adición de la entidad KeyPhrase 
En este tutorial, va a usar una aplicación que muestra cómo extraer los temas principales de las expresiones.

<!-- green checkmark -->
> [!div class="checklist"]
> * Descripción de las entidades keyPhrase 
> * Uso de la aplicación de LUIS en el dominio de recursos humanos (RR. HH.) 
> * Adición de la entidad keyPhrase para extraer contenido de una expresión
> * Entrenamiento y publicación de la aplicación
> * Consulta del punto de conexión de la aplicación para ver la respuesta JSON de LUIS que incluye elementos las frases principales

Para este artículo, puede usar la cuenta de [LUIS](luis-reference-regions.md#publishing-regions) gratuita para crear la aplicación de LUIS.

## <a name="before-you-begin"></a>Antes de empezar
Si no tiene la aplicación de recursos humanos del tutorial de [entidades simples](luis-quickstart-primary-and-secondary-data.md), [importe](luis-how-to-start-new-app.md#import-new-app) el archivo JSON en una nueva aplicación en el sitio web de [LUIS](luis-reference-regions.md#luis-website). La aplicación que se va a importar se encuentra en el repositorio de GitHub [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-simple-HumanResources.json).

Si desea conservar la aplicación original de Recursos humanos, clone la versión en la página [Configuración](luis-how-to-manage-versions.md#clone-a-version) y llámela `keyphrase`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. 

## <a name="keyphrase-entity-extraction"></a>Extracción de entidades keyPhrase
La entidad predefinida, **keyPhrase**, proporciona los temas principales. Esta entidad devuelve el tema principal de la expresión.

Las expresiones siguientes muestran ejemplos de frases clave:

|Expresión|Valores de la entidad KeyPhrase|
|--|--|
|¿Se ofrecerá el próximo año un nuevo plan médico con una franquicia menor?|"franquicia menor"<br>"nuevo plan médico"<br>"año"|
|¿El plan médico con franquicia alta incluye tratamiento de la visión?|"plan médico con franquicia alta"<br>"tratamiento de la visión"|

La aplicación cliente puede utilizar estos valores, junto con otras entidades extraídas, para decidir el siguiente paso de la conversación.

## <a name="add-keyphrase-entity"></a>Adición de la entidad KeyPhrase 
Agregue la entidad predefinida keyPhrase para extraer los temas principales de las expresiones.

1. Asegúrese de que la aplicación de recursos humanos se encuentra en la sección **Build** (Crear) de LUIS. Para cambiar a esta sección, seleccione **Build** (Crear) en la barra de menús superior derecha. 

    [ ![Captura de pantalla de la aplicación de LUIS con Build (Crear) resaltado en la barra de navegación superior derecha](./media/luis-quickstart-intent-and-key-phrase/hr-first-image.png)](./media/luis-quickstart-intent-and-key-phrase/hr-first-image.png#lightbox)

2. Seleccione **Entities** (Entidades) en el menú izquierdo.

    [ ![Captura de pantalla de entidades resaltado en la barra de navegación izquierda de la sección Build (Compilar)](./media/luis-quickstart-intent-and-key-phrase/hr-select-entities-button.png)](./media/luis-quickstart-intent-and-key-phrase/hr-select-entities-button.png#lightbox)

3. Seleccione **Manage prebuilt entities** (Administrar entidades predefinidas).

    [ ![Captura de pantalla del cuadro de diálogo emergente con la lista de entidades](./media/luis-quickstart-intent-and-key-phrase/hr-manage-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-manage-prebuilt-entities.png#lightbox)

4. En el cuadro de diálogo emergente, seleccione **keyphrase** y, a continuación, seleccione **Done** (Listo). 

    [ ![Captura de pantalla del cuadro de diálogo emergente con la lista de entidades](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png#lightbox)

    <!-- TBD: asking Carol
    You won't see these entities labeled in utterances on the intents pages. 
    -->
5. Seleccione **Intents** (Intenciones) en el menú izquierdo y, después, seleccione la intención **Utilities.Confirm**. La entidad keyPhrase está etiquetada en varias expresiones. 

    [ ![Captura de pantalla de la intención Utilities.Confirm intención con varias entidades keyPhrase etiquetadas en expresiones](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png)](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png#lightbox)

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS
La nueva versión de `keyphrase` versión de la aplicación debe recibir entrenamiento.  

1. En la parte superior derecha del sitio web de LUIS, seleccione el botón **Train** (Entrenar).

    ![Entrenamiento de la aplicación](./media/luis-quickstart-intent-and-key-phrase/train-button.png)

2. El entrenamiento se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

    ![El entrenamiento se realizó correctamente](./media/luis-quickstart-intent-and-key-phrase/trained.png)

## <a name="publish-app-to-endpoint"></a>Publicación de la aplicación en un punto de conexión

1. Seleccione **Publish** (Publicar) en el panel de navegación superior derecho.

    [![](media/luis-quickstart-intent-and-key-phrase/hr-publish-button-top-nav.png "Captura de pantalla de la página Publish (Publicar) con el botón de publicación en el espacio de producción resaltado")](media/luis-quickstart-intent-and-key-phrase/hr-publish-button-top-nav.png#lightbox)

2. Seleccione el espacio de producción y presione el botón **Publicar**.

    [![](media/luis-quickstart-intent-and-key-phrase/hr-publish-to-production-expanded.png "Captura de pantalla de la página Publish (Publicar) con el botón de publicación en el espacio de producción resaltado")](media/luis-quickstart-intent-and-key-phrase/hr-publish-to-production-expanded.png#lightbox)

3. La publicación se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

## <a name="query-the-endpoint-with-an-utterance"></a>Consulta del punto de conexión con una expresión

1. En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. 

    ![Captura de pantalla de la página Publish (Publicar) con la dirección URL del punto de conexión resaltada](media/luis-quickstart-intent-and-key-phrase/hr-endpoint-url-inline.png )

2. Vaya al final de la dirección URL en la dirección y escriba `does form hrf-123456 cover the new dental benefits and medical plan`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. 

```
{
  "query": "does form hrf-123456 cover the new dental benefits and medical plan",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.9300641
  },
  "intents": [
    {
      "intent": "FindForm",
      "score": 0.9300641
    },
    {
      "intent": "ApplyForJob",
      "score": 0.0359598845
    },
    {
      "intent": "GetJobInformation",
      "score": 0.0141798034
    },
    {
      "intent": "MoveEmployee",
      "score": 0.0112197418
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00507669244
    },
    {
      "intent": "None",
      "score": 0.00238501839
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00202810857
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00102957746
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.0008688423
    },
    {
      "intent": "Utilities.Confirm",
      "score": 3.557994E-05
    }
  ],
  "entities": [
    {
      "entity": "hrf-123456",
      "type": "HRF-number",git 
      "startIndex": 10,
      "endIndex": 19
    },
    {
      "entity": "new dental benefits",
      "type": "builtin.keyPhrase",
      "startIndex": 31,
      "endIndex": 49
    },
    {
      "entity": "medical plan",
      "type": "builtin.keyPhrase",
      "startIndex": 55,
      "endIndex": 66
    },
    {
      "entity": "hrf",
      "type": "builtin.keyPhrase",
      "startIndex": 10,
      "endIndex": 12
    },
    {
      "entity": "-123456",
      "type": "builtin.number",
      "startIndex": 13,
      "endIndex": 19,
      "resolution": {
        "value": "-123456"
      }
    }
  ]
}
```

Al buscar un formulario, el usuario ha proporcionado más información de la necesaria. La información adicional se devuelve como **builtin.keyPhrase**. La aplicación cliente puede utilizar esta información adicional para una pregunta de seguimiento, como "¿Desea para hablar con un representante de recursos humanos acerca de los nuevos beneficios en salud dental" o proporcionar un menú con más opciones, entre las que se incluya, "Más información acerca de los nuevos beneficios en salud dental o del seguro médico".

## <a name="what-has-this-luis-app-accomplished"></a>¿Qué ha logrado esta aplicación de LUIS?
Esta aplicación con detección de entidades habilitada ha identificado una intención de consulta en lenguaje natural y ha devuelto los datos extraídos, incluido el tema principal. 

Su bot de chat ya tiene ahora suficiente información para determinar el siguiente paso de la conversación. 

## <a name="where-is-this-luis-data-used"></a>¿Dónde se utilizan estos datos de LUIS? 
LUIS ha terminado con esta solicitud. La aplicación que realiza la llamada, como un bot de chat, puede tomar el resultado de topScoringIntent y los datos de keyPhrase de la expresión para realizar el siguiente paso. LUIS no realiza este trabajo de programación para el bot o la aplicación que realiza la llamada. LUIS solo determina cuál es la intención del usuario. 

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no sea necesaria, elimine la aplicación de LUIS. Seleccione **My apps** (Mis aplicaciones) en el menú superior izquierdo. Seleccione el botón de puntos suspensivos (***...***) a la derecha del nombre de la aplicación en la lista de aplicaciones y, después, seleccione **Delete** (Eliminar). En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), seleccione **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adición de análisis de opiniones](luis-quickstart-intent-and-sentiment-analysis.md)