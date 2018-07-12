---
title: Configuración del entorno de origen para la replicación de VMware en Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo configurar el entorno local para replicar máquinas virtuales de VMware en Azure con Azure Site Recovery.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: ramamill
ms.openlocfilehash: 1380c1bc820a815fae317a86fcd0ee4f46dd9aa5
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952661"
---
# <a name="set-up-the-source-environment-for-vmware-to-azure-replication"></a>Configuración del entorno de origen para la replicación de VMware en Azure

En este artículo se describe cómo configurar el entorno local de origen para replicar máquinas virtuales de VMware en Azure. Incluye los pasos para seleccionar el escenario de replicación, configurar una máquina local como el servidor de configuración de Site Recovery y detectar automáticamente las máquinas virtuales locales. 

## <a name="prerequisites"></a>requisitos previos

En este artículo se supone que ya ha seguido estos pasos:
- [Configurar los recursos](tutorial-prepare-azure.md) en [Azure Portal](http://portal.azure.com).
- [Configurar VMware local](vmware-azure-tutorial-prepare-on-premises.md), incluida una cuenta dedicada para la detección automática.



## <a name="choose-your-protection-goals"></a>Selección de los objetivos de protección

1. En Azure Portal, vaya a la hoja de los almacenes de **Recovery Services** y seleccione su almacén.
2. En el menú de recursos del almacén, haga clic en **Introducción** > **Site Recovery** > **Paso 1: Preparar la infraestructura** > **Objetivo de protección**.

    ![Elegir objetivos](./media/vmware-azure-set-up-source/choose-goals.png)
3. En **Objetivo de protección**, seleccione **To Azure** (En Azure) y **Yes, with VMware vSphere Hypervisor** (Sí, con VMware vSphere Hypervisor). A continuación, haga clic en **Aceptar**.

    ![Elegir objetivos](./media/vmware-azure-set-up-source/choose-goals2.png)

## <a name="set-up-the-configuration-server"></a>Configuración del servidor de configuración

Puede configurar el servidor de configuración como una máquina virtual de VMware local mediante una plantilla de Open Virtualization Application (OVA). [Más información](concepts-vmware-to-azure-architecture.md) sobre los componentes que se instalarán en la máquina virtual VMware.

1. Obtenga más información sobre los [requisitos previos](vmware-azure-deploy-configuration-server.md#prerequisites) para la implementación del servidor de configuración.
2. [Compruebe los números que representan la capacidad](vmware-azure-deploy-configuration-server.md#capacity-planning) para la implementación.
3. [Descargue](vmware-azure-deploy-configuration-server.md#download-the-template) e [importe](vmware-azure-deploy-configuration-server.md#import-the-template-in-vmware) la plantilla de OVA para configurar una máquina virtual de VMware local que se ejecute en el servidor de configuración. La licencia proporcionada con la plantilla es una licencia de evaluación y es válida durante 180 días. Tras este periodo, el cliente debe activar Windows con una licencia adquirida.
4. Encienda la máquina virtual VMware y [regístrela](vmware-azure-deploy-configuration-server.md#register-the-configuration-server-with-azure-site-recovery-services) en el almacén de Recovery Services.


## <a name="add-the-vmware-account-for-automatic-discovery"></a>Incorporación de la cuenta de VMware para detección automática

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

## <a name="connect-to-the-vmware-server"></a>Conexión al servidor de VMware

Para permitir que Azure Site Recovery detecte las máquinas virtuales que se ejecutan en el entorno local, debe conectar los hosts ESXi de vSphere o servidor vCenter de VMware con Site Recovery.

Seleccione **+vCenter** para comenzar a conectar un servidor vCenter de VMware o un host ESXi de vSphere de VMware.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Problemas comunes
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Pasos siguientes
[Configure el entorno de destino](./vmware-azure-set-up-target.md) en Azure.
