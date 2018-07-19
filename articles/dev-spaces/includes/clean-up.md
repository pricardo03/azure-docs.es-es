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
ms.openlocfilehash: 8dbc90c3888a9c53db7d2ebeea2dd5f3ee5746a0
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38943132"
---
## <a name="clean-up"></a>Limpieza
Para eliminar completamente una instancia de Azure Dev Spaces en un clúster, incluidos todos los espacios de desarrollo y los servicios en ejecución dentro de él, utilice el comando `az aks remove-dev-spaces`. Tenga en cuenta que esta acción es irreversible. Puede agregar compatibilidad para Azure Dev Spaces de nuevo en el clúster, pero será como si comienza de nuevo. No se restaurarán los servicios y espacios anteriores.

En el ejemplo siguiente se enumeran los controladores de Azure Dev Spaces en la suscripción activa y, a continuación, se elimina el controlador de Azure Dev Spaces que está asociado con el clúster de AKS "myaks" en el grupo de recursos "myaks-rg".

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```

