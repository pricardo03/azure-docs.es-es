---
title: 'Muestra de directiva de Azure API Management: use OAuth2 para la autorización entre la puerta de enlace y un back-end'
titleSuffix: Azure API Management
description: 'Ejemplo de directiva de Azure API Management: se explica cómo usar OAuth2 para la autorización entre la puerta de enlace y un back-end. Muestra cómo obtener un token de acceso en AAD y reenviarlo al back-end.'
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 09d51759c07e7dacc25d5b5ffce9698831c37a7d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75442367"
---
# <a name="use-oauth2-for-authorization-between-the-gateway-and-a-backend"></a>Use OAuth2 para la autorización entre la puerta de enlace y un back-end

En este artículo se explica un ejemplo de directiva de Azure API Management que demuestra cómo usar OAuth2 para la autorización entre la puerta de enlace y un back-end. Muestra cómo obtener un token de acceso en AAD y reenviarlo al back-end. 

Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-samples.md).

El siguiente script usa propiedades que aparecen en {{property}}. Para información sobre las propiedades y cómo se usan en las directivas de API Management, consulte [este](../api-management-howto-properties.md) tema.
 
## <a name="policy"></a>Directiva

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get OAuth2 access token from AAD and forward it to the backend.policy.xml)]
  
## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-samples.md)

