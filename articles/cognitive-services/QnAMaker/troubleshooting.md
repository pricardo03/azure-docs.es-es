---
title: 'Solución de problemas: QnA Maker'
titleSuffix: Azure Cognitive Services
description: La lista de las preguntas frecuentes seleccionadas sobre QnA Maker le ayudará a adoptar este servicio de forma más rápida y a obtener mejores resultados.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/28/2020
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 18d7e02689cc9c5fe9282a6a2456b8b1574ec85e
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76901641"
---
# <a name="troubleshooting-for-qna-maker"></a>Solución de problemas para QnA Maker

La lista de las preguntas frecuentes seleccionadas sobre QnA Maker le ayudará a adoptar este servicio de forma más rápida y a obtener mejores resultados.

<a name="how-to-get-the-qnamaker-service-hostname"></a>

## <a name="how-to-get-the-qnamaker-service-endpoint"></a>Cómo obtener el punto de conexión de servicio de QnAMaker

El punto de conexión de servicio de QnAMaker es útil para fines de depuración cuando se ponga en contacto con el soporte técnico de QnAMaker o UserVoice. El punto de conexión es una dirección URL en este formato: https://your-resource-name.azurewebsites.net.

1. Vaya a su servicio QnAMaker (grupo de recursos) en [Azure Portal](https://portal.azure.com)

    ![Grupo de recursos de Azure para QnAMaker en Azure Portal](./media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Seleccione la instancia de App Service asociada al recurso de QnA Maker. Normalmente, los nombres son los mismos.

     ![Selección del App Service QnAMaker](./media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

1. La dirección URL del punto de conexión está disponible en la sección Información general

    ![Punto de conexión de QnAMaker](./media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)

## <a name="manage-the-knowledge-base"></a>Administrar la base de conocimiento

### <a name="i-accidentally-deleted-a-part-of-my-qna-maker-what-should-i-do"></a>Eliminé por accidente una parte de QnA Maker, ¿qué debo hacer?

No elimine ninguno de los servicios de Azure creados junto con el recurso de QnA Maker, como Search o Web App. Estos son necesarios para que QnA Maker funcione; si elimina uno, QnA Maker dejará de funcionar correctamente.

Todas las eliminaciones son permanentes, incluidos los pares de preguntas y respuestas, archivos, direcciones URL, preguntas y respuestas personalizadas, bases de conocimiento o recursos de Azure. Asegúrese de exportar la base de conocimiento desde la página **Settings** (Configuración) antes de eliminar cualquier de sus partes.

### <a name="why-is-my-urlsfiles-not-extracting-question-answer-pairs"></a>¿Por qué mis direcciones URL o archivos no extraen pares de preguntas y respuestas?

Es posible que QnA Maker no pueda extraer automáticamente algún contenido de preguntas y respuestas (QnA) de las direcciones URL de P+F válidas. En tales casos, puede pegar el contenido de QnA en un archivo .txt y ver si la herramienta puede ingerirlo. Como alternativa, puede redactar contenido y agregarlo a la knowledge base a través del [portal de QnA Maker](https://qnamaker.ai).

### <a name="how-large-a-knowledge-base-can-i-create"></a>¿Cuál es el tamaño máximo para crear una knowledge base?

El tamaño de la knowledge base depende de la SKU de Azure Search que elija al crear el servicio QnA Maker. Obtenga más detalles [aquí](./Tutorials/choosing-capacity-qnamaker-deployment.md).

### <a name="why-cant-i-see-anything-in-the-drop-down-when-i-try-to-create-a-new-knowledge-base"></a>¿Por qué no se ve nada en el menú desplegable cuando intento crear una nueva knowledge base?

Todavía no ha creado ningún servicio QnA Maker en Azure. Lea [aquí](./How-To/set-up-qnamaker-service-azure.md) para saber cómo hacerlo.

### <a name="how-do-i-share-a-knowledge-base-with-others"></a>¿Cómo se puede compartir una knowledge base con otros usuarios?

El uso compartido funciona en el nivel de un servicio QnA Maker, es decir, todas las knowledge bases de los servicios se compartirán. Obtenga información [aquí](./How-To/collaborate-knowledge-base.md) para colaborar en una knowledge base.

### <a name="can-you-share-a-knowledge-base-with-a-contributor-that-is-not-in-the-same-aad-tenant-to-modify-a-knowledge-base"></a>¿Puede compartir una base de conocimiento con un colaborador que no esté en el mismo inquilino de AAD, para modificarla?

El uso compartido se basa en el control de acceso basado en rol (RBAC) de Azure. Si puede compartir _cualquier_ recursos en Azure con otro usuario, también puede compartir QnA Maker.

### <a name="if-you-have-an-app-service-plan-with-5-qnamaker-knowledge-bases-can-you-assign-readwrite-rights-to-5-different-users-so-each-of-them-can-access-only-1-qnamaker-knowledge-base"></a>Si tiene un plan de App Service con 5 bases de conocimiento de QnAMaker. ¿Puede asignar derechos de lectura o escritura a 5 usuarios distintos para que cada uno de ellos pueda acceder solo a 1 base de conocimiento de QnAMaker?

Puede compartir un servicio completo de QnAMaker, no bases de conocimiento individuales.

### <a name="how-can-i-change-the-default-message-when-no-good-match-is-found"></a>¿Cómo se puede cambiar el mensaje predeterminado cuando no se encuentra ninguna buena coincidencia?

El mensaje predeterminado es parte de la configuración de App Service.
- Vaya al recurso de App Service en Azure Portal.

![App Service QnA Maker](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- Haga clic en la opción **Configuración**.

![Configuración de App Service QnA Maker](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- Cambie el valor del parámetro **DefaultAnswer**.
- Reiniciar App Service

![Reiniciar App Service QnA Maker](./media/qnamaker-faq/qnamaker-appservice-restart.png)

### <a name="why-is-my-sharepoint-link-not-getting-extracted"></a>¿Por qué mi vínculo de SharePoint no se extrae?

Para más información, consulte [Ubicaciones de orígenes de datos](./Concepts/knowledge-base.md#data-source-locations).

### <a name="the-updates-that-i-made-to-my-knowledge-base-are-not-reflected-on-publish-why-not"></a>Las actualizaciones que realizo en mi knowledge base no se publican. ¿Por qué no?

Cada edición, ya sea para actualizar la tabla, hacer pruebas o cambiar la configuración, debe guardarse para poder publicarla. Asegúrese de hacer clic en el botón **Save and train** (Guardar y entrenar) después de cada edición que realice.

### <a name="does-the-knowledge-base-support-rich-data-or-multimedia"></a>¿La knowledge base admite datos enriquecidos o multimedia?

#### <a name="multimedia-auto-extraction-for-files-and-urls"></a>Extracción automática multimedia para archivos y direcciones URL

* Direcciones URL: capacidad limitada de conversión de HTML a Markdown.
* Archivos: no compatibles.

#### <a name="answer-text-in-markdown"></a>Texto de respuesta en formato Markdown
Una vez que los conjuntos de QnA están en la base de conocimiento, puede editar el texto de Markdown de la respuesta para incluir vínculos a los elementos multimedia disponibles a través de las direcciones URL públicas.

### <a name="does-qna-maker-support-non-english-languages"></a>¿Admite QnA Maker otros idiomas aparte de inglés?

Obtenga más detalles acerca de los [idiomas admitidos](./Overview/languages-supported.md).

Si tiene contenido en varios idiomas, asegúrese de crear un servicio independiente para cada idioma.

## <a name="manage-service"></a>Administración de servicios

### <a name="when-should-i-restart-my-app-service"></a>¿Cuándo debo reiniciar mi instancia de App Service?

Actualice su instancia de App Service cuando el icono de precaución aparezca junto al valor de versión de la base de conocimiento en la tabla **Claves de punto de conexión** de la [página](https://www.qnamaker.ai/UserSettings) **Configuración de usuario**.

### <a name="i-deleted-my-existing-search-service-how-can-i-fix-this"></a>Eliminé mi servicio de Search existente. ¿Cómo lo puedo corregir?

Si elimina un índice de Azure Cognitive Search, la operación es definitiva y no es posible recuperar el índice.

### <a name="i-deleted-my-testkb-index-in-my-search-service-how-can-i-fix-this"></a>Eliminé mi índice `testkb` en el servicio de Search. ¿Cómo lo puedo corregir?

No se pueden recuperar los datos antiguos. Cree un nuevo recurso de QnA Maker y cree su base de conocimiento nuevamente.

### <a name="when-should-i-refresh-my-endpoint-keys"></a>¿Cuándo debo actualizar mis claves de punto de conexión?

Actualice las claves de punto de conexión si sospecha que han sido objeto de alguna acción fraudulenta.

### <a name="can-i-use-the-same-azure-cognitive-search-resource-for-knowledge-bases-using-multiple-languages"></a>¿Se puede usar el mismo recurso de Azure Cognitive Search para bases de conocimiento que emplean varios idiomas?

Para usar varios idiomas y varias bases de conocimiento, el usuario tiene que crear un recurso de QnA Maker para cada idioma. De esta manera, se creará un servicio de Azure Search independiente por idioma. La combinación de bases de datos de distintos idiomas en un único servicio de Azure Search dará lugar a una importancia degradada de los resultados.

### <a name="how-can-i-change-the-name-of-the-azure-cognitive-search-resource-used-by-qna-maker"></a>¿Cómo se puede cambiar el nombre del recurso de Azure Cognitive Search usado por QnA Maker?

El nombre del recurso de Azure Cognitive Search es el nombre del recurso de QnA Maker con algunas letras aleatorias colocadas al final. Esto hace que sea más difícil distinguir entre varios recursos de Search en QnA Maker. Cree un servicio de búsqueda independiente (asígnele el nombre que desee) y conéctelo a su servicio de QnA. Los pasos son similares a los que debe hacer para [actualizar una instancia de Azure Search](How-To/set-up-qnamaker-service-azure.md#upgrade-the-azure-cognitive-search-service).

### <a name="when-qna-maker-returns-runtime-core-is-not-initialized-how-do-i-fix-it"></a>Cuando QnA Maker devuelve `Runtime core is not initialized,`, ¿cómo puedo corregirlo?

Es posible que el espacio en disco para el servicio de aplicaciones esté lleno. Pasos para corregir el espacio en disco:

1. En [Azure Portal](https://portal.azure.com), seleccione el servicio de aplicaciones de QnA Maker y, a continuación, detenga el servicio.
1. En el servicio de aplicaciones, seleccione **Herramientas de desarrollo**, **Herramientas avanzadas** y, a continuación, **Ir**. Se abre una nueva ventana del explorador.
1. Seleccione **Depurar consola** y, a continuación, **CMD** para abrir una herramienta de línea de comandos.
1. Vaya al directorio _site/wwwroot/Data/QnAMaker/_ .
1. Quite todas las carpetas cuyo nombre comience por `rd`.

    **No elimine** lo siguiente:

    * Archivo KbIdToRankerMappings.txt
    * Archivo EndpointSettings.json
    * Carpeta EndpointKeys

1. Inicie el servicio de aplicaciones.
1. Obtenga acceso a la base de conocimiento para comprobar que funcione ahora.


## <a name="integrate-with-other-services-including-bots"></a>Integración con otros servicios, como bots

### <a name="do-i-need-to-use-bot-framework-in-order-to-use-qna-maker"></a>¿Es necesario utilizar Bot Framework para usar QnA Maker?

No, no es necesario usar [Bot Framework](https://github.com/Microsoft/botbuilder-dotnet) con QnA Maker. Sin embargo, QnA Maker se ofrece como una de las diversas plantillas de  [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0). Bot Service permite el desarrollo rápido de bots inteligentes mediante Microsoft Bot Framework y se ejecuta en un entorno sin servidor.

### <a name="how-can-i-create-a-new-bot-with-qna-maker"></a>¿Cómo se puede crear un bot con QnA Maker?

Siga las instrucciones de [esta](./Tutorials/create-qna-bot.md) documentación para crear su Bot con Azure Bot Service.

### <a name="how-do-i-use-a-different-knowledge-base-with-an-existing-azure-bot-service"></a>¿Cómo se usa una base de conocimiento diferente con un servicio de bot existente de Azure?

Debe tener la siguiente información sobre la base de conocimiento:

* Id. de base de conocimiento.
* Nombre de subdominio personalizado del punto de conexión publicado de la base de conocimiento, conocido como `host`, que se encuentra en la página **Configuración** después de la publicación.
* Clave del punto de conexión publicado de la base de conocimiento; se encuentra en **Settings** (Configuración) después de publicarlo.

Con esta información, vaya al servicio de aplicaciones del bot en Azure Portal. En **Configuración -> Configuración -> Configuración de la aplicación**, cambie esos valores.

La clave del punto de conexión de la base de conocimiento se llama `QnAAuthkey` en el servicio ABS.

### <a name="can-two-or-more-client-applications-share-a-knowledge-base"></a>¿Pueden dos o más aplicaciones cliente compartir una base de conocimiento?

Sí, la base de conocimiento se puede consultar desde cualquier número de clientes. Si la respuesta de la base de conocimiento parece lenta o se agota el tiempo de espera, considere la posibilidad de actualizar el nivel de servicio del servicio de aplicaciones asociado a la base de conocimiento.

### <a name="how-do-i-embed-the-qna-maker-service-in-my-website"></a>¿Cómo puedo insertar el servicio QnA Maker en mi sitio web?

Siga estos pasos para insertar el servicio QnA Maker como control de chat en web en su sitio web:

1. Cree su bot de P+F siguiendo las instrucciones que encontrará [aquí](./Tutorials/create-qna-bot.md).
2. Habilite el chat en web mediante los pasos que se indican [aquí](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-webchat).

## <a name="data-storage"></a>Almacenamiento de datos

### <a name="what-data-is-stored-and-where-is-it-stored"></a>¿Qué datos se almacenan y dónde?

Cuando se crea el servicio de QnA Maker, se selecciona una región de Azure. Sus bases de conocimiento y los archivos de registro se almacenan en esta región.
