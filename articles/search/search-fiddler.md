---
title: 'Inicio rápido: Postman y las API REST - Azure Search'
description: Obtenga información sobre cómo llamar a las API REST de Azure Search con Postman y datos y definiciones de ejemplo.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: quickstart
ms.date: 05/16/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: bd3b9fe80a57a6a0dd824d92ae14a863ced240b2
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2019
ms.locfileid: "65793540"
---
# <a name="quickstart-explore-azure-search-rest-apis-using-postman"></a>Inicio rápido: Exploración de las API REST de Azure Search mediante Postman
> [!div class="op_single_selector"]
> * [Postman](search-fiddler.md)
> * [C#](search-create-index-dotnet.md)
> * [Python](search-get-started-python.md)
> * [Portal](search-get-started-portal.md)
> * [PowerShell](search-howto-dotnet-sdk.md)
>*

Una de las formas más sencillas de explorar la [API REST de Azure Search](https://docs.microsoft.com/rest/api/searchservice) es usar Postman o cualquier otra herramienta de pruebas web para formular solicitudes HTTP e inspeccionar las respuestas. Con las herramientas adecuadas y estas instrucciones, puede enviar solicitudes y ver las respuestas antes de escribir ningún código.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar y luego [regístrese en Azure Search](search-create-service-portal.md).

## <a name="prerequisites"></a>Requisitos previos

En este inicio rápido se usan los siguientes servicios y herramientas. 

+ [Cree un servicio Azure Search](search-create-service-portal.md) o [busque un servicio existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en su suscripción actual. Puede usar un servicio gratuito para este inicio rápido. 

+ [Aplicación de escritorio Postman](https://www.getpostman.com/) o [Telerik Fiddler](https://www.telerik.com/fiddler): se usa para enviar solicitudes a Azure Search.

## <a name="get-a-key-and-url"></a>Obtención de una clave y una dirección URL

Las llamadas de REST requieren la dirección URL del servicio y una clave de acceso en cada solicitud. Con ambos se crea un servicio de búsqueda, por lo que si ha agregado Azure Search a su suscripción, siga estos pasos para obtener la información necesaria:

1. [Inicie sesión en Azure Portal](https://portal.azure.com/) y en la página **Introducción** del servicio de búsqueda, obtenga la dirección URL. Un punto de conexión de ejemplo podría ser similar a `https://mydemo.search.windows.net`.

1. En **Configuración** > **Claves**, obtenga una clave de administrador para tener derechos completos en el servicio. Se proporcionan dos claves de administrador intercambiables para lograr la continuidad empresarial, por si necesitara sustituir una de ellas. Puede usar la clave principal o secundaria en las solicitudes para agregar, modificar y eliminar objetos.

![Obtención de una clave de acceso y un punto de conexión HTTP](media/search-fiddler/get-url-key.png "Get an HTTP endpoint and access key")

Todas las solicitudes requieren una clave de API en cada solicitud enviada al servicio. Tener una clave válida genera la confianza, solicitud a solicitud, entre la aplicación que envía la solicitud y el servicio que se encarga de ella.

## <a name="connect-to-azure-search"></a>Conexión con Azure Search

En esta sección, utilice la herramienta web que prefiera para configurar las conexiones a Azure Search. Cada herramienta conserva la información de encabezado de solicitud durante la sesión, lo que significa que solo hay que especificar api-key y content-type una vez.

Para cualquiera de las herramientas, tiene que elegir un comando (GET, POST, PUT, etc.), proporcionar un punto de conexión de dirección URL y, para algunas tareas, proporcionar JSON en el cuerpo de la solicitud. Reemplace el nombre de servicio de búsqueda (YOUR-SEARCH-SERVICE-NAME) por un valor válido. 

    https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes?api-version=2019-05-06

Tenga en cuenta el prefijo HTTPS, el nombre del servicio, el nombre de un objeto (en este caso, la colección de índices) y el valor de [api-version](search-api-versions.md). La versión de la API es una cadena obligatoria en minúsculas, que se especifica como `?api-version=2019-05-06` para la versión actual. Las versiones de la API se actualizan periódicamente. La inclusión de api-version en todas las solicitudes proporciona un control total sobre la que se usa.  

La composición del encabezado de solicitud incluye dos elementos, el tipo de contenido y el valor de api-key que se usa para autenticarse en Azure Search. Reemplace la clave de la API de administración (YOUR-ADMIN-API-KEY) por un valor válido. 

    api-key: <YOUR-ADMIN-API-KEY>
    Content-Type: application/json

En Postman, formule una solicitud similar a la de la siguiente captura de pantalla. Elija **GET** como verbo, proporcione la dirección URL y haga clic en **Enviar**. Este comando se conecta a Azure Search, lee la colección de índices y devuelve el código de estado HTTP 200 si la conexión es correcta. Si el servicio ya tiene índices, la respuesta incluirá también las definiciones de índice.

![Encabezado de solicitud de Postman][6]

## <a name="1---create-an-index"></a>1 - Creación de un índice

En Azure Search, normalmente puede crear el índice antes de cargarlo con datos. Para esta tarea se usa la [API REST Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) (Crear índice). 

La dirección URL se extiende para que incluya el nombre del índice `hotel`.

Para hacer esto en Postman:

1. Cambie el verbo a **PUT**.

2. Copie esta dirección URL `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotel?api-version=2019-05-06`.

3. Proporcione la definición del índice (que se muestra a continuación) en el cuerpo de la solicitud.

4. Haga clic en **Enviar**.

![Cuerpo de solicitud de Postman][8]

### <a name="index-definition"></a>Definición de índice

La colección de campos define la estructura del documento. Cada documento debe tener estos campos y cada campo debe tener un tipo de datos. Los campos de cadena se utilizan en la búsqueda de texto completo, por lo que puede convertir los datos numéricos en cadenas si necesita que se puedan realizar búsquedas en dicho contenido.

Los atributos del campo determinan la acción que se permite. Las API de REST permiten muchas acciones de forma predeterminada. Por ejemplo, de manera predeterminada es posible realizar búsquedas en todas las cadenas, se pueden recuperar, crear filtros y se pueden clasificar. A menudo, basta con establecer los atributos cuando haya que desactivar un comportamiento.

          {
         "name": "hotels",  
         "fields": [
           {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
           {"name": "baseRate", "type": "Edm.Double"},
           {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
           {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
           {"name": "hotelName", "type": "Edm.String"},
           {"name": "category", "type": "Edm.String"},
           {"name": "tags", "type": "Collection(Edm.String)"},
           {"name": "parkingIncluded", "type": "Edm.Boolean"},
           {"name": "smokingAllowed", "type": "Edm.Boolean"},
           {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
           {"name": "rating", "type": "Edm.Int32"},
           {"name": "location", "type": "Edm.GeographyPoint"}
          ]
         }

Al enviar esta solicitud, debería aparece una respuesta HTTP 201, que indica que el índice se creó correctamente. Esta acción se puede comprobar esta acción en el portal, pero tenga en cuenta que la página del portal se actualiza periódicamente, por lo que puede tardar un minuto o dos en ponerse al día.

> [!TIP]
> Si obtiene HTTP 504, compruebe que la URL especifique HTTPS. Si se muestra el error HTTP 400 o 404, compruebe el cuerpo de la solicitud para verificar que no haya errores al copiar/pegar. Un HTTP 403 indica normalmente que hay un problema con la clave de API (es una clave no válida o un problema de sintaxis sobre cómo se específica la clave de API).

## <a name="2---load-documents"></a>2 - Carga de documentos

La creación del índice y su rellenado son pasos independientes. En Azure Search, el índice contiene todos los datos en que se pueden realizar búsquedas, y que puede proporcionar como documentos JSON. La API REST [Add, Update, or Delete Documents](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) (Agregar, actualizar o eliminar documentos) se usa para esta tarea. 

La dirección URL se extiende para que incluya las colecciones de `docs` y la operación `index`.

Para hacer esto en Postman:

1. Cambie el verbo a **POST**.

2. Copie esta dirección URL `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels/docs/index?api-version=2019-05-06`.

3. Proporcione los documentos JSON (que se muestran a continuación) en el cuerpo de la solicitud.

4. Haga clic en **Enviar**.

![Carga de la solicitud de Postman][10]

### <a name="json-documents-to-load-into-the-index"></a>Documentos JSON para cargar en el índice

El cuerpo de la solicitud contiene cuatro documentos que se van agregar al índice de hoteles.

         {
         "value": [
         {
             "@search.action": "upload",
             "hotelId": "1",
             "baseRate": 199.0,
             "description": "Best hotel in town",
             "description_fr": "Meilleur hôtel en ville",
             "hotelName": "Fancy Stay",
             "category": "Luxury",
             "tags": ["pool", "view", "wifi", "concierge"],
             "parkingIncluded": false,
             "smokingAllowed": false,
             "lastRenovationDate": "2010-06-27T00:00:00Z",
             "rating": 5,
             "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "2",
             "baseRate": 79.99,
             "description": "Cheapest hotel in town",
             "description_fr": "Hôtel le moins cher en ville",
             "hotelName": "Roach Motel",
             "category": "Budget",
             "tags": ["motel", "budget"],
             "parkingIncluded": true,
             "smokingAllowed": true,
             "lastRenovationDate": "1982-04-28T00:00:00Z",
             "rating": 1,
             "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "3",
             "baseRate": 279.99,
             "description": "Surprisingly expensive",
             "hotelName": "Dew Drop Inn",
             "category": "Bed and Breakfast",
             "tags": ["charming", "quaint"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
           },
           {
             "@search.action": "upload",
             "hotelId": "4",
             "baseRate": 220.00,
             "description": "This could be the one",
             "hotelName": "A Hotel for Everyone",
             "category": "Basic hotel",
             "tags": ["pool", "wifi"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
           }
          ]
         }

En unos pocos segundos debe ver una respuesta HTTP 200 en la lista de sesiones. Esto indica que los documentos se crearon correctamente. 

Si obtiene un 207, al menos un documento no pudo cargarse. Si aparece el error 404, significa que se ha producido un error de sintaxis en el encabezado o en el cuerpo de la solicitud: compruebe que ha cambiado el punto de conexión para que incluya `/docs/index`.

> [!Tip]
> Para los orígenes de datos seleccionados, puede elegir el enfoque de *indizador* alternativo, que simplifica y reduce la cantidad de código necesario para la indexación. Para más información, consulte [Indexer operations](https://docs.microsoft.com/rest/api/searchservice/indexer-operations)(Operaciones del indexador).


## <a name="3---search-an-index"></a>3 - Búsqueda en un índice

Ahora que se han cargado el índice y los documentos, puede emitir consultas con ellos mediante la API REST [Search Documents](https://docs.microsoft.com/rest/api/searchservice/search-documents) (Buscar documentos).

La dirección URL se extiende para que incluya una cadena de consulta que se especifica mediante el operador de búsqueda.

Para hacer esto en Postman:

1. Cambie el verbo a **GET**.

2. Copie esta dirección URL `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels/docs?search=motel&$count=true&api-version=2019-05-06`.

3. Haga clic en **Enviar**.

Esta consulta busca el término "motel" y devuelve el número de documentos en los resultados de la búsqueda. La solicitud y respuesta deben ser similares a las de la siguiente captura de pantalla de Postman tras hacer clic en **Send** (Enviar). El código de estado debe ser 200.

 ![Respuesta a la consulta de Postman][11]


## <a name="get-index-properties"></a>Obtención de las propiedades del índice
También puede consultar la información del sistema para obtener recuentos de documentos y consumo de almacenamiento:`https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels/stats?api-version=2019-05-06`

En Postman, la solicitud debe ser similar al siguiente y la respuesta incluye el número de documentos y el espacio utilizado, en bytes.

 ![Consulta del sistema de Postman][12]

Observe que la sintaxis de api-version es distinta. En esta solicitud, utilice `?` para anexar api-version. `?` separa la ruta de acceso de la dirección URL de la cadena de consulta, mientras que & separa cada par " nombre = valor " par en la cadena de consulta. En esta consulta, api-version es el primer y único elemento de la cadena de consulta.

Para más información acerca de esta API, consulte la [API REST Get Index Statistics](https://docs.microsoft.com/rest/api/searchservice/get-index-statistics) (Obtener estadísticas de índice).


## <a name="use-fiddler"></a>Uso de Fiddler

Esta sección es equivalente a las secciones anteriores, solo que con capturas de pantalla e instrucciones de Fiddler

### <a name="connect-to-azure-search"></a>Conexión con Azure Search

Formule una solicitud similar a la de la siguiente captura de pantalla. Elija **GET** como verbo. Fiddler agrega `User-Agent=Fiddler`. Los dos encabezados de solicitud adicionales se pueden pegar en las líneas nuevas que hay debajo. Incluya el tipo de contenido y la clave de api del servicio, para lo que debe usar la clave de acceso de administrador del servicio.

Para el destino, copie una versión modificada de esta dirección URL: `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes?api-version=2019-05-06`

![Encabezado de la solicitud de Fiddler][1]

> [!Tip]
> Desactive el tráfico web para ocultar actividad HTTP extraña no relacionada. En el menú **File** (Archivo) de Fiddler, desactive **Capture Traffic** (Capturar tráfico). 

### <a name="1---create-an-index"></a>1 - Creación de un índice

Cambie el verbo a **PUT**. Copia en una versión modificada de esta dirección URL: `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotel?api-version=2019-05-06`. Copie la definición del índice que se proporcionó anteriormente en el cuerpo de solicitud. La página debe tener un aspecto similar a la siguiente captura de pantalla. Haga clic en **Ejecutar** en la parte superior derecha para enviar la solicitud completada.

![Cuerpo de la solicitud de Fiddler][7]

### <a name="2---load-documents"></a>2 - Carga de documentos

Cambie el verbo a **POST**. Cambie la dirección URL para que incluya `/docs/index`. Copie los documentos en el cuerpo de la solicitud, tal como se muestra en la siguiente captura de pantalla y, después, ejecute la solicitud.

![Carga de la solicitud de Fiddler][9]

### <a name="tips-for-running-our-sample-queries-in-fiddler"></a>Sugerencias para ejecutar consultas de ejemplo en Fiddler

La siguiente consulta de ejemplo proviene del artículo [Search Documents REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) (API REST de búsqueda de documentos). Muchas de las consultas de ejemplo de este artículo incluyen espacios, algo que no está permitido en Fiddler. Reemplace cada espacio por un carácter + antes de pegar la cadena de la consulta e intentar la consulta en Fiddler:

**Los espacios anteriores se sustituyen (en lastRenovationDate desc):**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2019-05-06

**Los espacios posteriores se sustituyen por + (en lastRenovationDate+desc):**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2019-05-06

### <a name="tips-for-viewing-index-statistic-in-fiddler"></a>Sugerencias para ver estadísticas de índices en Fiddler

En Fiddler, haga clic en la pestaña **Inspectors** (Inspectores), luego en **Headers** (Encabezados) y, finalmente, seleccione el formato JSON. Debe ver el recuento de documentos y el tamaño del almacenamiento (en KB).

## <a name="next-steps"></a>Pasos siguientes

Los clientes de REST son muy valiosos para la exploración improvisada, pero ahora que sabe cómo funcionan las API de REST, puede avanzar en el código. Para conocer los pasos siguientes, consulte los siguientes vínculos:

+ [Inicio rápido: Creación de un índice con el SDK de .NET](search-create-index-dotnet.md)
+ [Inicio rápido: Creación de un índice (REST) mediante PowerShell](search-create-index-rest-api.md)

<!--Image References-->
[1]: ./media/search-fiddler/fiddler-url.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
[6]: ./media/search-fiddler/postman-url.png
[7]: ./media/search-fiddler/fiddler-request.png
[8]: ./media/search-fiddler/postman-request.png
[9]: ./media/search-fiddler/fiddler-docs.png
[10]: ./media/search-fiddler/postman-docs.png
[11]: ./media/search-fiddler/postman-query.png
[12]: ./media/search-fiddler/postman-system-query.png