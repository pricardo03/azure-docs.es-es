---
title: Plataformas compatibles con Azure Security Center | Microsoft Docs
description: En este documento se proporciona una lista de sistemas operativos Windows y Linux compatibles con Azure Security Center.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2018
ms.author: terrylan
ms.openlocfilehash: 54d173caa0e3eb4bbd8bda7c924e56d546a99662
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297509"
---
# <a name="supported-platforms-in-azure-security-center"></a>Plataformas compatibles con Azure Security Center
La supervisión del estado de seguridad y las recomendaciones están disponibles para las máquinas virtuales, tanto las creadas con los modelos de implementación clásica como de Resource Manager, y para los equipos.

> [!NOTE]
> Obtenga más información sobre los [modelos de implementación clásico y de Resource Manager](../azure-classic-rm.md) para los recursos de Azure.
>
>

## <a name="supported-platforms-for-windows-computers-and-vms"></a>Plataformas compatibles con máquinas virtuales y equipos de Windows
Sistemas operativos Windows compatibles:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016


## <a name="supported-platforms-for-linux-computers-and-vms"></a>Plataformas compatibles con máquinas virtuales y equipos de Linux
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

## <a name="next-steps"></a>Pasos siguientes

- [Guía de planeamiento y operaciones de Azure Security Center](security-center-planning-and-operations-guide.md): aprenda a realizar planes y conozca las consideraciones de diseño necesarias para adoptar Azure Security Center.
- [Alertas de seguridad por tipo de Azure Security Center](security-center-alerts-type.md#virtual-machine-behavioral-analysis): obtenga más información sobre el análisis de comportamiento de máquinas virtuales y el análisis de la memoria de volcado en Security Center.
- [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md) : encuentre las preguntas más frecuentes sobre el uso del servicio.
- [Blog de seguridad de Azure](http://blogs.msdn.com/b/azuresecurity/): encuentre entradas de blog sobre cumplimiento y seguridad de Azure.
