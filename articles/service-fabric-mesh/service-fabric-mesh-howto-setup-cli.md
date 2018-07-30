---
title: Configuración de la CLI de Azure Service Fabric Mesh | Microsoft Docs
description: Aprenda a configurar la CLI de Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/11/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: cf899e236b7fa21a90a0784e1e2b4fce0feae055
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213442"
---
# <a name="set-up-the-service-fabric-mesh-cli"></a>Configuración de la CLI de Service Fabric Mesh
La CLI de Service Fabric Mesh es necesaria para implementar y administrar recursos de Service Fabric Mesh. 

En la versión preliminar, la CLI de Azure Service Fabric Mesh está escrita como una extensión de la CLI de Azure. Puede instalarla en Azure Cloud Shell o como una instalación local de la CLI de Azure. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Si decide instalar y usar la CLI localmente, debe instalar la versión 2.0.35 o superior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Para instalar o actualizar a la última versión de la CLI, consulte [Instalación de la CLI de Azure 2.0][azure-cli-install].

Quite cualquier instalación anterior del módulo de la CLI de Azure Service Fabric Mesh.

```azurecli-interactive
az extension remove --name mesh
```

Instale el módulo de extensión de la CLI de Azure Service Fabric Mesh mediante el siguiente comando. 

```azurecli-interactive
az extension add --source https://sfmeshcli.blob.core.windows.net/cli/mesh-0.8.1-py2.py3-none-any.whl
```

También puede configurar su [entorno de desarrollo de Windows](service-fabric-mesh-howto-setup-developer-environment-sdk.md).

[azure-cli-install]: /cli/azure/install-azure-cli