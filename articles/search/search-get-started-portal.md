---
title: Tutorial sobre indexación, consulta y filtrado en Azure Search mediante el portal | Microsoft Docs
description: En este tutorial, se usan Azure Portal y datos de ejemplo predefinidos para generar un índice en Azure Search. Explore la búsqueda de texto completo, los filtros, las facetas, la búsqueda aproximada, la búsqueda geográfica y más.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.topic: tutorial
ms.date: 07/10/2018
ms.author: heidist
ms.openlocfilehash: 0eb6701a7ea08c2dd63bd8b5d7d7c805e6eb1376
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365092"
---
# <a name="tutorial-use-built-in-tools-for-azure-search-indexing-and-queries"></a>Tutorial: Uso de herramientas integradas para la indexación de Azure Search y consultas

En una página de servicio de Azure Search en Azure Portal, puede usar herramientas integradas para pruebas de concepto y una experiencia práctica con un mínimo de esfuerzo. Las herramientas del portal no ofrecen una paridad completa con las API de REST y .NET, pero, para las pruebas rápidas de prueba de concepto, los asistentes y los editores proporcionan una asistencia fácil. Esta introducción sin código le permitirá comenzar a trabajar con un conjunto de datos predefinidos de modo que pueda escribir consultas interesantes de forma inmediata. 

> [!div class="checklist"]
> * Comience con datos de ejemplo públicos y genere automáticamente un índice de Azure Search mediante el asistente para **importación de datos**. 
> * Vea el esquema de índice y los atributos de todos los índices publicados en Azure Search.
> * Explore la búsqueda de texto completo, los filtros, las facetas, la búsqueda aproximada y la búsqueda geográfica con el **Explorador de búsqueda**.  

