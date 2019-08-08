---
title: 'Atributos de la entidad de autor: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de autor en Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: e63e9d3f8f17a2473caf3d31b83e318ddb132b43
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705087"
---
# <a name="author-entity"></a>Entidad de autor
<sub> *Los atributos siguientes son específicos de la entidad de autor. (Ty = "1") </sub>

NOMBRE    |DESCRIPCIÓN                            |type       | Operaciones
------- | ------------------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                              |Int64      |Equals
AuN     |Nombre normalizado de autor                 |Cadena     |Equals
DAuN    |Nombre para mostrar de autor                    |Cadena     |None
CC      |Recuento total de cita de autor            |Int32      |None  
ECC     |Recuento total de cita estimada de autor  |Int32      |None
E       |Metadatos extendidos (vea la tabla "Atributos de metadatos extendidos")  |Cadena     |None  


## <a name="extended-metadata-attributes"></a>Atributos de metadatos extendidos ##

NOMBRE    | DESCRIPCIÓN               
--------|---------------------------    
LKA.Afn     | Nombre para mostrar de la afiliación asociado con el autor  
LKA.AfId        | Id. de entidad de la afiliación asociado con el autor
