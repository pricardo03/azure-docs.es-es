---
title: 'Atributos de la entidad de autor: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de autor en Academic Knowledge API.
services: cognitive-services
author: darrine
manager: kuansanw
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: d5fc770c380397f605f8810fa41d3a8907f2358e
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2019
ms.locfileid: "74146493"
---
# <a name="author-entity"></a>Entidad de autor

> [!NOTE]
> Los atributos siguientes son específicos de la entidad de autor. (Ty = '1')

NOMBRE | DESCRIPCIÓN | type | Operaciones
--- | --- | --- | ---
Id      | El identificador de entidad                             |Int64      |Equals
AuN     | Nombre normalizado de autor                    |Cadena     |Equals
CC      | Recuento total de cita de autor           |Int32      |None  
DAuN    | Nombre para mostrar de autor                   |Cadena     |None
E | Metadatos extendidos</br></br>**IMPORTANTE**: Este atributo está en desuso y solo se admite para aplicaciones heredadas. La solicitud individual de este atributo (es decir, attributes=Id,Ti,E) tendrá como resultado la devolución de todos los atributos de metadatos extendidos en una *cadena JSON serializada*.</br></br>Todos los atributos contenidos en los metadatos extendidos están ahora disponibles como atributo de nivel superior y se pueden solicitar como tal (es decir, attributes=Id,Ti,DOI,IA). | [Extendido](#extended) | None
ECC     | Recuento total de cita estimada de autor |Int32      |None
LKA.AfId | Identificador de entidad de la última afiliación conocida encontrada para el autor | Int64 | None
LKA.AfN | Nombre normalizado de la última afiliación conocida encontrada para el autor | Cadena | None
PC | Recuento total de publicaciones del autor | Int32 | None

## <a name="extended"></a>Extendido

> [!IMPORTANT]
> Este atributo está en desuso y solo se admite para aplicaciones heredadas. La solicitud individual de este atributo (es decir, attributes=Id,Ti,E) tendrá como resultado la devolución de todos los atributos de metadatos extendidos en una *cadena JSON serializada*.</br></br>Todos los atributos contenidos en los metadatos extendidos están ahora disponibles como atributo de nivel superior y se pueden solicitar como tal (es decir, attributes=Id,Ti,DOI,IA).

> [!IMPORTANT]
> La compatibilidad para solicitar atributos extendidos individuales mediante el uso del ámbito "E." ; esto es, "E.DN", está en desuso. Aunque todavía se admite técnicamente, la solicitud de atributos extendidos individuales con el ámbito "E." dará como resultado que el valor del atributo se devuelva en dos lugares en la respuesta JSON, como parte del objeto "E" y como atributo de nivel superior.

NOMBRE | DESCRIPCIÓN | type | Operaciones
--- | --- | --- | ---
LKA.AfId | Identificador de entidad de la última afiliación conocida encontrada para el autor | Int64 | None
LKA.AfN | Nombre normalizado de la última afiliación conocida encontrada para el autor | Cadena | None
PC | Recuento total de publicaciones del autor | Int32 | None
