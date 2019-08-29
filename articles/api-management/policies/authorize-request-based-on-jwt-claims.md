---
title: 'Ejemplo de directiva de Azure API Management: autorización de acceso con notificaciones JWT | Microsoft Docs'
description: 'Ejemplo de directiva de Azure API Management: demuestra cómo autorizar el acceso a los métodos HTTP específicos en una API con notificaciones JWT.'
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
ms.openlocfilehash: dd99d9ed3eebe6ada60511b3f16c53b0d57a65d6
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70067805"
---
# <a name="authorize-access-based-on-jwt-claims"></a>Autorización de acceso con notificaciones JWT

Este artículo incluye un ejemplo de directiva de Azure API Management que muestra cómo autorizar el acceso a los métodos HTTP específicos en una API con notificaciones JWT. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-samples.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Pre-authorize requests based on HTTP method with validate-jwt.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-samples.md)

