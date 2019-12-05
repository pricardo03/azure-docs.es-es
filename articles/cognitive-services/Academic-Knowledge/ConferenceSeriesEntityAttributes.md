---
title: 'Atributos de la entidad de serie de conferencias: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de serie de conferencias.
services: cognitive-services
author: darrine
manager: kuansanw
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: ecd8042212987849d9d302642f03e59493235599
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2019
ms.locfileid: "74143915"
---
# <a name="conference-series-entity"></a>Entidad de serie de conferencias

> [!NOTE]
> Los atributos siguientes son específicos de la entidad de serie de conferencias. (Ty = '3')

NOMBRE | DESCRIPCIÓN | type | Operaciones
--- | --- | --- | ---
CC      |Recuento de citas total de la serie de conferencias         |Int32      |None  
CN      |Nombre normalizado de la serie de conferencias      |Cadena     |Equals
DCN     |Nombre para mostrar de la serie de conferencias         |Cadena     |None
ECC     |Recuento de citas estimado total de la serie de conferencias   |Int32      |None
F.FId   |Identificador de entidad del campo de estudio asociado a la serie de conferencias |Int64  | Equals
F.FN    |Nombre del campo de estudio asociado a la serie de conferencias  | Equals,<br/>StartsWith
Id      |El identificador de entidad                              |Int64      |Equals
PC    |Recuento de publicaciones total de la serie de conferencias |Int32 | None
