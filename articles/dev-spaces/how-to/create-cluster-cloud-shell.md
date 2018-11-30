---
title: Creación de un clúster de Kubernetes habilitado para Azure Dev Spaces con Azure Cloud Shell | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 10/04/2018
ms.topic: article
description: Aprenda a crear rápidamente un clúster de Kubernetes habilitado para Azure Dev Spaces directamente desde el explorador sin instalar nada.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
ms.openlocfilehash: 61a466efdacb56889a7e437d84d3c9de43927e65
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705759"
---
# <a name="create-a-kubernetes-cluster-using-azure-cloud-shell"></a>Creación de un clúster de Kubernetes mediante Azure Cloud Shell

Puede usar [Azure Cloud Shell](/azure/cloud-shell) para crear un clúster de Azure Dev Spaces mediante el botón **Pruébelo** desde esta página. Si no ha iniciado sesión, siga los avisos para iniciar sesión con una cuenta de Azure y, luego, escriba los comandos en el símbolo del sistema de Azure Cloud Shell cuando aparezca.

## <a name="create-the-cluster"></a>Creación de clústeres

En primer lugar, cree el grupo de recursos. Use alguna de las regiones que se admiten actualmente (Este de EE. UU., Este de EE. UU. 2, Centro de EE. UU., Oeste de EE. UU. 2, Europa Occidental, Sudeste Asiático, Centro de Canadá o Este de Canadá).

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Crear un clúster de Kubernetes con el siguiente comando:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --kubernetes-version 1.11.3 --enable-addons http_application_routing
```

La operación de creación del clúster tarda unos minutos.  Cuando haya finalizado, la salida se muestra en el formato JSON. Busque `provisioningState` y compruebe que su valor es `Succeeded`.

## <a name="next-steps"></a>Pasos siguientes

Consulte [Azure Dev Spaces](/azure/dev-spaces/) para obtener vínculos a tutoriales completos.
