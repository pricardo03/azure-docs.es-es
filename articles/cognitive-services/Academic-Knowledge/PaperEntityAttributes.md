---
title: 'Atributos de la entidad de documento: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de documento en Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: d36a39f10f9e0129af3b730a65387d7108209004
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55168386"
---
# <a name="paper-entity"></a>Entidad de documento

<sub> *Los atributos siguientes son específicos de la entidad de documento. (Ty = "0") </sub>


NOMBRE    |DESCRIPCIÓN                                        |Type       | Operaciones
------- | ------------------------------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                                          |Int64      |Equals
Ti      |Título del documento                                        |string     |Equals,<br/>StartsWith
L       |Código de idioma del documento separado con "\@@@"            |string     |Equals
Y       |Año del documento                                         |Int32      |Equals,<br/>IsBetween
D       |Fecha del documento                                         |Date       |Equals,<br/>IsBetween
CC      |Recuento de citas                                     |Int32      |None  
ECC     |Recuento de citas estimado                           |Int32      |None
AA.AuN  |Nombre del autor                                        |string     |Equals,<br/>StartsWith
AA.AuId |Identificador del autor                                          |Int64      |Equals
AA.AfN  |Nombre de afiliación del autor                            |string     |Equals,<br/>StartsWith
AA.AfId |Identificador de afiliación del autor                              |Int64      |Equals
AA.S    |Orden del autor para el documento                         |Int32      |Equals
F.FN    |Nombre de campo de estudio                                |string     |Equals,<br/>StartsWith
F.FId   |Identificador de campo de estudio                                  |Int64      |Equals
J.JN    |Nombre del diario                                       |string     |Equals,<br/>StartsWith
J.JId   |Identificador del diario                                         |Int64      |Equals
C.CN    |Nombre de la serie de conferencias                             |string     |Equals,<br/>StartsWith
C.CId   |Identificador de la serie de conferencias                               |Int64      |Equals
RId     |Identificador de los documentos a los que se hace referencia                              |Int64[]    |Equals
W       |Palabras del título del documento y descripción breve                |String[]   |Equals
E       |Metadatos ampliados (consulte la tabla siguiente)                |string     |None  
        


## <a name="extended-metadata-attributes"></a>Atributos de metadatos extendidos ##

NOMBRE    | DESCRIPCIÓN               
--------|---------------------------    
DN      | Nombre para mostrar del documento 
S       | Orígenes: lista de orígenes web del documento, ordenados por rango estático
S.Ty    | Tipo de origen (1:HTML, 2:Texto, 3:PDF, 4:DOC, 5:PPT, 6:XLS, 7:PS)
S.U     | Dirección URL de origen
VFN     | Nombre completo de la ubicación: nombre completo del diario o la conferencia
VSN     | Nombre corto de la ubicación: nombre corto del diario o la conferencia
V       | Volumen: volumen del diario
BV      | Nombre del diario
BT      | 
PB      | Abreviaturas del diario
I       | Número: número del diario
FP      | FirstPage: primera página del documento
LP      | LastPage - última página del documento
DOI     | Identificador de objeto digital
CC      | Contextos de cita: lista de los identificadores de documento a los que se hace referencia y el contexto correspondiente en el documento (por ejemplo, [{123: ["los murciélagos hindús son conocidos por comer kiwi, como se hace referencia en el documento 123", "las cigüeñas que tocan el saxofón son un nombre equivocado histórico como se muestra en el documento 123"]})
IA      | Descripción breve invertida
IA.IndexLength| Número de elementos del índice (recuento de palabras de la descripción breve)
IA.InvertedIndex| Lista de palabras de la descripción breve y su posición correspondiente en la descripción breve (por ejemplo, [{"el":[0, 15, 30]}, {"murciélago":[1]}, {"hindú":[2]}])
