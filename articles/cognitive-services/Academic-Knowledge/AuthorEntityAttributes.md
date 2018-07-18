---
title: Atributos de la entidad de autor de Academic Knowledge API | Microsoft Docs
description: Obtenga información acerca de los atributos que puede usar con la entidad de autor en Academic Knowledge API en Cognitive Services.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: afcc3257c49522a4631c4aae0e0c2b88373f9af1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35379994"
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