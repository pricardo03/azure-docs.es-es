---
title: 'Atributos de la entidad de Academic Graph: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Aprenda sobre los atributos de entidad que puede usar con Academic Graph en Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: f227cc03578adcfbf73fec3ae8941045e8352513
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55183006"
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

