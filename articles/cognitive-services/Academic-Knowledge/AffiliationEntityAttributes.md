---
title: Atributos de la entidad de afiliación de Academic Knowledge API | Microsoft Docs
description: Obtenga información acerca de los atributos que puede usar con la entidad de afiliación en Academic Knowledge API en Cognitive Services.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: a0ec67cb811ca207b3d038028491da2516028f0b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35379991"
---
# <a name="affiliation-entity"></a>Entidad de afiliación

<sub> *Los atributos siguientes son específicos de la entidad de afiliación. (Ty = '5') </sub>

NOMBRE    |DESCRIPCIÓN                            |Escriba       | Operaciones
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