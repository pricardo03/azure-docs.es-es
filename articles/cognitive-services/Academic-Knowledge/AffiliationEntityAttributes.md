---
title: Atributos de la entidad de afiliación de Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de afiliación en Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 82e6a5b66342e58e62da029d617cbd1d74c28149
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61340526"
---
# <a name="affiliation-entity"></a>Entidad de afiliación

<sub> *Los atributos siguientes son específicos de la entidad de afiliación. (Ty = '5') </sub>

NOMBRE    |DESCRIPCIÓN                            |Type       | Operaciones
------- | ------------------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                              |Int64      |Equals
AfN     |Nombre normalizado de la afiliación        |String     |Equals
DAfN    |Nombre para mostrar de la afiliación       |String     |None
CC      |Recuento de citas total de la afiliación           |Int32      |None  
ECC     |Recuento de citas estimado total de la afiliación |Int32      |None

## <a name="extended-metadata-attributes"></a>Atributos de metadatos extendidos ##

NOMBRE    | DESCRIPCIÓN               
--------|---------------------------    
PC      |Recuento de artículos de la afiliación
