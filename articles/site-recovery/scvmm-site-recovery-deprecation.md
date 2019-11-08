---
title: Próximo desuso de la recuperación ante desastres entre los sitios propiedad del cliente con Hyper-V y entre sitios administrados por SCVMM en Azure| Microsoft Docs
description: Detalles sobre el próximo desuso de la recuperación ante desastres entre los sitios propiedad del cliente con Hyper-V y entre sitios administrados por SCVMM en Azure y opciones alternativas
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 09/13/2019
ms.author: rajanaki
ms.openlocfilehash: 68881d57f251d2f26996b2a837780106326ec1d3
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73492285"
---
# <a name="upcoming-deprecation-of-dr-between-customer-owned-sites-using-hyper-v-and-between-sites-managed-by-scvmm-to-azure"></a>Próximo desuso de la recuperación ante desastres entre los sitios propiedad del cliente con Hyper-V y entre sitios administrados por SCVMM en Azure

En este artículo se describe el plan para el próximo desuso, las implicaciones correspondientes y las opciones alternativas disponibles para los clientes de los escenarios siguientes que se admiten en Azure Site Recovery. 

- Recuperación ante desastres entre máquinas virtuales de Hyper-V administradas por SCVMM entre sitios propiedad del cliente 
- Recuperación ante desastres de máquinas virtuales de Hyper-V administradas por SCVMM en Azure

> [!IMPORTANT]
> Actualmente, estos escenarios están marcados para desuso y se espera que los clientes realicen los pasos de corrección lo antes posible para evitar cualquier interrupción en su entorno. 
 

## <a name="what-changes-should-you-expect"></a>¿Qué cambios debe esperar?

- A partir de noviembre de 2019, no se permitirá la incorporación de usuarios nuevos en estos escenarios. Las **operaciones de administración y replicaciones existentes**, como la conmutación por error, la conmutación por error de prueba, la supervisión, etc. **no se verán afectadas**.

- Una vez que los escenarios estén en desuso, habrá las implicaciones siguientes, a menos que el cliente siga los pasos recomendados.

    - Recuperación ante desastres entre máquinas virtuales de Hyper-V administradas por SCVMM entre sitios propiedad del cliente: Las replicaciones seguirán funcionando, ya que la funcionalidad subyacente de la réplica de Hyper-V seguirá funcionando, pero los clientes no podrán ver, administrar ni realizar ninguna operación relacionada con la recuperación ante desastres a través de la experiencia de Azure Site Recovery en Azure Portal. 
    - Recuperación ante desastres de máquinas virtuales de Hyper-V administradas por SCVMM en Azure: Las replicaciones existentes se interrumpirán y los clientes no podrán ver, administrar ni realizar ninguna operación relacionada con la recuperación ante desastres a través de Azure Site Recovery


## <a name="recommended-actions-to-be-taken"></a>Acciones recomendadas que se deben realizar

A continuación, se muestran las opciones que el cliente tiene para asegurarse de que su estrategia de recuperación ante desastres no se vea afectada una vez que el escenario esté en desuso. 

- Elija [empezar a usar Azure como destino de la recuperación ante desastres para hosts de Hyper-V](hyper-v-azure-tutorial.md).

> [!IMPORTANT]
> Tenga en cuenta que el entorno local todavía puede tener SCVMMM, pero configurará ASR con referencias solo a los hosts de Hyper-V.

- Elija continuar con la replicación de sitio a sitio pero con la [solución de réplica de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica) subyacente, aunque no podrá administrar las configuraciones de recuperación ante desastres con Azure Site Recovery en Azure Portal. 


## <a name="next-steps"></a>Pasos siguientes
Planee el desuso y elija una opción alternativa que sea más adecuada para su infraestructura y negocios. En caso de que tenga consultas relacionadas, póngase en contacto con Soporte técnico de Microsoft.

