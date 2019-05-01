---
title: 'Ejemplo de directiva de Azure API Management: enrutamiento de la solicitud en función al tamaño de su cuerpo | Microsoft Docs'
description: 'Ejemplo de directiva de Azure API Management: muestra cómo enrutar las solicitudes en función del tamaño de su cuerpo.'
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 7656401115cc370d6eee60fb9bddb9bcd92e4201
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60860951"
---
# <a name="route-the-request-based-on-the-size-of-its-body"></a>Enrutamiento de la solicitud en función del tamaño de su cuerpo

Este artículo incluye un ejemplo de directiva de Azure API Management que muestra cómo enrutar las solicitudes en función del tamaño del cuerpo. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-samples.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Route requests based on size.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-samples.md)

