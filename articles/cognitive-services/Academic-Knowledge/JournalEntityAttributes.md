---
title: 'Atributos de la entidad de diario: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información acerca de los atributos que puede usar con la entidad de diario en Academic Knowledge API en Cognitive Services.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 93868bb014809693e7614e74bde7db864de95c04
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704959"
---
# <a name="journal-entity"></a>Entidad de diario

<sub> *Los atributos siguientes son específicos de la entidad de diario. (Ty = "2") </sub>

NOMBRE    |DESCRIPCIÓN                            |type       | Operaciones
------- | ------------------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                              |Int64      |Equals
DJN     |Nombre normalizado de diario                |Cadena     |None
JN      |Nombre para mostrar de diario                   |Cadena     |Equals
CC      |Recuento total de cita de diario           |Int32      |None  
ECC     |Recuento total de cita de estimación de diario |Int32      |None
