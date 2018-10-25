---
title: 'Atributos de la entidad de Academic Graph: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Aprenda sobre los atributos de entidad que puede usar con Academic Graph en Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: a203fdf6562dabb1b9d6e8ab5bb8f46ff6d5dc27
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902797"
---
# <a name="entity-attributes"></a>Atributos de entidad

Academic Graph consta de 7 tipos de entidad. Todas las entidades tendrán un identificador de entidad y un tipo de entidad.

## <a name="common-entity-attributes"></a>Atributos de entidad comunes
NOMBRE    |DESCRIPCIÓN                |Escriba       | Operaciones
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

