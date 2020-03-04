---
title: Características compatibles disponibles en Azure Security Center | Microsoft Docs
description: En este documento se proporciona una lista de servicios compatibles con Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 870ebc8d-1fad-435b-9bf9-c477f472ab17
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2020
ms.author: memildin
ms.openlocfilehash: 8d905da733ea0573b91b289da43684ed8083c804
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2020
ms.locfileid: "77616381"
---
# <a name="supported-features-available-in-azure-security-center"></a>Características compatibles disponibles en Azure Security Center

> [!NOTE]
>Algunas características solo están disponibles con el nivel estándar. Si aún no se ha registrado en el nivel estándar de Security Center, tiene disponible un período de evaluación gratuita. Para más información, vea la [página de precios de Security Center](https://azure.microsoft.com/pricing/details/security-center/).

En las secciones siguientes se muestran las características disponibles en Security Center para las [plataformas compatibles](security-center-os-coverage.md).

* [Máquinas virtuales y servidores](#vm-server-features)
* [Servicios de PaaS](#paas-services)


## Características admitidas de las máquinas virtuales y el servidor <a name="vm-server-features"></a>

### <a name="windows"></a>[Windows](#tab/features-windows)

|||||||||
|----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
||**Azure Virtual Machines**|**Azure Virtual Machine Scale Sets**|**Máquinas que no son de Azure**|**Precios**
|[Integración de ATP de Microsoft Defender](security-center-wdatp.md)|✔ (en versiones compatibles)|✔ (en versiones compatibles)|✔|Estándar|
|[Alertas de detección de amenazas de análisis de comportamiento de máquinas virtuales](threat-protection.md)|✔|✔|✔|Detección de amenazas de recomendaciones (gratis) (Estándar)|
|[Alertas de detección de amenazas sin archivo](alerts-reference.md#alerts-windows)|✔|✔|✔|Estándar|
|[Alertas de detección de amenazas basadas en red](threat-protection.md#network-layer)|✔|✔|-|Estándar|
|[Acceso de máquina virtual Just-In-Time](security-center-just-in-time.md)|✔|-|-|Estándar|
|[Evaluación de vulnerabilidades nativa](built-in-vulnerability-assessment.md)|✔|-|-|Estándar|
|[Supervisión de la integridad de los archivos](security-center-file-integrity-monitoring.md)|✔|✔|✔|Estándar|
|[Controles de aplicación adaptables](security-center-adaptive-application.md)|✔|-|✔|Estándar|
|[Mapa de red](security-center-network-recommendations.md#network-map)|✔|✔|-|Estándar|
|[Protección de red adaptable](security-center-adaptive-network-hardening.md)|✔|-|-|Estándar|
|Controles de red adaptables|✔|✔|-|Estándar|
|[Panel e informes de cumplimiento normativo](security-center-compliance-dashboard.md)|✔|✔|✔|Estándar|
|Recomendaciones y detección de amenazas en contenedores IaaS hospedados en Docker|-|-|-|Estándar|
|Evaluación de revisiones de SO que faltan|✔|✔|✔|Gratuito|
|Evaluación de configuraciones de seguridad incorrectas|✔|✔|✔|Gratuito|
|[Evaluación de EndPoint Protection](security-center-services.md#supported-endpoint-protection-solutions-)|✔|✔|✔|Gratuito|
|Evaluación de Disk Encryption|✔|✔|-|Gratuito|
|Evaluación de vulnerabilidades de terceros|✔|-|-|Gratuito|
|[Evaluación de la seguridad de red](security-center-network-recommendations.md)|✔|✔|-|Gratuito|


### <a name="linux"></a>[Linux](#tab/features-linux)

|||||||||
|----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
||**Azure Virtual Machines**|**Azure Virtual Machine Scale Sets**|**Máquinas que no son de Azure**|**Precios**
|[Integración de ATP de Microsoft Defender](security-center-wdatp.md)|-|-|-|Estándar|
|[Alertas de detección de amenazas de análisis de comportamiento de máquinas virtuales](security-center-alerts-iaas.md)|✔ (en versiones compatibles)|✔ (en versiones compatibles)|✔|Detección de amenazas de recomendaciones (gratis) (Estándar)|
|[Alertas de detección de amenazas sin archivo](alerts-reference.md#alerts-windows)|-|-|-|Estándar|
|[Alertas de detección de amenazas basadas en red](threat-protection.md#network-layer)|✔|✔|-|Estándar|
|[Acceso de máquina virtual Just-In-Time](security-center-just-in-time.md)|✔|-|-|Estándar|
|[Evaluación de vulnerabilidades nativa](built-in-vulnerability-assessment.md)|✔|-|-|Estándar|
|[Supervisión de la integridad de los archivos](security-center-file-integrity-monitoring.md)|✔|✔|✔|Estándar|
|[Controles de aplicación adaptables](security-center-adaptive-application.md)|✔|-|✔|Estándar|
|[Mapa de red](security-center-network-recommendations.md#network-map)|✔|✔|-|Estándar|
|[Protección de red adaptable](security-center-adaptive-network-hardening.md)|✔|-|-|Estándar|
|Controles de red adaptables|✔|✔|-|Estándar|
|[Panel e informes de cumplimiento normativo](security-center-compliance-dashboard.md)|✔|✔|✔|Estándar|
|Recomendaciones y detección de amenazas en contenedores IaaS hospedados en Docker|✔|✔|✔|Estándar|
|Evaluación de revisiones de SO que faltan|✔|✔|✔|Gratuito|
|Evaluación de configuraciones de seguridad incorrectas|✔|✔|✔|Gratuito|
|[Evaluación de EndPoint Protection](security-center-services.md#supported-endpoint-protection-solutions-)|-|-|-|Gratuito|
|Evaluación de Disk Encryption|✔|✔|-|Gratuito|
|Evaluación de vulnerabilidades de terceros|✔|-|-|Gratuito|
|[Evaluación de la seguridad de red](security-center-network-recommendations.md)|✔|✔|-|Gratuito|

--- 

## Soluciones de protección de punto de conexión compatibles <a name="endpoint-supported"></a>

En la tabla siguiente se proporciona una matriz de:

 - Si puede usar Azure Security Center para instalar cada solución para usted.
 - Qué soluciones de protección de punto de conexión puede detectar Security Center. Si se detecta una de las soluciones de protección de punto de conexión de esta lista, Security Center no recomendará instalar ninguna.

Para más información sobre cuándo se generan recomendaciones para cada una de estas protecciones, vea [Evaluación y recomendaciones de Endpoint Protection](security-center-endpoint-protection.md).

| Endpoint Protection| Plataformas | Instalación de Security Center | Detección de Security Center |
|------|------|-----|-----|
| Windows Defender (Microsoft Antimalware)| Windows Server 2016| No, se integra en el sistema operativo.| Sí |
| System Center Endpoint Protection (Microsoft Antimalware) | Windows Server 2012 R2, 2012, 2008 R2 (consulte la nota que hay a continuación) | Mediante extensión | Sí |
| Trend Micro: todas las versiones* | Familia de Windows Server  | Sin | Sí |
| Symantec v12.1.1100+| Familia de Windows Server  | Sin | Sí |
| McAfee v10 o posterior | Familia de Windows Server  | Sin | Sí |
| McAfee v10 o posterior | Familia de Linux Server  | Sin | Sí **\*** |
| Sophos V9+| Familia de Linux Server  | Sin | Sí **\***  |

 **\*** El estado de cobertura y los datos auxiliares solo están disponibles en el área de trabajo de Log Analytics asociada con las suscripciones protegidas. No se refleja en el portal de Azure Security Center.

> [!NOTE]
> - La detección de System Center Endpoint Protection (SCEP) en una máquina virtual de Windows Server 2008 R2 requiere que SCEP se instale después de PowerShell 3.0 (o una versión superior).
> - La detección de protección de Trend Micro es compatible con los agentes de Deep Security.  No se admiten agentes de OfficeScan.


## Características compatibles con los servicios PaaS <a name="paas-services"> </a>

Los siguientes recursos de PaaS de Azure son compatibles con Azure Security Center:

|Servicio|Recomendaciones (gratuito)|Alertas de detección de amenazas (estándar)|Evaluación de vulnerabilidades (estándar)|
|----|:----:|:----:|:----:|
|Instancias de SQL Database|✔|✔|✔|
|Azure Container Registry|-|-|✔|
|Azure Kubernetes Service|✔|✔|-|
|Azure Database for PostgreSQL*|✔|✔|-|
|Azure Database for MySQL*|✔|✔|-|
|Azure CosmosDB*|-|✔|-|
|Cuentas de almacenamiento|✔|-|-|
|Blob Storage|✔|✔|-|
|App Service|✔|✔|-|
|Aplicación de función|✔|-|-|
|Cloud Services|✔|-|-|
|Virtual Network|✔|-|-|
|Subnet|✔|-|-|
|NIC|✔|-|-|
|Grupos de seguridad de red|✔|-|-|
|Subscription|✔ **|✔|-|
|Cuenta de Batch|✔|-|-|
|Cuenta de Service Fabric|✔|-|-|
|Cuenta de Automation|✔|-|-|
|Load Balancer|✔|-|-|
|Cognitive Search|✔|-|-|
|Espacio de nombres de Service Bus|✔|-|-|
|Stream Analytics|✔|-|-|
|Espacio de nombres del centro de eventos|✔|-|-|
|Aplicaciones lógicas|✔|-|-|
|Caché para Redis|✔|-|-|
|Data Lake Analytics|✔|-|-|
|Azure Data Lake Storage|✔|-|-|
|Key Vault|✔|✔ *|-|

\* Estas características se admiten actualmente en la versión preliminar.

\*\* Las recomendaciones de Azure Active Directory (Azure AD) solo están disponibles para las suscripciones estándar.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [Security Center recopila datos y sobre el agente de Log Analytics](security-center-enable-data-collection.md).
- Obtenga información sobre cómo [Security Center administra y protege los datos](security-center-data-security.md).
- Aprenda a [planear y entender las consideraciones de diseño para adoptar Azure Security Center](security-center-planning-and-operations-guide.md).
- Revise las [plataformas compatibles con Security Center](security-center-os-coverage.md).
- Obtenga más información sobre la [detección de amenazas en VM y servidores en Azure Security Center](security-center-alerts-iaas.md).
- Consulte las [preguntas más frecuentes sobre Azure Security Center](faq-general.md).
- Encuentre [artículos de blog sobre el cumplimiento y la seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/).