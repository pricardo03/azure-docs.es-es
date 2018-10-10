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
ms.openlocfilehash: e3961878663a26d721478effc01d968e21e34c18
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47020802"
---
Inicie sesión en la suscripción de Azure con el comando [az login](/cli/azure/#login) y siga las instrucciones que aparecen en pantalla. Para más información sobre el inicio de sesión, consulte [Introducción a la CLI de Azure](/cli/azure/get-started-with-azure-cli).

```azurecli
az login
```

Si tiene más de una suscripción de Azure, enumere las suscripciones de la cuenta.

```azurecli
az account list --all
```

Especifique la suscripción que desea usar.

```azurecli
az account set --subscription <replace_with_your_subscription_id>
```
