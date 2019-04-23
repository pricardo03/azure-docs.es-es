---
title: Definir y usar los flujos de trabajo contenidos a través de la herramienta de revisión - Content Moderator
titlesuffix: Azure Cognitive Services
description: Puede usar el Diseñador de flujo de trabajo de Azure Content Moderator para definir flujos de trabajo personalizados y los umbrales según las directivas de contenido.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 04/04/2019
ms.author: sajagtap
ms.openlocfilehash: 006f7d6691b8872aaa7ff8ccacff484585761d00
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795804"
---
# <a name="define-and-use-moderation-workflows"></a>Definir y usar los flujos de trabajo de moderación

En esta guía, obtendrá información sobre cómo configurar y usar [flujos de trabajo](../review-api.md#workflows) en el [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com) sitio Web. Los flujos de trabajo son filtros personalizados basados en la nube que puede usar para controlar el contenido de forma más eficaz. Los flujos de trabajo pueden conectarse a una variedad de servicios para filtrar el contenido de maneras diferentes y, a continuación, realice la acción apropiada. Esta guía muestra cómo usar el conector de Content Moderator (que se incluye de forma predeterminada) para filtrar el contenido y configuración de revisiones humanas en un escenario típico de moderación.

## <a name="create-a-new-workflow"></a>Crear un nuevo flujo de trabajo

Vaya a la [herramienta de revisión de Content Moderator](https://contentmoderator.cognitive.microsoft.com/) e inicie sesión. En la pestaña **Settings** (Configuración), seleccione **Workflows** (Flujos de trabajo).

![Configuración de flujos de trabajo](images/2-workflows-0.png)

En la siguiente pantalla, seleccione **agregar flujo de trabajo**.

![Adición de un flujo de trabajo](images/2-workflows-1.png)

### <a name="assign-a-name-and-description"></a>Asignación de un nombre y una descripción

Nombre del flujo de trabajo, escriba una descripción y elija si va a controlar el flujo de trabajo imágenes o texto.

![Nombre y descripción del flujo de trabajo](images/image-workflow-create.PNG)

### <a name="define-evaluation-criteria"></a>Definir criterios de evaluación

En la siguiente pantalla, vaya a la **si** sección. En el menú desplegable superior, elija **condición**. Esto le permitirá configurar la condición en la que el flujo de trabajo realizará la acción. Elija si desea utilizar varias condiciones, **combinación** en su lugar. 

A continuación, seleccione un conector. En este ejemplo se usa **Content Moderator**. Según el conector que elija, obtendrá distintas opciones para la salida de datos. Consulte la [conectores](./configure.md#connectors) sección de la Guía de configuración de herramienta de revisión para obtener información sobre cómo configurar otros conectores.

![Seleccione el conector de flujo de trabajo](images/image-workflow-connect-to.PNG)

Elija el resultado deseado para usar y establecer las condiciones para protegerlo contra.

![Definición de la condición de flujo de trabajo](images/image-workflow-condition.PNG)

### <a name="define-the-action"></a>Defina la acción

Vaya a la **, a continuación,** sección, donde selecciona una acción. El ejemplo siguiente crea una revisión de la imagen y asigna una etiqueta. Si lo desea, puede agregar una ruta de acceso alternativa (Else) y establecer una acción para así.

![Definición de la acción del flujo de trabajo](images/image-workflow-action.PNG)

### <a name="save-the-workflow"></a>Guardar el flujo de trabajo

Tenga en cuenta el nombre del flujo de trabajo; se necesita el nombre para iniciar un trabajo de moderación con la API de flujo de trabajo (ver abajo). Por último, guarde el flujo de trabajo mediante el **guardar** situado en la parte superior de la página.

## <a name="test-the-workflow"></a>Probar el flujo de trabajo

Ahora que ha definido un flujo de trabajo personalizado, pruebe con contenido de ejemplo. Vaya a **flujos de trabajo** y seleccione la correspondiente **ejecutar el flujo de trabajo** botón.

![Prueba del flujo de trabajo](images/image-workflow-execute.PNG)

Guardar este [imagen de ejemplo](https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg) en la unidad local. A continuación, seleccione **elegir archivos** y cargar la imagen en el flujo de trabajo.

![Un ejecutor con una oferta superpuesto a la imagen](images/sample-text.jpg)

### <a name="track-progress"></a>Seguimiento del progreso

Puede ver el progreso del flujo de trabajo en la siguiente ventana emergente.

![Seguimiento de la ejecución del flujo de trabajo](images/image-workflow-job.PNG)

### <a name="verify-workflow-action"></a>Comprobar acción de flujo de trabajo

Vaya a la **imagen** pestaña **revisar** y compruebe que hay una revisión de la imagen recién creada.

![Revisar las imágenes](images/image-workflow-review.PNG)

## <a name="next-steps"></a>Pasos siguientes

En esta guía, ha aprendido a configurar y utilizar flujos de trabajo de moderación de Content Moderator [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com). A continuación, consulte el [Guía de la API de REST](../try-review-api-workflow.md) para aprender a crear flujos de trabajo mediante programación.
