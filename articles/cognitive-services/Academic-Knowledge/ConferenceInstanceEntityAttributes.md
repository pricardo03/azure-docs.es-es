---
title: 'Atributos de la entidad de instancia de conferencia: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los atributos que puede usar con la entidad de instancia de conferencia en Academic Knowledge API.
services: cognitive-services
author: darrine
manager: kuansanw
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: c6761206a58724dae96d9dc6f6234658892d4d18
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2019
ms.locfileid: "74146567"
---
# <a name="conference-instance-entity"></a>Entidad de instancia de conferencia

> [!NOTE]
> Los atributos siguientes son específicos de la entidad de instancia de conferencia. (Ty = '4')

NOMBRE | DESCRIPCIÓN | type | Operaciones
--- | --- | --- | ---
CC      |Recuento de citas total de la instancia de conferencia           |Int32      |None  
CD.D    |Fecha de un evento de la instancia de conferencia    |Date       |Equals, IsBetween
CD.T    |Título de un evento de la instancia de conferencia   |Date       |Equals, IsBetween
CIARD   |Fecha de vencimiento del registro del resumen de la instancia de conferencia  |Date       |Equals, IsBetween
CIED    |Fecha de finalización de la instancia de conferencia    |Date       |Equals, IsBetween
CIFVD   |Fecha de vencimiento de la versión final de la instancia de conferencia  |Date       |Equals, IsBetween
CIL     |Ubicación de la instancia de conferencia    |Cadena     |Equals, StartsWith
CIN     |Nombre normalizado de la instancia de conferencia |Cadena        |Equals
CINDD   |Fecha de notificación de la instancia de conferencia   |Date       |Equals, IsBetween
CISD    |Fecha de inicio de la instancia de conferencia  |Date       |Equals, IsBetween
CISDD   |Fecha de vencimiento del envío de la instancia de conferencia     |Date       |Equals, IsBetween
DCN     |Nombre para mostrar de la instancia de conferencia  |Cadena      |None
E | Metadatos extendidos</br></br>**IMPORTANTE**: Este atributo está en desuso y solo se admite para aplicaciones heredadas. La solicitud individual de este atributo (es decir, attributes=Id,Ti,E) tendrá como resultado la devolución de todos los atributos de metadatos extendidos en una *cadena JSON serializada*.</br></br>Todos los atributos contenidos en los metadatos extendidos están ahora disponibles como atributo de nivel superior y se pueden solicitar como tal (es decir, attributes=Id,Ti,DOI,IA). | [Extendido](#extended) | None
ECC     |Recuento de citas estimado total de la instancia de conferencia |Int32      |None
FN | Nombre completo de la instancia de conferencia | Cadena | None
Id      |El identificador de entidad                              |Int64      |Equals
PC | Recuento total de publicaciones de la instancia de conferencia | Int32 | None
PCS.CN  |Nombre de la serie de conferencias primarias de la instancia |Cadena  |Equals
PCS.CId |Identificador de la serie de conferencias primarias de la instancia |Int64     |Equals

## <a name="extended"></a>Extendido

> [!IMPORTANT]
> Este atributo está en desuso y solo se admite para aplicaciones heredadas. La solicitud individual de este atributo (es decir, attributes=Id,Ti,E) tendrá como resultado la devolución de todos los atributos de metadatos extendidos en una *cadena JSON serializada*.</br></br>Todos los atributos contenidos en los metadatos extendidos están ahora disponibles como atributo de nivel superior y se pueden solicitar como tal (es decir, attributes=Id,Ti,DOI,IA).

> [!IMPORTANT]
> La compatibilidad para solicitar atributos extendidos individuales mediante el uso del ámbito "E." ; esto es, "E.DN", está en desuso. Aunque todavía se admite técnicamente, la solicitud de atributos extendidos individuales con el ámbito "E." dará como resultado que el valor del atributo se devuelva en dos lugares en la respuesta JSON, como parte del objeto "E" y como atributo de nivel superior.

NOMBRE | DESCRIPCIÓN | type | Operaciones
--- | --- | --- | ---
FN | Nombre completo de la instancia de conferencia | Cadena | None
PC | Recuento total de publicaciones de la instancia de conferencia | Int32 | None
