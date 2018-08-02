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
ms.openlocfilehash: 567f2afdea44f439779212c61fb3a129f4f979be
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281579"
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
az extension add --source https://meshcli.blob.core.windows.net/cli/mesh-0.9.1-py2.py3-none-any.whl
```

También puede configurar su [entorno de desarrollo de Windows](service-fabric-mesh-howto-setup-developer-environment-sdk.md).

[azure-cli-install]: /cli/azure/install-azure-cli