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
ms.openlocfilehash: d4b33c06ab023023aadf403cf0ef0b08c2bafc5f
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878942"
---
# <a name="author-entity"></a>Entidad de autor
<sub> *Los atributos siguientes son específicos de la entidad de autor. (Ty = "1") </sub>

NOMBRE    |DESCRIPCIÓN                            |Type       | Operaciones
------- | ------------------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                              |Int64      |Equals
AuN     |Nombre normalizado de autor                 |string     |Equals
DAuN    |Nombre para mostrar de autor                    |string     |None
CC      |Recuento total de cita de autor            |Int32      |None  
ECC     |Recuento total de cita estimada de autor  |Int32      |None
E       |Metadatos extendidos (vea la tabla "Atributos de metadatos extendidos")  |string     |None  


## <a name="extended-metadata-attributes"></a>Atributos de metadatos extendidos ##

NOMBRE    | DESCRIPCIÓN               
--------|---------------------------    
LKA.Afn     | Nombre para mostrar de la afiliación asociado con el autor  
LKA.AfId        | Id. de entidad de la afiliación asociado con el autor
