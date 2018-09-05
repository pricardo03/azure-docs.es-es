---
title: Información general sobre OpenShift en Azure| Microsoft Docs
description: Introducción a OpenShift en Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
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
ms.openlocfilehash: e3ab060c1cea28f83c18dc89aeea7716ec86572a
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190350"
---
# <a name="openshift-in-azure"></a>OpenShift en Azure

OpenShift es una plataforma de aplicación de contenedor abierta y extensible que aporta Docker y Kubernetes a la empresa.  

OpenShift incluye Kubernetes para la administración y orquestación de contenedores. Incluye herramientas centradas en los desarrolladores y en las operaciones que permiten:

- Desarrollo rápido de aplicaciones.
- Implementación y escalado sencillos.
- Mantenimiento del ciclo de vida a largo plazo de los equipos y las aplicaciones.

Hay varias versiones de OpenShift disponibles:

- OKD (anteriormente OpenShift Origin)
- OpenShift Container Platform
- OpenShift Online
- OpenShift Dedicated

De las cuatro ofertas que cubre este artículo, solo dos están disponibles para que los clientes las implementen en Azure: OpenShift Origin y OpenShift Container Platform.

## <a name="okd-formerly-openshift-origin"></a>OKD (anteriormente OpenShift Origin)

OKD es un proyecto ascendente de [código abierto](https://www.okd.io/) de OpenShift al que da soporte la comunidad. OKD puede instalarse en CentOS o Red Hat Enterprise Linux (RHEL).

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Container Platform es una [versión comercial](https://www.openshift.com) preparada para la empresa, que procede de Red Hat y que cuenta con su respaldo. Con esta versión, los clientes adquieren los derechos necesarios para OpenShift Container Platform y son responsables de la instalación y la administración de toda la infraestructura.

Puesto que el cliente "posee" toda la plataforma, puede instalarla en su centro de datos local, o bien en una nube pública (como Azure, AWS o Google).

## <a name="openshift-online"></a>OpenShift Online

Online es una aplicación de OpenShift *multiinquilino* administrada por Red Hat, que usa Container Platform. Red Hat administra toda la infraestructura subyacente (por ejemplo, máquinas virtuales, clúster de OpenShift, redes y almacenamiento). 

Con esta versión, el cliente implementa los contenedores, pero no tiene ningún control sobre qué hosts ejecutan los contenedores. Puesto que Online es una aplicación multiinquilino, los contenedores pueden colocarse en los mismos hosts de máquina virtual que los contenedores de otros clientes. El coste es por contenedor.

## <a name="openshift-dedicated"></a>OpenShift Dedicated

Dedicated es una aplicación de OpenShift *de un solo inquilino* administrada por Red Hat, que usa Container Platform. Red Hat administra toda la infraestructura subyacente (máquinas virtuales, clúster de OpenShift, redes, almacenamiento, etc.). El clúster es específico para un cliente y se ejecuta en una nube pública (por ejemplo, AWS o Google; para Azure entrará en vigor a principios de 2018). Un clúster de inicio incluye cuatro nodos de aplicación por 48 000 USD al año (pago por adelantado).

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de los requisitos previos comunes para OpenShift en Azure](./openshift-prerequisites.md)
- [Implementación de OpenShift Origin en Azure](./openshift-origin.md)
- [Implementación de OpenShift Container Platform en Azure](./openshift-container-platform.md)
- [Tareas posteriores a la implementación](./openshift-post-deployment.md)
- [Solución de problemas de implementación de OpenShift](./openshift-troubleshooting.md)
