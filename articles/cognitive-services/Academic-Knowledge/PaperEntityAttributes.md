---
title: 'Atributos de la entidad de documento: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de documento en Academic Knowledge API.
services: cognitive-services
author: DarrinEide
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: fc3bb5987c31fe718951a3cae02ed7c4ddc29957
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2019
ms.locfileid: "74123082"
---
# <a name="paper-entity"></a>Entidad de documento

> [!NOTE]
> Los atributos siguientes son específicos de la entidad de documento. (Ty = '0')

NOMBRE | DESCRIPCIÓN | type | Operaciones
--- | --- | --- | ---
AA.AfId | Identificador de afiliación del autor | Int64 | Equals
AA.AfN | Nombre de afiliación del autor | Cadena | Equals, StartsWith
AA.AuId | Identificador del autor | Int64 | Equals
AA.AuN | Nombre normalizado del autor | Cadena | Equals, StartsWith
AA.DAuN | Nombre original del autor | Cadena | None
AA.DAfN | Nombre original de afiliación | Cadena | None
AA.S | Posición numérica en la lista de autores | Int32 | Equals
BT | Tipo de documento BibTex ("a": Artículo de revista, "b": Libro, "c": Capítulo del libro, "p": Artículo de conferencia) | Cadena | None
BV | BibTex venue name | Cadena | None
C.CId | Identificador de la serie de conferencias | Int64 | Equals
C.CN | Nombre de la serie de conferencias | Cadena | Equals, StartsWith
CC | Recuento de citas | Int32 | None  
CitCon | Contextos de cita</br></br>Lista de los identificadores de documento a los que se hace referencia y el contexto correspondiente en el documento (por ejemplo, [{123: ["los murciélagos hindús son conocidos por comer kiwi, como se hace referencia en el documento 123", "las cigüeñas que tocan el saxofón son un nombre equivocado histórico como se muestra en el documento 123"]}) | Personalizado | None
D | Fecha de publicación en formato AAAA-MM-DD | Date | Equals, IsBetween
DN | Título de documento original | Cadena | None
DOI | Identificador de objeto digital | Cadena | Equals, StartsWith
E | Metadatos extendidos</br></br>**IMPORTANTE**: Este atributo está en desuso y solo se admite para aplicaciones heredadas. La solicitud individual de este atributo (es decir, attributes=Id,Ti,E) tendrá como resultado la devolución de todos los atributos de metadatos extendidos en una *cadena JSON serializada*.</br></br>Todos los atributos contenidos en los metadatos extendidos están ahora disponibles como atributo de nivel superior y se pueden solicitar como tal (es decir, attributes=Id,Ti,DOI,IA). | [Extendido](#extended) | None
ECC | Recuento de citas estimado | Int32 | None
F.DFN | Nombre original de campo de estudio | Cadena | None
F.FId | Identificador de campo de estudio | Int64 | Equals
F.FN | Nombre normalizado de campo de estudio | Cadena | Equals, StartsWith
FP | Primera página del documento en la publicación | Cadena | Equals
I | Publicación | Cadena | Equals
IA | Descripción breve invertida | [InvertedAbstract](#invertedabstract) | None
Id | Identificador del documento | Int64 | Equals
J.JId | Identificador del diario | Int64 | Equals
J.JN | Nombre del diario | Cadena | Equals, StartsWith
LP | Última página del documento en la publicación | Cadena | Equals
PB | Publicador | Cadena | None
Pt | Tipo de publicación (0: Desconocido, 1: Artículo de revista, 2: Patente, 3: Artículo de conferencia, 4: Capítulo del libro, 5: Libro, 6: Entrada de referencia del libro, 7: Conjunto de documentos, 8: Repositorio) | Cadena | Equals
RId | Lista de identificadores de documentos a los que se hace referencia | Int64[] | Equals
S | Lista de direcciones URL de origen del documento, ordenadas por relevancia | [Origen](#source)[] | None
Ti | Título normalizado | Cadena | Equals, StartsWith
V | Volumen de publicación | Cadena | Equals
VFN | Nombre completo de la ubicación de la conferencia o la revista | Cadena | None
VSN | Nombre corto de la ubicación de la conferencia o la revista | Cadena | None
W | Palabras únicas en el título | String[] | Equals
Y | Año de publicación | Int32 | Equals, IsBetween

## <a name="extended"></a>Extendido
> [!IMPORTANT]
> Este atributo está en desuso y solo se admite para aplicaciones heredadas. La solicitud individual de este atributo (es decir, attributes=Id,Ti,E) tendrá como resultado la devolución de todos los atributos de metadatos extendidos en una *cadena JSON serializada*.</br></br>Todos los atributos contenidos en los metadatos extendidos están ahora disponibles como atributo de nivel superior y se pueden solicitar como tal (es decir, attributes=Id,Ti,DOI,IA).

> [!IMPORTANT]
> La compatibilidad para solicitar atributos extendidos individuales mediante el uso del ámbito "E." ; esto es, "E.DN", está en desuso. Aunque todavía se admite técnicamente, la solicitud de atributos extendidos individuales con el ámbito "E." dará como resultado que el valor del atributo se devuelva en dos lugares en la respuesta JSON, como parte del objeto "E" y como atributo de nivel superior.

NOMBRE | DESCRIPCIÓN | type | Operaciones
--- | --- | --- | ---
BT | Tipo de documento BibTex ("a": Artículo de revista, "b": Libro, "c": Capítulo del libro, "p": Artículo de conferencia) | Cadena | None
BV | BibTex venue name | Cadena | None
CC | Contextos de cita</br></br>Lista de los identificadores de documento a los que se hace referencia y el contexto correspondiente en el documento (por ejemplo, [{123: ["los murciélagos hindús son conocidos por comer kiwi, como se hace referencia en el documento 123", "las cigüeñas que tocan el saxofón son un nombre equivocado histórico como se muestra en el documento 123"]}) | Personalizado | None
DN | Título de documento original | Cadena | None
DOI | Identificador de objeto digital | Cadena | None
FP | Primera página del documento en la publicación | Cadena | None
I | Publicación | Cadena | None
IA | Descripción breve invertida | [InvertedAbstract](#invertedabstract) | None
LP | Última página del documento en la publicación | Cadena | None
PB | Publicador | Cadena | None
S | Lista de direcciones URL de origen del documento, ordenadas por relevancia | [Origen](#source)[] | None
V | Volumen de publicación | Cadena | None
VFN | Nombre completo de la ubicación de la conferencia o la revista | Cadena | None
VSN | Nombre corto de la ubicación de la conferencia o la revista | Cadena | None

## <a name="invertedabstract"></a>InvertedAbstract

> [!IMPORTANT]
> Los atributos InvertedAbstract no se pueden solicitar directamente como un atributo de devolución. Si necesita un atributo individual, debe solicitar el atributo de nivel superior "IA", es decir, para obtener IA.IndexLength solicite attributes=IA.

NOMBRE | DESCRIPCIÓN | type | Operaciones
--- | --- | --- | ---
IndexLength | Número de elementos del índice (recuento de palabras de la descripción breve) | Int32 | None
InvertedIndex | Lista de palabras de la descripción breve y su posición correspondiente en la descripción breve (por ejemplo, [{"el":[0, 15, 30]}, {"murciélago":[1]}, {"hindú":[2]}]) | Personalizado | None

## <a name="source"></a>Source

> [!IMPORTANT]
> Los atributos Source no se pueden solicitar directamente como un atributo de devolución. Si necesita un atributo individual, debe solicitar el atributo de nivel superior "S", es decir, para obtener S.U solicite attributes=S.

NOMBRE | DESCRIPCIÓN | type | Operaciones
--- | --- | --- | ---
Ty | Tipo de dirección URL de origen (1:HTML, 2:Texto, 3:PDF, 4:DOC, 5:PPT, 6:XLS, 7:PS) | Cadena | None
U | Dirección URL de origen | Cadena | None
