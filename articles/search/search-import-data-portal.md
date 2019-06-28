---
title: 'Importación de datos en un índice de búsqueda con Azure Portal: Azure Search'
description: Aprenda a usar el Asistente para la importación de datos en Azure Portal para rastrear datos de Azure desde Cosmos DB, Blob Storage, Table Storage, SQL Database y SQL Server en máquinas virtuales de Azure.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: a0eefe38fdffd04bb95826f960771bd6430ea687
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65024775"
---
# <a name="import-data-wizard-for-azure-search"></a>Asistente para la importación de datos de Azure Search

Azure Portal incluye un asistente para **importar datos** en el panel de Azure Search que le permite cargar datos en un índice. Internamente, el asistente configura e invoca un *origen de datos*, un *índice* y un *indexador* mediante la automatización de varios pasos del proceso de indexación: 

* Se conecta a un origen de datos externo en la misma suscripción de Azure.
* Opcionalmente, integra el reconocimiento óptico de caracteres o el procesamiento de lenguaje natural para extraer texto de datos no estructurados.
* Genera un índice basado en el muestreo de datos y los metadatos del origen de datos externo.
* Rastrea contenido que permite búsquedas en el origen de datos, serializando y cargando documentos JSON en el índice.

El asistente no puede conectarse a un índice predefinido ni ejecutar un indexador existente, pero en el propio asistente puede configurar un nuevo índice o indexador para admitir la estructura y los comportamientos que necesite.

¿Es la primera vez que usa Azure Search? Consulte el artículo [Inicio rápido: Uso de herramientas del portal para la importación, indexación y consultas](search-get-started-portal.md) para probar la importación e indexación mediante la opción **Importar datos** y el conjunto de datos real estate de ejemplo integrado.

## <a name="start-importing-data"></a>Inicio de la importación de datos

En esta sección se explica cómo iniciar el asistente y se ofrece una descripción general de cada paso.

