---
title: Definir flujos de trabajo de moderación con la consola de API de REST - Content Moderator
titlesuffix: Azure Cognitive Services
description: Puede usar las API de revisión de moderador de contenido de Azure para definir flujos de trabajo personalizados y los umbrales según las directivas de contenido.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/14/2019
ms.author: sajagtap
ms.openlocfilehash: e150b1321f2fbd348e737222c752203281503643
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60605900"
---
# <a name="define-and-use-moderation-workflows-rest"></a>Definir y utilizar flujos de trabajo de moderación (REST)

Los flujos de trabajo son filtros personalizados basados en la nube que puede usar para controlar el contenido de forma más eficaz. Los flujos de trabajo pueden conectarse a una variedad de servicios para filtrar el contenido de maneras diferentes y, a continuación, realice la acción apropiada. Esta guía muestra cómo usar la API de REST de flujo de trabajo a través de la consola de API, para crear y usar flujos de trabajo. Una vez que comprenda la estructura de las API, puede portar con facilidad estas llamadas a cualquier plataforma compatible con REST.

## <a name="prerequisites"></a>Requisitos previos

- Iniciar sesión o crear una cuenta de Content Moderator [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/) sitio.

## <a name="create-a-workflow"></a>Creación de un flujo de trabajo

Para crear o actualizar un flujo de trabajo, vaya a la **[flujo de trabajo: crear o actualizar](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59)** API hacen referencia a página y seleccione el botón de su región clave (puede encontrarlo en la dirección URL del extremo en el **credenciales**  página de la [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/)). Esto inicia la consola de API, donde puede crear y ejecutar llamadas API de REST con facilidad.

![Selección de región para Workflow - Create Or Update (Flujo de trabajo: Crear o actualizar)](images/test-drive-region.png)

### <a name="enter-rest-call-parameters"></a>Escriba los parámetros de llamada REST

Especifique los valores de **equipo**, **workflowname**, y **Ocp-Apim-Subscription-Key**:

- **team**: El identificador de equipo que creó al configurar su [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/) cuenta (se encuentra en la **Id** campo en pantalla de credenciales de la herramienta de revisión).
- **workflowname**: El nombre de un nuevo flujo de trabajo para agregar (o un nombre existente, si desea actualizar un flujo de trabajo existente).
- **Ocp-Apim-Subscription-Key**: La clave de Content Moderator. Puede encontrarlo en el **configuración** pestaña de la [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com).

![Parámetros y encabezados de consulta de la consola de Workflow - Create Or Update (Flujo de trabajo: Crear o actualizar)](images/workflow-console-parameters.PNG)

### <a name="enter-a-workflow-definition"></a>Especifique una definición de flujo de trabajo

1. Editar el **cuerpo de la solicitud** cuadro para especificar la solicitud JSON con los detalles de **descripción** y **tipo** (ya sea `Image` o `Text`).
2. Para **expresión**, copie la expresión JSON de flujo de trabajo predeterminado. La cadena JSON final debe tener este aspecto:

```json
{
  "Description":"<A description for the Workflow>",
  "Type":"Text",
  "Expression":{
    "Type":"Logic",
    "If":{
      "ConnectorName":"moderator",
      "OutputName":"isAdult",
      "Operator":"eq",
      "Value":"true",
      "Type":"Condition"
    },
    "Then":{
      "Perform":[
        {
          "Name":"createreview",
          "CallbackEndpoint":null,
          "Tags":[

          ]
        }
      ],
      "Type":"Actions"
    }
  }
}
```

> [!NOTE]
> Puede definir expresiones simples, complejas y anidadas incluso para los flujos de trabajo mediante esta API. El [flujo de trabajo: crear o actualizar](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) documentación contiene algunos ejemplos de una lógica más compleja.

### <a name="submit-your-request"></a>Enviar la solicitud
  
Seleccione **Enviar**. Si la operación se realiza correctamente, el **estado de la respuesta** es `200 OK` y el cuadro **Contenido de la respuesta** muestra `true`.

### <a name="examine-the-new-workflow"></a>Examinar el nuevo flujo de trabajo

En el [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/), seleccione **configuración** > **flujos de trabajo**. El nuevo flujo de trabajo debe aparecer en la lista.

![Lista de flujos de trabajo de la herramienta de revisión](images/workflow-console-new-workflow.PNG)

Seleccione el **editar** opción del flujo de trabajo y vaya a la **diseñador** ficha. En este caso, puede ver una representación intuitiva de la lógica JSON.

![Pestaña Diseñador para un flujo de trabajo seleccionado](images/workflow-console-new-workflow-designer.PNG)

## <a name="get-workflow-details"></a>Obtención de detalles del flujo de trabajo

Para recuperar los detalles de un flujo de trabajo existente, vaya a la **[flujo de trabajo - Get](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b44b3f9b0711b43c4c58)** API de referencia de página y seleccione el botón de su región (es decir, la región en la que se administra la clave).

![Selección de región para Workflow - Get (Flujo de trabajo: Obtener)](images/test-drive-region.png)

Escriba los parámetros de llamada REST como se muestra en la sección anterior. Asegúrese de que esta vez, **workflowname** es el nombre de un flujo de trabajo existente.

![Obtención de parámetros y encabezados de consulta](images/workflow-get-default.PNG)

Seleccione **Enviar**. Si la operación se realiza correctamente, el **estado de respuesta** es `200 OK`y el **contenido de la respuesta** cuadro muestra el flujo de trabajo en formato JSON, similar al siguiente:

```json
{
  "Name":"default",
  "Description":"Default",
  "Type":"Image",
  "Expression":{
    "If":{
      "ConnectorName":"moderator",
      "OutputName":"isadult",
      "Operator":"eq",
      "Value":"true",
      "AlternateInput":null,
      "Type":"Condition"
    },
    "Then":{
      "Perform":[
        {
          "Name":"createreview",
          "Subteam":null,
          "CallbackEndpoint":null,
          "Tags":[

          ]
        }
      ],
      "Type":"Actions"
    },
    "Else":null,
    "Type":"Logic"
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a usar flujos de trabajo con [trabajos de moderación de contenido](try-review-api-job.md).