---
title: Seguridad de los contenedores en Azure Security Center | Microsoft Docs
description: Obtenga más información sobre las características de seguridad de los contenedores de Azure Security Center.
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
ms.openlocfilehash: 9886f41b25e7c1b34f9805a2c16b716c642ae90e
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2020
ms.locfileid: "76936289"
---
# <a name="container-security-in-security-center"></a>Seguridad de los contenedores en Security Center

Azure Security Center es la solución nativa de Azure para la seguridad de los contenedores. Asimismo, Security Center también es el único panel óptimo que ofrece una experiencia transparente para garantizar la seguridad de sus cargas de trabajo, VM, servidores y contenedores en la nube.

En este artículo se describe cómo mejorar, supervisar y mantener la seguridad de los contenedores y sus aplicaciones. Aprenderá a usar Security Center para que le ayude con estos aspectos básicos de la seguridad de los contenedores:

* Administración de vulnerabilidades
* Protección del entorno del contenedor
* Protección en tiempo de ejecución

[![Pestaña de seguridad de los contenedores de Azure Security Center](media/container-security/container-security-tab.png)](media/container-security/container-security-tab.png#lightbox)

## <a name="vulnerability-management---scanning-container-images-preview"></a>Administración de vulnerabilidades: análisis de imágenes de contenedor (versión preliminar)
Para supervisar la instancia de Azure Container Registry basada en Azure Resource Manager, asegúrese de que está en el nivel estándar de Security Center (consulte [precios](/azure/security-center/security-center-pricing)). Luego, habilite el conjunto opcional de registros de contenedor. Cuando se inserta una nueva imagen, Security Center examina la imagen con un escáner del proveedor de detección de vulnerabilidades líder del sector, Qualys.

Si Qualys o Security Center encuentran problemas, recibirá una notificación en el panel de Security Center. Por cada vulnerabilidad, Security Center proporciona recomendaciones útiles, junto con una clasificación de gravedad e información sobre cómo corregir el problema. Para más información sobre las recomendaciones de Security Center, consulte la [lista de recomendaciones de referencia](recommendations-reference.md).

## <a name="environment-hardening"></a>Protección del entorno

### <a name="continuous-monitoring-of-your-docker-configuration"></a>Supervisión continua de la configuración de Docker
Azure Security Center identifica contenedores no administrados y que están hospedados en VM de IaaS Linux u otras máquinas de Linux que ejecutan contenedores de Docker. Security Center evalúa continuamente las configuraciones de estos contenedores. A continuación, las compara con el [Banco de prueba de Docker del Centro de seguridad de Internet (CIS)](https://www.cisecurity.org/benchmark/docker/). 

Security Center incluye todo el conjunto de reglas del banco de prueba de Docker de CIS y le avisa si los contenedores no cumplen ninguno de los controles. Cuando encuentra configuraciones inactivas, Security Center genera recomendaciones de seguridad. Use la **página de recomendaciones** para ver las recomendaciones y corregir los problemas. También verá las recomendaciones en la pestaña de **contenedores** que muestra todas las máquinas virtuales implementadas con Docker. Cuando esté revisando los problemas de seguridad de una máquina virtual, Security Center le proporcionará información adicional sobre los contenedores de la máquina. Esta información incluye la versión de Docker y el número de imágenes que se ejecutan en el host. Para obtener más información sobre las recomendaciones, consulte la información que tiene [aquí](https://docs.microsoft.com/azure/security-center/security-center-virtual-machine-protection). 

>[!NOTE]
> Estas comprobaciones del banco de prueba de CIS no se ejecutarán en instancias que administre AKS o en VM que administre Databricks.

### <a name="continuous-monitoring-of-your-kubernetes-clusters-preview"></a>Supervisión continua de los clústeres de Kubernetes (versión preliminar)
Junto con Security Center, Azure Kubernetes Service (AKS) es un servicio de orquestación de contenedores administrado de Microsoft y que le permite desarrollar, implementar y administrar las aplicaciones en contenedores.

AKS proporciona controles de seguridad y visibilidad sobre la postura de seguridad de los clústeres. Security Center utiliza estas características para:
* Supervisar constantemente la configuración de los clústeres de AKS
* Generar recomendaciones de seguridad que cumplan con los estándares del sector

Para obtener detalles sobre las recomendaciones de Security Center, consulte [Protección de máquinas virtuales](security-center-virtual-machine-protection.md).

## <a name="run-time-protection---real-time-threat-detection"></a>Protección en tiempo de ejecución: detección de amenazas en tiempo real

Security Center proporciona detección de amenazas en tiempo real para los entornos en contenedores y genera alertas de actividades sospechosas. Puede usar esta información para corregir problemas de seguridad y mejorar la seguridad de los contenedores rápidamente.

Detectamos amenazas en el nivel de clúster del host y AKS. Para obtener una información completa, consulte [Detección de amenazas para contenedores de Azure](https://docs.microsoft.com/azure/security-center/security-center-alerts-compute#azure-containers-).

## <a name="to-view-the-security-posture-of-your-container-related-resources"></a>Para ver la postura de seguridad de los recursos relacionados con los contenedores
1.  Abra Security Center en la página de **Procesos y aplicaciones**.
2.  Haga clic en la ficha **Contenedores**. Aparecerá la postura de los clústeres de AKS, los registros de ACR y las VM que ejecutan Docker.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la seguridad de los contenedores en Azure Security Center, consulte los siguientes recursos:
* Detalles de la [integración con Azure Kubernetes Service](azure-kubernetes-service-integration.md)

* Detalles de la [integración con Azure Container Registry](azure-container-registry-integration.md)