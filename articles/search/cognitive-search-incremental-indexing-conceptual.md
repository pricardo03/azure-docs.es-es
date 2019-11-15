---
title: Introducción a la indexación incremental (versión preliminar)
titleSuffix: Azure Cognitive Search
description: Configure la canalización de enriquecimiento con inteligencia artificial para que sus datos lleguen a alcanzar coherencia y puedan admitir actualizaciones de las aptitudes, los conjuntos de aptitudes, los indexadores o los orígenes de datos. Esta característica está actualmente en versión preliminar pública
manager: nitinme
author: Vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 94ce056185ff6a804521bf583ac4f6ffaa513fb0
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73715419"
---
# <a name="what-is-incremental-indexing-in-azure-cognitive-search"></a>¿Qué es la indexación incremental en Azure Cognitive Search?

> [!IMPORTANT] 
> La indexación incremental se encuentra actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). En la [API REST, versión 2019-05-06-Preview](search-api-preview.md) se proporciona esta característica. Por el momento, no hay compatibilidad con el portal ni con .NET SDK.

La indexación incremental es una característica nueva de Azure Cognitive Search que agrega almacenamiento en caché y estado al contenido enriquecido de un conjunto de aptitudes cognitivo, lo que le proporciona el control necesario para procesar y volver a procesar los pasos individuales de una canalización de enriquecimiento. Esto no solo le permite conservar su inversión monetaria en el procesamiento, sino que también le ofrece un sistema más eficaz. Cuando las estructuras y el contenido se almacenan en la caché, un indexador puede determinar qué aptitudes han cambiado y ejecutar solo aquellas que se han modificado, así como cualquier aptitud dependiente de bajada. 

Con la indexación incremental, la versión actual de la canalización de enriquecimiento realiza la menor cantidad posible de trabajo para garantizar la coherencia de todos los documentos del índice. En aquellos casos en los que necesita tener el control total, se pueden usar controles específicos para reemplazar los comportamientos esperados. Para obtener más información acerca de la configuración, consulte [Configuración de la indexación incremental](search-howto-incremental-index.md).

## <a name="indexer-cache"></a>Caché de indexador

La indexación incremental agrega una caché de indexador a la canalización de enriquecimiento. El indexador almacena en caché los resultados del descifrado de documentos y las salidas de todas las aptitudes de todos los documentos. Cuando se actualiza un conjunto de aptitudes, solo se vuelven a ejecutar las aptitudes modificadas o descendentes. Los resultados actualizados se escriben en la memoria caché y el documento se actualiza en el índice y en el almacén de conocimiento.

Físicamente, la memoria caché es una cuenta de almacenamiento. Todos los índices de un servicio de búsqueda pueden compartir la misma cuenta de almacenamiento para la caché del indexador. A cada indexador se le asigna un identificador de caché único e inmutable.

### <a name="cache-configuration"></a>Configuración de la caché

Para empezar a beneficiarse de la indexación incremental, será preciso que establezca la propiedad `cache` en el indexador. En el ejemplo siguiente se muestra un indexador con el almacenamiento en caché habilitado. En las secciones siguientes se describen partes específicas de esta configuración.

```json
{
    "name": "myIndexerName",
    "targetIndexName": "myIndex",
    "dataSourceName": "myDatasource",
    "skillsetName": "mySkillset",
    "cache" : {
        "storageConnectionString" : "Your storage account connection string",
        "enableReprocessing": true,
        "id" : "Auto generated Id you do not need to set"
    },
    "fieldMappings" : [],
    "outputFieldMappings": [],
    "parameters":{}
}
```

Si se establece esta propiedad por primera vez en un indexador existente, también será necesario restablecerla, lo que hará que todos los documentos del origen de datos se vuelvan a procesar. El objetivo de la indexación incremental es que los documentos del índice sean coherentes con el origen de datos y la versión actual de su conjunto de aptitudes. Restablecer el índice es el primer paso para lograr esta coherencia, ya que elimina los documentos enriquecidos por las versiones anteriores del conjunto de aptitudes. El indexador debe restablecerse para empezar con una línea de base coherente.

### <a name="cache-lifecycle"></a>Ciclo de vida de la caché

El indexador administra el ciclo de vida de la caché. Si la propiedad `cache` del indexador está establecida en null o la cadena de conexión ha cambiado, se eliminará la caché existente. El ciclo de vida de la caché también está asociado al ciclo de vida del indexador. Si se elimina un indexador, también se elimina la caché asociada.

### <a name="indexer-cache-mode"></a>Modo de la caché del indexador

