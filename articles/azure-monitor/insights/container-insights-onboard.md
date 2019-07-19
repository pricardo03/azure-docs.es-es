---
title: Cómo habilitar Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe cómo habilitar y configurar Azure Monitor para contenedores, de forma que pueda conocer el rendimiento de su contenedor y qué problemas relacionados con su rendimiento se han identificado.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/02/2019
ms.author: magoedte
ms.openlocfilehash: aff2dcebdab1ad93b8b1958164764b66eb755d1c
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514493"
---
# <a name="how-to-enable-azure-monitor-for-containers"></a>Cómo habilitar Azure Monitor para contenedores

En este artículo se proporciona información general acerca de las opciones disponibles para configurar la solución para contenedores de Azure Monitor para supervisar el rendimiento de las cargas de trabajo que se implementan en entornos de Kubernetes y se hospedan en [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/).

Azure Monitor para contenedores puede habilitarse para implementaciones de AKS nuevas, o en una o más existentes, mediante los siguientes métodos compatibles:

* Desde Azure Portal, Azure PowerShell o con la CLI de Azure
* Usando [Terraform y AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos
Antes de empezar, asegúrese de que dispone de lo siguiente:

* **Un área de trabajo de Log Analytics.**

    Azure Monitor para contenedores admite un área de trabajo de Log Analytics en las regiones enumeradas en los [productos por región](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor) de Azure, excepto la región **US Gov Virginia**.

    Puede crear un área de trabajo al habilitar la supervisión de su nuevo clúster de AKS o dejar que la experiencia de incorporación cree un área de trabajo predeterminada en el grupo de recursos predeterminado de la suscripción del clúster de AKS. Si opta por crear el área de trabajo usted mismo, puede usar [Azure Resource Manager](../platform/template-workspace-configuration.md), mediante [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json) o [Azure Portal](../learn/quick-create-workspace.md). Para obtener una lista de los pares de asignación compatibles utilizados para el área de trabajo predeterminado, consulte [Region mapping for Azure Monitor for containers](container-insights-region-mapping.md) (Asignación de regiones para Azure Monitor para contenedores).

* Es miembro del **rol de colaborador de Log Analytics** para habilitar la supervisión de contenedores. Para más información acerca de cómo controlar el acceso a un área de trabajo de Log Analytics, consulte [Administración de áreas de trabajo](../platform/manage-access.md).

* Es miembro del rol de **[propietario](../../role-based-access-control/built-in-roles.md#owner)** en el recurso de clúster de AKS.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

## <a name="components"></a>Componentes

La capacidad de supervisar el rendimiento se basa en un agente de Log Analytics en contenedores para Linux, desarrollado específicamente para Azure Monitor para contenedores. Este agente especializado recopila datos de rendimiento y de eventos de todos los nodos del clúster, y el agente se implementa y registra automáticamente en el área de trabajo de Log Analytics especificada durante la implementación. La versión del agente es microsoft/oms:ciprod04202018 o posterior y se representa mediante una fecha en el siguiente formato: *mmddaaaa*.

>[!NOTE]
>Con la versión preliminar de Windows Server para AKS, un clúster de AKS con nodos de Windows Server no tiene instalado un agente para recopilar datos y reenviarlos a Azure Monitor. En su lugar, un nodo de Linux implementado automáticamente en el clúster como parte de la implementación estándar recopila y reenvía los datos a Azure Monitor en nombre de todos los nodos de Windows del clúster.  
>

Cuando se publica una nueva versión del agente, se actualiza automáticamente en los clústeres de Kubernetes administrado que se hospedan en Azure Kubernetes Service (AKS). Para seguir las versiones publicadas, consulte los [anuncios de versiones del agente](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

>[!NOTE]
>Si ya ha implementado un clúster de AKS, puede habilitar la supervisión mediante la CLI de Azure o una plantilla de Azure Resource Manager que se proporciona, como se muestra más adelante en este artículo. No puede usar `kubectl` para actualizar, eliminar, volver a implementar o implementar el agente.
>La plantilla debe implementarse en el mismo grupo de recursos que el clúster.

Para habilitar Azure Monitor para contenedores, puede usar uno de los métodos descritos en la tabla siguiente.

| Estado de la implementación | Método | DESCRIPCIÓN |
|------------------|--------|-------------|
| Nuevo clúster de AKS | [Creación de un clúster con la CLI de Azure](../../aks/kubernetes-walkthrough.md#create-aks-cluster)| Puede habilitar la supervisión de un nuevo clúster de AKS que cree con la CLI de Azure. |
| | [Creación de un clúster con Terraform](container-insights-enable-new-cluster.md#enable-using-terraform)| Puede habilitar la supervisión de un nuevo clúster de AKS que cree mediante la herramienta de código abierto Terraform. |
| Clúster de AKS existente | [Habilitación con la CLI de Azure](container-insights-enable-existing-clusters.md#enable-using-azure-cli) | Puede habilitar la supervisión de un clúster de AKS ya implementado mediante la CLI de Azure. |
| |[Habilitación con Terraform](container-insights-enable-existing-clusters.md#enable-using-terraform) | Puede habilitar la supervisión de un clúster de AKS ya implementado mediante la herramienta de código abierto Terraform. |
| | [Habilitación desde Azure Monitor](container-insights-enable-existing-clusters.md#enable-from-azure-monitor-in-the-portal)| Puede habilitar la supervisión de uno o varios clústeres de AKS ya implementados desde la página de varios clústeres de AKS en Azure Monitor. |
| | [Habilitación desde el clúster de AKS](container-insights-enable-existing-clusters.md#enable-directly-from-aks-cluster-in-the-portal)| Puede habilitar la supervisión directamente desde un clúster de AKS en Azure Portal. |
| | [Habilitación mediante una plantilla de Azure Resource Manager](container-insights-enable-existing-clusters.md#enable-using-an-azure-resource-manager-template)| Puede habilitar la supervisión de un clúster de AKS con una plantilla preconfigurada de Azure Resource Manager. |

## <a name="next-steps"></a>Pasos siguientes

* Con la supervisión habilitada para capturar métricas de estado para los pods y nodos del clúster de AKS, estas métricas de estado están disponibles en Azure Portal. Para obtener información sobre cómo usar Azure Monitor para contenedores, consulte el artículo sobre cómo [ver el estado de Azure Kubernetes Service](container-insights-analyze.md).
