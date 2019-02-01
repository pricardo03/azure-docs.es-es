---
title: 'Flujos de trabajo de moderación de contenido de la consola de API: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Use las operaciones de flujo de trabajo en Azure Content Moderator para crear o actualizar un flujo de trabajo u obtener detalles de un flujo de trabajo mediante Review API.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: c71358828be1c5a2a83092f8079eca37c9e757dd
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207214"
---
# <a name="workflows-from-the-api-console"></a>Flujos de trabajo de la consola de API

Use las [operaciones de flujo de trabajo](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) en Azure Content Moderator para crear o actualizar un flujo de trabajo u obtener detalles de un flujo de trabajo mediante Review API. Puede definir expresiones simples, complejas e incluso anidadas para los flujos de trabajo mediante esta API. Los flujos de trabajo aparecen en la herramienta de revisión para que su equipo los use. Los flujos de trabajo también se utilizan en las operaciones de trabajo de Review API.

## <a name="prerequisites"></a>Requisitos previos

1. Vaya a la [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/). Suscríbase, si aún no lo ha hecho. 
2. En la herramienta de revisión, en **Configuración**, seleccione la pestaña **Flujos de trabajo** tal y como se indica en el [tutorial sobre flujos de trabajo](Review-Tool-User-Guide/Workflows.md) de la herramienta de revisión.

### <a name="browse-to-the-workflows-screen"></a>Desplazamiento hasta la pantalla de flujos de trabajo

En el panel de Content Moderator, seleccione **Revisar** > **Configuración** > **Flujos de trabajo**. Puede ver un flujo de trabajo predeterminado.

  ![Flujo de trabajo predeterminado](images/default-workflow-listed.PNG)

### <a name="get-the-json-definition-of-the-default-workflow"></a>Obtención de la definición JSON del flujo de trabajo predeterminado

