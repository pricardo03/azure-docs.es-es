---
title: Compatibilidad de Azure para máquinas virtuales de generación 2 (versión preliminar) | Microsoft Docs
description: Información general de compatibilidad de Azure para máquinas virtuales de generación 2
services: virtual-machines-windows
documentationcenter: ''
author: laurenhughes
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 05/23/2019
ms.author: lahugh
ms.openlocfilehash: fd794662ef41112cb04bdfde087253c8abdb6983
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70079377"
---
# <a name="support-for-generation-2-vms-preview-on-azure"></a>Compatibilidad para máquinas virtuales de generación 2 (versión preliminar) en Azure

> [!IMPORTANT]
> La compatibilidad de Azure para máquinas virtuales de generación 2 está actualmente en versión preliminar.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para obtener más información, consulte [Términos de uso complementarios de las Versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

La compatibilidad para las máquinas virtuales (VM) de generación 2 ahora está disponible en Azure en versión preliminar. No se puede cambiar la generación de una máquina virtual después de haberla creado, así que revise las consideraciones de esta página antes de elegir una generación. 

Las máquinas virtuales de generación 2 admiten características clave que no se admiten en las VM de generación 1. Estas características incluyen una memoria mayor, Intel Software Guard Extensions (SGX Intel) y memoria persistente virtualizada (vPMEM). Las VM de generación 2 también tienen algunas características que aún no se admiten en Azure. Para obtener más información, consulte la sección [Características y funcionalidades](#features-and-capabilities).

Las VM de generación 2 usan la nueva arquitectura de arranque basado en UEFI en lugar de la arquitectura basada en BIOS que utilizan las VM de generación 1. En comparación con las VM de generación 1, es posible las de generación 2 tengan tiempos de arranque e instalación mejorados. Para obtener una visión general de las VM de generación 2 y algunas de las diferencias entre la generación 1 y la generación 2, consulte [¿Debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)

## <a name="generation-2-vm-sizes"></a>Tamaños de VM de generación 2

Las VM de generación 1 son compatibles con todos los tamaños de máquina virtual en Azure. Azure ahora ofrece compatibilidad de generación 2 en vista previa para las siguientes series de VM seleccionadas:

* [Serie B](https://docs.microsoft.com/azure/virtual-machines/windows/b-series-burstable)
* [Serie Dsv2](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv2-series) y [serie Dsv3](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv3-series-1)
* [Serie Esv3](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#esv3-series)
* [Serie Fsv2](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-compute#fsv2-series-1)
* [Serie GS](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-previous-gen#gs-series)
* [Serie Ls](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-previous-gen#ls-series) y [serie Lsv2](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-storage#lsv2-series)
* [Serie Mv2](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#mv2-series)
* [Serie NCv2](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu#ncv2-series) y [serie NCv3](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu#ncv3-series)
* [Serie ND](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu#nd-series)

## <a name="generation-2-vm-images-in-azure-marketplace"></a>Imágenes de VM de generación 2 en Azure Marketplace

Las VM de generación 2 admiten las siguientes imágenes de Marketplace:

* Windows Server 2019 Datacenter
* Windows Server 2016 Datacenter
* Windows Server 2012 R2 Datacenter
* Windows Server 2012 Datacenter

## <a name="on-premises-vs-azure-generation-2-vms"></a>Máquinas virtuales locales frente a VM de generación 2 de Azure

Actualmente Azure no admite algunas de las características que admite Hyper-V en el entorno local para VM de generación 2.

| Características de la generación 2                | Hyper-V local | Azure |
|-------------------------------------|---------------------|-------|
| Arranque seguro                         | :heavy_check_mark:  | :x:   |
| VM Blindada                         | :heavy_check_mark:  | :x:   |
| vTPM                                | :heavy_check_mark:  | :x:   |
| Seguridad basada en virtualización (VBS) | :heavy_check_mark:  | :x:   |
| Formato VHDX                         | :heavy_check_mark:  | :x:   |

## <a name="features-and-capabilities"></a>Características y funcionalidades

### <a name="generation-1-vs-generation-2-features"></a>Características de la generación 1 frente a la generación 2

| Característica | Generación 1 | Generación 2 |
|---------|--------------|--------------|
| Arranque             | PCAT                      | UEFI                               |
| Controladoras de disco | IDE                       | SCSI                               |
| Tamaños de VM         | Todos los tamaños de VM | Solo las VM que admiten Premium Storage |

### <a name="generation-1-vs-generation-2-capabilities"></a>Funcionalidades de la generación 1 frente a la generación 2

| Capacidad | Generación 1 | Generación 2 |
|------------|--------------|--------------|
| Disco de SO > 2 TB                    | :x:                        | :heavy_check_mark: |
| Disco personalizado/imagen/intercambiar SO         | :heavy_check_mark:         | :heavy_check_mark: |
| Compatibilidad con los conjuntos de escalado de máquinas virtuales | :heavy_check_mark:         | :heavy_check_mark: |
| Copia de seguridad/ASR                        | :heavy_check_mark:         | :x:                |
| Galería de imágenes compartidas              | :heavy_check_mark:         | :x:                |
| Azure Disk Encryption             | :heavy_check_mark:         | :x:                |

## <a name="creating-a-generation-2-vm"></a>Creación de una VM de generación 2

### <a name="marketplace-image"></a>Imagen de Marketplace

En Azure Portal o la CLI de Azure, puede crear VM de generación 2 a partir de una imagen de Marketplace que admita el arranque UEFI.

La oferta de `windowsserver-gen2preview` contiene solo las imágenes de generación 2 de Windows. Este paquete evita la confusión entre las imágenes de la generación 1 y la generación 2. Para crear una VM de generación 2, seleccione **Imágenes** desde esta oferta y siga el proceso estándar para crear la máquina virtual.

Actualmente, Marketplace ofrece las siguientes imágenes de la generación 2 de Windows:

* 2019-datacenter-gen2
* 2016-datacenter-gen2
* 2012-r2-datacenter-gen2
* 2012-datacenter-gen2

Consulte la sección [Características y funcionalidades](#features-and-capabilities) para obtener una lista de imágenes compatibles de Marketplace.

### <a name="managed-image-or-managed-disk"></a>Imagen administrada o disco administrado

Puede crear una VM de generación 2 desde una imagen administrada o un disco administrado de la misma manera que crearía una VM de generación 1.

### <a name="virtual-machine-scale-sets"></a>Conjuntos de escalado de máquinas virtuales

También puede crear VM de generación 2 usando conjuntos de escalado de VM. En la CLI de Azure, use los conjuntos de escalado de Azure para crear VM de generación 2.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

* **¿Están disponibles las máquinas virtuales de generación 2 en todas las regiones de Azure?**  
    Sí. Pero no todos los [tamaños de VM de generación 2](#generation-2-vm-sizes) están disponibles en todas las regiones. La disponibilidad de las VM de generación 2 depende de la disponibilidad del tamaño de máquina virtual.

* **¿Hay diferencia de precio entre las VM de generación 1 y de generación 2?**  
   No.

* **¿Cómo se puede aumentar el tamaño del disco del SO?**  
  Los discos del SO mayores de 2 TB son nuevos en las máquinas virtuales de generación 2. De forma predeterminada, los discos del SO son menores que 2 TB para las máquinas virtuales de generación 2. Puede aumentar el tamaño del disco hasta un máximo recomendado de 4 TB. Use la CLI de Azure o Azure Portal para aumentar el tamaño del disco del SO. Para obtener información sobre cómo expandir los discos mediante programación, vea [Cómo ampliar la unidad de sistema operativo de una máquina virtual](expand-os-disk.md).

  Para aumentar el tamaño del disco del SO desde Azure Portal:

  1. En Azure Portal, vaya a la página de propiedades de la máquina virtual.
  1. Para apagar y desasignar la VM, haga clic en el botón **Detener**.
  1. En la sección **Discos**, seleccione el disco del SO que quiere aumentar.
  1. En la sección **Discos**, seleccione **Configuración** y actualice el **Tamaño** con el valor que quiera.
  1. Vuelva a la página de propiedades de la máquina virtual e **inicie** la VM.
  
  Es posible que vea una advertencia para los discos del SO mayores de 2 TB. La advertencia no se aplica a las máquinas virtuales de generación 2. Pero *no se recomiendan* los tamaños de disco del SO de más de 4 TB.

* **¿Las VM de generación 2 admiten las redes aceleradas?**  
    Sí. Para más información vea [Creación de una máquina virtual con redes aceleradas](../../virtual-network/create-vm-accelerated-networking-cli.md).

* **¿Admite la generación 2 VHDX?**  
    No, las VM de generación 2 solo admiten VHD.

* **¿Las máquinas virtuales de generación 2 admiten Almacenamiento en disco Ultra de Azure?**  
    Sí.

* **¿Puedo migrar una VM de generación 1 a la generación 2?**  
    No, no se puede cambiar la generación de una VM después de crearla. Si tiene que cambiar entre varias generaciones de VM, cree una nueva máquina virtual de otra generación.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre [las VM de generación 2 en Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

* Obtenga información sobre cómo [preparar un disco duro virtual](prepare-for-upload-vhd-image.md) para cargar desde sistemas locales a Azure.
