---
title: 'Conexión al almacén de conocimiento con Power BI: Azure Search'
description: Cree un almacén de conocimiento mediante el asistente para la importación de datos de Azure Portal y, a continuación, conéctese a Power BI para su análisis y exploración.
author: heidisteen
services: search
ms.service: search
ms.subservice: cognitive-search
ms.topic: tutorial
ms.date: 07/30/2019
ms.author: heidist
ms.openlocfilehash: 1c7f297092760f2a92f524347a3c1a5e353d0965
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69635547"
---
# <a name="create-an-azure-search-knowledge-store-and-connect-using-power-bi"></a>Creación de un almacén de conocimiento de Azure Search y conexión mediante Power BI

> [!Note]
> El almacén de conocimiento se encuentra en versión preliminar y no debe usarse en producción. Esta característica se proporciona en la [API REST de Azure Search, versión 2019-05-06-Preview](search-api-preview.md). Por el momento, no hay compatibilidad con .NET SDK.
>

El almacén de conocimiento es una característica de Azure Search que conserva la salida de una canalización de enriquecimiento de inteligencia artificial para un análisis posterior o para otro procesamiento en sentido descendente. Una canalización enriquecida de inteligencia artificial acepta archivos de imagen o archivos de texto no estructurados de un origen de datos compatible, los envía a la indexación de Azure Search, aplica los enriquecimientos de inteligencia artificial de Cognitive Services (como el análisis de imágenes y el procesamiento de lenguaje natural) y, a continuación, guarda los resultados en un almacén de conocimiento de Azure Storage. En el almacén de conocimiento puede adjuntar herramientas, como Power BI o el Explorador de Storage, para explorar los resultados.

En este artículo, cree un almacén de conocimiento en el portal y, a continuación, conéctese y explore con Power Query en Power BI Desktop. 

En este tutorial se usa un conjunto de datos que se compone de revisiones y clasificaciones de cliente, puntuación de opiniones de Cognitive Services y Power Query para consultar los documentos. Los datos proceden de los datos de reseñas sobre hoteles en Kaggle.com. 

## <a name="prerequisites"></a>Requisitos previos

