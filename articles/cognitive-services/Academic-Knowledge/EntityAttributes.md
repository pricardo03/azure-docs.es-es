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
ms.openlocfilehash: 485775660ecfdf2291365ab98c9188295ea2cbde
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61340186"
---
# <a name="entity-attributes"></a>Atributos de entidad

Academic Graph consta de 7 tipos de entidad. Todas las entidades tendrán un identificador de entidad y un tipo de entidad.

## <a name="common-entity-attributes"></a>Atributos de entidad comunes
NOMBRE    |DESCRIPCIÓN                |Type       | Operaciones
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

