---
title: 'Ejemplo de directiva de API Management: adición de funcionalidades a un servicio de back-end'
titleSuffix: Azure API Management
description: 'Ejemplo de directiva de Azure API Management: demuestra cómo agregar funcionalidades a un servicio de back-end. Por ejemplo, aceptar el nombre de un lugar en vez de su latitud y longitud en una API de pronóstico meteorológico.'
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
ms.openlocfilehash: e286f77009a17870332ed5caf1c66c18dd21d020
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75442484"
---
# <a name="add-capabilities-to-a-backend-service"></a>Adición de funcionalidades a un servicio de back-end

Este artículo incluye un ejemplo de directiva de Azure API Management que muestra cómo agregar funcionalidades a un servicio back-end. Por ejemplo, aceptar el nombre de un lugar en vez de su latitud y longitud en una API de pronóstico meteorológico. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-samples.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Call out to an HTTP endpoint and cache the response.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-samples.md)

