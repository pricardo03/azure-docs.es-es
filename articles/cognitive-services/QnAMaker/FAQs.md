---
title: 'Preguntas más frecuentes: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Lista de preguntas más frecuentes sobre el servicio QnA Maker
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: efefd595c43d7f46ff1ead91577d070cf8fb90e4
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47164623"
---
# <a name="frequently-asked-questions"></a>Preguntas frecuentes

## <a name="why-is-my-urlsfiles-is-not-extracting-question-answer-pairs"></a>¿Por qué es mis direcciones URL o archivos no extraen pares de preguntas y respuestas?

Es posible que QnA Maker no pueda extraer automáticamente algún contenido de preguntas y respuestas (QnA) de las direcciones URL de P+F válidas. En tales casos, puede pegar el contenido de QnA en un archivo .txt y ver si la herramienta puede ingerirlo. Como alternativa, puede redactar contenido y agregarlo a la knowledge base.

## <a name="how-large-a-knowledge-base-can-i-create"></a>¿Cuál es el tamaño máximo para crear una knowledge base?

El tamaño de la knowledge base depende de la SKU de Azure Search que elija al crear el servicio QnA Maker. Obtenga más detalles [aquí](./Tutorials/choosing-capacity-qnamaker-deployment.md).

## <a name="why-do-i-not-see-anything-in-the-drop-down-for-when-i-try-to-create-a-new-knowledge-base"></a>¿Por qué no aparece nada en la lista desplegable cuando intento crear una nueva knowledge base?

Todavía no ha creado ningún servicio QnA Maker en Azure. Obtenga información [aquí](./How-To/set-up-qnamaker-service-azure.md) sobre cómo hacerlo.

## <a name="how-do-i-share-a-knowledge-base-with-other"></a>¿Cómo se puede compartir una knowledge base con otro usuario?

El uso compartido funciona en el nivel de un servicio QnA Maker, es decir, todas las knowledge base de los servicios se compartirán. Obtenga información [aquí](./How-To/collaborate-knowledge-base.md) para colaborar en una knowledge base.

## <a name="how-can-i-change-the-default-message-when-no-good-match-is-found"></a>¿Cómo se puede cambiar el mensaje predeterminado cuando no se encuentra ninguna buena coincidencia?

El mensaje predeterminado es parte de la configuración de App Service.
- Vaya al recurso de App Service en Azure Portal.

![App Service QnA Maker](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- Haga clic en la opción **Configuración**.

![Configuración de App Service QnA Maker](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- Cambie el valor del parámetro **DefaultAnswer**.
- Reiniciar App Service

![Reiniciar App Service QnA Maker](./media/qnamaker-faq/qnamaker-appservice-restart.png)

## <a name="why-is-my-sharepoint-link-not-getting-extracted"></a>¿Por qué mi vínculo de SharePoint no se extrae?

La herramienta analiza únicamente direcciones URL públicas y no admite orígenes de datos autenticados en este momento. Como alternativa, puede descargar el archivo y utilice la opción de carga de archivos para extraer preguntas y respuestas.


## <a name="the-updates-that-i-made-to-my-knowledge-base-are-not-reflected-on-publish-why-not"></a>Las actualizaciones que realizo en mi knowledge base no se publican. ¿Por qué no?

Cada edición, ya sea para actualizar la tabla, hacer pruebas o cambiar la configuración, debe guardarse para poder publicarla. Asegúrese de hacer clic en el botón Save and train (Guardar y entrenar) después de cada edición que haga.

## <a name="when-should-i-refresh-my-endpoint-keys"></a>¿Cuándo debo actualizar mis claves de punto de conexión?

Debe actualizar las claves de punto de conexión si sospecha que han sido objeto de alguna acción fraudulenta.

## <a name="does-the-knowledge-base-support-rich-data-or-multimedia"></a>¿La knowledge base admite datos enriquecidos o multimedia?

La knowledge base admite Markdown. Sin embargo, la extracción automática de direcciones URL tiene una capacidad limitada de conversión de HTML a Markdown. Si quiere usar Markdown completo, puede modificar el contenido directamente en la tabla o cargar una knowledge base con el contenido enriquecido.

El contenido multimedia, como imágenes y vídeos, no se admite en este momento.

## <a name="does-qna-maker-support-non-english-languages"></a>¿Admite QnA Maker otros idiomas aparte de inglés?

Obtenga más detalles acerca de los [idiomas admitidos](./Overview/languages-supported.md).

Si tiene contenido en varios idiomas, asegúrese de crear un servicio independiente para cada idioma.

## <a name="do-i-need-to-use-bot-framework-in-order-to-use-qna-maker"></a>¿Es necesario utilizar Bot Framework para usar QnA Maker?

No, no es necesario usar Bot Framework con QnA Maker. Sin embargo, QnA Maker se ofrece como una de las diversas plantillas de Azure Bot Service. Bot Service permite el desarrollo rápido de bots inteligentes mediante Microsoft Bot Framework y se ejecuta en un entorno sin servidor.

## <a name="how-can-i-create-a-bot-with-qna-maker"></a>¿Cómo se puede crear un bot con QnA Maker?

Siga las instrucciones de [esta](./Tutorials/create-qna-bot.md) documentación para crear su Bot con Azure Bot Service.

## <a name="how-do-i-embed-the-qna-maker-service-in-my-website"></a>¿Cómo puedo insertar el servicio QnA Maker en mi sitio web?

Siga estos pasos para insertar el servicio QnA Maker como control de chat en web en su sitio web:

1. Cree su bot de P+F siguiendo las instrucciones que encontrará [aquí](./Tutorials/create-qna-bot.md).
2. Habilite el chat en web mediante los pasos que se indican [aquí](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-webchat).


