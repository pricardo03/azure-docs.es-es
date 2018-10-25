---
title: 'Atributos de la entidad de serie de conferencias: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de serie de conferencias.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: f9f28afd7005d7a61aa0d2f4dba69ca598034b52
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900755"
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