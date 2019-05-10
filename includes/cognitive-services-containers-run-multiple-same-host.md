---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 05/07/2019
ms.openlocfilehash: 19726330561abfad08aec3c4908c855616c6ee29
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2019
ms.locfileid: "65520892"
---
### <a name="running-multiple-containers-on-the-same-host"></a>Ejecución de varios contenedores en el mismo host

Si piensa ejecutar varios contenedores con puertos expuestos, asegúrese de que se ejecute cada contenedor con otro puerto expuesto. Por ejemplo, ejecute el primer contenedor en el puerto 5000 y el segundo en el puerto 5001.

Puede hacer que este contenedor y un contenedor de servicios cognitivos diferentes ejecuten conjuntamente en el HOST o puede tener varios contenedores del mismo contenedor Cognitive Service ejecutando.
