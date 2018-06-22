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
ms.openlocfilehash: 44ce986fcfdf079d3077c007a378f3467073d00d
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823187"
---
### <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>Configuración del clúster de AKS para usar Azure Dev Spaces

Abra una ventana de comandos y escriba los siguientes comandos de la CLI de Azure, mediante el grupo de recursos que contiene el clúster de AKS, y su nombre del clúster de AKS. El comando configura el clúster para la compatibilidad con Azure Dev Spaces.

   ```cmd
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```

