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
ms.openlocfilehash: ffb159dc684b4b6663dcb966706d4745ab88a403
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61337812"
---
# <a name="journal-entity"></a>Entidad de diario

<sub> *Los atributos siguientes son específicos de la entidad de diario. (Ty = "2") </sub>

NOMBRE    |DESCRIPCIÓN                            |Type       | Operaciones
------- | ------------------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                              |Int64      |Equals
DJN     |Nombre normalizado de diario                |String     |None
JN      |Nombre para mostrar de diario                   |String     |Equals
CC      |Recuento total de cita de diario           |Int32      |None  
ECC     |Recuento total de cita de estimación de diario |Int32      |None
