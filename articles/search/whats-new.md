---
title: Anuncios de nuevas características
titleSuffix: Azure Cognitive Search
description: Anuncios de características nuevas y mejoradas, como el cambio de nombre del servicio Azure Search por Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: e115316daf3673d9ad854e7ccd6d5256d729b5af
ms.sourcegitcommit: f34165bdfd27982bdae836d79b7290831a518f12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2020
ms.locfileid: "75921008"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Novedades de Azure Cognitive Search

Conozca las novedades del servicio. Marque esta página para mantenerse actualizado con el servicio.

<a name="new-service-name"></a>

## <a name="new-service-name-for-azure-search"></a>Nuevo nombre del servicio Azure Search

Azure Search se llama ahora **Azure Cognitive Search** para reflejar el uso ampliado de las aptitudes cognitivas y el procesamiento de inteligencia artificial en las operaciones principales. Aunque las aptitudes cognitivas agregan nuevas funcionalidades, el uso de la inteligencia artificial es estrictamente opcional. Puede seguir usando Azure Cognitive Search sin ella para crear soluciones de búsqueda de texto completo enriquecidas a través de contenido heterogéneo y privado en un índice que se cree y administre en la nube. 

Las versiones de API, los paquetes Nuget, los espacios de nombres y los puntos de conexión permanecen sin cambios. Las soluciones de búsqueda existentes no se ven afectadas por el cambio de nombre del servicio.

## <a name="feature-announcements"></a>Anuncios de características

### <a name="january-2020"></a>Enero de 2020

+ Las [claves de cifrado administradas por el cliente](search-security-manage-encryption-keys.md) ya están disponibles con carácter general. Si usa REST, puede acceder a la característica mediante `api-version=2019-05-06`. Para el código administrado, el paquete correcto sigue siendo el [SDK de .NET, versión 8.0-preview](search-dotnet-sdk-migration-version-9.md) aunque la característica no se incluya en la versión preliminar. 

+ El acceso privado a un servicio de búsqueda está disponible a través de dos mecanismos:

  + Puede restringir el acceso a direcciones IP específicas mediante la API REST de administración `api-version=2019-10-01-Preview` para crear el servicio. La API de vista previa tiene las propiedades nuevas **IpRule** y **NetworkRuleSet** en la [API CreateOrUpdate](https://docs.microsoft.com/rest/api/searchmanagement/services/createorupdate). Esta característica en vista previa está disponible en regiones seleccionadas. Para más información, vea [Uso de la API REST de administración](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api).

  + Actualmente disponible a través de una versión preliminar de acceso limitado, puede aprovisionar un servicio Search de Azure que admita el punto de conexión privado de Azure para las conexiones de los clientes en la misma red virtual. Para más información, vea [Creación de un punto de conexión privado para una conexión segura](service-create-private-endpoint.md).

### <a name="december-2019"></a>Diciembre de 2019

+ El Asistente para [crear aplicaciones (versión preliminar)](search-create-app-portal.md) es un nuevo asistente en el portal que genera un archivo HTML descargable. El archivo incluye un script insertado que representa una aplicación web de estilo "localhost" operativa, enlazada a un índice en el servicio de búsqueda. Las páginas se pueden configurar en el asistente y pueden contener una barra de búsqueda, un área de resultados, una barra de navegación lateral y compatibilidad con consultas de escritura anticipada. Puede modificar el código HTML sin conexión para extender o personalizar el flujo de trabajo o la apariencia.

+ En [Creación de un punto de conexión privado para conexiones seguras (versión preliminar)](service-create-private-endpoint.md) se explica cómo configurar un vínculo privado para las conexiones seguras al servicio de búsqueda. Esta característica en vista previa está disponible a petición y usa [Azure Private Link](../private-link/private-link-overview.md) y [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) como parte de la solución.

### <a name="november-2019---ignite-conference"></a>Noviembre de 2019: Conferencia Ignite

+ [Enriquecimiento incremental (versión preliminar)](cognitive-search-incremental-indexing-conceptual.md) agrega almacenamiento en caché y disponibilidad de estados a una canalización de enriquecimiento para que pueda trabajar en pasos o fases específicos sin perder el contenido que ya se ha procesado. Anteriormente, cualquier cambio en una canalización de enriquecimiento requería una recompilación completa. Con el enriquecimiento incremental, se conserva la salida del análisis costoso, especialmente el de las imágenes.

<!-- 
+ Custom Entity Lookup is a cognitive skill used during indexing that allows you to provide a list of custom entities (such as part numbers, diseases, or names of locations you care about) that should be found within the text. It supports fuzzy matching, case-insensitive matching, and entity synonyms. -->

+ La [extracción de documentos (versión preliminar)](cognitive-search-skill-document-extraction.md) es una aptitud cognitiva que se usa durante la indexación y que permite extraer el contenido de un archivo de un conjunto de aptitudes. Antes, el descifrado de documentos solo tenía lugar antes de la ejecución del conjunto de aptitudes. Con la incorporación de esta aptitud, también puede realizar esta operación dentro de la ejecución del conjunto de aptitudes.

+ La [traducción de texto (versión preliminar)](cognitive-search-skill-text-translation.md) es una aptitud cognitiva que se usa durante la indexación que evalúa el texto y devuelve en todos los registros el texto traducido al idioma de destino especificado.

+ Las [plantillas de Power BI](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md) pueden impulsar las visualizaciones y el análisis de contenido enriquecido en un almacén de conocimiento en Power BI Desktop. Esta plantilla está diseñada para las proyecciones de tablas de Azure creadas mediante el [Asistente para importar datos](knowledge-store-create-portal.md).

+ [Azure Data Lake Storage Gen2 (versión preliminar)](search-howto-index-azure-data-lake-storage.md), [Gremlin API de Cosmos DB (versión preliminar)](search-howto-index-cosmosdb.md) y [Cassandra API de Cosmos DB (versión preliminar)](search-howto-index-cosmosdb.md) ahora se admiten en los indexadores. Para registrarse, utilice [este formulario](https://aka.ms/azure-cognitive-search/indexer-preview). Recibirá un correo de confirmación cuando se le haya aceptado en el programa de versión preliminar.

### <a name="july-2019"></a>Julio de 2019

+ Está disponible generalmente en la [nube de Azure Government](../azure-government/documentation-government-services-webandmobile.md#azure-cognitive-search).

## <a name="service-updates"></a>Actualizaciones del servicio

Los [anuncios de actualización de servicios](https://azure.microsoft.com/updates/?product=search&status=all) de Azure Cognitive Search se pueden encontrar en el sitio web de Azure.