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
ms.date: 1/9/2019
ms.author: rkarlin
ms.openlocfilehash: 98d2d29e7822d9ca97ba488fcf67298a0b40efbf
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2019
ms.locfileid: "54265471"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Características y plataformas compatibles con Azure Security Center

La supervisión del estado de seguridad y las recomendaciones están disponibles para las máquinas virtuales, creadas con el modelo de implementación clásica o con el modelo de implementación de Resource Manager, y para los equipos.

> [!NOTE]
> Obtenga más información sobre los [modelos de implementación clásico y de Resource Manager](../azure-classic-rm.md) para los recursos de Azure.
>
>

## <a name="platforms-that-support-the-data-collection-agent"></a>Plataformas compatibles con el agente de recopilación de datos 

En esta sección se enumeran las plataformas en las que puede ejecutarse el agente de Azure Security Center y desde las que puede recopilar datos.

### <a name="supported-platforms-for-windows-computers-and-vms"></a>Plataformas compatibles con máquinas virtuales y equipos de Windows
Se admiten los siguientes sistemas operativos Windows:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

> [!NOTE]
> La integración con ATP de Windows Defender solo admite Windows Server 2012 R2 y Windows Server 2016.
>
>

### <a name="supported-platforms-for-linux-computers-and-vms"></a>Plataformas compatibles con máquinas virtuales y equipos de Linux
Se admiten los siguientes sistemas operativos Linux:

* Versiones de Ubuntu 12.04 LTS, 14.04 LTS y 16.04 LTS.
* Versiones de Debian 6, 7, 8 y 9.
* Versiones de CentOS 5, 6 y 7.
* Versiones de Red Hat Enterprise Linux (RHEL) 5, 6 y 7.
* Versiones de SUSE Linux Enterprise Server (SLES) 11 y 12.
* Versiones de Oracle Linux 5, 6 y 7.
* Amazon Linux 2012.09 a 2017.
* OpenSSL 1.1.0 solo se admite en plataformas x86_64, de 64 bits.

## <a name="vms-and-cloud-services"></a>Cloud Services y Virtual Machines
También se admiten máquinas virtuales que se ejecuten en un servicio en la nube. Se supervisan los roles de trabajo y web de servicios en la nube que se ejecutan en espacios de producción. Para más información sobre Cloud Services, consulte la [información general sobre Azure Cloud Services](../cloud-services/cloud-services-choose-me.md).


## <a name="supported-iaas-features"></a>Características admitidas de IaaS

> [!div class="mx-tableFixed"]
> 

|Server| Windows||Linux||
|----|----|----|----|----|
|Entorno|Azure|Ajeno a Azure|Azure|Ajeno a Azure|
|Alertas de detección de amenazas de VMBA|✔|✔|✔ (en versiones compatibles)|✔|
|Alertas de detección de amenazas basadas en red|✔|X|✔|X|
|Integración con ATP de Windows Defender|✔ (en versiones compatibles)|✔|X|X|
|Revisiones que faltan|✔|✔|✔|✔|
|Configuraciones de seguridad|✔|✔|✔|✔|
|Endpoint Protection|✔|✔|X|X|
|Acceso a VM JIT|✔|X|✔|X|
|Controles de aplicación adaptables|✔|X|X|X|
|FIM|✔|✔|✔|✔|
|Cifrado de discos|✔|X|✔|X|
|Implementación de aplicaciones de terceros|✔|X|✔|X|
|Grupos de seguridad de red|✔|X|✔|X|
|Detección de amenazas sin archivo|✔|✔|X|X|
|Mapa de red|✔|X|✔|X|
|Controles de red adaptables|✔|X|✔|X|


### <a name="supported-endpoint-protection-solutions"></a>Soluciones de protección de punto de conexión compatibles

En la tabla siguiente se proporciona una matriz de:
 - Si puede usar Azure Security Center para instalar cada solución para usted.
 - Qué soluciones de protección de punto de conexión puede detectar Security Center. Si se detecta una de estas soluciones de protección de punto de conexión, Security Center no recomendará instalar ninguna.

| Endpoint Protection| Plataformas | Instalación de Security Center | Detección de Security Center |
|------|------|-----|-----|
| Windows Defender (Microsoft Antimalware)| Windows Server 2016| No, se integra en el sistema operativo.| SÍ |
| System Center Endpoint Protection (Microsoft Antimalware) | Windows Server 2012 R2, 2012, 2008 R2 (consulte la nota que hay a continuación) | Mediante extensión | SÍ |
| Trend Micro: todas las versiones | Familia de Windows Server  | Sin  | SÍ |
| Symantec v12.1.1100+| Familia de Windows Server  | Sin  | SÍ |
| McAfee v10 o posterior | Familia de Windows Server  | Sin  | SÍ |
| Kaspersky| Familia de Windows Server  | Sin  | Sin   |
| Sophos| Familia de Windows Server  | Sin  | Sin   |

> [!NOTE]
> - La detección de System Center Endpoint Protection (SCEP) en una máquina virtual de Windows Server 2008 R2 requiere que SCEP se instale después de PowerShell 3.0 (o una versión superior).
>
>

## <a name="supported-paas-features"></a>Características admitidas de PaaS 


|Servicio|Recomendaciones|Detección de amenazas|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL*|✔| ✔|
|MySQL*|✔| ✔|
|Cuentas de almacenamiento de Azure Blob*|✔| ✔|
|App Services|✔| ✔|
|Cloud Services|✔| X|
|Redes virtuales|✔| N/D|
|Subredes|✔| N/D|
|Tarjetas de red|✔| ✔|
|Grupos de seguridad de red|✔| N/D|
|Subscription|✔| ✔|

\* Estas características se admiten actualmente en versión preliminar pública. 



## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [planear y entender las consideraciones de diseño para adoptar Azure Security Center](security-center-planning-and-operations-guide.md).
- Obtenga más información acerca del [análisis de comportamiento de máquinas virtuales y análisis de la memoria de volcado en Azure Security Center](security-center-alerts-type.md#virtual-machine-behavioral-analysis).
- Consulte las [preguntas más frecuentes sobre Azure Security Center](security-center-faq.md).
- Encuentre [artículos de blog sobre el cumplimiento y la seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/).
