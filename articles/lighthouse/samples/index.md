---
title: Ejemplos y plantillas de Azure Lighthouse
description: En estos ejemplos y plantillas de Azure Resource Manager se muestra cómo incorporar clientes para la administración de recursos delegados de Azure y cómo dar soporte a escenarios de Azure Lighthouse.
author: JnHs
manager: carmonm
ms.service: lighthouse
ms.topic: sample
ms.date: 07/11/2019
ms.author: jenhayes
ms.openlocfilehash: d888fb0d50bf0336f8fe830c567586e66065871f
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286139"
---
# <a name="azure-lighthouse-samples"></a>Ejemplos de Azure Lighthouse

En la tabla siguiente se incluyen vínculos a plantillas de Azure Resource Manager para Azure Lighthouse. Estos archivos y otros más se pueden encontrar también en el [repositorio de ejemplos de Azure Lighthouse](https://github.com/Azure/Azure-Lighthouse-samples/).

| **Plantilla** | **Descripción** |
|---------|---------|
| [create-multiple-rgs](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/create-multiple-rgs) | Crea varios grupos de recursos mediante una única plantilla de Azure Resource Manager. |
| [cross-rg-deployment](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/cross-rg-deployment) | Implementa cuentas de almacenamiento en dos grupos de recursos diferentes. |
| [cross-subscription-deployment](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/cross-subscription-deployment) | Implementa plantillas de Azure Resource Manager en varias suscripciones. |
| [delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/delegated-resource-management) | Incorpora una suscripción de cliente a la administración de recursos delegados de Azure. |
| [deploy-azure-mgmt-services](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/deploy-azure-mgmt-services) | Crea los servicios de administración de Azure, los vincula entre sí e implementa soluciones adicionales. |
| [deploy-azure-security-center](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/deploy-azure-security-center) | Habilita y configura Azure Security Center dentro de la suscripción de Azure de destino. |
| [marketplace-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/marketplace-delegated-resource-management) | Incorpora una suscripción de cliente a la administración de recursos delegados de Azure basándose en una oferta de servicios administrados que se publica en Marketplace. |
| [rg-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management) | Incorpora uno o varios de los grupos de recursos de un cliente a la administración de recursos delegados de Azure. |

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [administración de recursos delegados de Azure](../concepts/azure-delegated-resource-management.md).
- Aprenda a usar las plantillas de Azure Resource Manager para [incorporar clientes a la administración de recursos delegados de Azure](../how-to/onboard-customer.md).
