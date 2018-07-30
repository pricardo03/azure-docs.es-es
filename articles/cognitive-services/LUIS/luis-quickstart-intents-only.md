---
title: 'Creación de una aplicación simple con dos intenciones: Azure | Microsoft Docs'
description: Obtenga información sobre cómo crear una aplicación simple de LUIS con dos intenciones y sin identidades para identificar expresiones de usuario en este inicio rápido.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 06/27/2018
ms.author: v-geberr
ms.openlocfilehash: 0668ba050a6918995deb42d8feea5afbbab3b010
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865818"
---
# <a name="tutorial-1-build-app-with-custom-domain"></a>Tutorial: 1. Compilación de aplicación con dominio personalizado
En este tutorial, cree una aplicación que muestre cómo usar las **intenciones** para determinar la _intención_ del usuario en función de la expresión (texto) que envía a la aplicación. Cuando termine, tendrá un punto de conexión LUIS en ejecución en la nube.

Esta aplicación es el tipo más simple de aplicación LUIS porque no extrae datos a partir de las expresiones. Solo determina la intención de la expresión del usuario.

<!-- green checkmark -->
> [!div class="checklist"]
> * Compilar una aplicación nueva para un dominio de recursos humanos (RR. HH.) 
> * Agregar la intención GetJobInformation
> * Agregar expresiones de ejemplo a la intención GetJobInformation 
> * Entrenar y publicar la aplicación
> * Consultar el punto de conexión de la aplicación para ver la respuesta JSON de LUIS
> * Agregar la intención ApplyForJob
> * Agregar expresiones de ejemplo a la intención ApplyForJob 
> * Entrenar, publicar y consultar nuevamente el punto de conexión 