La caché del indexador se puede usar en modos en que los datos se escriben solo en la caché o en que los datos se escriben en la caché y se usan para volver a enriquecer los documentos.  Para suspender temporalmente el enriquecimiento incremental, establezca la propiedad `enableReprocessing` de la caché en `false` y, posteriormente, reanude el enriquecimiento incremental y potencie una eventual coherencia estableciéndola en `true`. Este control es especialmente útil si se desea dar prioridad a la indexación de nuevos documentos sobre garantizar la coherencia entre los corpus de documentos.

## <a name="change-detection-override"></a>Invalidación de la detección de cambios

La indexación incremental proporciona un control exhaustivo de todos los aspectos de la canalización de enriquecimiento. Este control permite abordar situaciones en las que un cambio puede tener consecuencias imprevistas. Por ejemplo, la edición de un conjunto de aptitudes y la actualización de la dirección URL de una aptitud personalizada provocará que el indexador invalide los resultados almacenados en la caché de esa aptitud. Si solo va a mover el punto de conexión a otra máquina virtual o va a volver a implementar la aptitud con una clave de acceso nueva, en realidad no quiere que se vuelvan a procesar los documentos existentes.

Para asegurarse de que el indexador solo realiza las mejoras que se requieren explícitamente, las actualizaciones del conjunto de aptitudes pueden establecer opcionalmente el parámetro de la cadena de consulta `disableCacheReprocessingChangeDetection` en `true`. Cuando se establezca, este parámetro garantizará que solo se confirman las actualizaciones del conjunto de aptitudes y que el cambio no se evalúa para los efectos en el corpus existente.

En el ejemplo siguiente se ilustra el uso de una cadena de consultas. Esto forma parte de la solicitud, que cuenta con pares clave-valor separados con &. 

```http
PUT https://customerdemos.search.windows.net/skillsets/callcenter-text-skillset?api-version=2019-05-06-Preview&disableCacheReprocessingChangeDetection=true
```

## <a name="cache-invalidation"></a>Invalidación de la caché

