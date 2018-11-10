---
title: Procedimiento para actualizar las herramientas de Azure Dev Spaces | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: iainfoulds
ms.author: iainfou
ms.date: 07/03/2018
ms.topic: article
ms.technology: azds-kubernetes
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, contenedores
ms.openlocfilehash: 61e59abde2c3a06b8cf650306b67ac5142729a8c
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2018
ms.locfileid: "50976943"
---
# <a name="how-to-upgrade-azure-dev-spaces-tools"></a>Procedimiento para actualizar las herramientas de Azure Dev Spaces

Si hay una nueva versión y ya está usando Azure Dev Spaces, es posible que deba actualizar sus herramientas de cliente.

## <a name="update-the-azure-cli"></a>Actualización de la CLI de Azure

Al actualizar a la CLI de Azure más reciente, también obtendrá la versión más reciente de la extensión de la CLI de Dev Spaces.

No es necesario desinstalar la versión anterior, basta con buscar la descarga correspondiente en la [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest).


## <a name="update-the-dev-spaces-cli-extension-and-command-line-tools"></a>Actualización de la extensión de la CLI de Dev Spaces y las herramientas de línea de comandos

Ejecute el siguiente comando:

```cmd
az aks use-dev-spaces -n <your-aks-cluster> -g <your-aks-cluster-resource-group> --update
```

## <a name="update-the-vs-code-extension"></a>Actualización de la extensión de VS Code

Una vez instalada, la extensión se actualiza automáticamente. Es posible que deba volver a cargar la extensión para usar las nuevas características. En Visual Studio Code, abra el panel **Extensiones**, elija las extensiones de **Azure Dev Spaces** y elija **Volver a cargar**.

## <a name="update-the-visual-studio-extension"></a>Actualización de la extensión de Visual Studio

Al igual que con otras extensiones y actualizaciones, Visual Studio le notificará cuando haya una actualización disponible para Visual Studio Tools para Kubernetes, que incluye Azure Dev Spaces. Busque un icono de marca en la parte superior derecha de la pantalla.

Para actualizar las herramientas de Visual Studio, elija el elemento de menú **Herramientas > Extensiones y actualizaciones** y, en el lado izquierdo, elija **Actualizaciones**. Busque **Visual Studio Tools para Kubernetes** y elija el botón **Actualizar**.

## <a name="next-steps"></a>Pasos siguientes

Pruebe las nuevas herramientas creando un clúster. Pruebe las guías de inicio rápido y los tutoriales en [Azure Dev Spaces](/azure/dev-spaces).

> [!WARNING]
> La revisión de Azure Dev Spaces en los clústeres existentes no se aplicará inmediatamente, por lo que, para asegurarse de que usa la versión más reciente en todas las implementaciones de Azure, cree un clúster nuevo después de actualizar las herramientas.
