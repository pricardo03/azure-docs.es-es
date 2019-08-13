---
title: Traslado de la configuración de Azure Site Recovery a otra región de Azure | Microsoft Docs
description: Instrucciones para mover la configuración de Site Recovery a otra región de Azure
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/31/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: ba0e2577d6fb8bac66322917936fe06825af0d96
ms.sourcegitcommit: 6ad03fa28a0f60cb6dce6144f728c2ceb56ff6e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68708468"
---
# <a name="moving-recovery-services-vault-and--azure-site-recovery-configuration-to-another-azure-region"></a>Traslado del almacén de Recovery Services y la configuración de Azure Site Recovery a otra región de Azure

Hay varios escenarios en los que desearía trasladar los recursos existentes de Azure de una región a otra: por motivos de administración, de gobierno o por fusiones/adquisiciones de la empresa, por ejemplo. Uno de los recursos relacionados que podría querer trasladar al mover las máquinas virtuales de Azure es su configuración de recuperación ante desastres. No hay una manera que sea la mejor para trasladar una configuración de recuperación ante desastres existente de una región a otra. En esencia, esto se debe a que la región de destino se configura en función de la región de la máquina virtual de origen y, cuando decide cambiarla, no se pueden volver a usar las configuraciones anteriores de la región de destino y deben restablecerse. En este artículo se define el proceso paso a paso para volver a configurar la recuperación ante desastres y pasar a otra región.

En este documento se hará lo siguiente:

> [!div class="checklist"]
> * Comprobar los requisitos previos para el traslado
> * Identificar los recursos que usaba Azure Site Recovery 
> * Deshabilitar replicación
> * Eliminación de los recursos 
> * Volver a configurar Site Recovery en función de la nueva región de origen para las máquinas virtuales.

> [!IMPORTANT]
> Actualmente no existe un método que sea el mejor para trasladar un almacén de Recovery Services y la configuración de recuperación ante desastres tal cual a otra región; este documento guía al cliente por el proceso de deshabilitar la replicación y reconfigurarla en la nueva región.

## <a name="prerequisites"></a>Requisitos previos

- Asegúrese de quitar y eliminar la configuración de recuperación ante desastres antes de intentar trasladar las máquinas virtuales de Azure a otra región. 

> [!NOTE]
> Si la nueva región de destino para la máquina virtual de Azure es la misma que la región de destino de recuperación ante desastres, puede usar la configuración de replicación existente y moverla según los pasos que se mencionan [aquí](azure-to-azure-tutorial-migrate.md). 

- Asegúrese de que va a tomar una decisión informada y de que las partes interesadas están informadas, ya que la máquina virtual no estará protegida contra desastres hasta que se complete su migración. 

## <a name="identify-the-resources-that-were-used-by-azure-site-recovery"></a>Identificar los recursos que usaba Azure Site Recovery
Se recomienda realizar este paso antes de continuar con el siguiente, ya que será más fácil identificar los recursos pertinentes mientras las máquinas virtuales todavía se están replicando.

Para cada una de las máquinas virtuales de Azure que se estén replicando, vaya a **Elementos protegidos** > **Elementos replicados**>**Propiedades** e identifique los siguientes recursos.

- Grupo de recursos de destino
- Cuenta de almacenamiento en caché
- Cuenta de almacenamiento de destino (en el caso de una máquina virtual de Azure basada en disco no administrado) 
- Red de destino


## <a name="disable-existing-disaster-recovery-configuration"></a>Deshabilitación de la configuración de recuperación ante desastres existente

1. Vaya a **Almacén de Recovery Services** 
2.  En **Elementos protegidos** > **Elementos replicados**, haga clic con el botón derecho en la máquina > **Deshabilitar replicación**.
3. Repita este paso para todas las máquinas virtuales que desee trasladar.
> [!NOTE]
> No se desinstalará el servicio de movilidad de los servidores protegidos; tendrá que desinstalarlo manualmente. Si va a proteger el servidor de nuevo, puede omitir la desinstalación del servicio de movilidad.

## <a name="delete-the-resources"></a>Eliminación de los recursos

1. Vaya a **Almacén de Recovery Services**
2. Haga clic en **Eliminar**
3. Continúe con la eliminación de todos los demás recursos identificados en el [paso anterior](#identify-the-resources-that-were-used-by-azure-site-recovery)
 
## <a name="move-azure-vms-to-the-new-target-region"></a>Traslado de máquinas virtuales de Azure a la nueva región de destino

Siga los pasos que se mencionan a continuación en función de sus necesidades para trasladar máquinas virtuales de Azure a la región de destino.

- [Traslado de máquinas virtuales de Azure a otra región](azure-to-azure-tutorial-migrate.md)
- [Traslado de máquinas virtuales de Azure a zonas de disponibilidad](move-azure-VMs-AVset-Azone.md)

## <a name="re-set-up-site-recovery-based-on-the-new-source-region-for-the-vms"></a>Reconfiguración de Site Recovery en función de la nueva región de origen para las máquinas virtuales

Configure la recuperación ante desastres para las máquinas virtuales de Azure que se han migrado a la nueva región mediante los pasos mencionados [aquí](azure-to-azure-tutorial-enable-replication.md).
