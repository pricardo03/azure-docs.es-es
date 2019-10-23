---
title: Requisitos del servidor de configuración para realizar la recuperación ante desastres de VMware en Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se describen el soporte y los requisitos al implementar el servidor de configuración para realizar la recuperación ante desastres de VMware en Azure con Azure Site Recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.date: 09/09/2019
ms.author: raynew
ms.openlocfilehash: d83b99ea540d6232f4c0786d3a743f97332e1c9f
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792315"
---
# <a name="configuration-server-requirements-for-vmware-disaster-recovery-to-azure"></a>Requisitos del servidor de configuración para la recuperación ante desastres de VMware en Azure

Cuando se usa [Azure Site Recovery](site-recovery-overview.md) para realizar la recuperación ante desastres de servidores físicos y máquinas virtuales de VMware en Azure, se implementa un servidor de configuración local.

- El servidor de configuración coordina la comunicación entre Azure y VMware local. También administra la replicación de datos.
- [Más información](vmware-azure-architecture.md) acerca de los componentes y procesos del servidor de configuración.

## <a name="configuration-server-deployment"></a>Implementación del servidor de configuración

Para la recuperación ante desastres de máquinas virtuales de VMware en Azure, implemente el servidor de configuración como una VM de VMware.

- Site Recovery proporciona una plantilla OVA que puede descargar desde Azure Portal y, después, importar en vCenter Server para configurar la máquina virtual del servidor de configuración.
- Al implementar el servidor de configuración con la plantilla OVA, la máquina virtual cumplirá automáticamente con los requisitos que se enumeran en este artículo.
- Se recomienda que use la plantilla OVA para configurar el servidor de configuración. Sin embargo, si está configurando la recuperación ante desastres para máquinas virtuales de VMware y no puede usar la plantilla OVA, puede implementar el servidor de configuración con [las instrucciones que se proporcionan](physical-azure-set-up-source.md).
- Si va a implementar el servidor de configuración para la recuperación ante desastres de máquinas físicas locales en Azure, siga las instrucciones que se incluyen [en este artículo](physical-azure-set-up-source.md). 

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="next-steps"></a>Pasos siguientes
Configure la recuperación ante desastres en Azure para [máquinas virtuales VMware](vmware-azure-tutorial.md).
