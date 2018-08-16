---
title: archivo de inclusión
description: archivo de inclusión
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 875dd9d768b5f40f2d4ed3fad5b14a6cd6f6f6ba
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2018
ms.locfileid: "40129369"
---
### <a name="sign-in-to-azure-cli"></a>Inicio de sesión en la CLI de Azure
Inicie sesión en Azure. Escriba el siguiente comando en una ventana de terminal:

```cmd
az login
```

> [!Note]
> Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita](https://azure.microsoft.com/free).

#### <a name="if-you-have-multiple-azure-subscriptions"></a>Si tiene varias suscripciones a Azure...
Para ver sus suscripciones, ejecute: 

```cmd
az account list
```
Busque la suscripción que tiene `isDefault: true` en la salida de JSON.
Si esta no es la suscripción que desea usar, puede cambiar la suscripción predeterminada:

```cmd
az account set --subscription <subscription ID>
```
