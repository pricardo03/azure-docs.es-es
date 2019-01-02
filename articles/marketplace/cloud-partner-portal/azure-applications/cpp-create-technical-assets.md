---
title: Creación de recursos técnicos de una aplicación de Azure | Microsoft Docs
description: Creación de recursos técnicos para una oferta de aplicación de Azure.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 12/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 6050ad98c87dbe38516a6ee3c4862495ad868031
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53414355"
---
# <a name="prepare-your-azure-application-technical-assets"></a>Preparación de los recursos técnicos de aplicaciones de Azure

En este artículo se describen los recursos para preparar los recursos técnicos para su oferta de aplicaciones de Azure.

## <a name="before-you-begin"></a>Antes de empezar

Revise la documentación de la aplicación de Azure siguiente, que proporciona artículos de inicio rápido, tutoriales y ejemplos.

- [Nociones sobre las plantillas de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)
- Guías de inicio rápido:

  - [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/documentation/templates/)
  - [Plantillas de inicio rápido de Azure en GitHub](https://github.com/azure/azure-quickstart-templates)
  - [Publicación de definición de aplicación](https://docs.microsoft.com/azure/managed-applications/publish-managed-app-definition-quickstart)
  - [Implementación de la aplicación de catálogo de servicios](https://docs.microsoft.com/azure/managed-applications/deploy-service-catalog-quickstart)

  
- Tutoriales:

  - [Creación de archivos de definición](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
  - [Publicación de la aplicación Marketplace](https://docs.microsoft.com/azure/managed-applications/publish-marketplace-app)

 - Ejemplos:

   - [CLI de Azure](https://docs.microsoft.com/azure/managed-applications/cli-samples)
   - [Azure PowerShell](https://docs.microsoft.com/azure/managed-applications/powershell-samples)
   - [Soluciones para aplicaciones administradas](https://docs.microsoft.com/azure/managed-applications/sample-projects)

## <a name="fundamental-technical-knowledge"></a>Conocimientos técnicos básicos

El diseño, la compilación y las pruebas de estos recursos lleva tiempo y requiere conocimientos técnicos de la plataforma de Azure y las tecnologías que se usan para crear la oferta.

El equipo de ingeniería debe tener conocimientos sobre las tecnologías de Microsoft siguientes:

- Conocimientos básicos de los [Servicios de Azure](https://azure.microsoft.com/services/).
- Cómo [diseñar y estructurar las aplicaciones de Azure](https://azure.microsoft.com/solutions/architecture/).
- Conocimientos prácticos de [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage) y [redes de Azure](https://azure.microsoft.com/services/?filter=networking).
- Conocimientos prácticos de [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/).
- Conocimientos prácticos de [JSON](https://www.json.org/)

## <a name="suggested-tools"></a>Herramientas sugeridas

Elija uno o ambos de los entornos de scripting siguientes para ayudar a administrar la aplicación de Azure:

- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
- [CLI de Azure](https://docs.microsoft.com/cli/azure)

Se recomienda agregar las herramientas siguientes al entorno de desarrollo:

- [Explorador de Azure Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
- [Visual Studio Code](https://code.visualstudio.com/) con las siguientes extensiones:

  - Extensión: [Herramientas de Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  - Extensión: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  - Extensión: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

También se recomienda revisar las herramientas disponibles en la página [Herramientas de desarrollo de Azure](https://azure.microsoft.com/tools/) y, si usa Visual Studio, en [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

## <a name="next-steps"></a>Pasos siguientes

[Creación de una oferta de aplicación de Azure](./cpp-create-offer.md)

