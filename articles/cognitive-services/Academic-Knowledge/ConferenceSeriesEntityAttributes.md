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
ms.openlocfilehash: 38b4aa4c899668a68041f042ce6981ddd8c58219
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884926"
---
# <a name="conference-series-entity"></a>Entidad de serie de conferencias

<sub> *Los atributos siguientes son específicos de la entidad de serie de conferencias. (Ty = '3') </sub>

NOMBRE    |DESCRIPCIÓN                            |Type       | Operaciones
------- | ------------------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                              |Int64      |Equals
CN      |Nombre normalizado de la serie de conferencias      |string     |Equals
DCN     |Nombre para mostrar de la serie de conferencias         |string     |None
CC      |Recuento de citas total de la serie de conferencias         |Int32      |None  
ECC     |Recuento de citas estimado total de la serie de conferencias   |Int32      |None
F.FId   |Identificador de entidad del campo de estudio asociado a la serie de conferencias |Int64  | Equals
F.FN    |Nombre del campo de estudio asociado a la serie de conferencias  | Equals,<br/>StartsWith
