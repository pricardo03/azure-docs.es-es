---
title: Ejemplos y plantillas de Azure Lighthouse
description: En estos ejemplos y plantillas de Azure Resource Manager se muestra cómo incorporar clientes para la administración de recursos delegados de Azure y cómo dar soporte a escenarios de Azure Lighthouse.
author: JnHs
manager: carmonm
ms.service: lighthouse
ms.topic: sample
ms.date: 10/17/2019
ms.author: jenhayes
ms.openlocfilehash: 6d47534026b6fe815f9756a74ba3438dc67a8e02
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72553686"
---
# <a name="azure-lighthouse-samples"></a>Ejemplos de Azure Lighthouse

En la tabla siguiente se incluyen vínculos a plantillas de Azure Resource Manager para Azure Lighthouse. Estos archivos y otros más se pueden encontrar también en el [repositorio de ejemplos de Azure Lighthouse](https://github.com/Azure/Azure-Lighthouse-samples/).

## <a name="onboarding-customers-to-azure-delegated-resource-management"></a>Incorporación de clientes a la administración de recursos delegados de Azure

Proporcionamos diferentes plantillas para abordar escenarios de incorporación específicos. Elija la opción que mejor funcione y asegúrese de modificar el archivo de parámetros para que refleje su entorno. Para más información sobre cómo usar estos archivos en su implementación, consulte [Incorporación de un cliente a la administración de recursos delegados de Azure](../how-to/onboard-customer.md).

| **Plantilla** | **Descripción** |
|---------|---------|
| [delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/delegated-resource-management) | Incorpora una suscripción de cliente a la administración de recursos delegados de Azure. Se debe realizar una implementación independiente para cada suscripción. |
| [rg-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management) | Incorpora uno o varios de los grupos de recursos de un cliente a la administración de recursos delegados de Azure. Use **rgDelegatedResourceManagement** para un solo grupo de recursos o **multipleRgDelegatedResourceManagement** para incorporar varios grupos de recursos en la misma suscripción. |
| [marketplace-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/marketplace-delegated-resource-management) | Si ha [publicado una oferta de servicios administrados en Azure Marketplace](../how-to/publish-managed-services-offers.md) también puede usar esta plantilla para incorporar recursos para los clientes que han aceptado la oferta. Los valores de Marketplace en el archivo de parámetros deben coincidir con los valores que usó al publicar la oferta. |

Normalmente, se requiere una implementación independiente para cada suscripción que se incorpora, pero también puede implementar plantillas en varias suscripciones.

| **Plantilla** | **Descripción** |
|---------|---------|
| [cross-subscription-deployment](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/cross-subscription-deployment) | Implementa plantillas de Azure Resource Manager en varias suscripciones. |

## <a name="azure-policy"></a>Azure Policy

En estos ejemplos se muestra cómo usar Azure Policy con las suscripciones que se han incorporado a la administración de recursos delegados de Azure.

| **Plantilla** | **Descripción** |
|---------|---------|
| [policy-add-or-replace-tag](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-add-or-replace-tag) | Asigna una directiva que agrega o elimina una etiqueta (mediante el efecto Modify) en una suscripción delegada. Para más información, consulte [Deploy a policy that can be remediated within a delegated subscription](../how-to/deploy-policy-remediation.md) (Implementación de una directiva que se pueda corregir en una suscripción delegada). |
| [policy-audit-delegation](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-audit-delegation) | Asigna una directiva que auditará las asignaciones de delegaciones. |
| [policy-enforce-keyvault-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-enforce-keyvault-monitoring) | Asigna una directiva que habilita el diagnóstico en los recursos de Azure Key Vault de una suscripción delegada (mediante el efecto deployIfNotExists). Para más información, consulte [Deploy a policy that can be remediated within a delegated subscription](../how-to/deploy-policy-remediation.md) (Implementación de una directiva que se pueda corregir en una suscripción delegada). |
| [policy-enforce-sub-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-enforce-sub-monitoring) | Asigna varias directivas para habilitar los diagnósticos en una suscripción delegada y conecta todas las máquinas virtuales Windows y Linux al área de trabajo de Log Analytics que la directiva crea. Para más información, consulte [Deploy a policy that can be remediated within a delegated subscription](../how-to/deploy-policy-remediation.md) (Implementación de una directiva que se pueda corregir en una suscripción delegada). |
| [policy-initiative](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-initiative) | Aplica una iniciativa (varias definiciones de directivas relacionadas) a una suscripción delegada. |

## <a name="azure-monitor"></a>Azure Monitor

En estos ejemplos se muestra cómo usar Azure Monitor para crear alertas para las suscripciones que se han incorporado a la administración de recursos delegados de Azure.

| **Plantilla** | **Descripción** |
|---------|---------|
| [alert-using-actiongroup](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/alert-using-actiongroup) | Esta plantilla crea una alerta de Azure y se conecta a un grupo de acciones existente.|
| [multiple-loganalytics-alerts](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/multiple-loganalytics-alerts) | Crea varias alertas de registro basadas en consultas de Kusto.|
| [delegation-alert-for-customer](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/delegation-alert-for-customer) | Implementa una alerta en un inquilino cuando un usuario delega una suscripción en un inquilino de administración.|

## <a name="additional-cross-tenant-scenarios"></a>Escenarios adicionales entre inquilinos

En estos ejemplos se muestran varias tareas que se pueden realizar en escenarios de administración entre inquilinos.

| **Plantilla** | **Descripción** |
|---------|---------|
| [cross-rg-deployment](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/cross-rg-deployment) | Implementa cuentas de almacenamiento en dos grupos de recursos diferentes.|
| [deploy-azure-mgmt-services](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/deploy-azure-mgmt-services) | Crea los servicios de administración de Azure, los vincula entre sí e implementa soluciones adicionales. Para una implementación completa, use la plantilla **rgWithAzureMgmt.json**. |
| [deploy-azure-security-center](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/deploy-azure-security-center) | Habilita y configura Azure Security Center dentro de la suscripción de Azure de destino. |
| [deploy-azure-sentinel](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/deploy-azure-sentinel) | Implementa y habilita Azure Sentinel en un área de trabajo de Log Analytics existente de una suscripción delegada. |
| [deploy-log-analytics-vm-extensions](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/deploy-log-analytics-vm-extensions) | Estas plantillas le permiten implementar extensiones de máquinas virtuales de Log Analytics en máquinas virtuales Windows y Linux mediante la conexión al área de trabajo de Log Analytics designada. |

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [administración de recursos delegados de Azure](../concepts/azure-delegated-resource-management.md).
- Explore el [repositorio de ejemplos de Azure Lighthouse](https://github.com/Azure/Azure-Lighthouse-samples/).