El escenario opuesto es aquél en el que se puede implementar una nueva versión de una aptitud personalizada y no cambia nada dentro de la canalización de enriquecimiento, pero se necesita tanto que se invalide una aptitud concreta como que se vuelvan a procesar todos los documentos afectados para reflejar las ventajas de un modelo actualizado. En estos casos, puede llamar a la operación de invalidación de aptitudes en el conjunto de aptitudes. La API de restablecimiento de aptitudes acepta una solicitud POST con la lista de salidas de aptitudes de la memoria caché que se deben invalidar. Para obtener más información sobre la API de restablecimiento de aptitudes, consulte [Restablecer el indexador (API de REST de búsqueda)](https://docs.microsoft.com/rest/api/searchservice/reset-indexer).

## <a name="bi-directional-change-detection"></a>Detección de cambios bidireccional

Los indexadores no solo avanzan y procesan nuevos documentos, sino que ahora retroceden y hacen que los documentos procesados previamente sean coherentes. Con esta nueva funcionalidad, es importante saber de qué forma los cambios en los componentes de la canalización de enriquecimiento generan trabajo del indexador. El indexador pondrá en cola el trabajo que se va a realizar cuando identifique un cambio que sea invalidador o incoherente y que esté relacionado con el contenido de la caché.

### <a name="invalidating-changes"></a>Cambios invalidadores

Los cambios invalidadores son poco frecuentes, pero tienen un efecto significativo en el estado de la canalización de enriquecimiento. Un cambio invalidador es aquél en el que la caché completa deja de ser válida. Un ejemplo de cambio invalidador es en el que se actualiza el origen de datos. En escenarios en los que se sabe que el cambio no debería invalidar la caché, como la rotación de la clave en la cuenta de almacenamiento, el parámetro de cadena de consulta `ignoreResetRequirement` debe establecerse en `true` en la operación de actualización del recurso específico para asegurarse de que la operación no se rechaza.

Esta es la lista completa de cambios que invalidarían la memoria caché:

* Cambiar al tipo de origen de datos
* Cambiar al contenedor del origen de datos
* Credenciales del origen de datos
* Directiva de detección de cambios en el origen de datos
* Directiva de detección de eliminaciones en el origen de datos
* Asignaciones de campos del indexador
* Parámetros de un indexador
    * Modo de análisis
    * Extensiones de nombre de archivo excluidas
    * Extensiones de nombre de archivo indexadas
    * Metadatos de almacenamiento de índice solo para documentos de gran tamaño
    * Encabezados de texto delimitado
    * Delimitador de texto delimitado
    * Raíz del documento
    * Acción de imagen (cambios en la forma en que se extraen las imágenes)

### <a name="inconsistent-changes"></a>Cambios incoherentes

Un ejemplo de cambio incoherente es una actualización en un conjunto de aptitudes que modifica una aptitud. La modificación puede hacer que una parte de la caché sea incoherente. El indexador identificará el trabajo para que todo vuelva a ser coherente.  

La lista completa de los cambios que dan lugar a una incoherencia en la caché:

* La aptitud del conjunto de aptitudes tiene un tipo diferente. El tipo de oData de la aptitud está actualizado
* Los valores predeterminados de los parámetros específicos de la aptitud actualizados, como la dirección URL, u otros parámetros
* Cambios en las salidas de la aptitud, la aptitud devuelve salidas adicionales o diferentes
* Las actualizaciones de aptitudes resultantes tienen otra ascendencia, el encadenamiento de aptitudes ha cambiado, es decir, las entradas de las aptitudes
* Cualquier invalidación de aptitudes ascendente, si se actualiza una aptitud que proporcione una entrada a esta aptitud
* Actualizaciones de la ubicación de la proyección del almacén de información, lo que provoca que se vuelvan a proyectar los documentos
* Cambios en las proyecciones del almacén de información, lo que provoca que se vuelvan a proyectar los documentos
* Las asignaciones de campos de salida cambiadas en un indexador provocan que se vuelvan a proyectos los documentos en el índice

## <a name="rest-api-reference-for-incremental-indexing"></a>Referencia de la API de REST para la indexación incremental

REST `api-version=2019-05-06-Preview` proporciona las API para la indexación incremental, con adiciones a indexadores, conjuntos de aptitudes y orígenes de datos. La documentación de referencia no incluye actualmente estas adiciones. En la sección siguiente se describen los cambios en las API.

### <a name="indexers"></a>Indexadores

Las opciones [Crear indexador](https://docs.microsoft.com/rest/api/searchservice/create-indexer) y [Actualizar indexador](https://docs.microsoft.com/rest/api/searchservice/update-indexer) ahora exponen nuevas propiedades relacionadas con la caché:

* `StorageAccountConnectionString`: la cadena de conexión a la cuenta de almacenamiento que se usará para almacenar en caché los resultados intermedios.

* `CacheId`: `cacheId` es el identificador del contenedor de la cuenta de almacenamiento `annotationCache` que se usará como caché para este indexador. Esta caché será única para este indexador y, si el indexador se elimina y se vuelve a crear con el mismo nombre, se volverá a generar `cacheId`. `cacheId` no se puede establecer, siempre lo genera el servicio.

* `EnableReprocessing`: Establecido en `true` de forma predeterminada, si se establece en `false`, los documentos se seguirán escribiendo en la memoria caché, pero no se volverá a procesar ningún documento existente en función de los datos de la caché.

Algunos indexadores (a través de [orígenes de datos](https://docs.microsoft.com/rest/api/searchservice/create-data-source)) recuperan datos mediante consultas. En cuanto a las consultas que recuperan los datos, los indexadores también admitirán un nuevo parámetro de cadena de consulta: `ignoreResetRequirement` se tiene que establecer en `true` si la acción de actualización no debe invalidar la caché.

### <a name="skillsets"></a>Conjuntos de aptitudes

Los conjuntos de aptitudes no admitirán operaciones nuevas, pero admitirán un nuevo parámetro QueryString: `disableCacheReprocessingChangeDetection` debe establecerse en `true` si no se desean actualizaciones de los documentos existentes en función de la acción actual.

### <a name="datasources"></a>Orígenes de datos

Los orígenes de datos no admitirán operaciones nuevas, pero admitirán un parámetro QueryString nuevo: `ignoreResetRequirement` se debe establecer en `true` si la acción de actualización no debe invalidar la caché.

## <a name="best-practices"></a>Procedimientos recomendados

El enfoque recomendado para usar la indexación incremental es configurarla estableciendo la propiedad cache en un nuevo indexador o restablecer un indexador existente y establecer la propiedad cache.

Use `ignoreResetRequirement` con moderación, ya que podría dar lugar a incoherencias no intencionadas en los datos que no se detectarían fácilmente.

## <a name="takeaways"></a>Puntos clave

La indexación incremental es una característica eficaz que extiende el seguimiento de cambios desde el origen de datos a todos los aspectos de la canalización de enriquecimiento, incluido el origen de datos, la versión actual del conjunto de aptitudes y el indexador. Cuando las aptitudes, los conjuntos de aptitudes o los enriquecimientos evolucionan, la canalización de enriquecimiento garantiza que se realiza el menor trabajo posible mientras logra la coherencia final de los documentos.

## <a name="next-steps"></a>Pasos siguientes

Para empezar a usar la indexación incremental, agregue una caché a un indexador existente o agregue la caché al definir un nuevo indexador.

> [!div class="nextstepaction"]
> [Configuración de la indexación incremental](search-howto-incremental-index.md)
