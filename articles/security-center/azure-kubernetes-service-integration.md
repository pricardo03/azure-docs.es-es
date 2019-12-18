---
title: Azure Security Center y Azure Kubernetes Service | Microsoft Docs
description: Obtenga más información sobre la integración de Azure Security Center con Azure Kubernetes Services
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 2a5d01978d8a4883d760e7ecf84afa381dfd563d
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74868565"
---
# <a name="azure-kubernetes-services-integration-with-security-center-preview"></a>Integración de Azure Kubernetes Services con Security Center (versión preliminar)
Azure Kubernetes Service (AKS) es un servicio que administra Microsoft y que le permite desarrollar, implementar y administrar las aplicaciones en contenedores. 

Use AKS junto con el nivel estándar de Azure Security Center (consulte los [precios](security-center-pricing.md)) para obtener una mayor visibilidad de los nodos de AKS, el tráfico en la nube y los controles de seguridad.

Security Center aporta ventajas de seguridad a los clústeres de AKS gracias a los datos que recopiló el nodo principal de AKS. 

![Información general de alto nivel de Azure Security Center y Azure Kubernetes Service (AKS)](./media/azure-kubernetes-service-integration/aks-asc-integration-overview.png)

Si trabajan juntas, estas dos herramientas proporcionan la mejor oferta de seguridad Kubernetes nativa en la nube. 

## <a name="benefits-of-integration"></a>Ventajas de la integración

Si usa de forma conjunta estos dos servicios, obtendrá lo siguiente:

* **Recomendaciones de seguridad**: Security Center identifica los recursos de AKS y los categoriza; desde clústeres hasta máquinas virtuales individuales. Puede ver las recomendaciones de seguridad de cada recurso. Para obtener más información, consulte [Cómo implementar las recomendaciones de seguridad](security-center-recommendations.md). 

    > [!NOTE]
    > Si el nombre de una recomendación de Security Center finaliza con una etiqueta "(Versión preliminar)", hace referencia a que la recomendación, y no la característica, es una versión preliminar.

* **Sistema de protección del entorno**: Security Center supervisa constantemente la configuración de los clústeres de Kubernetes y genera recomendaciones de seguridad que reflejan los estándares del sector.

* **Protección en tiempo de ejecución**: gracias al análisis continuo de los siguientes orígenes de AKS, Security Center le alerta de las amenazas y actividades malintencionadas que se detectan en el host *y* el nivel de clúster de AKS (para obtener más información, consulte [Detección de amenazas para contenedores de Azure](https://docs.microsoft.com/azure/security-center/security-center-alerts-compute#azure-containers-)):
    * Eventos de seguridad sin procesar, como datos de red y creación de procesos
    * Registro de auditoría de Kubernetes

![Información detallada de Azure Security Center y Azure Kubernetes Service (AKS)](./media/azure-kubernetes-service-integration/aks-asc-integration-detailed.png)

> [!NOTE]
> Algunos de los datos del entorno de Kubernetes que examina Azure Security Center pueden contener información confidencial.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las características de seguridad de los contenedores de Security Center, consulte:

* [Seguridad de los contenedores y Azure Security Center](container-security.md)

* [Integración con Azure Container Registry](azure-container-registry-integration.md)

* [Protección de máquinas virtuales](security-center-virtual-machine-protection.md): describe las recomendaciones de Security Center

* [Administración de datos en Microsoft](https://www.microsoft.com/trust-center/privacy/data-management): aquí se describen las directivas de datos de los servicios de Microsoft (incluidos Azure, Intune y Office 365), los detalles de la administración de datos de Microsoft y las directivas de retención que afectan a los datos.