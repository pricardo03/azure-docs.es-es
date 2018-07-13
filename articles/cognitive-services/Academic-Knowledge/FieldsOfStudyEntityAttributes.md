---
title: Atributos de la entidad de campo de estudio de Academic Knowledge API | Microsoft Docs
description: Obtenga información acerca de los atributos que puede usar con la entidad de campo de estudio en Academic Knowledge API en Cognitive Services.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: a8176370f5b2f63b7741f7e892ed5a8c775f19c1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35379998"
---
# <a name="field-of-study-entity"></a>Entidad de campo de estudio

<sub> *Los atributos siguientes son específicos de la entidad de campo de estudio. (Ty = '6') </sub>

NOMBRE    |DESCRIPCIÓN                            |Escriba       | Operaciones
------- | ------------------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                              |Int64      |Equals
FN      |Nombre normalizado de campo de estudio         |string     |Equals
DFN     |Nombre para mostrar de campo de estudio            |string     |None
CC      |Recuento de citas total de campo de estudio    |Int32      |None  
ECC     |Recuento de citas estimado total de campo de estudio|Int32      |None
FL      |Nivel en jerarquía de campos de estudio     |Int32      |Equals, <br/>IsBetween
FP.FN   |Nombre de campo de estudio primario             |string     |Equals
FP.FId  |Identificador de campo de estudio primario               |Int64      |Equals
FC.FN   |Nombre de campo de estudio secundario              |string     |Equals
FC.FId  |Identificador de campo de estudio secundario                |Int64      |Equals