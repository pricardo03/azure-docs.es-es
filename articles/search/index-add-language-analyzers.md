---
title: 'Adición de analizadores de idiomas: Azure Search'
description: Análisis de texto de léxico en varios idiomas para consultas e índices en idiomas distintos del inglés en Azure Search.
ms.date: 01/31/2019
services: search
ms.service: search
ms.topic: conceptual
author: Yahnoosh
ms.author: jlembicz
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: b5c562994c169a8c5d51ee31a9606c5c40162603
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "56007613"
---
# <a name="add-language-analyzers-to-an-azure-search-index"></a>Incorporación de analizadores de idiomas a un índice de Azure Search

Un *analizador de idiomas* es un componente específico de un [motor de búsqueda de texto completo](https://docs.microsoft.com/azure/search/search-lucene-query-architecture) que realiza un análisis léxico mediante las reglas lingüísticas del idioma de destino. Cada campo de búsqueda tiene una propiedad `analyzer`. Si el índice contiene cadenas traducidas, como campos independientes para texto en inglés y en chino, puede especificar los analizadores de idiomas en cada campo para acceder a las funcionalidades lingüísticas enriquecidas de esos analizadores.  

Azure Search admite 35 analizadores respaldados por la tecnología de Lucene y 50 analizadores respaldados por la tecnología de procesamiento de lenguaje natural de Microsoft que se usa en Office y Bing.

## <a name="compare-language-analyzer-types"></a>Comparación entre tipos de analizadores de idiomas 

Es posible que algunos desarrolladores prefieran la solución más familiar, simple y de código abierto de Lucene. Los analizadores de idiomas de Lucene son más rápidos, pero los analizadores de Microsoft disponen de capacidades avanzadas, como la lematización, la descomposición de palabras (en idiomas como el alemán, danés, neerlandés, sueco, noruego, estonio, finés, húngaro, eslovaco) y el reconocimiento de entidades (direcciones URL, correos electrónicos, fechas y números). Si es posible, debe ejecutar las comparaciones de los analizadores de Microsoft y Lucene para decidir cuál es la que se ajusta mejor. 

La indexación con analizadores de Microsoft es entre dos y tres veces más lenta de media que sus equivalentes de Lucene en función del idioma. El rendimiento de la búsqueda no debería verse afectado significativamente en las consultas de tamaño medio. 

### <a name="english-analyzers"></a>Analizadores de inglés

El analizador predeterminado es Standard Lucene, que funciona bien con el inglés, pero quizás no tan bien como el analizador de inglés de Lucene o el analizador de inglés de Microsoft. 
 
+ El analizador de inglés de Lucene amplía el analizador estándar. Elimina los posesivos (los "'s" finales) de las palabras, aplica la lematización conforme al Algoritmo de lematización Porter y elimina las palabras no significativas del inglés.  

+ El analizador de inglés de Microsoft utiliza lemas en lugar de lexemas. Esto significa que puede controlar mucho mejor formas de palabras derivadas e irregulares, lo que da como resultado unos resultados de búsqueda más pertinentes 

 > [!Tip]
 > La [versión de demostración de Search Analyzer](https://alice.unearth.ai/) muestra una comparación entre los resultados que proporciona el analizador de Lucene estándar, el analizador del lenguaje para inglés de Lucene y el procesador de lenguaje natural para inglés de Microsoft. Cada vez que se realiza una entrada de búsqueda, los resultados de cada analizador se muestran en los paneles adyacentes.

## <a name="analyzer-configuration"></a>Configuración del analizador

Para cada campo de la definición del índice, puede establecer la propiedad `analyzer` en un nombre de analizador que especifica el idioma y el proveedor. Se aplicará el mismo analizador durante la búsqueda e indización de ese campo. Por ejemplo, puede tener campos separados para descripciones de hoteles en inglés, francés y español que existen en paralelo dentro del mismo índice.  

Use el parámetro de consulta **searchFields** para especificar qué campo concreto del lenguaje buscar en las consultas. Puede revisar ejemplos de consultas que incluyan la propiedad del analizador en Buscar documentos. 

Para más información sobre las propiedades del índice, consulte [Create Index &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index) (Creación de un índice [API REST de Azure Search Service]). Para más información sobre el análisis en Azure Search, consulte [Analizadores de Azure Search](https://docs.microsoft.com/azure/search/search-analyzers).

## <a name="analyzer-list"></a>Lista de analizadores  
 A continuación se muestra la lista de idiomas admitidos y los nombres de analizadores Lucene y Microsoft.  

|Idioma|Nombre del analizador de Microsoft|Nombre del analizador de Lucene|  
|--------------|-----------------------------|--------------------------|  
|Árabe|ar.microsoft|ar.lucene|  
|Armenio||hy.lucene|  
|Bangla|bn.microsoft||  
|Vasco||eu.Lucene|  
|Búlgaro|bg.microsoft|bg.lucene|  
|Catalán|ca.microsoft|ca.lucene|  
|Chino simplificado|zh-Hans.microsoft|zh-Hans.lucene|  
|Chino tradicional|zh-Hant.microsoft|zh-Hant.lucene|  
|Croata|hr.microsoft||  
|Checo|cs.microsoft|cs.lucene|  
|Danés|da.microsoft|da.lucene|  
|Neerlandés|nl.microsoft|nl.lucene|  
|English|en.microsoft|en.lucene|  
|Estonio|et.microsoft||  
|Finés|fi.microsoft|fi.lucene|  
|Francés|fr.microsoft|fr.lucene|  
|Gallego||gl.lucene|  
|Alemán|de.microsoft|de.lucene|  
|Griego|el.microsoft|el.lucene|  
|Gujarati|gu.microsoft||  
|Hebreo|he.microsoft||  
|Hindi|hi.microsoft|hi.lucene|  
|Húngaro|hu.microsoft|hu.lucene|  
|Islandés|is.microsoft||  
|Indonesio (Bahasa)|id.microsoft|id.lucene|  
|Irlandés||ga.lucene|  
|Italiano|it.microsoft|it.lucene|  
|Japonés|ja.microsoft|ja.lucene|  
|Canarés|ka.microsoft||  
|Coreano|ko.Microsoft|ko.lucene|  
|Letón|lv.microsoft|lv.lucene|  
|Lituano|lt.microsoft||  
|Malayalam|ml.microsoft||  
|Malayo (latino)|ms.microsoft||  
|Maratí|mr.microsoft||  
|Noruego|nb.microsoft|no.lucene|  
|Persa||fa.lucene|  
|Polaco|pl.microsoft|pl.lucene|  
|Portugués (Brasil)|pt-Br.microsoft|pt-Br.lucene|  
|Portugués (Portugal)|pt-Pt.microsoft|pt-Pt.lucene|  
|Punjabi|pa.microsoft||  
|Rumano|ro.microsoft|ro.lucene|  
|Ruso|ru.microsoft|ru.lucene|  
|Serbio (cirílico)|sr-cyrillic.microsoft||  
|Serbio (latino)|sr-latin.microsoft||  
|Eslovaco|sk.microsoft||  
|Esloveno|sl.microsoft||  
|Español|es.Microsoft|es.lucene|  
|Sueco|sv.microsoft|sv.lucene|  
|Tamil|ta.microsoft||  
|Telugu|te.microsoft||  
|Tailandés|th.microsoft|th.lucene|  
|Turco|tr.microsoft|tr.lucene|  
|Ucraniano|uk.microsoft||  
|Urdu|ur.microsoft||  
|Vietnamita|vi.microsoft||  

 Todos los analizadores con nombres anotados con **lucene** disponen de la tecnología de [analizadores de idioma de Apache Lucene](https://lucene.apache.org/core/4_9_0/core/overview-summary.html ).

## <a name="see-also"></a>Otras referencias  
 [Create Index &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index) (Creación de un índice [API REST de Azure Search Service])  
 [Clase AnalyzerName](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername)  
 [Vídeo: Módulo 7 de la presentación MVA de Azure Search](https://channel9.msdn.com/Series/Adding-Microsoft-Azure-Search-to-Your-Websites-and-Apps/07).  

