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
ms.openlocfilehash: 1aca39a7ff162aa3c42fdb3ca5999c71091ec02e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60364763"
---
 Si utilizas Azure Cloud Shell, iniciar sesión su cuenta de Azure automáticamente después de hacer clic en 'Try'. Para iniciar sesión localmente, abra la consola de PowerShell con privilegios elevados y ejecute el cmdlet para conectarse.

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