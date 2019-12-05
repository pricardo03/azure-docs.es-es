---
title: Anuncios de nuevas características
titleSuffix: Azure Cognitive Search
description: Anuncios de características nuevas y mejoradas, como el cambio de nombre del servicio Azure Search por Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 247378461a99d42f21a67a2c0aebc7019727f3a0
ms.sourcegitcommit: 428fded8754fa58f20908487a81e2f278f75b5d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2019
ms.locfileid: "74555151"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Novedades de Azure Cognitive Search

Conozca las novedades del servicio. Marque esta página para mantenerse actualizado con el servicio.

<a name="new-service-name"></a>

## <a name="new-service-name-for-azure-search"></a>Nuevo nombre del servicio Azure Search

Azure Search se llama ahora **Azure Cognitive Search** para reflejar el uso ampliado de las aptitudes cognitivas y el procesamiento de inteligencia artificial en las operaciones principales. Aunque las aptitudes cognitivas agregan nuevas funcionalidades, el uso de la inteligencia artificial es estrictamente opcional. Puede seguir usando Azure Cognitive Search sin ella para crear soluciones de búsqueda de texto completo enriquecidas a través de contenido heterogéneo y privado en un índice que se cree y administre en la nube. 

Las versiones de API, los paquetes Nuget, los espacios de nombres y los puntos de conexión permanecen sin cambios. Las soluciones de búsqueda existentes no se ven afectadas por el cambio de nombre del servicio.

## <a name="feature-announcements"></a>Anuncios de características

4 de noviembre de 2019: Conferencia Ignite

+ La [indexación incremental (versión preliminar)](cognitive-search-incremental-indexing-conceptual.md) permite elegir qué pasos se deben volver a procesar al realizar modificaciones en una canalización de enriquecimiento. La indexación incremental es especialmente útil si el contenido de la imagen se ha analizado anteriormente. La salida del costoso análisis se almacena y se usa como base para la indexación o el enriquecimiento adicionales.

<!-- 
+ Custom Entity Lookup is a cognitive skill used during indexing that allows you to provide a list of custom entities (such as part numbers, diseases, or names of locations you care about) that should be found within the text. It supports fuzzy matching, case-insensitive matching, and entity synonyms. -->

+ La [extracción de documentos (versión preliminar)](cognitive-search-skill-document-extraction.md) es una aptitud cognitiva que se usa durante la indexación y que permite extraer el contenido de un archivo de un conjunto de aptitudes. Antes, el descifrado de documentos solo tenía lugar antes de la ejecución del conjunto de aptitudes. Con la incorporación de esta aptitud, también puede realizar esta operación dentro de la ejecución del conjunto de aptitudes.

+ La [traducción de texto (versión preliminar)](cognitive-search-skill-text-translation.md) es una aptitud cognitiva que se usa durante la indexación que evalúa el texto y devuelve en todos los registros el texto traducido al idioma de destino especificado.

+ Las [plantillas de Power BI](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md) pueden impulsar las visualizaciones y el análisis de contenido enriquecido en un almacén de conocimiento en Power BI Desktop. Esta plantilla está diseñada para las proyecciones de tablas de Azure creadas mediante el [Asistente para importar datos](knowledge-store-create-portal.md).

+ [Azure Data Lake Storage Gen2 (versión preliminar)](search-howto-index-azure-data-lake-storage.md), [Gremlin API de Cosmos DB (versión preliminar)](search-howto-index-cosmosdb.md) y [Cassandra API de Cosmos DB (versión preliminar)](search-howto-index-cosmosdb.md) ahora se admiten en los indexadores. Para registrarse, utilice [este formulario](https://aka.ms/azure-cognitive-search/indexer-preview). Recibirá un correo de confirmación cuando se le haya aceptado en el programa de versión preliminar.

24 de julio de 2019

+ Está disponible generalmente en la [nube de Azure Government](../azure-government/documentation-government-services-webandmobile.md#azure-cognitive-search).

## <a name="service-updates"></a>Actualizaciones del servicio

Los [anuncios de actualización de servicios](https://azure.microsoft.com/updates/?product=search&status=all) de Azure Cognitive Search se pueden encontrar en el sitio web de Azure.