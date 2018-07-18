---
title: 'Ejemplo de directiva de Azure API Management: generación de una firma de acceso compartido | Microsoft Docs'
description: 'Ejemplo de directiva de Azure API Management: muestra cómo generar una firma de acceso compartido mediante expresiones y reenviar la solicitud a Azure Storage con la directiva de reescritura del identificador URI.'
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
ms.openlocfilehash: c8a4d25211a0030c013628e69865406bb6e8899e
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36286295"
---
# <a name="generate-shared-access-signature"></a>Generación de una firma de acceso compartido

Este artículo muestra un ejemplo de directiva de Azure API Management: que muestra cómo generar una [firma de acceso compartido](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) mediante expresiones y reenviar la solicitud a Azure Storage con la directiva de reescritura del identificador URI. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-samples.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Generate Shared Access Signature and forward request to Azure storage.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-samples.md)

