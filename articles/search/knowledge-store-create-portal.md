---
title: Creación de un almacén de conocimiento (versión preliminar) en Azure Portal
titleSuffix: Azure Cognitive Search
description: Use el Asistente para la importación de datos a fin de crear un almacén de información utilizado para conservar el contenido enriquecido. Conéctese a un almacén de información para el análisis desde otras aplicaciones o envíe contenido enriquecido a los procesos de nivel inferior. Esta característica actualmente está en su versión preliminar pública.
author: HeidiSteen
ms.author: heidist
manager: nitinme
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 01/29/2020
ms.openlocfilehash: 21279b2b4735a25210e8373d76d0d63f9c711bfc
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77472373"
---
# <a name="quickstart-create-an-azure-cognitive-search-knowledge-store-in-the-azure-portal"></a>Inicio rápido: Creación de un almacén de conocimiento de Azure Cognitive Search en Azure Portal

> [!IMPORTANT] 
> El almacén de conocimiento está actualmente en versión preliminar pública. La funcionalidad de versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no es aconsejable usarla para cargas de trabajo de producción. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

El almacén de conocimiento es una característica de Azure Cognitive Search que conserva la salida de una canalización de aptitudes cognitivas para su procesamiento o análisis posteriores. 

Una canalización acepta imágenes y texto no estructurado como contenido sin procesar, aplica inteligencia artificial mediante Cognitive Services (como OCR, análisis de imágenes y procesamiento de lenguaje natural), extrae información y genera nuevas estructuras e información. Uno de los artefactos físicos creados por una canalización es un [almacén de conocimiento](knowledge-store-concept-intro.md), al que se puede acceder mediante herramientas para analizar y explorar el contenido.

En este inicio rápido, combinará servicios y datos en la nube de Azure para crear un almacén de conocimiento. Una vez que todo esté en su lugar, ejecutará el **Asistente para la importación de datos** en el portal para extraerlo todo junto. El resultado final es el contenido de texto original más el generado por inteligencia artificial que puede ver en el portal ([Explorador de Storage](knowledge-store-view-storage-explorer.md)).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-services-and-load-data"></a>Creación de servicios y carga de datos

En este inicio rápido, se usa Azure Cognitive Search, Azure Blob Storage y [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/) para la inteligencia artificial. 

Dado que la carga de trabajo es tan pequeña, Cognitive Services se aprovecha en segundo plano para proporcionar el procesamiento gratuito de hasta 20 transacciones al día. Como el conjunto de datos es tan pequeño, puede omitir la creación o asociación de un recurso de Cognitive Services.

1. [Descarga de HotelReviews_Free.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?sp=r&st=2019-11-04T01:23:53Z&se=2025-11-04T16:00:00Z&spr=https&sv=2019-02-02&sr=b&sig=siQgWOnI%2FDamhwOgxmj11qwBqqtKMaztQKFNqWx00AY%3D). Estos datos son las reseñas del hotel guardadas en un archivo CSV (procede de Kaggle.com) y contiene 19 fragmentos de comentarios de clientes sobre un solo hotel. 

1. [Cree una cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal) o [busque una](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2storageAccounts/) en su suscripción actual. Usará Azure Storage tanto para el contenido sin procesar que se va a importar como para el almacén de información que es el resultado final.

   Elija el tipo de cuenta **StorageV2 (uso general V2)** .

1. Abra las páginas de Blob service y cree un contenedor denominado *hotel-reviews*.

1. Haga clic en **Cargar**.

    ![Carga de los datos](media/knowledge-store-create-portal/upload-command-bar.png "Carga de las reseñas de hoteles")

1. Seleccione el archivo **HotelReviews-Free.csv** que descargó en el primer paso.

    ![Creación del contenedor de Azure Blob Storage](media/knowledge-store-create-portal/hotel-reviews-blob-container.png "Creación del contenedor de Azure Blob Storage")

1. Casi ha terminado con este recurso, pero, antes de dejar estas páginas, use un vínculo en el panel de navegación izquierdo para abrir la página **Claves de acceso**. Obtiene una cadena de conexión para recuperar datos de Blob Storage. La cadena de conexión es similar a la del ejemplo siguiente: `DefaultEndpointsProtocol=https;AccountName=<YOUR-ACCOUNT-NAME>;AccountKey=<YOUR-ACCOUNT-KEY>;EndpointSuffix=core.windows.net`

1. Todavía en el portal, cambie a Azure Cognitive Search. [Cree un servicio](search-create-service-portal.md) o [busque uno existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). Puede usar un servicio gratuito para este inicio rápido.

Ahora ya está preparado para continuar con el Asistente para la importación de datos.

## <a name="run-the-import-data-wizard"></a>Ejecutar el Asistente para la importación de datos

En la página de información general del servicio de búsqueda, haga clic en **Importar datos** en la barra de comandos para crear un almacén de información en cuatro pasos.

  ![Comando de importación de datos](media/cognitive-search-quickstart-blob/import-data-cmd2.png)

### <a name="step-1-create-a-data-source"></a>Paso 1: Creación de un origen de datos

1. En **Conectarse a los datos**, elija **Azure Blob Storage** y seleccione la cuenta y el contenedor que creó. 
1. En **Nombre**, escriba `hotel-reviews-ds`.
1. En **Modo de análisis**, seleccione **Texto delimitado** y, a continuación, active la casilla **La primera línea contiene encabezado**. Asegúrese de que el **Carácter delimitador** es una coma (,).
1. En **Cadena de conexión**, pegue la cadena de conexión que ha copiado de la página **Claves de acceso** en Azure Storage.
1. En **Contenedores**, escriba el nombre del contenedor de blobs que contiene los datos.

    La página debe tener un aspecto similar a la siguiente captura de pantalla.

    ![Creación de un objeto de origen de datos](media/knowledge-store-create-portal/hotel-reviews-ds.png "Creación de un objeto de origen de datos")

