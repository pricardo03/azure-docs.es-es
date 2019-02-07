---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 10/23/2018
ms.author: diberry
ms.openlocfilehash: dee31fca841ea309128681637cc41fa0fb1e7aea
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55480572"
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
