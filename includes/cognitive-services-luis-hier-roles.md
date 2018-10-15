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
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: dd99218765c873120c4117a3ec1712fe0a605e66
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164594"
---
**Pregunta**: ¿Se debería utilizar una entidad jerárquica o un patrón con una entidad simple con roles? 

**Respuesta**: Depende. Los patrones y las expresiones de ejemplo son comparables en el sentido de que representan la expresión de un usuario y son específicos de una intención.  

Si las expresiones no tienen un patrón claro, utilice entidades jerárquicas. 

|Entidades jerárquicas|Entidades simples con roles|
|--|--|
|deben tener expresiones de ejemplo con entidades secundarias etiquetadas en intenciones|deben tener expresiones de ejemplo, **los roles no se pueden etiquetar en intenciones**|
|se pueden usar en patrones|se **deben** usar en patrones|
|pueden necesitar **más** expresiones de ejemplo en la intención|pueden necesitar **menos expresiones de ejemplo en la intención|