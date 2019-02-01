---
title: Atributos de la entidad de afiliación de Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de afiliación en Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 344b26b16f74cd44982e3c93fa69295792daa9a0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190653"
---
# <a name="affiliation-entity"></a>Entidad de afiliación

<sub> *Los atributos siguientes son específicos de la entidad de afiliación. (Ty = '5') </sub>

NOMBRE    |DESCRIPCIÓN                            |Type       | Operaciones
------- | ------------------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                              |Int64      |Equals
AfN     |Nombre normalizado de la afiliación        |string     |Equals
DAfN    |Nombre para mostrar de la afiliación       |string     |None
CC      |Recuento de citas total de la afiliación           |Int32      |None  
ECC     |Recuento de citas estimado total de la afiliación |Int32      |None

## <a name="extended-metadata-attributes"></a>Atributos de metadatos extendidos ##

NOMBRE    | DESCRIPCIÓN               
--------|---------------------------    
PC      |Recuento de artículos de la afiliación
