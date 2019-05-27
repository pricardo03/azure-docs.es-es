---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/01/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: deabef0c2c3540e515fe72a161710c95a20fa86f
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66146963"
---
Abra la consola de PowerShell con privilegios elevados.



Si ejecuta Azure PowerShell localmente, conéctese a la cuenta de Azure. El cmdlet *Connect-AzAccount* le pide las credenciales. Después de la autenticación, descarga la configuración de la cuenta para que esté disponible en Azure PowerShell. Si no ejecuta PowerShell localmente y, en su lugar, usa Azure Cloud Shell 'Try it' en el explorador web, omita este primer paso. Se conectará a la cuenta de Azure automáticamente.

```azurepowershell
Connect-AzAccount
```

Si tiene más de una suscripción, obtenga una lista de las suscripciones de Azure.

```azurepowershell-interactive
Get-AzSubscription
```

Especifique la suscripción que desea usar.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName "Name of subscription"
```