---
title: Atributos de la entidad Academic Graph para Academic Knowledge API | Microsoft Docs
description: Aprenda sobre los atributos de entidad que puede usar con Academic Graph en Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: f771969c3c6f05e5d2c99b1fbf593ae039ccb12c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35379990"
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

