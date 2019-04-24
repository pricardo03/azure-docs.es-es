---
title: 'Ejemplo de directiva de Azure API Management: implementación del patrón X-CSRF | Microsoft Docs'
description: 'Ejemplo de directiva de Azure API Management: muestra cómo implementar el patrón X-CSRF que usan numerosas API. Este ejemplo es específico de puerta de enlace de SAP.'
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
ms.openlocfilehash: 2f4d26702443ef3113dad98cde1d13b292fe657a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60306706"
---
# <a name="implement-x-csrf-pattern"></a>Implementación del patrón de X-CSRF

Este artículo muestra un ejemplo de directiva de Azure API Management que demuestra cómo implementar el patrón X-CSRF que usan numerosas API. Este ejemplo es específico de puerta de enlace de SAP. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-samples.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get X-CSRF token from SAP gateway using send request.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-samples.md)

