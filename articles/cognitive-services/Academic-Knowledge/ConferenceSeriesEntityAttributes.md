---
title: Atributos de la entidad de serie de conferencias de Academic Knowledge API | Microsoft Docs
description: Obtenga información acerca de los atributos que puede utilizar con la entidad de serie de conferencias en Cognitive Services.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 332736c927bdaa00334546f626a6eabb8e11d3b5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35379999"
---
# <a name="conference-series-entity"></a>Entidad de serie de conferencias

<sub> *Los atributos siguientes son específicos de la entidad de serie de conferencias. (Ty = '3') </sub>

NOMBRE    |DESCRIPCIÓN                            |Escriba       | Operaciones
------- | ------------------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                              |Int64      |Equals
CN      |Nombre normalizado de la serie de conferencias      |string     |Equals
DCN     |Nombre para mostrar de la serie de conferencias         |string     |None
CC      |Recuento de citas total de la serie de conferencias         |Int32      |None  
ECC     |Recuento de citas estimado total de la serie de conferencias   |Int32      |None
F.FId   |Identificador de entidad del campo de estudio asociado a la serie de conferencias |Int64  | Equals
F.FN    |Nombre del campo de estudio asociado a la serie de conferencias  | Equals,<br/>StartsWith