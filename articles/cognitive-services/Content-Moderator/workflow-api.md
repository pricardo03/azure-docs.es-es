---
title: 'Flujos de trabajo de moderación: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Use flujos de trabajo con las operaciones Job de Review API para automatizar las revisiones con humanos conforme a sus umbrales y directivas de contenido.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 4abe3a864b798552caa27d07a422a7d67b2e4018
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55221392"
---
# <a name="automate-moderation-reviews-with-workflows"></a>Automatización de revisiones de moderación con flujos de trabajo

Content Moderator incluye herramientas y API para administrar flujos de trabajo. Los flujos de trabajo se usan las [operaciones Job de Review API](review-api.md) para automatizar la creación de revisiones con humanos conforme a sus umbrales y directivas de contenido.

Review API ofrece los siguientes métodos para incluir supervisión humana en el proceso de moderación de contenido:

1. Operaciones **Job**, para iniciar la moderación asistida por máquina y la creación de revisiones humanas en un solo paso.
1. Operaciones **Review**, para la creación de revisiones humanas fuera del paso de moderación.
1. Operaciones **Workflow**, para administrar flujos de trabajo que automatizan el análisis con umbrales para la creación de revisiones.

Este artículo describe las operaciones **Workflow**. Consulte la introducción a [Job y Review](review-api.md) para más información acerca de la moderación de contenido de trabajos y revisiones.

Usar el flujo de trabajo **default** es la mejor manera de empezar a trabajar con los flujos de trabajo en Content Moderator.

## <a name="your-first-workflow"></a>El primer flujo de trabajo

El primer flujo de trabajo se incluye con su [equipo de la herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/). Suscríbase, si aún no lo ha hecho.

Vaya a la pantalla [Workflows de la herramienta de revisión](Review-Tool-User-Guide/Workflows.md), en la pestaña Settings (Configuración). Verá el flujo de trabajo **default**, como muestra la siguiente imagen:

![Flujos de trabajo de Content Moderator](Review-Tool-User-Guide/images/2-workflows-1.png)

### <a name="open-the-default-workflow"></a>Abra el flujo de trabajo predeterminado

Use la opción **edit** para abrir la página de edición de flujos de trabajo, tal y como muestra la siguiente imagen: ![Flujo de trabajo default de Content Moderator](images/default-workflow-listed.PNG)

### <a name="the-designer-view"></a>Vista del diseñador

Verá la pestaña **Designer** (Diseñador) del flujo de trabajo. La vista del diseñador muestra los siguientes pasos.

1. La **condición** del flujo de trabajo que se va a evaluar. En este caso, el flujo de trabajo llama a la API de imagen de Content Moderator y comprueba si la salida de `isAdult` es igual que `true`.
1. La **acción** que se llevará a cabo si se cumple la condición. En este caso, el flujo de trabajo crea una revisión en la herramienta de revisión si la salida de `isAdult` es `true`.

![Flujo de trabajo default de Content Moderator: diseñador](images/default-workflow-designer.png)

### <a name="the-json-view"></a>Vista JSON

Seleccione la pestaña **JSON** para ver la definición JSON del flujo de trabajo.

    {
        "Type": "Logic",
        If": {
            "ConnectorName": "moderator",
            "OutputName": "isAdult",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
        },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": []
        }
        ],
        "Type": "Actions"
        }
    }

### <a name="key-learning"></a>Aprendizaje clave

En Content Moderator, los flujos de trabajo son fáciles de configurar y flexibles. Si el diseñador integrado no cumple sus requisitos, escriba la definición del flujo de trabajo con el formato **JSON**. A continuación, use la definición JSON con [Workflow API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) para crear y administrar el flujo de trabajo de la aplicación.

## <a name="define-a-custom-workflow"></a>Definición de un flujo de trabajo personalizado

Las funcionalidades de flujo de trabajo de Content Moderator permiten definir y utilizar flujos de trabajo personalizados. Use el artículo de [procedimientos de flujos de trabajo de la herramienta de revisión](Review-Tool-User-Guide/Workflows.md) para definir un flujo de trabajo personalizado. Este flujo de trabajo usa la funcionalidad OCR de Content Moderator para extraer el texto de una imagen de ejemplo. Después, crea una revisión en la herramienta de revisión.

### <a name="the-sample-image"></a>Imagen de ejemplo

Guarde la [imagen de ejemplo](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) en la unidad local. Necesitará esta imagen para el ejercicio.

### <a name="the-designer-view"></a>Vista del diseñador

Seleccione la pestaña **Designer** (Diseñador) y el [tutorial de creación de flujos de trabajo](Review-Tool-User-Guide/Workflows.md) para definir un flujo de trabajo personalizado. La siguiente imagen muestra la vista **Condition** (Condición) del diseñador. Consulte el tutorial para ver el resto de los pasos.

![Content Moderator: condición Workflow](Review-Tool-User-Guide/images/ocr-workflow-step-2-condition.PNG)

### <a name="the-json-view"></a>Vista JSON

Seleccione la pestaña **JSON** para ver la definición JSON del flujo de trabajo personalizado. Observe cómo las instrucciones **If-Then** en la definición JSON se corresponden con los pasos definidos en la vista del diseñador.

    {
        "Type": "Logic",
        "If": {
            "ConnectorName": "moderator",
            "OutputName": "hasText",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
        },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": [
            {
                "Tag": "a",
                "IfCondition": {
                    "ConnectorName": "moderator",
                    "OutputName": "hasText",
                    "Operator": "eq",
                    "Value": "true",
                    "Type": "Condition"
                }
            }
            ]
        }
        ],
        "Type": "Actions"
        }
    }

