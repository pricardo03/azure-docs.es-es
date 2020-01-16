---
title: Procedimiento para actualizar las herramientas de Azure Dev Spaces
services: azure-dev-spaces
ms.date: 07/03/2018
ms.topic: conceptual
description: Obtenga información sobre cómo actualizar las herramientas de línea de comandos de Azure Dev Spaces, y la extensión de Visual Studio.
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, contenedores
ms.openlocfilehash: 07d55689ac94a865527f4b595765d67b28ddb97a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75438415"
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