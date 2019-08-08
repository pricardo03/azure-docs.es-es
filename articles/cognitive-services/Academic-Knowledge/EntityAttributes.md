---
title: 'Atributos de la entidad de Academic Graph: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Aprenda sobre los atributos de entidad que puede usar con Academic Graph en Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 596ce35a0d744691edd75d6071d4758691e0031b
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705032"
---
# <a name="entity-attributes"></a>Atributos de entidad

Academic Graph consta de 7 tipos de entidad. Todas las entidades tendrán un identificador de entidad y un tipo de entidad.

## <a name="common-entity-attributes"></a>Atributos de entidad comunes
NOMBRE    |DESCRIPCIÓN                |type       | Operaciones
------- | ------------------------- | --------- | ----------------------------
Id      |El identificador de entidad                  |Int64      |Equals
Ty      |Tipo de entidad                |enum   |Equals

## <a name="entity-type-enum"></a>Tipo de entidad enum
NOMBRE                                                            |value
----------------------------------------------------------------|-----
[Documento](PaperEntityAttributes.md)                               |0
[Autor](AuthorEntityAttributes.md)                             |1
[Diario](JournalEntityAttributes.md)                           |2
[Serie de conferencias](JournalEntityAttributes.md)                 |3
[Instancia de conferencia](ConferenceInstanceEntityAttributes.md)    |4
[Afiliación](AffiliationEntityAttributes.md)                   |5
[Campo de estudio](FieldsOfStudyEntityAttributes.md)                      |6

