---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: include
ms.custom: include file
ms.date: 10/23/2018
ms.author: diberry
ms.openlocfilehash: 0ad3b360611a12b95568e9a20f13a57c93b2e603
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51208311"
---
## <a name="roles-versus-hierarchical-entities"></a>Roles frente a entidades jerárquicas

¿Se debería utilizar una entidad jerárquica o un patrón con una entidad simple con roles? 

Depende. Los patrones y las expresiones de ejemplo son comparables en el sentido de que representan la expresión de un usuario y son específicos de una intención.  

Si las expresiones no tienen un patrón claro, utilice entidades jerárquicas. 

|Entidades jerárquicas|Entidades simples con roles|
|--|--|
|Disponibles en los patrones y las expresiones de ejemplo de las intenciones|Solo disponibles en los patrones|
|Deben tener expresiones de ejemplo en intenciones con entidades secundarias etiquetadas|Los roles no se pueden etiquetar en las expresiones de ejemplo en las intenciones|
|Pueden necesitar **más** expresiones de ejemplo en la intención para extraer la entidad|Deben necesitar **menos** expresiones de ejemplo en la intención|
