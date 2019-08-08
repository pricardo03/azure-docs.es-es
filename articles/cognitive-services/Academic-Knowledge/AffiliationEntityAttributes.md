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
ROBOTS: NOINDEX
ms.openlocfilehash: 333875472d9b859196c4d828061301b9d32c4d5a
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705112"
---
# <a name="affiliation-entity"></a>Entidad de afiliación

<sub> *Los atributos siguientes son específicos de la entidad de afiliación. (Ty = '5') </sub>

NOMBRE    |DESCRIPCIÓN                            |type       | Operaciones
------- | ------------------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                              |Int64      |Equals
AfN     |Nombre normalizado de la afiliación        |Cadena     |Equals
DAfN    |Nombre para mostrar de la afiliación       |Cadena     |None
CC      |Recuento de citas total de la afiliación           |Int32      |None  
ECC     |Recuento de citas estimado total de la afiliación |Int32      |None

## <a name="extended-metadata-attributes"></a>Atributos de metadatos extendidos ##

NOMBRE    | DESCRIPCIÓN               
--------|---------------------------    
PC      |Recuento de artículos de la afiliación
