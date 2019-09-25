---
title: Configuraciones de red personalizadas para la máquina virtual de conmutación por error | Microsoft Docs
description: Se proporciona información general sobre las configuraciones de red personalizadas para la máquina virtual de conmutación por error en la replicación de máquinas virtuales de Azure mediante Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 08/07/2019
ms.author: rajanaki
ms.openlocfilehash: 1905d6afb023b1bed15f1359fed8477d815acb45
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71087702"
---
# <a name="customize-networking-configurations-of-the-target-azure-vm"></a>Personalización de las configuraciones de red de la máquina virtual de Azure de destino

En este artículo se proporciona una guía sobre cómo personalizar las configuraciones de red en la máquina virtual de Azure de destino al replicar y recuperar máquinas virtuales de Azure de una región a otra mediante [Azure Site Recovery](site-recovery-overview.md).

## <a name="before-you-start"></a>Antes de comenzar

Obtenga información sobre cómo Site Recovery proporciona recuperación ante desastres para [este escenario](azure-to-azure-architecture.md).

## <a name="supported-networking-resources"></a>Compatibilidad con los recursos de red

Al replicar máquinas virtuales de Azure, se pueden proporcionar las siguientes configuraciones de recursos clave para la máquina virtual de conmutación por error.

- [Equilibrador de carga interno](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#what-is-standard-load-balancer)
- [Dirección IP pública](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses)
- [Grupo de seguridad de red](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group) para la subred y para la NIC

 > [!IMPORTANT]
  > Esta configuración solo se admite, por el momento, en la operación de conmutación por error y no en la de conmutación por error de prueba.

## <a name="pre-requisites"></a>Requisitos previos

- Asegúrese de planear las configuraciones de recuperación de antemano.
- Los recursos de red se deben crear por adelantado. Proporciónelos como una entrada para que el servicio de Azure Site Recovery pueda respetar esta configuración y asegúrese de que la máquina virtual de conmutación por error se adhiere a ella.

## <a name="steps-to-customize-failover-networking-configurations"></a>Pasos para personalizar las configuraciones de red de conmutación por error

1. Vaya a **Elementos replicados**. 
2. Haga clic en la máquina virtual de Azure que desee.
3. Haga clic en **Proceso y red** y en **Editar**. Observará que las opciones de configuración de la NIC incluyen los recursos correspondientes en el origen. 

     ![Personalización](media/azure-to-azure-customize-networking/edit-networking-properties.png)

4. Haga clic en **Editar** junto a la NIC que quiere configurar. En la siguiente hoja que se abre, seleccione los recursos creados previamente correspondientes en el destino.

    ![NIC: obtención de detalles](media/azure-to-azure-customize-networking/nic-drilldown.png) 

5. Haga clic en **OK**.

Site Recovery ahora respetará esta configuración y garantizará que la máquina virtual de la conmutación por error esté conectada al recurso seleccionado a través de la NIC correspondiente.

## <a name="troubleshooting"></a>solución de problemas

### <a name="unable-to-view-or-select-a-resource"></a>No se puede ver o seleccionar un recurso

Si no puede seleccionar o ver un recurso de red, siga las siguientes comprobaciones y condiciones:

- El campo de destino de un recurso de red solo se habilita si la máquina virtual de origen tenía una entrada correspondiente. Esto se basa en el principio de que en un escenario de recuperación ante desastres, se necesita la versión exacta o una versión reducida verticalmente del origen.
- Para cada uno de los recursos de red en cuestión, se aplican algunos filtros de la lista desplegable para tener la seguridad de que la máquina virtual de conmutación por error pueda asociarse al recurso seleccionado y se mantenga la confiabilidad de la conmutación por error. Estos filtros se basan en las mismas condiciones de red que se habrían verificado al configurar la máquina virtual de origen.

Validaciones del equilibrador de carga interno:

1. La suscripción y la región del equilibrador de carga y de la máquina virtual de destino deben ser iguales.
2. La red virtual asociada con el equilibrador de carga interno y la de la máquina virtual de destino debe ser igual.
3. La SKU de IP pública de la máquina virtual de destino y la SKU del equilibrador de carga interno deben ser iguales.
4. Si la máquina virtual de destino está configurada para colocarse en una zona de disponibilidad, compruebe si el equilibrador de carga tiene redundancia de zona o forma parte de cualquier zona de disponibilidad. (Los equilibradores de carga de la SKU básica no admiten zonas y, en este caso, no se mostrarán en la lista desplegable).
5. Asegúrese de que el equilibrador de carga interno tenga un grupo back-end y una configuración de front-end creados previamente.


Dirección IP pública:
    
1. La suscripción y la región de la dirección IP pública y de la máquina virtual de destino deben ser iguales.
2. La SKU de IP pública de la máquina virtual de destino y la SKU del equilibrador de carga interno deben ser iguales.

Grupo de seguridad de red:
1. La suscripción y la región del grupo de seguridad de red y de la máquina virtual de destino deben ser iguales.


> [!WARNING]
> Si la máquina virtual de destino está asociada a un conjunto de disponibilidad, debe asociar el equilibrador de carga interno o la dirección IP pública de la misma SKU que la del equilibrador de carga interno o la dirección IP pública de la otra máquina virtual del conjunto de disponibilidad. Si no lo hace, podría producirse un error de conmutación por error.
