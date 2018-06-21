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
ms.openlocfilehash: 46b6d391d6a1ee569dc27c31a0718b23a120c632
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36286725"
---
# <a name="send-errors-to-stackify-for-logging"></a>Envío de errores a Stackify para su registro

Este artículo muestra un ejemplo de directiva de Azure API Management que demuestra cómo agregar una directiva de registro de errores para enviar errores a Stackify para su registro. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-samples.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque **on-error**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Log errors to Stackify.policy.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-samples.md)

