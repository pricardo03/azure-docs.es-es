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
ms.openlocfilehash: a93e1d9fecea59ebb68c512b96c8381b5b1a9346
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284767"
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

