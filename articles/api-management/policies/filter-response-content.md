---
title: 'Ejemplo de directiva de Azure API Management: filtro de contenido de respuestas | Microsoft Docs'
description: 'Ejemplo de directiva de Azure API Management: demuestra cómo filtrar los elementos de datos de la carga de respuesta según el producto asociado a la solicitud.'
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
ms.openlocfilehash: 26829dfc04acdac2a25fe0d4fdc3e95e4d219057
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52869184"
---
# <a name="filter-response-content"></a>Filtro de contenido de respuestas

Este artículo muestra un ejemplo de directiva de Azure API Management que demuestra cómo filtrar los elementos de datos de la carga de respuesta según el producto asociado a la solicitud. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-samples.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque de **salida**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Filter response content based on product name.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-samples.md)

