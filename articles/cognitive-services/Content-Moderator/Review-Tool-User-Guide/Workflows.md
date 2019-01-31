---
title: 'Definición y uso de flujos de trabajo de moderación de contenido: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Puede usar el diseñador y las API de flujos de trabajo de Content Moderator para definir flujos de trabajo y umbrales personalizados en función de las directivas de contenido.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 8fe380e3015e5b6929aebcb898eef44d6f6bceda
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55213283"
---
# <a name="define-test-and-use-workflows"></a>Definición, prueba y uso de flujos de trabajo

Puede usar el diseñador y las API de flujos de trabajo de Content Moderator para definir flujos de trabajo y umbrales personalizados en función de las directivas de contenido.

Los flujos de trabajo "conectan" con la API de Content Moderator mediante conectores. Si un conector para una API determinada no está disponible, puede usar otras API. En este ejemplo se usa el conector de Content Moderator que se incluye de forma predeterminada.

## <a name="browse-to-the-workflows-section"></a>Desplazamiento hasta la sección de flujos de trabajo

En la pestaña **Settings** (Configuración), seleccione **Workflows** (Flujos de trabajo).

  ![Configuración de flujos de trabajo](images/2-workflows-0.png)

## <a name="start-a-new-workflow"></a>Inicio de un nuevo flujo de trabajo

Seleccione **Add Workflow** (Agregar flujo de trabajo).

  ![Adición de un flujo de trabajo](images/2-workflows-1.png)

## <a name="assign-a-name-and-description"></a>Asignación de un nombre y una descripción

Asigne un nombre al flujo de trabajo, escriba una descripción y elija si el flujo de trabajo administra imágenes o texto.

  ![Nombre y descripción del flujo de trabajo](images/ocr-workflow-step-1.PNG)

## <a name="define-the-evaluation-criteria-condition"></a>Definición de los criterios de evaluación ("condición")

En la siguiente captura de pantalla, puede ver los campos y las selecciones de If-Then-Else que debe definir para los flujos de trabajo. Elija un conector. En este ejemplo se usa **Content Moderator**. Según el conector que elija, cambiarán las opciones disponibles para la salida.

  ![Definición de la condición de flujo de trabajo](images/ocr-workflow-step-2-condition.PNG)

Después de elegir el conector y la salida que prefiera, seleccione un operador y el valor de la condición.

## <a name="define-the-action-to-take"></a>Definición de la acción que se realizará

Seleccione la acción que se realizará y la condición que se debe cumplir. En el ejemplo siguiente se crea una revisión de imagen, se asigna una etiqueta `a` y se resalta para la condición que se muestra. También puede combinar varias condiciones para obtener los resultados deseados. Opcionalmente, agregue una ruta de acceso alternativa (Else).

  ![Definición de la acción del flujo de trabajo](images/ocr-workflow-step-3-action.PNG)

## <a name="save-your-workflow"></a>Almacenamiento del flujo de trabajo

Por último, guarde el flujo de trabajo y anote el nombre. Necesitará el nombre para iniciar un trabajo de moderación mediante la API de revisión.

## <a name="test-the-workflow"></a>Probar el flujo de trabajo

Ahora que ha definido un flujo de trabajo personalizado, pruébelo con contenido de ejemplo.

Seleccione el botón correspondiente **Execute Workflow** (Ejecutar flujo de trabajo).

  ![Prueba del flujo de trabajo](images/ocr-workflow-step-6-list.PNG)

### <a name="upload-a-file"></a>Cargar un archivo

Guarde la [imagen de ejemplo](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) en la unidad local. Para probar el flujo de trabajo, seleccione **Choose File(s)** (Elegir archivos) y cargue la imagen.

  ![Carga del archivo de imagen](images/ocr-workflow-step-7-upload.PNG)

### <a name="track-the-workflow"></a>Seguimiento del flujo de trabajo

Realice el seguimiento del flujo de trabajo mientras se ejecuta.

  ![Seguimiento de la ejecución del flujo de trabajo](images/ocr-workflow-step-4-test.PNG)

### <a name="review-any-images-flagged-for-human-moderation"></a>Revisión de las imágenes marcadas para moderación humana

Para ver la revisión de imagen, vaya a la pestaña **Image** (Imagen) en **Review** (Revisar).

  ![Revisar las imágenes](images/ocr-sample-image-workflow1.PNG)

## <a name="next-steps"></a>Pasos siguientes 

Para invocar el flujo de trabajo desde el código, use flujos de trabajo personalizados con el [`Job`inicio rápido de consola de API](../try-review-api-job.md) y el [inicio rápido de SDK de .NET](../moderation-jobs-quickstart-dotnet.md).
