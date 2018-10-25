---
title: 'Atributos de la entidad de diario: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información acerca de los atributos que puede usar con la entidad de diario en Academic Knowledge API en Cognitive Services.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 45b2860b56e79c197d96998350131a142326157a
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902814"
---
# <a name="journal-entity"></a>Entidad de diario

<sub> *Los atributos siguientes son específicos de la entidad de diario. (Ty = "2") </sub>

NOMBRE    |DESCRIPCIÓN                            |Escriba       | Operaciones
------- | ------------------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                              |Int64      |Equals
DJN     |Nombre normalizado de diario                |string     |None
JN      |Nombre para mostrar de diario                   |string     |Equals
CC      |Recuento total de cita de diario           |Int32      |None  
ECC     |Recuento total de cita de estimación de diario |Int32      |None