### <a name="workflow-result"></a>Resultado del flujo de trabajo

Después de probar el flujo de trabajo en la pantalla de flujos de trabajo, se crea la siguiente revisión. Vaya a la pestaña **Image** (Imagen) en **Review** (Revisión) para ver su revisión.
El flujo de trabajo creó la revisión porque la condición principal fue positiva para presencia de texto. La revisión también resaltó la etiqueta **`a`** en la revisión de la imagen.

![Content Moderator: salida de flujo de trabajo simple](images/ocr-sample-image-workflow1.PNG)


## <a name="advanced-workflow-with-combination"></a>Flujo de trabajo avanzado con combinación

### <a name="the-sample-image"></a>Imagen de ejemplo

Use la misma [imagen de ejemplo](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) que usó en la sección anterior.

Sin embargo, esta vez, cambie la condición principal por una combinación de dos comprobaciones. Además de buscar texto, compruebe si el texto incluye palabras soeces. El flujo de trabajo crea una revisión si encuentra texto **y** si detecta palabras soeces en él.

### <a name="the-designer-view"></a>Vista del diseñador

Para cambiar de **Condition** a **Combination**, modifique el flujo de trabajo. La siguiente imagen muestra la nueva vista que se ve en el diseñador.

![Content Moderator: condición de flujo de trabajo modificada](images/ocr-workflow-2-designer.PNG)

### <a name="the-json-view"></a>Vista JSON

Seleccione la pestaña **JSON** para ver la definición JSON del flujo de trabajo modificado. Observe cómo las instrucciones **If-Then** en la definición JSON se corresponden con los pasos nuevo agregados al flujo de trabajo.

    {
        "Type": "Logic",
        "If": {
        "Left": {
            "ConnectorName": "moderator",
            "OutputName": "hasText",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
            },
        "Right": {
            "ConnectorName": "moderator",
            "OutputName": "text.HasProfanity",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition",
            "AlternateInput": "moderator.ocrText"
            },
        "Combine": "AND",
        "Type": "Combine"
        },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": [
            {
                "Tag": "a",
                "IfCondition": {
                    "ConnectorName": "moderator",
                    "OutputName": "hasText",
                    "Operator": "eq",
                    "Value": "true",
                    "Type": "Condition"
                }
            }
            ]
        }
        ],
        "Type": "Actions"
        }
    }

    
### <a name="workflow-result"></a>Resultado del flujo de trabajo

Después de probar el flujo de trabajo de nuevo, verá que no se ha creado ninguna revisión. Para confirmar la ausencia de revisiones, vaya a la pestaña **Image** (Image) en **Review** (Revisión).
El flujo de trabajo no creó la revisión porque no detectó palabras soeces en el texto extraído.

![Content Moderator: salida del flujo de trabajo modificado](images/ocr-workflow-2-result.PNG)


## <a name="the-workflow-api"></a>Workflow API

Las [operaciones Workflow](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) proporcionan la interfaz de programación a las funcionalidades de flujo de trabajo. Para crear flujos de trabajo, obtener detalles de los flujos de trabajo y actualizar las definiciones de los flujos de trabajo, se usa Workflow API.

### <a name="get-all-workflow-details"></a>Obtención de todos los detalles del flujo de trabajo

La operación **Workflow-Get** acepta los siguientes datos de entrada:

- **team**: Nombre del equipo que creó al configurar la [cuenta de la herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: Nombre del flujo de trabajo. Use `default` para comenzar.
- **Ocp-Apim-Subscription-Key**: Ubicada en la pestaña **Configuración**. Para más información, consulte [Información general](overview.md).

Si la operación se realiza correctamente, **Response status** (Estado de la respuesta) es `200 OK` y aparece el cuadro **Response content** (Contenido de la respuesta) que muestra la definición de flujo de trabajo con el formato JSON.
Para obtener más información, lea la [guía de inicio rápido de la consola de Workflow API](try-review-api-job.md).

### <a name="create-or-update-workflow"></a>Creación o actualización del flujo de trabajo

La operación de creación y actualización permite crear flujos de trabajo desde la API.

La operación **Workflow-Create o Workflow-Update** acepta los siguientes datos de entrada:

- **team**: Nombre del equipo que creó al configurar la [cuenta de la herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: Nombre del flujo de trabajo. Use `default` para comenzar.
- **Ocp-Apim-Subscription-Key**: Ubicada en la pestaña **Configuración**. Para más información, consulte [Información general](overview.md).

Si la operación se realiza correctamente, **Response status** (Estado de la respuesta) es `200 OK` y el cuadro **Response content** (Contenido de la respuesta) muestra `true`. Para más información, [pruebe la operación `Create`](try-review-api-job.md).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo crear flujos de trabajo personalizados, consulte el [tutorial sobre flujos de trabajo de la herramienta de revisión](Review-Tool-User-Guide/Workflows.md). 

Pruebe la [consola de Workflow API](try-review-api-job.md) y use los ejemplos de código de la API REST. 

Por último, utilice los flujos de trabajo personalizados con las operaciones **Job** tal y como se muestra en la [consola de Job API](try-review-api-job.md) y en la [guía de inicio rápido de Jobs para .NET](moderation-jobs-quickstart-dotnet.md).
