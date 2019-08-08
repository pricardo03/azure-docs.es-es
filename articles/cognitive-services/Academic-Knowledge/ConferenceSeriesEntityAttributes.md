---
title: 'Atributos de la entidad de serie de conferencias: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de serie de conferencias.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: feed324202f6a75ceb7e9089875b899c51cd8ae6
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705036"
---
# <a name="conference-series-entity"></a>Entidad de serie de conferencias

<sub> *Los atributos siguientes son específicos de la entidad de serie de conferencias. (Ty = '3') </sub>

NOMBRE    |DESCRIPCIÓN                            |type       | Operaciones
------- | ------------------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                              |Int64      |Equals
CN      |Nombre normalizado de la serie de conferencias      |Cadena     |Equals
DCN     |Nombre para mostrar de la serie de conferencias         |Cadena     |None
CC      |Recuento de citas total de la serie de conferencias         |Int32      |None  
ECC     |Recuento de citas estimado total de la serie de conferencias   |Int32      |None
F.FId   |Identificador de entidad del campo de estudio asociado a la serie de conferencias |Int64  | Equals
F.FN    |Nombre del campo de estudio asociado a la serie de conferencias  | Equals,<br/>StartsWith
