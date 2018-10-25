---
title: Características y plataformas compatibles con Azure Security Center | Microsoft Docs
description: En este documento se proporciona una lista de características y plataformas compatibles con Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2018
ms.author: rkarlin
ms.openlocfilehash: 279818e6b43e53206deb9e33591f75ef381a8962
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49319989"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Características y plataformas compatibles con Azure Security Center

La supervisión del estado de seguridad y las recomendaciones están disponibles para las máquinas virtuales, tanto las creadas con los modelos de implementación clásica como de Resource Manager, y para los equipos.

> [!NOTE]
> Obtenga más información sobre los [modelos de implementación clásico y de Resource Manager](../azure-classic-rm.md) para los recursos de Azure.
>
>

## <a name="supported-platforms"></a>Plataformas compatibles 

En esta sección se enumeran las plataformas en las que puede ejecutarse el agente de Azure Security Center y desde las que puede recopilar datos.

### <a name="supported-platforms-for-windows-computers-and-vms"></a>Plataformas compatibles con máquinas virtuales y equipos de Windows
Sistemas operativos Windows compatibles:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016


### <a name="supported-platforms-for-linux-computers-and-vms"></a>Plataformas compatibles con máquinas virtuales y equipos de Linux
Sistemas operativos Linux compatibles:

* Ubuntu versiones 12.04 LTS, 14.04 LTS, 16.04 LTS
* Debian versiones 6, 7, 8, 9
* CentOS versiones 5, 6, 7
* Red Hat Enterprise Linux (RHEL) versiones 5, 6, 7
* SUSE Linux Enterprise Server (SLES) versiones 11, 12
* Oracle Linux versiones 5, 6, 7
* Amazon Linux 2012.09 hasta 2017
* OpenSSL 1.1.0 solo se admite en plataformas x86_64 (64 bits)

> [!NOTE]
> El análisis de comportamiento de máquinas virtuales todavía no está disponible para los sistemas operativos Linux.
>
>

## <a name="vms-and-cloud-services"></a>Cloud Services y Virtual Machines
También se admiten máquinas virtuales que se ejecuten en un servicio en la nube. Se supervisan los roles de web y de trabajo de servicios en la nube que se ejecutan en espacios de producción. Para aprender más sobre el servicio en la nube, consulte la [información general sobre Cloud Services](../cloud-services/cloud-services-choose-me.md).


## <a name="supported-iaas-features"></a>Características admitidas de IaaS

> [!div class="mx-tableFixed"]
> 

|Server|Windows||Linux||
|----|----|----|----|----|
|Entorno|Azure|Ajeno a Azure|Azure|Ajeno a Azure|
|Alertas de detección de amenazas de VMBA|✔|✔|✔ (en versiones compatibles)|✔|
|Alertas de detección de amenazas basadas en red|✔|X|✔|X|
|Integración con ATP de Windows Defender*|✔ (en versiones compatibles)|✔|X|X|
|Revisiones que faltan|✔|✔|✔|✔|
|Configuraciones de seguridad|✔|✔|✔|✔|
|Antimalware|✔|✔|X|X|
|Acceso a VM JIT|✔|X|✔|X|
|Controles de aplicación adaptables|✔ (solo Azure)|X|X|X|
|FIM|✔|✔|✔|✔|
|Cifrado de discos|✔|X|✔|X|
|Implementación de terceros|✔|X|✔|X|
|Grupos de seguridad de red|✔|X|✔|X|
|Filess V1|✔|✔|X|X|
|Mapa de red|✔|X|✔|X|
|Protección de red adaptable|✔|X|✔|X|

* Estas características se admiten actualmente en versión preliminar pública.


## <a name="supported-paas-features"></a>Características admitidas de PaaS


|Servicio|Recomendaciones|Detección de amenazas|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL*|✔| ✔|
|MySQL*|✔| ✔|
|Cuentas de almacenamiento de blobs*|✔| ✔|
|App Services|✔| ✔|
|Servicios en la nube|✔| X|
|Redis Cache|✔| X|
|Service Fabric|✔| X|
|Azure Automation|✔| X|
|Lago de datos |✔| X|
|Almacén de claves|✔| X|
|Service Bus|✔| X|
|Stream Analytics|✔| X|
|Batch|✔| X|
|Aplicaciones lógicas|✔| X|
|Redes virtuales|✔| N/D|
|Subredes|✔| N/D|
|Tarjetas de red|✔| ✔|
|Grupos de seguridad de red|✔| N/D|
|Subscription|✔| ✔|

* Estas características se admiten actualmente en versión preliminar pública.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de planeamiento y operaciones de Azure Security Center](security-center-planning-and-operations-guide.md): aprenda a realizar planes y conozca las consideraciones de diseño necesarias para adoptar Azure Security Center.
- [Alertas de seguridad por tipo de Azure Security Center](security-center-alerts-type.md#virtual-machine-behavioral-analysis): obtenga más información sobre el análisis de comportamiento de máquinas virtuales y el análisis de la memoria de volcado en Security Center.
- [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md) : encuentre las preguntas más frecuentes sobre el uso del servicio.
- [Blog de seguridad de Azure](http://blogs.msdn.com/b/azuresecurity/): encuentre entradas de blog sobre cumplimiento y seguridad de Azure.
