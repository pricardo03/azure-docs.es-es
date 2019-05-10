---
title: Información general sobre OpenShift en Azure| Microsoft Docs
description: Introducción a OpenShift en Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/7/2019
ms.author: haroldw
ms.openlocfilehash: c8ebd10544331d3b7d887124542fe4ee2753c714
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233140"
---
# <a name="openshift-in-azure"></a>OpenShift en Azure

OpenShift es una plataforma de aplicación de contenedor abierta y extensible que aporta Docker y Kubernetes a la empresa.  

OpenShift incluye Kubernetes para la administración y orquestación de contenedores. Incluye herramientas centradas en los desarrolladores y en las operaciones que permiten:

- Desarrollo rápido de aplicaciones.
- Implementación y escalado sencillos.
- Mantenimiento del ciclo de vida a largo plazo de los equipos y las aplicaciones.

Hay varias versiones de OpenShift.  De estas versiones, sólo dos están disponibles actualmente para los clientes implementar en Azure: OpenShift Container Platform y OKD (anteriormente OpenShift Origin).

## <a name="azure-red-hat-openshift"></a>Red Hat OpenShift en Azure

Microsoft Azure Red Hat OpenShift es una oferta totalmente administrada de OpenShift que se ejecuta en Azure. Microsoft y Red Hat realizan la administración y el soporte técnico de este servicio conjuntamente. Documentación completa puede ser [aquí](https://docs.microsoft.com/azure/openshift/). 

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Container Platform es una [versión comercial](https://www.openshift.com) preparada para la empresa, que procede de Red Hat y que cuenta con su respaldo. Con esta versión, los clientes adquieren los derechos necesarios para OpenShift Container Platform y son responsables de la instalación y la administración de toda la infraestructura.

Dado que los clientes "poseen" toda la plataforma, puede instalar en su centro de datos de forma local o en una nube pública (por ejemplo, Azure).

## <a name="okd"></a>OKD

OKD es un proyecto ascendente de [código abierto](https://www.okd.io/) de OpenShift al que da soporte la comunidad. OKD puede instalarse en CentOS o Red Hat Enterprise Linux (RHEL).

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de los requisitos previos comunes para OpenShift en Azure](./openshift-prerequisites.md)
- [Implementación de OpenShift Container Platform en Azure](./openshift-container-platform.md)
- [Implementar la oferta de Marketplace autoadministrados de plataforma de contenedor de OpenShift](./openshift-marketplace-self-managed.md)
- [Implementación de OpenShift en Azure Stack](./openshift-azure-stack.md)
- [Tareas posteriores a la implementación](./openshift-post-deployment.md)
- [Solución de problemas de implementación de OpenShift](./openshift-troubleshooting.md)
