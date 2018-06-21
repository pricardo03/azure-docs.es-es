---
title: 'Ejemplo de directiva de Azure API Management: envío de información contextual de la solicitud al servicio de back-end | Microsoft Docs'
description: 'Ejemplo de directiva de Azure API Management: demuestra cómo enviar información contextual de la solicitud al servicio back-end.'
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
ms.openlocfilehash: d6cfd6e63dbc8a56179197b2942c52d15539ae74
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285554"
---
# <a name="send-request-context-information-to-the-backend-service"></a>Envío de información contextual de la solicitud al servicio de back-end

Este artículo incluye un ejemplo de directiva de Azure API Management que muestra cómo enviar información contextual de la solicitud al servicio back-end. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-samples.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Send request context information to the backend service.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-samples.md)

