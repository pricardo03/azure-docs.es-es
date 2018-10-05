---
title: 'Moderación del contenido con revisiones humanas en la consola de API: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre cómo crear revisiones humanas en la consola de Content Moderator API.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: bb95341a09f09ce8020f34476e720270fd401909
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219760"
---
# <a name="create-reviews-from-the-api-console"></a>Crear revisiones en la consola de API

Use las [operaciones de revisión](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4) de la API de revisión para crear revisiones de imagen o texto para la moderación humana. Los moderadores humanos usan la herramienta de revisión para revisar el contenido. Use esta operación en función de la lógica de negocios posterior a la moderación. Úsela después de haber examinado el contenido con cualquiera de las API de imagen o texto de Content Moderator u otras Cognitive Services APIs. 

Después de que un moderador humano revise las etiquetas asignadas automáticamente y los datos de predicción y de que envíe una decisión de moderación final, la API de revisión envía toda la información al punto de conexión de API.

## <a name="use-the-api-console"></a>Usar la consola de API
Para probar la API mediante la consola en línea, debe especificar algunos valores en la consola:

- **teamName**: el nombre del equipo que creó al configurar la cuenta de la herramienta de revisión. 
- **ContentId**: esta cadena se pasa a la API y se devuelve a través de la devolución de llamada. ContentId es útil para asociar los identificadores internos o metadatos con los resultados de un trabajo de moderación.
- **Metadatos**: pares de clave-valor personalizados devueltos al punto de conexión de API durante la devolución de llamada. Si la clave es un código corto definido en la herramienta de revisión, aparece como una etiqueta.
- **Ocp-Apim-Subscription-Key**: ubicada en la pestaña **Configuración**. Para más información, consulte [Información general](overview.md).

La manera más sencilla de obtener acceso a una consola de prueba es desde la ventana **Credenciales**.

1.  En la ventana **Credenciales**, seleccione [Review API reference](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4) (Referencia de API de revisión).

  Se abre la página **Review - Create** (Revisar: crear).

2.  En **Open API testing console** (Abrir consola de prueba de API), seleccione la región que mejor describa su ubicación.

  ![Selección de la región en la página Review - Create (Revisar: crear)](images/test-drive-region.png)

  Se abre la consola de API **Review - Create** (Revisar: crear).
  
3.  Especifique valores para los parámetros de consulta necesarios, el tipo de contenido y su clave de suscripción. En el cuadro **Cuerpo de la solicitud**, especifique el contenido (por ejemplo, la ubicación de la imagen), los metadatos y otra información asociada con el contenido.

  ![Parámetros de consulta de la consola Review - Create (Revisar: crear), encabezados y cuadro del cuerpo de la solicitud](images/test-drive-review-1.PNG)
  
4.  Seleccione **Enviar**. Se crea una id. de revisión. Copie esta id. para usarla en los pasos siguientes.

  ![Cuadro de contenido de la repuesta de la consola Review - Create (Revisar: crear) muestra la id. de revisión](images/test-drive-review-2.PNG)
  
5.  Seleccione **Obtener** y, a continuación, abra la API seleccionando el botón que coincida con su región. En la página resultante, escriba los valores de **teamName**, **ReviewID** y **clave de suscripción**. Seleccione el botón **Enviar** en la página. 

  ![Resultados de Obtener de la consola Review - Create (Revisar: crear)](images/test-drive-review-3.PNG)
  
6.  Verá los resultados del examen.

  ![Cuadro Contenido de la respuesta de la consola Review - Create (Revisar: crear)](images/test-drive-review-4.PNG)
  
7.  En el panel de Content Moderator, seleccione **Revisar** > **Imagen**. Aparece la imagen que ha examinado, lista para revisión humana.

  ![Imagen de un balón de fútbol de la herramienta de revisión](images/test-drive-review-5.PNG)

## <a name="next-steps"></a>Pasos siguientes

Usar la API de REST en el código o empezar en [Reviews .NET quickstart](moderation-reviews-quickstart-dotnet.md) (Inicio rápido de las revisiones con .NET) para integrar la aplicación.
