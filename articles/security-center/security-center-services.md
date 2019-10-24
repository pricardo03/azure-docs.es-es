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
ms.date: 09/24/2019
ms.author: memildin
ms.openlocfilehash: 275598aa50c252512348f4a04543e1beaf538626
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72529490"
---
# <a name="supported-features-available-in-azure-security-center"></a>Características compatibles disponibles en Azure Security Center

> [!NOTE]
>Algunas características solo están disponibles con el nivel estándar. Si aún no se ha registrado en el nivel estándar de Security Center, tiene disponible un período de evaluación gratuita. Para más información, vea la [página de precios de Security Center](https://azure.microsoft.com/pricing/details/security-center/).

En las secciones siguientes se muestran las características disponibles en Security Center para las [plataformas compatibles](security-center-os-coverage.md).

* [Máquinas virtuales y servidores](#vm-server-features)
* [Servicios de PaaS](#paas-services)


## Características admitidas de las máquinas virtuales y el servidor <a name="vm-server-features"></a>

> [!div class="mx-tableFixed"]

|Server|Windows|||Linux|||Precios|
|----|----|----|----|----|----|----|----|
|**Entorno**|**Las tablas de Azure**||**No de Azure**|**Las tablas de Azure**||**No de Azure**||
||**Máquina virtual**|**Conjunto de escalado de máquinas virtuales**||**Máquina virtual**|**Conjunto de escalado de máquinas virtuales**|
|[Integración de ATP de Microsoft Defender](https://docs.microsoft.com/azure/security-center/security-center-wdatp)|✔ (en versiones compatibles)|✔ (en versiones compatibles)|✔|X|X|X|Estándar|
|[Alertas de detección de amenazas de análisis de comportamiento de máquinas virtuales](https://docs.microsoft.com/azure/security-center/security-center-alerts-iaas)|✔|✔|✔|✔ (en versiones compatibles)|✔ (en versiones compatibles)|✔|Detección de amenazas de recomendaciones (gratis) (Estándar)|
|[Alertas de detección de amenazas sin archivo](https://docs.microsoft.com/azure/security-center/security-center-alerts-iaas#fileless-attack-detection-)|✔|✔|✔|X|X|X|Estándar|
|[Alertas de detección de amenazas basadas en red](https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer#azure-network-layer)|✔|✔|X|✔|✔|X|Estándar|
|[Acceso de máquina virtual Just-In-Time](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)|✔|X|X|✔|X|X|Estándar|
|[Supervisión de la integridad de los archivos](https://docs.microsoft.com/azure/security-center/security-center-file-integrity-monitoring)|✔|✔|✔|✔|✔|✔|Estándar|
|[Controles de aplicación adaptables](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)|✔|X|✔|✔|X|✔|Estándar|
|[Mapa de red](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations#network-map)|✔|✔|X|✔|✔|X|Estándar|
|[Protección de red adaptable](https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening)|✔|X|X|✔|X|X|Estándar|
|Controles de red adaptables|✔|✔|X|✔|✔|X|Estándar|
|[Panel e informes de cumplimiento normativo](https://docs.microsoft.com/azure/security-center/security-center-compliance-dashboard)|✔|✔|✔|✔|✔|✔|Estándar|
|Recomendaciones y detección de amenazas en contenedores IaaS hospedados en Docker|X|X|X|✔|✔|✔|Estándar|
|Evaluación de revisiones de SO que faltan|✔|✔|✔|✔|✔|✔|Gratuito|
|Evaluación de configuraciones de seguridad incorrectas|✔|✔|✔|✔|✔|✔|Gratuito|
|[Evaluación de EndPoint Protection](https://docs.microsoft.com/azure/security-center/security-center-services#supported-endpoint-protection-solutions-)|✔|✔|✔|X|X|X|Gratuito|
|Evaluación de Disk Encryption|✔|✔|X|✔|✔|X|Gratuito|
|Evaluación de vulnerabilidades de terceros|✔|X|X|✔|X|X|Gratuito|
|[Evaluación de la seguridad de red](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)|✔|✔|X|✔|✔|X|Gratuito|

### Soluciones de protección de punto de conexión compatibles <a name="endpoint-supported"></a>

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
>
> - La detección de System Center Endpoint Protection (SCEP) en una máquina virtual de Windows Server 2008 R2 requiere que SCEP se instale después de PowerShell 3.0 (o una versión superior).
> - La detección de protección de Trend Micro es compatible con los agentes de Deep Security.  No se admiten agentes de OfficeScan.


## Características compatibles con los servicios PaaS <a name="paas-services"> </a>

Los siguientes recursos de PaaS de Azure son compatibles con Azure Security Center:

|Servicio|Recomendaciones (gratuito)|Detección de amenazas (Estándar)|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL*|✔| ✔|
|MySQL*|✔| ✔|
|CosmosDB*|X| ✔|
|Almacenamiento de blobs|✔| ✔|
|Cuenta de almacenamiento|✔| X|
|App Service|✔| ✔|
|Función|✔| X|
|Servicio en la nube|✔| X|
|VNet|✔| X|
|Subnet|✔| X|
|NIC|✔| X|
|Grupo de seguridad de red|✔| X|
|Subscription|✔ **| ✔|
|Cuenta de Batch|✔| X|
|Cuenta de Service Fabric|✔| X|
|Cuenta de Automation|✔| X|
|Equilibrador de carga|✔| X|
|Search|✔| X|
|Espacio de nombres de Service bus|✔| X|
|Stream Analytics|✔| X|
|Espacio de nombres del centro de eventos|✔| X|
|Aplicaciones lógicas|✔| X|
|Redis|✔| X|
|Data Lake Analytics|✔| X|
|Data Lake Store|✔| X|
|Almacén de claves|✔| X|

\* Estas características se admiten actualmente en versión preliminar pública.

\*\* Las recomendaciones de Azure Active Directory (Azure AD) solo están disponibles para las suscripciones estándar.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [Security Center recopila datos y sobre el agente de Log Analytics](security-center-enable-data-collection.md).
- Obtenga información sobre cómo [Security Center administra y protege los datos](security-center-data-security.md).
- Aprenda a [planear y entender las consideraciones de diseño para adoptar Azure Security Center](security-center-planning-and-operations-guide.md).
- Revise las [plataformas compatibles con Security Center](security-center-os-coverage.md).
- Obtenga más información sobre la [detección de amenazas en VM y servidores en Azure Security Center](security-center-alerts-iaas.md).
- Consulte las [preguntas más frecuentes sobre Azure Security Center](security-center-faq.md).
- Encuentre [artículos de blog sobre el cumplimiento y la seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/).
