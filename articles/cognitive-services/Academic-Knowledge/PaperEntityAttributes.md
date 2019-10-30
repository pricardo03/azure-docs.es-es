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
ms.date: 10/22/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: c300a6477daa5759a68d5d11d40b1a71b46bd808
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72793837"
---
# <a name="paper-entity"></a>Entidad de documento

<sub> *Los atributos siguientes son específicos de la entidad de documento. (Ty = "0") </sub>

NOMBRE | DESCRIPCIÓN | type | Operaciones
--- | --- | --- | ---
AA.AfId | Identificador de afiliación del autor | Int64 | Equals
AA.AfN | Nombre de afiliación del autor | Cadena | Equals, StartsWith
AA.AuId | Identificador del autor | Int64 | Equals
AA.AuN | Nombre normalizado del autor | Cadena | Equals, StartsWith
AA.DAuN | Nombre original del autor | Cadena | None
AA.DAfN | Nombre original de afiliación | Cadena | None
AA.S | Posición numérica en la lista de autores | Int32 | Equals
CC | Recuento de citas | Int32 | None  
C.CId | Identificador de la serie de conferencias | Int64 | Equals
C.CN | Nombre de la serie de conferencias | Cadena | Equals, StartsWith
D | Fecha de publicación en formato AAAA-MM-DD | Date | Equals, IsBetween
E | Metadatos ampliados (consulte la tabla siguiente) | Cadena | N/D  
ECC | Recuento de citas estimado | Int32 | None
F.DFN | Nombre original de campo de estudio | Cadena | None
F.FId | Identificador de campo de estudio | Int64 | Equals
F.FN | Nombre normalizado de campo de estudio | Cadena | Equals, StartsWith
Id | Identificador del documento | Int64 | Equals
J.JId | Identificador del diario | Int64 | Equals
J.JN | Nombre del diario | Cadena | Equals, StartsWith
Pt | Tipo de publicación (0: Desconocido, 1: Artículo de revista, 2: Patente, 3: Artículo de conferencia, 4: Capítulo del libro, 5: Libro, 6: Entrada de referencia del libro, 7: Conjunto de documentos, 8: Repositorio) | Cadena | Equals
RId | Lista de identificadores de documentos a los que se hace referencia | Int64[] | Equals
Ti | Título normalizado | Cadena | Equals, StartsWith
W | Palabras únicas en el título | String[] | Equals
Y | Año de publicación | Int32 | Equals, IsBetween

## <a name="extended-metadata-attributes"></a>Atributos de metadatos extendidos ##

NOMBRE | DESCRIPCIÓN               
--- | ---
BT | Tipo de documento BibTex ("a": Artículo de revista, "b": Libro, "c": Capítulo del libro, "p": Artículo de conferencia)
BV | BibTex venue name
CC | Contextos de cita: lista de los identificadores de documento a los que se hace referencia y el contexto correspondiente en el documento (por ejemplo, [{123: ["los murciélagos hindús son conocidos por comer kiwi, como se hace referencia en el documento 123", "las cigüeñas que tocan el saxofón son un nombre equivocado histórico como se muestra en el documento 123"]})
DN | Título de documento original
DOI | Identificador de objeto digital
FP | Primera página del documento en la publicación
I | Publicación
IA | Descripción breve invertida
IA.IndexLength | Número de elementos del índice (recuento de palabras de la descripción breve)
IA.InvertedIndex | Lista de palabras de la descripción breve y su posición correspondiente en la descripción breve (por ejemplo, [{"el":[0, 15, 30]}, {"murciélago":[1]}, {"hindú":[2]}])
LP | Última página del documento en la publicación
PB | Publicador
S | Orígenes: lista de orígenes web del documento, ordenados por rango estático
S.Ty | Tipo de origen (1:HTML, 2:Texto, 3:PDF, 4:DOC, 5:PPT, 6:XLS, 7:PS)
S.U | Dirección URL de origen
V | Volumen de publicación
VFN | Nombre completo de la ubicación de la conferencia o la revista
VSN | Nombre corto de la ubicación de la conferencia o la revista
