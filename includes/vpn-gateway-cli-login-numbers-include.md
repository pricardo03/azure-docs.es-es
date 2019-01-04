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
ms.openlocfilehash: cf3bbb6c45061d6f4885839fbfb7f069264fe986
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444359"
---
1. Inicie sesión en la suscripción de Azure con el comando [az login](/cli/azure/#login) y siga las instrucciones que aparecen en pantalla. Para más información sobre el inicio de sesión, consulte [Introducción a la CLI de Azure](/cli/azure/get-started-with-azure-cli).

   ```azurecli
   az login
   ```
2. Si tiene más de una suscripción de Azure, enumere las suscripciones de la cuenta.

   ```azurecli
   az account list --all
   ```
3. Especifique la suscripción que desea usar.

   ```azurecli
   az account set --subscription <replace_with_your_subscription_id>
   ```
