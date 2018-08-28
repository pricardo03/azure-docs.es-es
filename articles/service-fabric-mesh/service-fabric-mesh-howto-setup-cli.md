---
title: Configuración de la CLI de Azure Service Fabric Mesh | Microsoft Docs
description: Aprenda a configurar la CLI de Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/26/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: c0e2aefe1222263b169e21490da079b165a57321
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "42108485"
---
# <a name="set-up-the-service-fabric-mesh-cli"></a>Configuración de la CLI de Service Fabric Mesh
La CLI de Service Fabric Mesh es necesaria para implementar y administrar recursos de Service Fabric Mesh. 

En la versión preliminar, la CLI de Azure Service Fabric Mesh está escrita como una extensión de la CLI de Azure. Puede instalarla en Azure Cloud Shell o como una instalación local de la CLI de Azure. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Si decide instalar y usar la CLI localmente, debe instalar la versión 2.0.43 o superior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Para instalar o actualizar a la última versión de la CLI, consulte [Instalación de la CLI de Azure 2.0][azure-cli-install].

Instale el módulo de extensión de la CLI de Azure Service Fabric Mesh mediante el siguiente comando. 

```azurecli-interactive
az extension add --name mesh
```

Para actualizar un módulo de la CLI de Azure Service Fabric Mesh, ejecute el comando siguiente.

```azurecli-interactive
az extension update --name mesh
```

También puede configurar su [entorno de desarrollo de Windows](service-fabric-mesh-howto-setup-developer-environment-sdk.md).

[azure-cli-install]: /cli/azure/install-azure-cli