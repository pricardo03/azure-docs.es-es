---
title: Acerca de los servidores de configuración, proceso y destino maestro de Azure Site Recovery | Microsoft Docs
description: En este artículo se proporciona información general sobre el uso de servidores de configuración, proceso y destino maestro al configurar la recuperación ante desastres de máquinas virtuales locales de VMware en Azure con Azure Site Recovery.
author: rayne-wiselman
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 8b3815fc9dc44484779a70b51ebff4802265d53a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66417745"
---
# <a name="about-site-recovery-components-configuration-process-master-target"></a>Acerca de los componentes de Site Recovery (configuración, proceso y destino maestro)

En este artículo se describen los servidores de configuración, proceso y destino maestro que se usan al replicar máquinas virtuales de VMware y servidores físicos en Azure con el servicio [Site Recovery](site-recovery-overview.md).

## <a name="configuration-server"></a>Servidor de configuración

Para la recuperación ante desastres de servidores físicos y máquinas virtuales de VMware locales, necesita un servidor de configuración de Site Recovery implementado localmente.

**Configuración** | **Detalles** | **Vínculos**
--- | --- | ---
**Componentes**  | La máquina del servidor de configuración ejecuta todos los componentes locales de Site Recovery, incluidos el servidor de configuración, el servidor de procesos y el servidor de destino maestro.<br/><br/> Al configurar el servidor de configuración, todos los componentes se instalan automáticamente. | [Lea](vmware-azure-common-questions.md#configuration-server) las preguntas más frecuentes sobre el servidor de configuración.
**Rol** | El servidor de configuración coordina la comunicación entre el entorno local y Azure, además de administrar la replicación de datos. | Obtenga más información sobre la arquitectura de recuperación ante desastres de [VMware](vmware-azure-architecture.md) y [servidor físico](physical-azure-architecture.md) en Azure.
**Requisitos de VMware** | Para la recuperación ante desastres de máquinas virtuales locales de VMware, debe instalar y ejecutar el servidor de configuración como una máquina virtual de VMware local de alta disponibilidad. | [Obtenga información](vmware-azure-deploy-configuration-server.md#prerequisites) sobre los requisitos previos.
**Implementación de VMware** | Se recomienda implementar el servidor de configuración con una plantilla OVA descargada. Este método permite configurar de manera sencilla un servidor de configuración que cumpla todos los requisitos y requisitos previos.<br/><br/> Si por algún motivo no puede implementar una máquina virtual de VMware mediante una plantilla OVA, puede configurar las máquinas del servidor de configuración manualmente, tal como se describe a continuación para la recuperación ante desastres de máquinas físicas. | [Implementación](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template) con una plantilla OVA.
**Requisitos del servidor físico** | Para la recuperación ante desastres en servidores físicos locales, debe implementar manualmente el servidor de configuración. | [Obtenga información](physical-azure-set-up-source.md#prerequisites) sobre los requisitos previos.
**Implementación de servidor físico** | Si no se puede instalar como una máquina virtual de VMware, se puede instalar en un servidor físico. | [Implemente](physical-azure-set-up-source.md#set-up-the-source-environment) el servidor de configuración manualmente.


## <a name="process-server"></a>Servidor de proceso

**Configuración** | **Detalles** | **Vínculos**
--- | --- | ---
**Implementación**  | Para la recuperación ante desastres de servidores físicos y máquinas virtuales de VMware locales, necesita un servidor de configuración de Site Recovery implementado localmente. De manera predeterminada, el servidor de procesos se instala en el servidor de configuración cuando se implementa. | [Más información](vmware-azure-architecture.md?#architectural-components).
**Rol (entorno local)** | - Recibe datos de replicación de las máquinas habilitadas para la replicación.<br/> - Optimiza los datos de replicación con el almacenamiento en caché, la compresión y el cifrado, y los envía a Azure Storage.<br/> - Realiza una instalación de inserción de Mobility Service de Azure Site Recovery en las máquinas virtuales de VMware y los servidores físicos locales que quiera replicar.<br/> - Realiza la detección automática de máquinas locales. | [Más información](vmware-physical-azure-config-process-server-overview.md#process-server). 
**Rol (conmutación por recuperación desde Azure)** | Después de la conmutación por error desde su sitio local, debe configurar un servidor de procesos en Azure, como máquina virtual de Azure, para controlar la conmutación por recuperación en su ubicación local.<br/><br/> El servidor de procesos de Azure es temporal. La máquina virtual de Azure se puede eliminar después de realizar la conmutación por recuperación. | [Más información](vmware-azure-set-up-process-server-azure.md).
**Escalado** | Para implementaciones más grandes, puede configurar servidores de procesos de escalabilidad horizontal adicionales en el entorno local. Los servidores adicionales escalan horizontalmente la capacidad controlando una cantidad mayor de máquinas de replicación y volúmenes mayores de tráfico de replicación.<br/><br/> Puede mover máquinas entre dos servidores de procesos para equilibrar la carga del tráfico de replicación. | [Más información](vmware-azure-set-up-process-server-scale.md).


## <a name="master-target-server"></a>Servidor de destino principal

El servidor de destino maestro controla los datos de replicación durante la conmutación por recuperación desde Azure.

- Se instala de forma predeterminada en el servidor de configuración.
- En el caso de las implementaciones de gran tamaño, puede agregar un servidor de destino maestro independiente adicional para la conmutación por recuperación.


## <a name="next-steps"></a>Pasos siguientes
- Revise la [arquitectura](vmware-azure-architecture.md) de recuperación ante desastres de máquinas virtuales de VMware y servidores físicos.
- Revise los [requisitos y requisitos previos](vmware-physical-azure-support-matrix.md) para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure. 
