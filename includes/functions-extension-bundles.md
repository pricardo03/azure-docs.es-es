---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/27/2019
ms.author: glenga
ms.openlocfilehash: 8110d0a9d574c6691322df2162ca877b031cbc59
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442274"
---
La forma más fácil de instalar extensiones de enlace es habilitar [conjuntos de extensiones](../articles/azure-functions/functions-bindings-register.md#extension-bundles). Con los conjuntos habilitados, un conjunto predefinido de paquetes de extensiones se instala automáticamente.

Para habilitar los conjuntos de extensiones, abra el archivo *host.json* y actualice su contenido para que coincida con el siguiente código:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```