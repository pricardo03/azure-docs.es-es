---
title: 'Creación de un almacén de conocimiento en Azure Portal: Azure Search'
description: Cree un almacén de conocimiento de Azure Search para conservar los enriquecimientos de la canalización de búsqueda cognitiva mediante el Asistente para la importación de datos en Azure Portal.
author: lisaleib
services: search
ms.service: search
ms.subservice: cognitive-search
ms.topic: tutorial
ms.date: 09/03/2019
ms.author: v-lilei
ms.openlocfilehash: 14996d0ac9ee4e086a5dccd9275ef694adca06ca
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70963192"
---
# <a name="create-an-azure-search-knowledge-store-in-the-azure-portal"></a>Creación de un almacén de conocimiento de Azure Search en Azure Portal

> [!Note]
> El almacén de conocimiento se encuentra en versión preliminar y no debe usarse en producción. Esta característica se proporciona en la [API REST de Azure Search, versión 2019-05-06-Preview](search-api-preview.md). Por el momento, no hay compatibilidad con .NET SDK.
>

El almacén de conocimiento es una característica de Azure Search que conserva la salida de una canalización de enriquecimiento de inteligencia artificial para su análisis posterior o para otro procesamiento descendente. Una canalización enriquecida de inteligencia artificial acepta archivos de imagen o archivos de texto no estructurados, los indexa mediante Azure Search, aplica los enriquecimientos de inteligencia artificial de Cognitive Services (como el análisis de imágenes y el procesamiento de lenguaje natural) y, a continuación, guarda los resultados en un almacén de conocimiento en Azure Storage. A continuación, puede usar herramientas como Power BI o el Explorador de Storage para explorar el almacén de conocimiento.

En este artículo, usará el Asistente para la importación de datos de Azure Portal para ingerir, indexar y aplicar los enriquecimientos de AI a un conjunto de reseñas sobre hoteles. Las reseñas sobre hoteles se importan en Azure Blob Storage y los resultados se guardan como un almacén de conocimiento en Azure Table Storage.

Después de crear el almacén de conocimiento, puede obtener información sobre cómo acceder a este almacén de conocimiento mediante el Explorador de Storage o Power BI.

## <a name="prerequisites"></a>Requisitos previos

