---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 853bd32cf3ea97571929d54fb7d3ba04bde16b81
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60405851"
---
Antes de comenzar esta configuración, debe iniciar sesión en su cuenta de Azure. El cmdlet pide las credenciales de inicio de sesión para la cuenta de Azure. Después de iniciar la sesión, se descarga la configuración de la cuenta a fin de que esté disponible para Azure PowerShell. Para obtener más información, consulte [Uso de Windows PowerShell con el Administrador de recursos](../articles/powershell-azure-resource-manager.md).

Para iniciar sesión abra la consola de PowerShell con privilegios elevados y conéctese a su cuenta. Use el siguiente ejemplo para conectarse:

```powershell
Connect-AzAccount
```

Si tiene varias suscripciones de Azure, compruebe las suscripciones de la cuenta.

```powershell
Get-AzSubscription
```

Especifique la suscripción que desea usar.

```powershell
Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
 ```