1. Continúe en la siguiente página.

### <a name="step-2-add-cognitive-skills"></a>Paso 2: Agregar conocimientos cognitivos

En este paso del asistente, creará un conjunto de aptitudes con enriquecimientos de aptitudes cognitivas. Los datos de origen se componen de revisiones de clientes en varios idiomas. Las aptitudes pertinentes para este conjunto de datos incluyen la extracción de frases clave, la detección de opiniones y la traducción de texto. En un paso posterior, estos enriquecimientos se "proyectarán" en un almacén de conocimiento como tablas de Azure.

1. Expanda **Adjuntar Cognitive Services**. De forma predeterminada, está seleccionada la opción **Gratis (enriquecimientos limitados)** . Puede usar este recurso porque el número de registros de HotelReviews-Free.csv es 19 y este recurso gratuito permite hasta 20 transacciones al día.
1. Expanda **Agregar enriquecimientos**.
1. En **Nombre del conjunto de aptitudes**, escriba `hotel-reviews-ss`.
1. En el campo **Datos de origen**, seleccione **reviews_text**.
1. En **Nivel de granularidad de enriquecimiento**, seleccione **Páginas (fragmentos de 5 000 caracteres)** .
1. Seleccione estas aptitudes cognitivas:
    + **Extracción de frases clave**
    + **Traducir texto**
    + **Detección de opiniones**

      ![Creación de un conjunto de aptitudes](media/knowledge-store-create-portal/hotel-reviews-ss.png "Creación de un conjunto de aptitudes")

1. Expanda **Guardar enriquecimientos en el almacén de conocimiento**.
1. Seleccione estas **Proyecciones de tabla de Azure**:
    + **Documentos**
    + **Páginas**
    + **Frases clave**
1. Escriba la **Cadena de conexión de la cuenta de almacenamiento** que guardó en un paso anterior.

    ![Configuración del almacén de conocimiento](media/knowledge-store-create-portal/hotel-reviews-ks.png "Configuración del almacén de conocimiento")

1. Opcionalmente, descargue una plantilla de Power BI. Al tener acceso a la plantilla desde el asistente, el archivo .pbit local se adapta para reflejar la forma de los datos.

1. Continúe en la siguiente página.

### <a name="step-3-configure-the-index"></a>Paso 3: Configuración del índice

En este paso del asistente, configurará un índice para las consultas de búsqueda de texto completo opcionales. El asistente examinará el origen de datos para deducir los campos y los tipos de datos. Solo tiene que seleccionar los atributos para el comportamiento deseado. Por ejemplo, el atributo **Retrievable** permitirá que el servicio de búsqueda devuelva un valor de campo, mientras que **Searchable** habilitará la búsqueda de texto completo en el campo.

1. En **Nombre de índice**, escriba `hotel-reviews-idx`.
1. En el caso de los atributos, acepte las selecciones predeterminadas: **Retrievable** (Recuperable) y **Searchable** (Permite búsquedas) para los nuevos campos que crea la canalización.

    El índice debe tener un aspecto similar al de la siguiente imagen. Dado que la lista es larga, no todos los campos están visibles en la imagen.

    ![Configuración de un índice](media/knowledge-store-create-portal/hotel-reviews-idx.png "Configuración de un índice")

1. Continúe en la siguiente página.

### <a name="step-4-configure-the-indexer"></a>Paso 4: Configurar el indexador

En este paso del asistente, configurará un indexador que reunirá el origen de datos, las aptitudes y el índice que definió en los pasos anteriores del asistente.

1. En **Nombre**, escriba `hotel-reviews-idxr`.
1. En **Programación**, mantenga el valor predeterminado **Una vez**.
1. Haga clic en **Enviar** para ejecutar el indexador. La extracción de datos, la indexación y la aplicación de aptitudes cognitivas se producen en este paso.

## <a name="monitor-status"></a>Supervisión de estado

La indexación cognitiva de aptitudes tarda más en completarse que la indexación típica basada en texto. El asistente debe abrir la lista de indexadores en la página de introducción para que pueda realizar un seguimiento del progreso. En cuanto a la navegación automática, vaya a la página Introducción y haga clic en **Indizadores**.

En Azure Portal, supervise el registro de actividad de notificaciones para obtener un vínculo de estado **Notificación de Azure Cognitive Search** en el que se pueda hacer clic. La ejecución puede tardar varios minutos en completarse.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha enriquecido los datos con Cognitive Services y ha proyectado los resultados en un almacén de conocimiento, puede usar el Explorador de Storage o Power BI para explorar el conjunto de datos enriquecido.

Puede ver el contenido en Explorador de Storage o realizar un paso más allá con Power BI para obtener información mediante la visualización.

> [!div class="nextstepaction"]
> [Vista con Explorador de Storage](knowledge-store-view-storage-explorer.md)
> [Conectar con Power BI](knowledge-store-connect-power-bi.md)

> [!Tip]
> Si desea repetir este ejercicio o realizar otro tutorial de enriquecimiento de inteligencia artificial, elimine el indexador *hotel-reviews-idxr*. La eliminación del indexador restablece el contador de transacciones diarias gratis a cero para el procesamiento de Cognitive Services.