Las herramientas del portal no admiten la gama completa de funcionalidades de Azure Search. Si las herramientas son demasiado limitadas, consulte la [introducción basada en código a la programación de Azure Search en .NET](search-howto-dotnet-sdk.md) o considere las [herramientas de pruebas web para realizar llamadas de la API de REST](search-fiddler.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. También puede ver una demostración de seis minutos de los pasos de este tutorial, comenzando por este [vídeo de introducción a Azure Search](https://channel9.msdn.com/Events/Connect/2016/138) de aproximadamente tres minutos.

## <a name="prerequisites"></a>Requisitos previos

[Cree un servicio Azure Search](search-create-service-portal.md) o busque un servicio existente en su suscripción actual. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Abra el panel del servicio Azure Search. Si no ancló el icono del servicio en el panel, pude encontrar su servicio de esta manera: 
   
   * En la barra de accesos, haga clic en **Todos los servicios** en el panel de navegación izquierdo.
   * En el cuadro de búsqueda, escriba *search* (búsqueda) para obtener una lista de servicios de búsqueda para su suscripción. Haga clic en **Search services** (Servicios de búsqueda). El servicio debe aparecer en la lista. 

### <a name="check-for-space"></a>Búsqueda de espacio
Muchos clientes comienzan con el servicio gratis. Esta versión está limitada a tres índices, tres orígenes de datos y tres indexadores. Asegúrese de que tiene espacio para elementos adicionales antes de empezar. Este tutorial crea uno de cada objeto. 

> [!TIP] 
> Los iconos del panel de servicio muestran el número de índices, indexadores y orígenes de datos que ya tiene. El icono de indexador muestra indicadores de acierto y error. Haga clic en el icono para ver el recuento de indexadores. 
>
> ![Iconos para indexadores y orígenes de datos][1]
>

## <a name="create-index"></a> Creación de un índice y carga de datos
Las consultas de búsqueda recorren en iteración un [*índice*](search-what-is-an-index.md) que contiene los datos de búsqueda, los metadatos y las construcciones usados para optimizar determinados comportamientos de búsqueda.

Para mantener esta tarea en el portal, usamos un conjunto de datos de ejemplo integrado que se puede rastrear mediante un [*indexador*](search-indexer-overview.md) con el Asistente para la **importación de datos**. Un indexador es un rastreador específico del origen que puede leer los metadatos y el contenido de los orígenes de datos de Azure admitidos. En el código, puede crear y administrar indexadores como recursos independientes. En el portal, los indexadores se exponen mediante el asistente para la **importación de datos**. 

#### <a name="step-1-start-the-import-data-wizard"></a>Paso 1: Inicio del Asistente para la importación de datos
1. En el panel del servicio Azure Search, haga clic en **Importar datos** en la barra de comandos para iniciar un asistente que crea y rellena un índice.
   
    ![Comando de importación de datos][2]

2. En el asistente, haga clic en **Conectar a los datos** > **Ejemplos** > **realestate-us-sample**. Este origen de datos está preconfigurado con el nombre, el tipo y la información de conexión. Una vez creado, se convierte en un "origen de datos existente" que se puede reutilizar en otras operaciones de importación.

    ![Selección del conjunto de datos de ejemplo][9]

3. Haga clic en **Aceptar** para usarlo.

#### <a name="skip-cognitive-skills"></a>Omitir conocimientos cognitivos

**Importar datos** incluye un paso opcional de aptitud cognitiva que agrega algoritmos de inteligencia artificial para la indexación. Esta característica no se trata en este tutorial, por lo que debe ir directamente a **Personalización del índice de destino**. Si tiene curiosidad acerca de la nueva característica de la versión preliminar de la búsqueda cognitiva en Azure Search, pruebe con el [inicio rápido de la búsqueda cognitiva](cognitive-search-quickstart-blob.md) o con el [tutorial](cognitive-search-tutorial-blob.md).

   ![Omitir el paso sobre aptitud cognitiva][11]

#### <a name="step-2-define-the-index"></a>Paso 2: Definición del índice
La creación de un índice es normalmente una tarea manual y basada en código, pero el asistente puede generar un índice para cualquier origen de datos que pueda rastrear. Como mínimo, un índice necesita un nombre y una colección de campos, uno de los cuales debe estar marcado como la clave de documento para identificar de forma única cada documento.

Los campos tienen tipos de datos y atributos. Las casillas de la parte superior son *atributos de índice* que controlan cómo se usa el campo. 

* **Retrievable** significa que se muestra en la lista de resultados de búsqueda. Puede desactivar esta casilla para marcar los campos individuales como fuera de los resultados de búsqueda, por ejemplo, cuando los campos se usan solo en expresiones de filtro. 
* **Filterable**, **Sortable** y **Facetable** determinan si un campo se puede usar en un filtro, una ordenación o una estructura de navegación de facetas. 
* **Searchable** significa que se incluye un campo en la búsqueda de texto completo. Las cadenas permiten realizar búsquedas. Los campos numéricos y los booleanos a menudo se marcan como no utilizables en búsquedas. 

De forma predeterminada, el asistente busca en el origen de datos identificadores únicos como base para el campo de clave. Las cadenas se atribuyen como que se pueden recuperar y permiten realizar búsquedas. Los enteros se atribuyen como que se pueden recuperar, filtrar, ordenar y clasificar.

  ![Índice realestate generado][3]

Haga clic en **Aceptar** para crear el índice.

#### <a name="step-3-define-the-indexer"></a>Paso 3: Definición del indexador
Aún en el **Asistente para la importación de datos**, haga clic en **Indexador** > **Nombre** y escriba un nombre para el indexador. 

Este objeto define un proceso ejecutable. Podría colocarlo en una programación recurrente; sin embargo, por ahora, use la opción predeterminada para ejecutar el indexador enseguida, inmediatamente, cuando haga clic en **Aceptar**.  

  ![indexador realestate][8]

## <a name="check-progress"></a>Comprobación del progreso
Para supervisar la importación de datos, vuelva al panel de servicio, desplácese hacia abajo y haga doble clic en el icono **Indexadores** para abrir la lista de indexadores. Debe aparecer en la lista el indexador recién creado, con el estado "en curso" o correcto, junto con el número de documentos indexados.

   ![Mensaje de progreso del indexador][4]

## <a name="view-the-index"></a>Ver el índice

Los iconos en el panel de servicio proporcionan información de resumen así como acceso a información detallada. Por ejemplo, en el icono **Índices**, debería ver una lista de los índices existentes, incluido el índice *realestate-us-sample* que acaba de crear en el paso anterior.

Haga clic en el índice *realestate-us-sample* ahora para ver las opciones de portal de su definición. Una opción **Agregar/editar campos** le permite crear nuevos campos de atributos completos. Los campos existentes tienen una representación física en Azure Search y, por tanto, no son modificables, ni siquiera en el código. Para cambiar mucho un campo, cree uno nuevo y elimine el original. 

   ![definición de índice de ejemplo][10]

Otras construcciones, como los perfiles de puntuación y las opciones de CORS, pueden agregarse en cualquier momento. 

Para saber claramente qué puede y qué no puede modificar durante el diseño del índice, tómese un minuto para ver las opciones de definición de índice. Las opciones atenuadas son un indicador de que un valor no se puede modificar o eliminar.

## <a name="query-index"></a> Consulta del índice
Más adelante, debería tener ahora un índice de búsqueda que esté preparado para la consulta con la página de consulta del [**Explorador de búsqueda**](search-explorer.md) integrada. Proporciona un cuadro de búsqueda para que pueda probar las cadenas de consulta arbitraria. 

> [!TIP]
> En el [vídeo de introducción a Azure Search](https://channel9.msdn.com/Events/Connect/2016/138), los siguientes pasos se demuestran en 6 minutos y 8 segundos.
>

1. Haga clic en **Explorador de búsqueda** en la barra de comandos.

   ![Comando del explorador de búsqueda][5]

2. Haga clic en **Change index** (Cambiar índice) en la barra de comandos para cambiar a *realestate-us-sample*. Haga clic en **Definir versión de API** en la barra de comandos para ver cuáles son las API de REST que están disponibles. Con las siguientes consultas, use la versión disponible con carácter general (2017-11-11). 

   ![Comandos de índice y API][6]

3. En la barra de búsqueda, escriba las cadenas de consulta siguientes y haga clic en **Buscar**.

    > [!NOTE]
    > El **Explorador de búsqueda** solo está preparado para controlar la [solicitud de API de REST](https://docs.microsoft.com/rest/api/searchservice/search-documents). Acepta [sintaxis de consulta simple](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) y el [analizador de consultas completo de Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), además de todos los parámetros de búsqueda disponibles en las operaciones de [búsqueda en documentos](https://docs.microsoft.com/rest/api/searchservice/search-documents).
    > 


#### <a name="example-string-searchseattle"></a>Ejemplo (cadena): `search=seattle`

+ El parámetro **search** se usa para la entrada de la búsqueda con una palabra clave en la búsqueda de texto completo; en este caso, se devuelven listados de King County, en el estado de Washington, que contiene *Seattle* en los campos del documento que permiten la búsqueda. 

+ El **Explorador de búsqueda** devuelve resultados en JSON, que es detallado y difícil de leer si los documentos tienen una estructura densa. Este comportamiento es deliberado; la visibilidad de todo el documento es importante para el desarrollo, especialmente durante las pruebas. Para una mejor experiencia de usuario, deberá escribir código que [trate los resultados de la búsqueda](search-pagination-page-layout.md) para hacer destacar los elementos importantes.

+ Los documentos se componen de todos los campos marcados como recuperables en el índice. Para ver los atributos del índice en el portal, haga clic en *realestate-us-sample* en el icono **Índices**.

#### <a name="example-parameterized-searchseattlecounttruetop100"></a>Ejemplo (con parámetros): `search=seattle&$count=true&$top=100`

+ El símbolo **&** se usa para anexar parámetros de búsqueda, que pueden especificarse en cualquier orden. 

+  El parámetro **$count=true** devuelve un recuento de la suma de todos los documentos devueltos. Este valor aparece en la parte superior de los resultados de búsqueda. Puede comprobar las consultas de filtro mediante la supervisión de los cambios notificados por **$count=true**. Los recuentos más pequeños indican que su filtro está funcionando.

+ El parámetro **$top=100** devuelve los 100 documentos mejor clasificados del total. De forma predeterminada, Azure Search devuelve las primeras 50 mejores coincidencias. Puede aumentar o disminuir la cantidad mediante **$top**.

## <a name="filter-query"></a> Filtro de la consulta

Los filtros se incluyen en las solicitudes de búsqueda al anexar el parámetro **$filter**. 

#### <a name="example-filtered-searchseattlefilterbeds-gt-3"></a>Ejemplo (filtrado): `search=seattle&$filter=beds gt 3`

+ El parámetro **$filter** devuelve resultados que coinciden con los criterios que proporcionó. En este caso, más de 3 dormitorios. 

+ La sintaxis de filtro es una construcción de OData. Para más información, consulte la [sintaxis de filtro de OData](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

## <a name="facet-query"></a> Faceta de la consulta

Los filtros de faceta se incluyen en las solicitudes de búsqueda. Puede usar el parámetro de faceta para devolver un recuento agregado de los documentos que coincidan con el valor de faceta que proporcione. 

#### <a name="example-faceted-with-scope-reduction-searchfacetcitytop2"></a>Ejemplo (con faceta con reducción del ámbito): `search=*&facet=city&$top=2`

+ **search=*** es una búsqueda vacía. Las búsquedas vacías buscan en todo. Una de las razones para enviar una búsqueda vacía es filtrar o clasificar el conjunto completo de documentos. Por ejemplo, quiere una estructura de navegación en facetas que conste de todas las ciudades del índice.

+  **facet** devuelve una estructura de navegación que puede pasar a un control de interfaz de usuario. Devuelve categorías y un recuento. En este caso, las categorías se basan en el número de ciudades. No hay ninguna agregación en Azure Search, pero puede aproximarla mediante `facet`, que proporciona un recuento de documentos de cada categoría.

+ **$top=2** devuelve dos documentos, lo cual muestra que puede usar `top` para reducir o aumentar los resultados.

#### <a name="example-facet-on-numeric-values-searchseattlefacetbeds"></a>Ejemplo (faceta en valores numéricos): `search=seattle&facet=beds`**

+ Esta consulta es una faceta de "beds", en una búsqueda de texto de *Seattle*. El término *beds* se puede especificar como faceta, porque el campo está marcado como que se puede recuperar, filtrar y clasificar en el índice, y los valores que contiene (numéricos del 1 al 5), son adecuados para clasificar los listados en grupos (listados con 3 dormitorios, 4 dormitorios). 

+ Solo los campos que se pueden filtrar se pueden clasificar. Solo se pueden devolver en los resultados campos recuperables.

## <a name="highlight-query"></a> Resaltado

El proceso de resaltado de referencias se refiere al formato en el texto que coincide con la palabra clave, dadas las coincidencias encontradas en un campo determinado. Si el término de búsqueda está profundamente enterrado en una descripción, puede agregar resaltado de referencias para que sea más fácil detectarlo. 

#### <a name="example-highlighter-searchgranite-countertopshighlightdescription"></a>Ejemplo (con resaltado): `search=granite countertops&highlight=description`

+ En este ejemplo, la frase con formato *granite countertops* es más fácil de detectar en el campo de descripción.

#### <a name="example-linguistic-analysis-searchmicehighlightdescription"></a>Ejemplo (Análisis lingüístico): `search=mice&highlight=description`

+ La búsqueda de texto completo busca formas de palabras con una semántica parecida. En este caso, los resultados de búsqueda contienen texto resaltado para "mouse", para hogares que tienen infestación de ratones, en respuesta a una búsqueda por palabra clave de "mice". Diferentes formas de la misma palabra pueden aparecer en los resultados debido al análisis lingüístico. 

+ Azure Search admite 56 analizadores de Lucene y Microsoft. El valor predeterminado usado por Azure Search es el analizador de Lucene estándar. 

## <a name="fuzzy-search"></a> Probar la búsqueda aproximada

De forma predeterminada, los términos de consulta mal escritos, como *samamish* en referencia a la meseta de Samammish de la zona de Seattle, no devuelven coincidencias en una búsqueda normal. El siguiente ejemplo no devuelve resultados.

#### <a name="example-misspelled-term-unhandled-searchsamamish"></a>Ejemplo (término mal escrito, no controlado): `search=samamish`

Para controlar los errores de ortografía, puede usar la búsqueda aproximada. La búsqueda aproximada se habilita cuando se usa la sintaxis de consulta de Lucene completa, que se produce al hacer dos cosas: establecer **queryType=full** en la consulta y anexar **~** a la cadena de búsqueda. 

#### <a name="example-misspelled-term-handled-searchsamamishquerytypefull"></a>Ejemplo (término mal escrito, controlado): `search=samamish~&queryType=full`

Ahora, este ejemplo devuelve documentos que contengan coincidencias con "Sammamish".

Cuando **queryType** no se especifica, se usa el analizador de consultas sencillo de manera predeterminada. El analizador de consultas sencillo es más rápido, pero si necesita búsqueda aproximada, expresiones regulares, búsqueda de proximidad u otros tipos de consultas avanzadas, necesitará la sintaxis completa. 

La búsqueda aproximada y la búsqueda con caracteres comodín tienen implicaciones en los resultados de búsqueda. No se realiza un análisis lingüístico en estos formatos de consulta. Antes de usar la búsqueda con caracteres comodín y la búsqueda aproximada, consulte [Cómo funciona la búsqueda de texto completo en Azure Search](search-lucene-query-architecture.md#stage-2-lexical-analysis) y busque la sección sobre las excepciones para el análisis léxico.

Para más información sobre los escenarios de consulta habilitados por el analizador de consultas completo, consulte [Lucene query syntax in Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) (Sintaxis de consulta de Lucene en Azure Search).

## <a name="geo-search"></a> Prueba de la búsqueda geoespacial

Se admite la búsqueda geoespacial mediante el [tipo de dato edm.GeographyPoint](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) en un campo que contenga coordenadas. La búsqueda geográfica es un tipo de filtro, que se especifica en la [sintaxis de filtro de OData](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search). 

#### <a name="example-geo-coordinate-filters-searchcounttruefiltergeodistancelocationgeographypoint-122121513-47673988-le-5"></a>Ejemplo (filtros de coordenadas geográficas): `search=*&$count=true&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5`

La consulta de ejemplo filtra todos los resultados por datos de posición, donde los resultados sean inferiores a 5 kilómetros desde un punto dado (especificado como coordenadas de latitud y longitud). Al agregar **$count**, verá cuántos resultados se devuelven al cambiar la distancia o las coordenadas. 

La búsqueda geoespacial resulta útil si su aplicación de búsqueda tiene una característica "buscar cerca de mí" o utiliza la navegación mediante mapas. No obstante, no es una búsqueda de texto completo. Si tiene requisitos de usuario de buscar en una ciudad o país por nombre, agregue campos que contengan nombres de ciudades o de países, además de coordenadas.

## <a name="takeaways"></a>Puntos clave

Este tutorial muestra los pasos básicos para usar el Asistente para **importación de datos** y el **Explorador de búsqueda** en Azure Portal.

Como fundamento subyacente en el Asistente para importación de datos, ha aprendido acerca de los [indexadores](search-indexer-overview.md), así como del flujo de trabajo básico para el diseño de índices, incluidas las [modificaciones admitidas en un índice publicado](ttps://docs.microsoft.com/rest/api/searchservice/update-index). 

Ha aprendido la sintaxis de consulta mediante ejemplos prácticos de las funcionalidades clave, como los filtros, el resaltado de referencias, la búsqueda aproximada y la búsqueda geográfica.

Por último, ha aprendido a obtener información, haciendo clic en los iconos del panel para cualquier índice, indexador u origen de datos que cree para su suscripción. Más adelante, cuando trabaje con sus propios índices o con aquellos creados por sus compañeros, podrá usar el portal para comprobar rápidamente una definición de origen de datos o la construcción de una colección de campos, sin tener que examinar código desconocido.

## <a name="clean-up-resources"></a>Limpieza de recursos

La manera más rápida de borrar el contenido después de un tutorial es eliminar el grupo de recursos que contiene el servicio Azure Search. Para eliminar de forma definitiva todo lo que contenga el grupo de recursos, elimine el grupo. En el portal, el nombre del grupo de recursos está en la página Información general de cada servicio Azure Search.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información adicional sobre la exploración basada en herramientas de Azure Search, considere el uso de una herramienta de prueba de REST como Postman o Fiddler:

> [!div class="nextstepaction"]
> [Herramientas de prueba web para llamar a las API de REST de Azure Search](search-fiddler.md)


<!--Image references-->
[1]: ./media/search-get-started-portal/tiles-indexers-datasources2.png
[2]: ./media/search-get-started-portal/import-data-cmd2.png
[3]: ./media/search-get-started-portal/realestateindex2.png
[4]: ./media/search-get-started-portal/indexers-inprogress2.png
[5]: ./media/search-get-started-portal/search-explorer-cmd2.png
[6]: ./media/search-get-started-portal/search-explorer-changeindex-se2.png
[7]: ./media/search-get-started-portal/search-explorer-query2.png
[8]: ./media/search-get-started-portal/realestate-indexer2.png
[9]: ./media/search-get-started-portal/import-datasource-sample2.png
[10]: ./media/search-get-started-portal/sample-index-def.png
[11]: ./media/search-get-started-portal/skip-cog-skill-step.png