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
ms.openlocfilehash: 82de8eab089e5f666e1a2ce4eab09bfd2895185b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47020088"
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
