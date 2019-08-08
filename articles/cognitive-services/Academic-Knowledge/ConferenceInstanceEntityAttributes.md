---
title: 'Atributos de la entidad de instancia de conferencia: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de instancia de conferencia en Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 37a353fbb86ca199b2316dcfba5904f4b46b0276
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705053"
---
# <a name="conference-instance-entity"></a>Entidad de instancia de conferencia

<sub> *Los atributos siguientes son específicos de la entidad de instancia de conferencia. (Ty = '4') </sub>

NOMBRE    |DESCRIPCIÓN                            |type       | Operaciones
------- | ------------------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                              |Int64      |Equals
CIN     |Nombre normalizado de instancia de conferencia ({ConferenceSeriesNormalizedName} {ConferenceInstanceYear})        |Cadena     |Equals
DCN     |Nombre para mostrar de instancia de conferencia ({ConferenceSeriesName} : {ConferenceInstanceYear})       |Cadena     |None
CIL     |Ubicación de la instancia de conferencia    |Cadena     |Equals,<br/>StartsWith
CISD    |Fecha de inicio de la instancia de conferencia  |Date       |Equals,<br/>IsBetween
CIED    |Fecha de finalización de la instancia de conferencia    |Date       |Equals,<br/>IsBetween
CIARD   |Fecha de vencimiento del registro del resumen de la instancia de conferencia  |Date       |Equals,<br/>IsBetween
CISDD   |Fecha de vencimiento del envío de la instancia de conferencia     |Date       |Equals,<br/>IsBetween
CIFVD   |Fecha de vencimiento de la versión final de la instancia de conferencia  |Date       |Equals,<br/>IsBetween
CINDD   |Fecha de notificación de la instancia de conferencia   |Date       |Equals,<br/>IsBetween
CD.T    |Título de un evento de la instancia de conferencia   |Date       |Equals,<br/>IsBetween
CD.D    |Fecha de un evento de la instancia de conferencia    |Date       |Equals,<br/>IsBetween
PCS.CN  |Nombre de la serie de conferencias de la instancia |Cadena     |Equals
PCS.CId |Identificador de la serie de conferencias de la instancia |Int64    |Equals
CC      |Recuento de citas total de la instancia de conferencia           |Int32      |None  
ECC     |Recuento de citas estimado total de la instancia de conferencia |Int32      |None


## <a name="extended-metadata-attributes"></a>Atributos de metadatos extendidos ##

NOMBRE    | DESCRIPCIÓN               
--------|---------------------------    
FN      | Nombre completo de la instancia de conferencia