+ [Cree un servicio Azure Search](search-create-service-portal.md) o [busque un servicio existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en su suscripción actual. Puede usar un servicio gratuito para este tutorial.

+ [Cree una cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) para almacenar los datos de ejemplo y el almacén de conocimiento. La cuenta de almacenamiento debe usar la misma ubicación (por ejemplo, Oeste de EE. UU.) que el servicio Azure Search y el *tipo de cuenta* debe ser *StorageV2 (uso general V2)* (valor predeterminado) o *Storage (uso general V1)* .

## <a name="load-the-data"></a>Carga de los datos

Cargue el archivo CSV de reseñas de hoteles en Azure Blob Storage para que un indexador de Azure Search pueda acceder a él y se pueda realizar el suministro mediante la canalización de enriquecimiento de inteligencia artificial.

### <a name="create-an-azure-blob-container-with-the-data"></a>Creación de un contenedor de blobs de Azure con los datos

1. [Descargue los datos de reseñas de hoteles guardados en un archivo CSV (HotelReviews_Free.csv)](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Estos datos proceden de Kaggle.com y contienen comentarios de los clientes sobre hoteles.
1. [Inicie sesión en Azure Portal](https://portal.azure.com) y vaya a la cuenta de Azure Storage.
1. [Cree un contenedor de blobs](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal). Para hacerlo, en la barra de navegación izquierda de la cuenta de almacenamiento, haga clic en **Blobs** y, a continuación, haga clic en **+ Contenedor** en la barra de comandos.
1. En el **Nombre** del nuevo contenedor, escriba `hotel-reviews`.
1. Seleccione cualquier **Nivel de acceso público**. Usamos el valor predeterminado.
1. Haga clic en **Aceptar** para crear el contenedor de blobs de Azure.
1. Abra el nuevo contenedor `hotels-review`, haga clic en **Cargar** y seleccione el archivo **HotelReviews-Free.csv** que descargó en el primer paso.

    ![Carga de los datos](media/knowledge-store-create-portal/upload-command-bar.png "Carga de las reseñas de hoteles")

1. Haga clic en **Cargar** para importar el archivo CSV en Azure Blob Storage. Aparecerá el nuevo contenedor.

    ![Creación del contenedor de blobs de Azure](media/knowledge-store-create-portal/hotel-reviews-blob-container.png "Create the Azure Blob container")

### <a name="get-the-azure-storage-account-connection-string"></a>Obtención de la cadena de conexión de la cuenta de Azure Storage

1. Vaya a la cuenta de Azure Storage en el portal.
1. En el panel de navegación izquierdo del servicio, haga clic en **Claves de acceso**.
1. En **Clave 1**, copie y guarde la *Cadena de conexión*. La cadena comienza por `DefaultEndpointsProtocol=https`. El nombre y la clave de la cuenta de almacenamiento están insertados en la cadena. Mantenga esta cadena a mano. La necesitará en pasos posteriores.

## <a name="create-and-run-ai-enrichments"></a>Creación y ejecución de enriquecimientos de IA

Use el asistente para la importación de datos para crear el almacén de conocimiento. Creará un origen de datos, elegirá características enriquecidas, configurará un almacén de conocimiento y un índice y, a continuación, realizará la ejecución.

### <a name="start-the-import-data-wizard"></a>Inicio del Asistente para la importación de datos

1. [Busque el servicio de búsqueda](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en Azure Portal.

1. En la barra de comandos, haga clic en **Importar datos** para iniciar el Asistente para importación.

### <a name="connect-to-your-data-import-data-wizard"></a>Conectarse a los datos (Asistente para importar datos)

En este paso del asistente, creará un origen de datos a partir del blob de Azure con los datos de hoteles.

1. En la lista **Origen de datos**, seleccione **Azure Blob Storage**.
1. En **Nombre**, escriba `hotel-reviews-ds`.
1. En **Modo de análisis**, seleccione **Texto delimitado** y, a continuación, active la casilla **La primera línea contiene encabezado**. Asegúrese de que el **Carácter delimitador** es una coma (,).
1. Escriba la **Cadena de conexión** del servicio de almacenamiento que guardó en un paso anterior.
1. En **Nombre de contenedor**, escriba `hotel-reviews`.
1. Haga clic en **Siguiente: agregar búsqueda cognitiva (opcional)** .

      ![Creación de un objeto de origen de datos](media/knowledge-store-create-portal/hotel-reviews-ds.png "Create a data source object")

## <a name="add-cognitive-search-import-data-wizard"></a>Agregar búsqueda cognitiva (Asistente para importar datos)

En este paso del asistente, creará un conjunto de aptitudes con enriquecimientos de aptitudes cognitivas. Las aptitudes que usamos en este ejemplo extraerán las frases clave y detectarán el idioma y la opinión. Estos enriquecimientos se "proyectarán" en un almacén de conocimiento como tablas de Azure.

1. Expanda **Adjuntar Cognitive Services**. De forma predeterminada, está seleccionada la opción **Gratis (enriquecimientos limitados)** . Puede usar este recurso porque el número de registros de HotelReviews-Free.csv es 19 y este recurso gratuito permite hasta 20 transacciones al día.
1. Expanda **Agregar enriquecimientos**.
1. En **Nombre del conjunto de aptitudes**, escriba `hotel-reviews-ss`.
1. En el campo **Datos de origen**, seleccione **reviews_text*.
1. En **Nivel de granularidad de enriquecimiento**, seleccione **Páginas (fragmentos de 5 000 caracteres)** .
1. Seleccione estas aptitudes cognitivas:
    + **Extracción de frases clave**
    + **Detección de idioma**
    + **Detección de opiniones**

      ![Creación de un conjunto de aptitudes](media/knowledge-store-create-portal/hotel-reviews-ss.png "Create a skillset")

1. Expanda **Guardar enriquecimientos en el almacén de conocimiento**.
1. Escriba la **Cadena de conexión de la cuenta de almacenamiento** que guardó en un paso anterior.
1. Seleccione estas **Proyecciones de tabla de Azure**:
    + **Documentos**
    + **Páginas**
    + **Frases clave**

    ![Configuración de almacén de conocimiento](media/knowledge-store-create-portal/hotel-reviews-ks.png "Configure knowledge store")

1. Haga clic en **Siguiente: personalizar índice de destino**.

### <a name="import-data-import-data-wizard"></a>Importar datos (Asistente para importar datos)

En este paso del asistente, configurará un índice para las consultas de búsqueda de texto completo opcionales. El asistente examinará el origen de datos para deducir los campos y los tipos de datos. Solo tiene que seleccionar los atributos para el comportamiento deseado. Por ejemplo, el atributo **Retrievable** permitirá que el servicio de búsqueda devuelva un valor de campo, mientras que **Searchable** habilitará la búsqueda de texto completo en el campo.

1. En **Nombre de índice**, escriba `hotel-reviews-idx`.
1. En los atributos, realice estas selecciones:
    + Seleccione **Retrievable** para todos los campos.
    + Seleccione **Filterable** y **Facetable** para estos campos: *Sentiment*, *Language* y *Keyphrases*
    + Seleccione **Searchable** para estos campos: *city*, *name*, *reviews_text*, *language* y *Keyphrases*.

    El índice debe tener un aspecto similar al de la siguiente imagen. Dado que la lista es larga, no todos los campos están visibles en la imagen.

    ![Configuración de un índice](media/knowledge-store-create-portal/hotel-reviews-idx.png "Configure an index")

1. Haga clic en **Siguiente: crear un indexador**.

### <a name="create-an-indexer"></a>Creación de un indexador

En este paso del asistente, configurará un indexador que reunirá el origen de datos, las aptitudes y el índice que definió en los pasos anteriores del asistente.

1. En **Nombre**, escriba `hotel-reviews-idxr`.
1. En **Programación**, mantenga el valor predeterminado **Una vez**.
1. Haga clic en **Enviar** para ejecutar el indexador. La extracción de datos, la indexación y la aplicación de aptitudes cognitivas se producen en este paso.

### <a name="monitor-the-notifications-queue-for-status"></a>Supervisión del estado de la cola de notificaciones

1. En Azure Portal, supervise el registro de actividad de notificaciones para obtener un vínculo de estado **Notificación de Azure Search** en el que se pueda hacer clic. La ejecución puede tardar varios minutos en completarse.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha enriquecido los datos con servicios cognitivos y ha proyectado los resultados en un almacén de conocimiento, puede usar el Explorador de Storage o Power BI para explorar el conjunto de datos enriquecido.

Para obtener información sobre cómo explorar este almacén de conocimiento con el Explorador de Storage, consulte el siguiente tutorial.

> [!div class="nextstepaction"]
> [Visualización con el Explorador de Storage](knowledge-store-view-storage-explorer.md)

Para obtener información sobre cómo conectar este almacén de conocimiento a Power BI, consulte el siguiente tutorial.

> [!div class="nextstepaction"]
> [Conexión con Power BI](knowledge-store-connect-power-bi.md)

Si desea repetir este ejercicio o realizar otro tutorial de enriquecimiento de inteligencia artificial, elimine el indexador *hotel-reviews-idxr*. La eliminación del indexador restablece el contador de transacciones diarias gratis a cero.
