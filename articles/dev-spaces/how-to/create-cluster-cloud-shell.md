---
title: Creación de un clúster de Kubernetes habilitado para Azure Dev Spaces con Azure Cloud Shell | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 10/04/2018
ms.topic: article
description: Aprenda a crear rápidamente un clúster de Kubernetes habilitado para Azure Dev Spaces directamente desde el explorador sin instalar nada.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
manager: douge
ms.openlocfilehash: f10a84a602ce152d5c428525aa50f678b50c8b41
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "48872561"
---
# <a name="create-a-kubernetes-cluster-using-azure-cloud-shell"></a>Creación de un clúster de Kubernetes mediante Azure Cloud Shell

Puede usar [Azure Cloud Shell](/azure/cloud-shell) para crear un clúster de Azure Dev Spaces mediante el botón **Pruébelo** desde esta página. Si no ha iniciado sesión, siga los avisos para iniciar sesión con una cuenta de Azure y, luego, escriba los comandos en el símbolo del sistema de Azure Cloud Shell cuando aparezca.

## <a name="create-the-cluster"></a>Creación de clústeres

En primer lugar, cree el grupo de recursos. Use alguna de las regiones que se admiten actualmente (EastUS, CentralUS, WestUS2, WestEurope, CanadaCentral o CanadaEast).

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Crear un clúster de Kubernetes con el siguiente comando:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --kubernetes-version 1.11.2 --enable-addons http_application_routing
```

La operación de creación del clúster tarda unos minutos.  Cuando haya finalizado, la salida se muestra en el formato JSON. Busque `provisioningState` y compruebe que su valor es `Succeeded`.

## <a name="next-steps"></a>Pasos siguientes

Consulte [Azure Dev Spaces](/azure/dev-spaces/) para obtener vínculos a tutoriales completos.