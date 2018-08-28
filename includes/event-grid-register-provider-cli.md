---
title: archivo de inclusión
description: archivo de inclusión
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 08/17/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 74d96952d6cafb6bd7bb16ccf2f4d69b4fbd6de3
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "40233972"
---
## <a name="enable-event-grid-resource-provider"></a>Habilitación del proveedor de recursos de Event Grid

Si aún no ha usado anteriormente Event Grid en su suscripción de Azure, puede que tenga que registrar el proveedor de recursos de Event Grid. Ejecute el siguiente comando para registrar el proveedor:

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

Puede tardar unos instantes en finalizarse el registro. Para comprobar el estado, ejecute:

```azurecli-interactive
az provider show --namespace Microsoft.EventGrid --query "registrationState"
```

Cuando `registrationState` sea `Registered`, estará preparado para continuar.