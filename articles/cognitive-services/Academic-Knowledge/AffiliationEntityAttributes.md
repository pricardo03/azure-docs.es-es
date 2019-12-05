---
title: Atributos de la entidad de afiliación de Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de afiliación en Academic Knowledge API.
services: cognitive-services
author: darrine
manager: kuansanw
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: 52dcccebe1a1cbab7a6afadeb6b543b34b8b1eb7
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2019
ms.locfileid: "74143920"
---
# <a name="affiliation-entity"></a>Entidad de afiliación

> [!NOTE]
> Los atributos siguientes son específicos de la entidad de afiliación. (Ty = '5')

NOMBRE | DESCRIPCIÓN | type | Operaciones
--- | --- | --- | ---
AfN | Nombre normalizado de la afiliación |Cadena |Equals
CC | Recuento de citas total de la afiliación |Int32        |None  
DAfN | Nombre para mostrar de la afiliación |Cadena |None
E | Metadatos extendidos</br></br>**IMPORTANTE**: Este atributo está en desuso y solo se admite para aplicaciones heredadas. La solicitud individual de este atributo (es decir, attributes=Id,Ti,E) tendrá como resultado la devolución de todos los atributos de metadatos extendidos en una *cadena JSON serializada*.</br></br>Todos los atributos contenidos en los metadatos extendidos están ahora disponibles como atributo de nivel superior y se pueden solicitar como tal (es decir, attributes=Id,Ti,DOI,IA). | [Extendido](#extended) | None
ECC | Recuento de citas estimado total de la afiliación |Int32 |None
Id | El identificador de entidad |Int64 |Equals
PC | Número total de publicaciones escritas en afiliación con esta entidad | Int32 | None

## <a name="extended"></a>Extendido

> [!IMPORTANT]
> Este atributo está en desuso y solo se admite para aplicaciones heredadas. La solicitud individual de este atributo (es decir, attributes=Id,Ti,E) tendrá como resultado la devolución de todos los atributos de metadatos extendidos en una *cadena JSON serializada*.</br></br>Todos los atributos contenidos en los metadatos extendidos están ahora disponibles como atributo de nivel superior y se pueden solicitar como tal (es decir, attributes=Id,Ti,DOI,IA).

> [!IMPORTANT]
> La compatibilidad para solicitar atributos extendidos individuales mediante el uso del ámbito "E." ; esto es, "E.DN", está en desuso. Aunque todavía se admite técnicamente, la solicitud de atributos extendidos individuales con el ámbito "E." dará como resultado que el valor del atributo se devuelva en dos lugares en la respuesta JSON, como parte del objeto "E" y como atributo de nivel superior.

NOMBRE | DESCRIPCIÓN | type | Operaciones
--- | --- | --- | ---
PC | Número total de publicaciones escritas en afiliación con esta entidad | Int32 | None
