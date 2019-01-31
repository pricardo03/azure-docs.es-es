---
title: 'Atributos de la entidad de serie de conferencias: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de serie de conferencias.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: ff71b489cce01d8d6ea29e09905d7d3ac8429e34
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55155614"
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
