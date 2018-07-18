---
title: 'Ejemplo de directiva de Azure API Management: adición de un encabezado de reenviado | Microsoft Docs'
description: 'Ejemplo de directiva de Azure API Management: demuestra cómo agregar un encabezado de reenviado a la solicitud de entrada para permitir que la API de back-end cree direcciones URL correctas.'
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
ms.openlocfilehash: a9dfcbf4be4b659d761d66d67d2ae4c7b70a245e
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284171"
---
# <a name="add-a-forwarded-header"></a>Adición de un encabezado de reenviado

Este artículo muestra un ejemplo de directiva de Azure API Management que demuestra cómo agregar un encabezado de reenviado a la solicitud de entrada para permitir que la API de back-end cree direcciones URL correctas. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-samples.md).

## <a name="code"></a>Código

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Forward gateway hostname to backend for generating correct urls in responses.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-samples.md)
