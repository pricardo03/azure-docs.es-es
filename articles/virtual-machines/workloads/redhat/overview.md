---
title: Información general sobre cargas de trabajo de Red Hat en Azure | Microsoft Docs
description: Más información sobre las ofertas de productos de Red Hat disponibles en Azure
services: virtual-machines-linux
author: asinn826
manager: borisb2015
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: overview
ms.date: 02/10/2020
ms.author: alsin
ms.openlocfilehash: daba49e6861eb67fd07c6fcf618b2b2d6cdd8c89
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77133818"
---
# <a name="red-hat-workloads-on-azure"></a>Cargas de trabajo de Red Hat en Azure
Las cargas de trabajo de Red Hat se admiten mediante diversas ofertas de Azure. Las imágenes de Red Hat Enterprise Linux (RHEL) son el núcleo de las cargas de trabajo de RHEL, como Red Hat Update Infrastructure (RHUI).

## <a name="red-hat-enterprise-linux-rhel-images"></a>Imágenes Red Hat Enterprise Linux (RHEL)
Azure ofrece una amplia oferta de imágenes de RHEL en Azure. Estas imágenes están disponibles a través de dos modelos de licencia diferentes: Pago por uso (PAYG) y "Bring Your Own Subscription" (BYOS). Las nuevas imágenes de RHEL en Azure se publican cuando se crean nuevas versiones de RHEL y se actualizan a lo largo de sus ciclos de vida según sea necesario.

### <a name="pay-as-you-go-payg-images"></a>Imágenes de Pago por uso
Azure ofrece una amplia variedad de imágenes de Pago por uso de RHEL. Estas imágenes están correctamente autorizadas para RHEL y se adjuntan a un origen de actualizaciones (Red Hat Update Infrastructure). Estas imágenes cobrarán un precio prémium por el derecho y las actualizaciones de RHEL. Las variantes de imagen de Pago por uso de RHEL incluyen:
* RHEL estándar
* RHEL for SAP
* RHEL for SAP con alta disponibilidad y servicios de actualización.

Puede que desee usar las imágenes de Pago por uso si no desea preocuparse por el pago por separado por el número de suscripciones adecuado.

### <a name="red-hat-gold-images-rhel-byos"></a>Imágenes de Red Hat Gold (`rhel-byos`)
Azure también ofrece imágenes de Red Hat Gold. Estas imágenes pueden ser útiles para los clientes que tienen suscripciones de Red Hat existentes y desean usarlas en Azure. Debe habilitar las suscripciones de Red Hat existentes para Red Hat Cloud Access antes de poder usarlas en Azure. El acceso a estas imágenes se concede automáticamente cuando las suscripciones de Red Hat están habilitadas para el acceso a la nube y cumplen los requisitos de idoneidad. El uso de estas imágenes permite a un cliente evitar la facturación doble, lo que puede incurrir en el uso de las imágenes de Pago por uso.
* [Obtenga información sobre cómo habilitar las suscripciones de Red Hat para el acceso a la nube con Azure](https://access.redhat.com/documentation/en-us/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/con-enable-subs).
* [Obtenga información sobre cómo buscar imágenes de Red Hat Gold en Azure Portal, la CLI o el cmdlet de PowerShell](./byos.md).

> [!NOTE]
> Nota sobre la doble facturación: La facturación doble se produce cuando un usuario paga dos veces para las suscripciones RHEL. Esto suele suceder cuando un cliente utiliza el administrador de suscripciones para adjuntar un derecho en una máquina virtual de Pago por uso de RHEL. Por ejemplo, un cliente que usa el administrador de suscripciones para adjuntar un derecho a los paquetes de SAP en una imagen de RHEL de Pago por uso se facturará de forma indirecta, ya que se pagará dos veces por RHEL, una vez mediante la tarifa prémium de Pago por uso y una vez con la suscripción de SAP. Esto no ocurrirá a los usuarios de imágenes BYOS.

### <a name="generation-2-images"></a>Imágenes de segunda generación
Las máquinas virtuales de segunda generación proporcionan algunas características más recientes, en comparación con las máquinas virtuales de la primera generación. Los detalles se incluyen en la [documentación de la segunda generación](https://docs.microsoft.com/azure/virtual-machines/linux/generation-2). La principal diferencia desde la perspectiva de una imagen de Red Hat Enterprise Linux es que las máquinas virtuales de segunda generación usan una UEFI, en lugar de la interfaz de firmware de BIOS, y usan una tabla de particiones GUID (GPT), en lugar de un registro de arranque maestro (MBR) en el momento del arranque. Esto permite, entre otras cosas, tamaños de disco de sistema operativo superiores a 2 TB. Además, las [máquinas virtuales de la serie Mv2](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory#mv2-series) solo se ejecutan en imágenes de segunda generación.

Las imágenes de segunda generación de Red Hat Enterprise Linux están disponibles en Marketplace. Busque "gen2" en la SKU de la imagen al enumerar todas las imágenes mediante la CLI de Azure y vaya a la pestaña "Opciones avanzadas" en el proceso de implementación de la máquina virtual para implementar una máquina virtual de segunda generación.

## <a name="red-hat-update-infrastructure-rhui"></a>Red Hat Update Infrastructure (RHUI)
Azure proporciona la infraestructura Red Hat Update Infrastructure solo para las máquinas virtuales de RHEL de Pago por uso. RHUI es, en realidad, un reflejo de las redes CDN de Red Hat, pero solo es accesible para las máquinas virtuales de RHEL de Pago por uso de Azure. Tendrá acceso a los paquetes correspondientes en función de la imagen RHEL que haya implementado. Por ejemplo, una imagen de RHEL para SAP tendrá acceso a los paquetes SAP además de a los paquetes base de RHEL.

### <a name="rhui-update-behavior"></a>Comportamiento de actualización de RHUI
De forma predeterminada, las imágenes de RHEL conectadas a RHUI se actualizan a la versión secundaria más reciente de RHEL cuando se ejecuta `yum update`. Este comportamiento significa que una máquina virtual RHEL 7.4 puede actualizarse a RHEL 7.7 si se ejecuta una operación `yum update`. Esto es por diseño de RHUI. Sin embargo, este comportamiento de actualización se puede mitigar mediante el cambio de repositorios de RHEL normales a los [repositorios de compatibilidad de actualización extendida (EUS)](./redhat-rhui.md#rhel-eus-and-version-locking-rhel-vms).

## <a name="next-steps"></a>Pasos siguientes
* Más información sobre las [imágenes de RHEL en Azure](./redhat-images.md)
* Más información sobre la [Red Hat Update Infrastructure](./redhat-rhui.md).
* Obtenga más información sobre la oferta de [imagen de Red Hat Gold (`rhel-byos`) ](./byos.md).
