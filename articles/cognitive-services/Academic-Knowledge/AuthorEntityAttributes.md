---
title: 'Atributos de la entidad de autor: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de autor en Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 62e6da6d558a0494fb83115b1e307081099399d4
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900696"
---
# <a name="author-entity"></a>Entidad de autor
<sub> *Los atributos siguientes son específicos de la entidad de autor. (Ty = "1") </sub>

NOMBRE    |DESCRIPCIÓN                            |Escriba       | Operaciones
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