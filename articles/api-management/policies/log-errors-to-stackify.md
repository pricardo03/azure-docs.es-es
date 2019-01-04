---
title: 'Ejemplo de directiva de Azure API Management: envío de errores a Stackify para su registro | Microsoft Docs'
description: 'Ejemplo de directiva de Azure API Management: demuestra cómo agregar una directiva de registro de errores para enviar errores a Stackify para su registro.'
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
ms.openlocfilehash: 07cc83830fe2d467c611622bb66dfbb8c9429c2d
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52864883"
---
# <a name="send-errors-to-stackify-for-logging"></a>Envío de errores a Stackify para su registro

Este artículo muestra un ejemplo de directiva de Azure API Management que demuestra cómo agregar una directiva de registro de errores para enviar errores a Stackify para su registro. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-samples.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque **on-error**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Log errors to Stackify.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-samples.md)