Para este artículo, necesita una cuenta gratuita de [LUIS](luis-reference-regions.md#luis-website) para crear la aplicación.

## <a name="purpose-of-the-app"></a>Propósito de la aplicación
Esta aplicación tiene algunas intenciones. La primera intención, **`GetJobInformation`**, identifica cuando un usuario quiere información sobre los trabajos disponibles dentro de una empresa. La segunda intención, **`None`**, identifica cualquier otro tipo de expresión. Más adelante en la guía de inicio rápido se agrega una tercera intención, `ApplyForJob`. 

## <a name="create-a-new-app"></a>Creación de una nueva aplicación
1. Inicie sesión en el sitio web de [LUIS](luis-reference-regions.md#luis-website). Asegúrese de que inicia sesión en la [región](luis-reference-regions.md#publishing-regions) en la que necesita publicar los puntos de conexión de LUIS.

2. En el sitio web de [LUIS](luis-reference-regions.md#luis-website), seleccione **Create new app** (Crear aplicación).  

    [![](media/luis-quickstart-intents-only/app-list.png "Captura de pantalla de la página My Apps (Mis aplicaciones)")](media/luis-quickstart-intents-only/app-list.png#lightbox)

3. En el cuadro de diálogo emergente, escriba el nombre `HumanResources`. Esta aplicación abarca preguntas sobre el departamento de Recursos Humanos de la empresa. Ese tipo de departamento controla los asuntos relacionados con el empleo, como los puestos de trabajo que se deben rellenar en la empresa.

    ![Nueva aplicación de LUIS](./media/luis-quickstart-intents-only/create-app.png)

4. Cuando el proceso finalice, la aplicación mostrará la página **Intents** (Intenciones) con la intención **None**. 

    [![](media/luis-quickstart-intents-only/intents-list.png "Captura de pantalla de la página Intents list (Lista de intenciones)")](media/luis-quickstart-intents-only/intents-list.png#lightbox)

## <a name="create-getjobinformation-intention"></a>Creación de la intención GetJobInformation
1. Seleccione **Create new intent** (Crear nueva intención). Escriba el nombre de la intención nueva `GetJobInformation`. Esta intención se predice cada vez que un usuario quiere información sobre las ofertas de trabajo en la empresa.

    ![](media/luis-quickstart-intents-only/create-intent.png "Captura de pantalla del cuadro de diálogo de intención nueva")

    Al crear una intención, se crea una categoría de información que desea identificar. Dar un nombre a la categoría permite que otras aplicaciones que usen los resultados de consulta de LUIS puedan usar ese nombre de categoría para encontrar una respuesta apropiada. LUIS no responde a estas preguntas, solo identifica el tipo de información que se solicita en lenguaje natural. 

2. Agregue siete expresiones a esta intención que espera que un usuario pida, por ejemplo:

    | Expresiones de ejemplo|
    |--|
    |Any new jobs posted today? (¿Se publicó algún trabajo nuevo hoy?)|
    |What positions are available for Senior Engineers? (¿Qué puestos hay disponibles para ingenieros de alto nivel?)|
    |Is there any work in databases? (¿Hay algún trabajo con bases de datos?)|
    |Looking for a new situation with responsibilities in accounting (Busco una nueva situación con responsabilidades en contabilidad)|
    |Where is the job listings (¿Dónde están las listas de vacantes?)|
    |New jobs? (¿Trabajos nuevos?)|
    |Are there any new positions in the Seattle office? (¿Hay algún puesto nuevo en la oficina de Seattle?)|

    [![](media/luis-quickstart-intents-only/utterance-getstoreinfo.png "Captura de pantalla del ingreso de expresiones nuevas para la intención MyStore")](media/luis-quickstart-intents-only/utterance-getstoreinfo.png#lightbox)

3. La aplicación de LUIS no tiene actualmente expresiones en la intención **None**. Necesita expresiones que la aplicación no responde. No la deje en blanco. Seleccione **Intents** (Intenciones) en el panel izquierdo. 

4. Seleccione la intención **None**. Agregue tres expresiones que el usuario podría solicitar pero que no son pertinentes para la aplicación. Si la aplicación está relacionada con los puestos de trabajo, algunas expresiones del tipo **None** que puede usar son las siguientes:

    | Expresiones de ejemplo|
    |--|
    |Barking dogs are annoying (Los perros que ladran son molestos)|
    |Order a pizza for me (Pídanme una pizza)|
    |Penguins in the ocean (Pingüinos en el océano)|

    En la aplicación que realiza la llamada de LUIS, por ejemplo, un bot de chat, si LUIS devuelve la intención **None** para una expresión, el bot puede preguntar al usuario si desea finalizar la conversación. El bot de chat también puede dar más instrucciones para que continúe la conversación si el usuario no desea terminarla. 

## <a name="train-and-publish-the-app"></a>Entrenamiento y publicación de la aplicación
1. En la parte superior derecha del sitio web de LUIS, seleccione el botón **Train** (Entrenar). 

    ![Botón Train (Entrenar)](./media/luis-quickstart-intents-only/train-button.png)

    El entrenamiento se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

    ![Barra de estado que indica que el entrenamiento se realizó](./media/luis-quickstart-intents-only/trained.png)

2. En la parte superior derecha del sitio web de LUIS, seleccione el botón **Publish** (Publicar) para abrir la página Publish (Publicar). El espacio de producción se selecciona de manera predeterminada. Seleccione el botón **Publish** (Publicar) según la selección del espacio de producción. La publicación se completa cuando ve la barra de estado verde en la parte superior del sitio web que confirma que se ha realizado correctamente.

    No es necesario crear una clave de punto de conexión de LUIS en Azure Portal antes de publicar o de probar la dirección URL del punto de conexión. Cada aplicación de LUIS tiene una clave de inicio gratis para la creación. Ofrece una creación ilimitada y [algunas visitas del punto de conexión](luis-boundaries.md#key-limits). 

## <a name="query-endpoint-for-getjobinformation-intent"></a>Consulta del punto de conexión para la intención GetJobInformation
1. En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. Esta acción abre otra ventana del explorador con la dirección URL del punto de conexión en la barra de direcciones. 

2. Vaya al final de la dirección URL en la dirección y escriba `I'm looking for a job with Natual Language Processing`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que ninguna de las expresiones de ejemplo del paso 4, por lo que es una buena prueba y debería devolver la intención `GetJobInformation` como la intención con la puntuación más alta. 

    ```
    {
      "query": "I'm looking for a job with Natual Language Processing",
      "topScoringIntent": {
        "intent": "GetJobInformation",
        "score": 0.8965092
      },
      "intents": [
        {
          "intent": "GetJobInformation",
          "score": 0.8965092
        },
        {
          "intent": "None",
          "score": 0.147104025
        }
      ],
      "entities": []
    }
    ```

## <a name="create-applyforjob-intention"></a>Creación de la intención ApplyForJob
Vuelva a la pestaña del explorador del sitio web de LUIS y cree una intención nueva para postular a un trabajo.

1. Seleccione **Build** (Compilar) en el menú superior de la derecha para volver a la compilación de aplicaciones.

2. Seleccione **Intents** (Intenciones) en el menú izquierdo.

3. Seleccione **Create new intent** (Crear nueva intención) y escriba el nombre `ApplyForJob`. 

    ![Cuadro de diálogo LUIS para crear una intención](./media/luis-quickstart-intents-only/create-applyforjob-intent.png)

4. Agregue varias expresiones a esta intención que espera que un usuario pida como, por ejemplo:

    | Expresiones de ejemplo|
    |--|
    |I want to apply for the new accounting job. (Quiero postular al nuevo trabajo de contabilidad)|
    |Fill out application for Job 123456 (Rellenar la postulación para el trabajo 123456)|
    |Submit resume for engineering position (Enviar el currículum vítae para el puesto de ingeniero)|
    |This is my c.v. for position 654234 (Este es mi currículum vítae para el puesto 654234)|
    |Job 567890 and my paperwork (Trabajo 567890 y mi documentación)|

    [![](media/luis-quickstart-intents-only/utterance-applyforjob.png "Captura de pantalla del ingreso de expresiones nuevas para la intención ApplyForJob")](media/luis-quickstart-intents-only/utterance-applyforjob.png#lightbox)

    La intención etiquetada aparece en rojo porque actualmente LUIS no está seguro de que la intención esté correcta. Entrenar la aplicación indica a LUIS que las expresiones están en la intención correcta. 

    [Entrene y publique](#train-and-publish-the-app) nuevamente. 

## <a name="query-endpoint-for-applyforjob-intent"></a>Punto de conexión de consulta para la intención ApplyForJob
En la página **Publish** (Publicar), seleccione el vínculo **endpoint** (Punto de conexión) en la parte inferior de la página. En la nueva ventana del explorador, escriba `Can I submit my resume for job 235986` al final de la dirección URL. 

    ```
    {
      "query": "Can I submit my resume for job 235986",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.9166808
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.9166808
        },
        {
          "intent": "GetJobInformation",
          "score": 0.07162977
        },
        {
          "intent": "None",
          "score": 0.0262826588
        }
      ],
      "entities": []
    }
    ```

## <a name="what-has-this-luis-app-accomplished"></a>¿Qué ha logrado esta aplicación de LUIS?
Esta aplicación, con tan solo algunas intenciones, identificó una consulta de lenguaje natural de la misma intención, pero escrita de manera distinta. 

El resultado de JSON identifica la intención que tiene la puntuación más alta. Todas las puntuaciones están comprendidas entre 1 y 0, y la mejor está próxima a 1. La puntuación de las intenciones `GetJobInformation` y `None` son mucho más cercanas a cero. 

## <a name="where-is-this-luis-data-used"></a>¿Dónde se usan estos datos de LUIS? 
LUIS ha terminado con esta solicitud. La aplicación que realiza la llamada, como un bot de chat, puede tomar el resultado topScoringIntent y encontrar información (no almacenada en LUIS) para responder la pregunta o para finalizar la conversación. Estas son opciones programáticas para el bot o la aplicación que realiza la llamada. LUIS no hace ese trabajo. LUIS solo determina cuál es la intención del usuario. 

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no sea necesaria, elimine la aplicación de LUIS. Para hacerlo, seleccione **My apps** (Mis aplicaciones) en el menú superior izquierdo. Seleccione los puntos suspensivos (***...***) a la derecha del nombre de la aplicación en la lista de aplicaciones y, después, seleccione **Eliminar**. En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), seleccione **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Add prebuilt intents and entities to this app](luis-tutorial-prebuilt-intents-entities.md) (Incorporación de entidades e intenciones creadas previamente a esta aplicación)
