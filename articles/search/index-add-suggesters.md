---
title: Incorporación de proveedores de sugerencias a un índice de Azure Search
description: Habilita campos para las acciones de consulta con escritura anticipada, en las que las consultas que se sugieren están compuestas de texto procedente de campos de un índice de Azure Search.
ms.date: 02/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
ms.author: brjohnst
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
ms.openlocfilehash: fd4b29134fd45ed2888fbc81ded413ecf7286959
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2019
ms.locfileid: "57308659"
---
# <a name="add-suggesters-to-an-azure-search-index"></a>Incorporación de proveedores de sugerencias a un índice de Azure Search

Un **proveedor de sugerencias** es una construcción de Azure Search que admite la característica [Sugerencias](https://docs.microsoft.com/rest/api/searchservice/suggestions) de "búsqueda mientras se escribe" y la característica [Autocompletar (versión preliminar)](search-autocomplete-tutorial.md). Para poder llamar a la API de sugerencias, debe definir un **proveedor de sugerencias** en un índice para habilitar las sugerencias en campos específicos.

Aunque un **proveedor de sugerencias** tiene varias propiedades, es principalmente una colección de campos para el que va a habilitar la [API de sugerencias](https://docs.microsoft.com/rest/api/searchservice/suggestions). Por ejemplo, una aplicación de viajes desea habilitar la búsqueda con escritura anticipada de destinos, ciudades y atracciones. Por lo tanto, estos tres campos deberían estar en la colección de campos.

Puede tener un solo recurso de **proveedor de sugerencias** para cada índice (en concreto, un **proveedor de sugerencias** de la colección de **proveedores de sugerencias**).

## <a name="creating-a-suggester"></a>Creación de un proveedor de sugerencias

Puede crear un **proveedor de sugerencias** en cualquier momento, pero el impacto en el índice variará según los campos.

+ Los nuevos campos que se agregan a un proveedor de sugerencias como parte de la misma actualización son los que menos impacto provocan ya que no se requiere ninguna recompilación del índice.
+ La incorporación de campos existentes a un proveedor de sugerencias, sin embargo, cambia la definición del campo, lo cual hace necesaria una recompilación completa del índice.

Los **proveedores de sugerencias** funcionan mejor cuando se usan para sugerir documentos específicos en lugar de términos o frases flexibles. Los campos de mejores candidatos son títulos, nombres y demás frases relativamente cortas que pueden identificar un elemento. Los campos repetitivos son menos efectivos, por ejemplo, las categorías y etiquetas, o campos muy largos como campos de descripciones o comentarios.

Después de crear un proveedor de sugerencias, agregue la [API de sugerencias](https://docs.microsoft.com/rest/api/searchservice/suggestions) a la lógica de la consulta para invocar la característica.

Entre las propiedades que definen a un **proveedor de sugerencias** se incluyen las siguientes:

|Propiedad|DESCRIPCIÓN|
|--------------|-----------------|
|`name`|El nombre del **proveedor de sugerencias**. Use el nombre del **proveedor de sugerencias** al llamar a la [API REST de sugerencias de Azure Search Service](https://docs.microsoft.com/rest/api/searchservice/suggestions).|
|`searchMode`|La estrategia que se usa para buscar las frases candidatas. El único modo que se admite actualmente es `analyzingInfixMatching`, que establece una correspondencia flexible de frases al principio o en medio de las oraciones.|
|`sourceFields`|Una lista de uno o más campos que son el origen del contenido para obtener sugerencias. Solo los campos de tipo `Edm.String` y `Collection(Edm.String)` pueden ser orígenes para obtener sugerencias. Solo se pueden usar los campos que no tienen un analizador de lenguaje personalizado establecido. |

## <a name="suggester-example"></a>Ejemplo de proveedor de sugerencias
Un **proveedor de sugerencias** forma parte de la definición del índice. Solo puede existir un **proveedor de sugerencias** en la colección de **proveedores de sugerencias** de la versión actual, junto con la colección de **campos** y **scoringProfiles**.

```
{
  "name": "hotels",
  "fields": [
    . . .
  ],
  "suggesters": [
    {
      "name": "sg",
      "searchMode": "analyzingInfixMatching",
      "sourceFields": ["hotelName", "category"]
    }
  ],
  "scoringProfiles": [
    . . .
  ]
}

```

## <a name="see-also"></a>Vea también
[Create Index &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index) (Creación de un índice [API REST de Azure Search Service])  
[Actualizar índice &#40;API de REST del servicio Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/update-index)  
[Sugerencias &#40;API de REST del servicio Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/suggestions)  
[Operaciones de índice &#40;API de REST del servicio Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/index-operations)  
[Servicio REST de Azure Search](https://docs.microsoft.com/rest/api/searchservice/)  
[SDK de .NET de Azure Search+](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)
