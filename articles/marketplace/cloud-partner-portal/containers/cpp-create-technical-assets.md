---
title: Crear imagen de contenedores de Azure activos técnicos | Azure Marketplace
description: Cree los recursos técnicos para un contenedor de Azure.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: pabutler
ms.openlocfilehash: 32002f8b88a2fbeba29f15b3b9289ecf3d7560d0
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64942731"
---
# <a name="prepare-your-container-technical-assets"></a>Preparación de los recursos técnicos de contenedor

En este artículo se describen los pasos y los requisitos para configurar una oferta de contenedor de Azure Marketplace.

## <a name="before-you-begin"></a>Antes de empezar

Revise la documentación de [Azure Container Instances](https://docs.microsoft.com/azure/container-instances), que proporciona guías de inicio rápido, tutoriales y ejemplos.

## <a name="fundamental-technical-knowledge"></a>Conocimientos técnicos básicos

El diseño, la compilación y las pruebas de estos recursos lleva tiempo y requiere conocimientos técnicos de la plataforma de Azure y las tecnologías que se usan para crear la oferta.
 
Además del dominio de la solución, el equipo de ingeniería debe tener conocimientos sobre las tecnologías de Microsoft siguientes:

-   Conocimientos básicos de los [Servicios de Azure](https://azure.microsoft.com/services/). 
-   Cómo [diseñar y estructurar las aplicaciones de Azure](https://azure.microsoft.com/solutions/architecture/).
-   Conocimientos prácticos de [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage) y [redes de Azure](https://azure.microsoft.com/services/?filter=networking).
-   Conocimientos prácticos de [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/).
-   Conocimientos prácticos de [JSON](https://www.json.org/).

## <a name="suggested-tools"></a>Herramientas sugeridas

Elija uno o ambos de los entornos de scripting siguientes para ayudar a administrar la imagen de contenedor:

-   [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
-   [CLI de Azure](https://docs.microsoft.com/cli/azure)

Además, se recomienda agregar las herramientas siguientes al entorno de desarrollo:

-   [Explorador de Azure Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
-   [Visual Studio Code](https://code.visualstudio.com/)
    *   Extensión: [Herramientas de Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
    *   Extensión: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
    *   Extensión: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

También se recomienda revisar las herramientas disponibles en la página [Herramientas de desarrollo de Azure](https://azure.microsoft.com/tools/) y, si usa Visual Studio, en [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

## <a name="create-the-container-image"></a>Creación de la imagen de contenedor

- Cree y configure el disco duro virtual (VHD) para la máquina virtual (VM) de contenedor. Este disco duro virtual contiene el sistema operativo (Windows, Ubuntu o Linux) del contenedor. Puede que sean necesarios discos de datos adicionales.
- Configure el sistema operativo y el tamaño de la máquina virtual, los puertos que se abrirán y los discos de datos asociados.
- Instale la aplicación y otro software necesario para la oferta. Por ejemplo, software de base de datos, software de terceros o una aplicación personalizada.

## <a name="next-steps"></a>Pasos siguientes

[Creación de la oferta de contenedor](./cpp-create-offer.md)