+ [Descargar el archivo CSV de reseñas sobre hoteles (HotelReviews_Free. csv)](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Este conjunto de datos contiene registros de comentarios sobre hoteles de los clientes.

+ [Power BI Desktop](https://powerbi.microsoft.com/downloads/)

+ [Azure Search](search-create-service-portal.md). Puede usar un servicio gratuito para realizar el tutorial. 

+ [Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal). Asegúrese de que la cuenta es de "uso general", ya sea *StorageV2 (uso general V2)* , que es el valor predeterminado, o *Storage (uso general V1)* . Elija la misma región de Azure Search.
 
## <a name="prepare-data"></a>Preparación de los datos 

Cargue el archivo .csv en Azure Blob Storage para que un indexador de Azure Search pueda acceder a él.

1. [Inicie sesión en Azure Portal](https://portal.azure.com), vaya a su cuenta de Azure Storage, haga clic en **Blobs** y, después, en **+Contenedor**.

1. [Cree un contenedor de blobs](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) que contenga los datos de ejemplo: 

   1. Asigne al contenedor el nombre `hotel-reviews`. 
   
   1. Establezca el nivel de acceso público en cualquiera de sus valores válidos. Usamos el valor predeterminado.

1. Una vez creado el contenedor, ábralo y seleccione **Cargar** en la barra de comandos.

1. Vaya a la carpeta que contiene **HotelReviews-Free.csv**, seleccione el archivo y haga clic en **Cargar**.

   ![Cargar el archivo .csv](media/knowledge-store-howto-powerbi/hotel-reviews-blob-container.png "Upload the .csv file")

1. Mientras está en Azure Storage, obtenga la cadena de conexión y el nombre del contenedor.  Necesitará estas dos cadenas en la creación de un objeto de origen de datos:

   1. En la página de introducción, haga clic en **Claves de acceso** y copie una *cadena de conexión*. Comienza por `DefaultEndpointsProtocol=https;` y termina con `EndpointSuffix=core.windows.net`. En medio, están el nombre de su cuenta y la clave. 

   1. El nombre del contenedor debe ser `hotel-reviews` u otro nombre que le haya asignado. 

## <a name="create-and-run-ai-enrichments"></a>Creación y ejecución de enriquecimientos de IA

Use el asistente para la importación de datos para crear el almacén de conocimiento. Importará el conjunto de datos, elegirá características enriquecidas, configurará un almacén de conocimiento y un índice y, a continuación, realizará la ejecución.

1. [Busque el servicio de búsqueda](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en Azure Portal.

1. Haga clic en **Importar datos** en la barra de comandos.

1. Cree el objeto de origen de datos en la primera página del asistente.

   - Seleccione **Azure Blob Storage**.

   - Asígnele un nombre al origen de datos, como *hotel-reviews-ds*.

   - Dado que los datos son. csv, seleccione **Texto delimitado** para el modo de análisis, seleccione **La primera línea contiene encabezado**, y asegúrese de que el carácter delimitador sea una coma.

   - Puede obtener la cadena de conexión a su cuenta de Azure Storage en el portal, en **Claves de acceso**.

   - El nombre del contenedor también se puede obtener desde el portal en la lista de Azure Storage Blob.

      ![Creación de un objeto de origen de datos](media/knowledge-store-howto-powerbi/hotel-reviews-ds.png "Create a data source object")

1. Agregue enriquecimientos y configure un almacén de conocimiento en la segunda página del asistente.

   - En **Adjuntar Cognitive Services**, puede usar el recurso gratuito que permite hasta 20 transacciones al día. El número de registros en HotelReviews-Free.csv es inferior a 20.

   - En **Agregar enriquecimiento**, asigne el nombre *hotel-reviews-ss* al conjunto de aptitudes, elija el campo *reviews_text*, seleccione un nivel de granularidad de páginas de *Páginas (5000 fragmentos de caracteres)* y, a continuación, seleccione estas tres aptitudes cognitivas: *Extraer frases clave*, *Detectar idioma* y *Detectar sentimiento*.

      ![Creación de un conjunto de aptitudes](media/knowledge-store-howto-powerbi/hotel-reviews-ss.png "Create a skillset")

   - En **Guardado de enriquecimientos en un almacén de conocimiento**, proporcione la cadena de conexión a la cuenta de Azure Storage de uso general. Al seleccionar las opciones de *Azure table projections* (Proyecciones de tabla de Azure) en esta sección se crea un almacén de conocimiento en Azure Table Storage.

      ![Configuración de almacén de conocimiento](media/knowledge-store-howto-powerbi/hotel-reviews-ks.png "Configure knowledge store")
   
   Haga clic en **Siguiente: Personalizar índice de destino** para continuar con el siguiente paso.

1. Configure un índice para las consultas de búsqueda de texto completo opcionales en Azure Search. Aunque este tutorial se centra en la conexión de Power BI a Azure Table Storage, el asistente para la **importación de datos** también puede crear un índice que se use para la búsqueda de texto completo en Azure Search. 

   El asistente muestrea el origen de datos para inferir campos y tipos de datos, así que lo único que tiene que hacer es seleccionar los atributos necesarios para lograr los comportamientos deseados. Por ejemplo, *Recuperable* significa que el contenido del campo se puede recuperar del servicio, mientras *Buscable* permite la búsqueda de texto completo en los campos seleccionados.

   - Asígnele un nombre al índice, como *hotel-reviews-idx*.
   - Establezca todos los campos como **Recuperables**.
   - Establezca *city*, *name*, *reviews_text*, *language*, *keyphrases* como **Buscable**.
   - Establezca *sentiment*, *language*, *keyphrases* como **Filtrable** y **Clasificable**. 
   
    El índice debe tener un aspecto similar al de la siguiente imagen.

     ![Configuración de un índice](media/knowledge-store-howto-powerbi/hotel-reviews-idx.png "Configure an index")

   Haga clic en **Siguiente: Crear indizador** para continuar con el siguiente paso.

1. Configure un indexador asignándole un nombre y una cadencia de ejecución. En este tutorial, use *hotel-reviews-idxr* como nombre para el indexador y use la programación predeterminada **Una vez** para ejecutar inmediatamente el indexador.

   La ejecución del indexador pone en movimiento a todas las configuraciones anteriores. La extracción, el procesamiento y la ingesta se producen todos en este paso.

1. Supervise la indexación en la cola de notificaciones del portal. La ejecución tarda varios minutos en completarse.

## <a name="connect-with-power-bi"></a>Conexión con Power BI

1. Inicie Power BI Desktop y seleccione **Obtener datos**.

1. En Obtener datos, seleccione **Azure** y después **Azure Table Storage**. Haga clic en **Conectar**.

1. En nombre o dirección URL de la cuenta, pegue el nombre de la cuenta de Azure Storage (la dirección URL completa se resuelve automáticamente).

1. Escriba la clave de cuenta.

1. Seleccione Document, KeyPhrases y Pages. Estas son las tablas que el asistente para la importación de datos crea al seleccionar los mismos elementos con el mismo nombre en la configuración del almacén de conocimiento. Haga clic en **Cargar**.

1. Para abrir Power Query, haga clic en el comando **Editar consultas**.

   ![Abrir Power Query](media/knowledge-store-howto-powerbi/powerbi-edit-queries.png "Open Power Query")

1. Para Documents:

   - Quite las columnas PartitionKey, RowKey y Timestamp creadas por Azure Table Storage. El almacén de conocimiento proporciona las relaciones usadas en este análisis.

   - Expanda la columna de contenido.

     ![Editar tablas](media/knowledge-store-howto-powerbi/powerbi-edit-table.png "Edit tables")

1. Seleccione todos los campos y, a continuación, anule la selección de los que empiezan por "metadatos".

1. Corrija el tipo de datos de las columnas siguientes mediante el icono ABC-123 en cada columna:

   - Las columnas de fecha deben ser **DateTime**
   - La *Latitud* debe ser **Número decimal**
   - La *Longitud* debe ser **Número decimal**

1. Repita los pasos anteriores para KeyPhrases, eliminando PartitionKey y otras columnas, expandiendo columnas de contenido y estableciendo *SentimentScore* en **Número decimal**.

1. Repita de nuevo lo mismo para Pages, eliminando PartitionKey y otras columnas, y expandiendo las columnas de contenido. No hay modificaciones de tipo de datos para esta tabla.

1. Haga **Close and Apply** (Cerrar y aplicar) en el extremo izquierdo de la barra de comandos de Power Query.

1. Asegúrese de que Power BI reconoce las relaciones que el almacén de conocimiento ha creado en los datos. Haga clic en el icono de relaciones en el panel de navegación izquierdo. Las tres tablas deben estar relacionadas. Edite las relaciones y asegúrese de que la "dirección de filtro cruzado" está establecida en ambas, con lo que se asegurará de que todos los objetos visuales se actualicen cuando se aplique un filtro.

   ![Validar relaciones](media/knowledge-store-howto-powerbi/powerbi-relationships.png "Validate relationships")

## <a name="try-with-larger-data-sets"></a>Pruebe con conjuntos de datos más grandes

Hemos mantenido el conjunto de datos pequeño para evitar cargos por la realización de un tutorial de demostración. Para obtener una experiencia más realista, puede crear y, posteriormente, adjuntar un recurso de Cognitive Services facturable para habilitar un mayor número de transacciones con el analizador de opiniones, la extracción de frase clave y las aptitudes del detector de idiomas.

Cree nuevos contenedores en Azure Blob Storage y cargue cada archivo CSV en su propio contenedor. Especifique uno de estos contenedores en el paso de creación de orígenes de datos en el asistente para la importación de datos.

| DESCRIPCIÓN | Vínculo |
|-------------|------|
| Nivel Gratis   | [HotelReviews_Free.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Pequeño (500 registros) | [HotelReviews_Small.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Small.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Medio (6000 registros)| [HotelReviews_Medium.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Medium.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D)
| Grande (conjunto de datos completo con 35 000 registros) | [HotelReviews_Large.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Large.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Tenga en cuenta que los conjuntos de datos grandes son caros de procesar. Este cuesta aproximadamente 1000 dólares estadounidenses.|

En el paso de enriquecimiento del asistente, adjunte un recurso de [Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) facturable, creado en el nivel *S0*, en la misma región que Azure Search para usar conjuntos de datos más grandes. 

  ![Creación de un recurso de Cognitive Services](media/knowledge-store-howto-powerbi/create-cognitive-service.png "Create a Cognitive Services resource")

## <a name="next-steps"></a>Pasos siguientes

Si desea repetir este ejercicio o realizar otro tutorial de enriquecimiento de inteligencia artificial, elimine el indexador *hotel-reviews-idx* que acaba de crear. Al eliminar el indexador, se restablece el contador de transacciones diarias gratis a cero. 

Para ver más tutoriales paso a paso que muestren el almacén de conocimiento, continúe con el artículo siguiente, donde se muestra cómo crear un almacén de conocimiento mediante las API REST y Postman.

> [!div class="nextstepaction"]
> [Introducción al almacén de conocimiento](knowledge-store-howto.md)
