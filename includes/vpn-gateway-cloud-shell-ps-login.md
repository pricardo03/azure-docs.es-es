---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: fee97b29f24d8bb4f50a2929c3ceb33af85a5e21
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54201489"
---
Abra la consola de PowerShell con privilegios elevados.



Si ejecuta Azure PowerShell localmente, conéctese a la cuenta de Azure. El cmdlet *Connect-AzureRmAccount* le pide las credenciales. Después de la autenticación, descarga la configuración de la cuenta para que esté disponible en Azure PowerShell. Si no ejecuta PowerShell localmente y, en su lugar, usa Azure Cloud Shell 'Try it' en el explorador web, omita este primer paso. Se conectará a la cuenta de Azure automáticamente.

```azurepowershell
Connect-AzureRmAccount
```

Si tiene más de una suscripción, obtenga una lista de las suscripciones de Azure.

```azurepowershell-interactive
Get-AzureRmSubscription
```

Especifique la suscripción que desea usar.

```azurepowershell-interactive
Select-AzureRmSubscription -SubscriptionName "Name of subscription"
```