1. En [Azure Portal](https://portal.azure.com), abra la página del servicio de búsqueda desde el panel o [busque el servicio](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en la lista.

2. En la página de información general del servicio en la parte superior, haga clic en **Importar datos**.

   ![Comando de importación de datos en el portal](./media/search-import-data-portal/import-data-cmd2.png "Iniciar el asistente para la importación de datos")

   > [!NOTE]
   > Puede iniciar **Importar datos** desde otros servicios de Azure, como Azure Cosmos DB, Azure SQL Database y Azure Blob Storage. Busque **Agregar Azure Search** en el panel de navegación izquierdo en la página de información general del servicio.

3. El asistente se abrirá en **Conexión a los datos**, donde puede elegir un origen de datos externo que se usará para esta importación. Hay varios aspectos que deben tenerse en cuenta en este paso, por lo que se recomienda encarecidamente que lea la sección [Entradas de origen de datos](#data-source-inputs) para obtener información detallada.

   ![Asistente para la importación de datos en el portal](./media/search-import-data-portal/import-data-wizard-startup.png "Asistente para la importación de datos de Azure Search")

4. El siguiente paso es **Agregar Cognitive Search**, en caso de que desee incluir reconocimiento óptico de caracteres (OCR) de texto en archivos de imagen o análisis de texto en datos no estructurados. Para esta tarea se extraen algoritmos de inteligencia artificial de Cognitive Services. Este paso se divide en dos partes:
  
   En primer lugar, [asocie un recurso de Cognitive Services](cognitive-search-attach-cognitive-services.md) con un conjunto de aptitudes de Azure Search.
  
   A continuación, seleccione qué enriquecimientos de inteligencia artificial incluir en el conjunto de aptitudes. Para ver un tutorial de demostración, consulte este [artículo de inicio rápido](cognitive-search-quickstart-blob.md).

   Si solo desea importar datos, omita este paso y vaya directamente a la definición del índice.

5. El siguiente paso es **Personalizar índice de destino**, donde puede aceptar o modificar el esquema de índice que se presenta en el asistente. El asistente deduce los campos y tipos de datos mediante el muestreo de datos y la lectura de metadatos del origen de datos externo.

   Para cada campo, [compruebe los atributos de índice](#index-definition) para habilitar comportamientos específicos. Si no selecciona ningún atributo, el índice no se podrá utilizar. 

6. El siguiente paso es **Crear un indexador**, que es un producto de este asistente. Un indexador es un rastreador que extrae metadatos y datos que permiten búsquedas de un origen de datos externo de Azure. Al seleccionar el origen de datos y asociar conjuntos de aptitudes (opcionalmente) y un índice, ha ido configurando un indexador mientras completaba cada paso del asistente.

   Asigne un nombre al indexador y haga clic en **Enviar** para iniciar el proceso de importación. 

Puede supervisar la indexación en el portal. Para ello, haga clic en el indexador en la lista **Indexadores**. A medida que se carguen documentos, aumentará el recuento de documentos para el índice que se ha definido. A veces, el portal tarda unos minutos en actualizarse.

El índice está listo para que se realicen consultas en cuanto se carga el primer documento. El [Explorador de búsqueda](search-explorer.md) se puede usar para esta tarea.

<a name="data-source-inputs"></a>

## <a name="data-source-inputs"></a>Entradas de origen de datos

El asistente para la **importación de datos** crea un objeto de origen de datos persistente al especificar información de conexión a un origen de datos externo. El objeto de origen de datos se utiliza exclusivamente con [indexadores](search-indexer-overview.md) y puede crearse para los orígenes de datos siguientes: 

* [SQL de Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Azure Table Storage](search-howto-indexing-azure-tables.md) (no es compatible con canalizaciones de [búsquedas cognitivas](cognitive-search-concept-intro.md))

Solo se puede importar desde una única tabla, vista de base de datos o estructura de datos equivalente; sin embargo, la estructura puede incluir subestructuras jerárquicas o anidadas. Para más información, consulte el artículo sobre la [Cómo modelar tipos complejos](search-howto-complex-data-types.md).

Debe crear esta estructura de datos antes de ejecutar el asistente y debe incluir contenido. No ejecute el asistente para **Importar datos** en un origen de datos vacío.

|  Número de selección | DESCRIPCIÓN |
| ---------- | ----------- |
| **Origen de datos existente** |Si ya tiene indexadores definidos en el servicio de búsqueda, puede seleccionar una definición de origen de datos existente para otra importación. En Azure Search, solo los indexadores usan objetos de origen de datos. Puede crear un objeto de origen de datos mediante programación o con el asistente para la **importación de datos**.|
| **Muestras**| Azure Search hospeda una base de datos de Azure SQL global y gratuita que puede usar para obtener información acerca de las solicitudes de importación y consulta en Azure Search. Consulte [Quickstart: Uso de herramientas del portal para la importación, indexación y consultas](search-get-started-portal.md) para ver un tutorial. |
| **Azure SQL Database** |Se puede especificar el nombre del servicio, las credenciales de un usuario de base de datos con permiso de lectura y un nombre de base de datos en la página o a través de una cadena de conexión de ADO.NET. Elija la opción de cadena de conexión para ver o personalizar las propiedades. <br/><br/>En la página debe especificarse la tabla o vista que proporciona el conjunto de filas. Esta opción aparece una vez realizada correctamente la conexión, lo que proporciona una lista desplegable para que pueda realizar una selección. |
| **SQL Server en máquinas virtuales de Azure** |Especifique un nombre de servicio completo, el identificador de usuario y la contraseña, y la base de datos como una cadena de conexión. Para utilizar este origen de datos, debe haber instalado un certificado en el almacén local que cifra la conexión. Para obtener instrucciones, consulte [Conexión de máquina virtual de SQL a Azure Search](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md). <br/><br/>En la página debe especificarse la tabla o vista que proporciona el conjunto de filas. Esta opción aparece una vez realizada correctamente la conexión, lo que proporciona una lista desplegable para que pueda realizar una selección. |
| **Cosmos DB** |Los requisitos incluyen la cuenta, la base de datos y la colección. Todos los documentos de la colección se incluirán en el índice. Puede definir una consulta para aplanar o filtrar el conjunto de filas o dejar en blanco la consulta. No se requiere una consulta en este asistente.|
| **Azure Blob Storage** |Los requisitos incluyen la cuenta de almacenamiento y un contenedor. Opcionalmente, si los nombres de blobs siguen una convención de nomenclatura virtual con fines de agrupación, puede especificar la parte del directorio virtual del nombre como una carpeta en el contenedor. Consulte [Indexación de Blob Storage](search-howto-indexing-azure-blob-storage.md) para más información. |
| **Azure Table Storage** |Los requisitos incluyen la cuenta de almacenamiento y un nombre de tabla. Opcionalmente, puede especificar una consulta para recuperar un subconjunto de las tablas. Consulte [Indexación de Table Storage](search-howto-indexing-azure-tables.md) para más información. |


<a name="index-definition"></a>

## <a name="index-attributes"></a>Atributos de índice

El asistente para la **importación de datos** genera un índice, que se rellenará con los documentos que se obtienen del origen de datos de entrada. 

Para que el índice sea funcional, asegúrese de que tiene definidos los elementos siguientes.

1. Un campo debe marcarse como **Clave**, que se usa para identificar de forma única cada documento. La **Clave** debe ser *Edm.string*. 

   Si los valores de campo incluyen espacios o guiones, debe establecer la opción **Claves de codificación Base 64** del paso **Crear un indexador** en **Opciones avanzadas**, a fin de suprimir la comprobación de validación para estos caracteres.

1. Establezca los atributos de índice para cada campo. Si no selecciona ningún atributo, el índice estará esencialmente vacío, salvo el campo de clave obligatorio. Como mínimo, elija uno o varios de estos atributos para cada campo.
   
   + **Retrievable** devuelve el campo en los resultados de búsqueda. Todos los campos que proporcionan contenido a los resultados de búsqueda deben tener este atributo. Al establecer este campo, el tamaño del índice no se verá afectado de manera apreciable.
   + **Filterable** permite que se haga referencia al campo en las expresiones de filtro. Cada campo utilizado en una expresión **$filter** debe tener este atributo. Las expresiones de filtro son para coincidencias exactas. Dado que las cadenas de texto permanecen intactas, se necesita almacenamiento adicional para dar cabida al contenido textual.
   + **Searchable** permite la búsqueda de texto completo. Todos los campos utilizados en consultas de formato libre o en expresiones de consulta deben tener este atributo. Para cada campo que marque como **Searchable**, se crearán índices invertidos.

1. Opcionalmente, puede establecer estos atributos según sea necesario:

   + **Sortable** permite que el campo se use en una ordenación. Cada campo utilizado en una expresión **$Orderby** debe tener este atributo.
   + **Facetable** permite que el campo se use en la navegación con facetas. Solo los campos marcados también como **Filterable** pueden marcarse como **Facetable**.

1. Establezca un **analizador** si desea indexación y consultas mejoradas para un idioma. La opción predeterminada es *Estándar - Lucene*, pero puede elegir *Inglés - Microsoft* si desea usar el analizador de Microsoft para procesamiento léxico avanzado, como para la resolución de formas verbales y sustantivos irregulares.

   + Seleccione **Searchable** para habilitar la lista **Analizador**.
   + Elija un analizador de la lista. 
   
   Solo se pueden especificar en este momento los analizadores de lenguaje. Si se usa un analizador personalizado o un analizador que no sea de lenguaje, como palabra clave o patrón, se requerirá código. Para más información acerca de los analizadores, consulte [Creación de un índice para documentos en varios idiomas](search-language-support.md).

1. Active la casilla **Proveedor de sugerencias** para habilitar las sugerencias de consulta anticipadas en los campos seleccionados.


## <a name="next-steps"></a>Pasos siguientes
Siga estos vínculos para más información sobre los indexadores:

* [Indexación de Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Indexación de Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Indexación de Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Indexación de Table Storage](search-howto-indexing-azure-tables.md)