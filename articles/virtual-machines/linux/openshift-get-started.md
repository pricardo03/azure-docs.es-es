---
title: Información general sobre OpenShift en Azure| Microsoft Docs
description: Introducción a OpenShift en Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: d68215359d50ac153d6df3bbcce5a9b6171698bb
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085446"
---
# <a name="openshift-in-azure"></a>OpenShift en Azure

OpenShift es una plataforma de aplicación de contenedor abierta y extensible que aporta Docker y Kubernetes a la empresa.  

OpenShift incluye Kubernetes para la administración y orquestación de contenedores. Incluye herramientas centradas en los desarrolladores y en las operaciones que permiten:

- Desarrollo rápido de aplicaciones.
- Implementación y escalado sencillos.
- Mantenimiento del ciclo de vida a largo plazo de los equipos y las aplicaciones.

Hay varias versiones de OpenShift disponibles:

- OpenShift Container Platform
- OpenShift en Azure (OpenShift totalmente administrado que se presenta a principios de 2019)
- OKD (anteriormente OpenShift Origin)
- OpenShift Dedicated
- OpenShift Online

De las cinco versiones que describe este artículo, solo dos están disponibles en la actualidad para que los clientes las implementen en Azure: OpenShift Container Platform y OKD.

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Container Platform es una [versión comercial](https://www.openshift.com) preparada para la empresa, que procede de Red Hat y que cuenta con su respaldo. Con esta versión, los clientes adquieren los derechos necesarios para OpenShift Container Platform y son responsables de la instalación y la administración de toda la infraestructura.

Puesto que el cliente "posee" toda la plataforma, puede instalarla en su centro de datos local, o bien en una nube pública (como Azure, AWS o Google).

## <a name="openshift-on-azure"></a>OpenShift en Azure

OpenShift en Azure es una oferta totalmente administrada de OpenShift que se ejecuta en Azure. Microsoft y Red Hat realizan la administración y el soporte técnico de este servicio conjuntamente. El clúster se implementará en la suscripción de Azure del cliente. El servicio está actualmente en versión preliminar privada y se ha programado la disponibilidad general a principios de 2019. Se proporcionará más información cuando la oferta se aproxime a la fecha de disponibilidad general.

## <a name="okd-formerly-openshift-origin"></a>OKD (anteriormente OpenShift Origin)

OKD es un proyecto ascendente de [código abierto](https://www.okd.io/) de OpenShift al que da soporte la comunidad. OKD puede instalarse en CentOS o Red Hat Enterprise Linux (RHEL).

## <a name="openshift-dedicated"></a>OpenShift Dedicated

Dedicated es una implementación de OpenShift *de un solo inquilino* administrada por Red Hat que usa OpenShift Container Platform. Red Hat administra toda la infraestructura subyacente (máquinas virtuales, clúster de OpenShift, redes, almacenamiento, etc.). El clúster es específico para un cliente y se ejecuta en una nube pública (por ejemplo, AWS o Google). Un clúster inicial incluye cuatro nodos de aplicación y todos los costos son anuales y de pago por adelantado.

## <a name="openshift-online"></a>OpenShift Online

Online es una aplicación de OpenShift *multiinquilino* administrada por Red Hat, que usa Container Platform. Red Hat administra toda la infraestructura subyacente (por ejemplo, máquinas virtuales, clúster de OpenShift, redes y almacenamiento). 

Con esta versión, el cliente implementa los contenedores, pero no tiene ningún control sobre qué hosts ejecutan los contenedores. Puesto que Online es una aplicación multiinquilino, los contenedores pueden colocarse en los mismos hosts de máquina virtual que los contenedores de otros clientes. El coste es por contenedor.

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de los requisitos previos comunes para OpenShift en Azure](./openshift-prerequisites.md)
- [Implementación de OpenShift Container Platform en Azure](./openshift-container-platform.md)
- [Implementación de OKD en Azure](./openshift-okd.md)
- [Implementación de OpenShift en Azure Stack](./openshift-azure-stack.md)
- [Tareas posteriores a la implementación](./openshift-post-deployment.md)
- [Solución de problemas de implementación de OpenShift](./openshift-troubleshooting.md)
