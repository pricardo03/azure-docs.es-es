---
title: 'Atributos de la entidad de campo de estudio: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de campo de estudio en Academic Knowledge API.
services: cognitive-services
author: darrine
manager: kuansanw
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: b6bb5e0fd822541a0a4c10ef5f275d46664f8e10
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2019
ms.locfileid: "74146477"
---
# <a name="field-of-study-entity"></a>Entidad de campo de estudio

> [!NOTE]
> Los atributos siguientes son específicos de la entidad de campo de estudio. (Ty = '6')

NOMBRE | DESCRIPCIÓN | type | Operaciones
--- | --- | --- | ---
CC      |Recuento de citas total de campo de estudio    |Int32      |None  
DFN     |Nombre para mostrar de campo de estudio            |Cadena     |None
ECC     |Recuento de citas estimado total de campo de estudio|Int32      |None
FL      |Nivel en jerarquía de campos de estudio     |Int32      |Equals, IsBetween
FN      |Nombre normalizado de campo de estudio         |Cadena     |Equals
FC.FId  |Identificador de campo de estudio secundario                |Int64      |Equals
FC.FN   |Nombre de campo de estudio secundario              |Cadena     |Equals
FP.FId  |Identificador de campo de estudio primario               |Int64      |Equals
FP.FN   |Nombre de campo de estudio primario             |Cadena     |Equals
Id      |El identificador de entidad                              |Int64      |Equals
PC    | Recuento total de publicaciones de campo de estudio | Int32 | None
