---
title: 'Ejemplo de directiva de Azure API Management: adición de un encabezado que contiene un identificador de correlación | Microsoft Docs'
description: 'Ejemplo de directiva de Azure API Management: muestra cómo agregar un encabezado con un identificador de correlación para la solicitud de entrada.'
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
ms.openlocfilehash: 9f091345a4eaf174c47959cad3cb4525fd926689
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70074208"
---
# <a name="add-a-header-containing-a-correlation-id"></a>Incorporación de un encabezado con un identificador de correlación

Este artículo muestra un ejemplo de directiva de Azure API Management que demuestra cómo agregar un encabezado con un identificador de correlación para la solicitud de entrada. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-samples.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Add correlation id to inbound request.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-samples.md)