Seleccione la opción **Editar** del flujo de trabajo y, a continuación, seleccione la pestaña **JSON**. Puede ver la siguiente expresión JSON:

    {
        "Type": "Logic",
        "If": {
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

## <a name="get-workflow-details"></a>Obtención de detalles del flujo de trabajo

Use la operación **Workflow - Get** (Flujo de trabajo: Obtener) para obtener detalles del flujo de trabajo predeterminado existente.

En la herramienta de revisión, vaya a la sección [Credenciales](Review-Tool-User-Guide/credentials.md#the-review-tool).

### <a name="browse-to-the-api-reference"></a>Desplazamiento hasta la referencia de API

1. En la vista **Credenciales**, seleccione [Referencia de la API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59). 
2. Cuando se abra la página **Workflow - Create Or Update** (Flujo de trabajo: Crear o actualizar), vaya a la referencia de [Workflow - Get](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b44b3f9b0711b43c4c58) (Flujo de trabajo: Obtener).

### <a name="select-your-region"></a>Seleccionar la región

En **Open API testing console** (Abrir consola de pruebas de API), seleccione la región que mejor describa su ubicación.

  ![Selección de región para Workflow - Get (Flujo de trabajo: Obtener)](images/test-drive-region.png)

  Se abre la consola de API de **Workflow - Get** (Flujo de trabajo: Obtener).

### <a name="enter-parameters"></a>Escribir parámetros

Especifique los valores para **team**, **workflowName** y **Ocp-Apim-Subscription-Key** (la clave de la suscripción):

- **team**: Nombre del equipo que creó al configurar la [cuenta de la herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: Nombre del flujo de trabajo. Mediante `default`.
- **Ocp-Apim-Subscription-Key**: Ubicada en la pestaña **Configuración**. Para más información, consulte [Información general](overview.md).

  ![Obtención de parámetros y encabezados de consulta](images/workflow-get-default.PNG)

### <a name="submit-your-request"></a>Enviar la solicitud
  
Seleccione **Enviar**. Si la operación se realiza correctamente, el **estado de la respuesta** es `200 OK` y aparece el cuadro **Contenido de la respuesta** que muestra el siguiente flujo de trabajo JSON:

    {
        "Name": "default",
        "Description": "Default",
        "Type": "Image",
        "Expression": {
        "If": {
            "ConnectorName": "moderator",
            "OutputName": "isadult",
            "Operator": "eq",
            "Value": "true",
            "AlternateInput": null,
            "Type": "Condition"
            },
        "Then": {
            "Perform": [{
                "Name": "createreview",
                "Subteam": null,
                "CallbackEndpoint": null,
                "Tags": []
            }],
            "Type": "Actions"
            },
            "Else": null,
            "Type": "Logic"
            }
    }


## <a name="create-a-workflow"></a>Creación de un flujo de trabajo

En la herramienta de revisión, vaya a la sección [Credenciales](Review-Tool-User-Guide/credentials.md#the-review-tool).

### <a name="browse-to-the-api-reference"></a>Desplazamiento hasta la referencia de API

En la vista **Credenciales**, seleccione [Referencia de la API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59). Se abre la página **Workflow - Create Or Update** (Flujo de trabajo: Crear o actualizar).

### <a name="select-your-region"></a>Seleccionar la región

En **Open API testing console** (Abrir consola de pruebas de API), seleccione la región que mejor describa su ubicación.

  ![Selección de región para Workflow - Create Or Update (Flujo de trabajo: Crear o actualizar)](images/test-drive-region.png)

  Se abre la consola de API de **Workflow - Create Or Update** (Flujo de trabajo: Crear o actualizar).

### <a name="enter-parameters"></a>Escribir parámetros

Especifique los valores para **team**, **workflowName** y **Ocp-Apim-Subscription-Key** (la clave de la suscripción):

- **team**: Nombre del equipo que creó al configurar la [cuenta de la herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: El nombre del nuevo flujo de trabajo.
- **Ocp-Apim-Subscription-Key**: Ubicada en la pestaña **Configuración**. Para más información, consulte [Información general](overview.md).

  ![Parámetros y encabezados de consulta de la consola de Workflow - Create Or Update (Flujo de trabajo: Crear o actualizar)](images/workflow-console-parameters.PNG)

### <a name="enter-the-workflow-definition"></a>Especificación de la definición del flujo de trabajo

1. Edite el cuadro de diálogo **Cuerpo de la solicitud** para especificar la solicitud JSON con la información para **Descripción** y **Tipo** (imagen o texto). 
2. Para **Expresión**, copie la expresión del flujo de trabajo predeterminado de la sección anterior, tal y como se muestra aquí:

        {
            "Description": "Default workflow from API console",
            "Type": "Image",
            "Expression": 
                // Copy the default workflow expression from the preceding section
        }

    El cuerpo de la solicitud se parece a la siguiente solicitud JSON:

        {
            "Description": "Default workflow from API console",
            "Type": "Image",
            "Expression": {
                "Type": "Logic",
                "If": {
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
                    "Tags": [ ]
                }
                ],
                "Type": "Actions"
                }
            }
        }
 
### <a name="submit-your-request"></a>Enviar la solicitud
  
Seleccione **Enviar**. Si la operación se realiza correctamente, el **estado de la respuesta** es `200 OK` y el cuadro **Contenido de la respuesta** muestra `true`.

### <a name="check-out-the-new-workflow"></a>Comprobación del nuevo flujo de trabajo

En la herramienta de revisión, seleccione **Revisión** > **Configuración** > **Flujos de trabajo**. El nuevo flujo de trabajo aparece y está listo para usarse.

  ![Lista de flujos de trabajo de la herramienta de revisión](images/workflow-console-new-workflow.PNG)
  
### <a name="review-your-new-workflow-details"></a>Revisión de los detalles del nuevo flujo de trabajo

1. Seleccione la opción **Editar** del flujo de trabajo y, a continuación, seleccione las pestañas **Diseñador** y **JSON**.

   ![Pestaña Diseñador para un flujo de trabajo seleccionado](images/workflow-console-new-workflow-designer.PNG)

2. Para ver la vista JSON del flujo de trabajo, seleccione la pestaña **JSON**.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener ejemplos de flujos de trabajo más complejos, consulte la [introducción a los flujos de trabajo](workflow-api.md).
* Aprenda a usar flujos de trabajo con [trabajos de moderación de contenido](try-review-api-job.md).
