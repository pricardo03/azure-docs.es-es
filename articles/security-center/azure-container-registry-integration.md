---
title: Azure Security Center y Azure Container Registry
description: Obtenga más información sobre la integración de Azure Security Center con Azure Container Registry
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/19/2019
ms.author: memildin
ms.openlocfilehash: 4cc88e7c04d10907a9a6386b1266eb8031d60926
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552685"
---
# <a name="azure-container-registry-integration-with-security-center-preview"></a>Integración de Azure Container Registry con Security Center (versión preliminar)

Azure Container Registry (ACR) es un servicio de registro de Docker privado y administrado que almacena y administra las imágenes de contenedor de las implementaciones de Azure en un registro central. Se basa en el registro 2.0 de Docker de código abierto.

Para obtener una mayor visibilidad sobre el registro y las vulnerabilidades de las imágenes, los usuarios del nivel estándar de Azure Security Center pueden habilitar la agrupación opcional de registros de contenedor. El costo de usar esta característica se cobra por imagen, no por examen. Para más información, consulte los [precios](security-center-pricing.md). Con la agrupación habilitada, Security Center examina automáticamente las imágenes del registro cada vez que se inserta en él una imagen.

> [!NOTE]
> El primer análisis de Security Center de un registro solo se producirá una vez que se habilite la agrupación de registros de contenedor y se inserte una imagen en el registro.

Cuando finaliza el examen (al cabo de 10 minutos aproximadamente), los resultados están disponibles en recomendaciones de Security Center como esta:

[![Ejemplo de recomendación Azure Security Center sobre puntos vulnerables detectados en una imagen hospedada de Azure Container Registry (ACR)](media/azure-container-registry-integration/container-security-acr-page.png)](media/azure-container-registry-integration/container-security-acr-page.png#lightbox)

## <a name="benefits-of-integration"></a>Ventajas de la integración

Security Center identifica los registros de ACR en su suscripción y proporciona de forma fácil lo siguiente:

* **Detección de vulnerabilidades nativas de Azure** para todas las imágenes de Linux insertadas. Security Center examina la imagen con un escáner del proveedor de detección de vulnerabilidades líder del sector, Qualys. Esta solución nativa se integra sin problemas de forma predeterminada.

* **Recomendaciones de seguridad** para las imágenes de Linux con vulnerabilidades conocidas. Security Center proporciona detalles de cada vulnerabilidad detectada y una clasificación de gravedad. Asimismo, ofrece instrucciones sobre cómo corregir las vulnerabilidades específicas encontradas en cada imagen insertada en el registro.

![Introducción de alto nivel de Azure Security Center y Azure Container Registry (ACR)](./media/azure-container-registry-integration/aks-acr-integration-detailed.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las características de seguridad de los contenedores de Security Center, consulte:

* [Seguridad de los contenedores y Azure Security Center](container-security.md)

* [Integración con Azure Kubernetes Service](azure-kubernetes-service-integration.md)

* [Protección de máquinas virtuales](security-center-virtual-machine-protection.md): describe las recomendaciones de Security Center
