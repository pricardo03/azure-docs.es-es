---
title: Soporte técnico de Azure con máquinas virtuales de generación 2
description: Información general de compatibilidad de Azure para máquinas virtuales de generación 2
services: virtual-machines-linux
documentationcenter: ''
author: ju-shim
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 01/28/2020
ms.author: jushiman
ms.openlocfilehash: 766ac4f67c0d448f3988eb66c84dddbf44076ab5
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76841153"
---
# <a name="support-for-generation-2-vms-on-azure"></a>Compatibilidad para máquinas virtuales de generación 2 en Azure

La compatibilidad para las máquinas virtuales (VM) de generación 2 ahora está disponible en Azure. No se puede cambiar la generación de una máquina virtual después de haberla creado, así que revise las consideraciones de esta página antes de elegir una generación.

Las máquinas virtuales de generación 2 admiten características clave que no se admiten en las VM de generación 1. Estas características incluyen una memoria mayor, Intel Software Guard Extensions (SGX Intel) y memoria persistente virtualizada (vPMEM). Las VM de generación 2 que se ejecutan en el entorno local también tienen algunas características que aún no se admiten en Azure. Para obtener más información, consulte la sección [Características y funcionalidades](#features-and-capabilities).

Las VM de generación 2 usan la nueva arquitectura de arranque basado en UEFI en lugar de la arquitectura basada en BIOS que utilizan las VM de generación 1. En comparación con las VM de generación 1, es posible las de generación 2 tengan tiempos de arranque e instalación mejorados. Para obtener una visión general de las VM de generación 2 y algunas de las diferencias entre la generación 1 y la generación 2, consulte [¿Debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)

## <a name="generation-2-vm-sizes"></a>Tamaños de VM de generación 2

Las VM de generación 1 son compatibles con todos los tamaños de máquina virtual en Azure (salvo con las VM de la serie Mv2). Azure ahora ofrece compatibilidad de generación 2 para las siguientes series de VM seleccionadas:

* [Serie B](https://docs.microsoft.com/azure/virtual-machines/linux/b-series-burstable)
* [Serie DC](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general#dc-series)
* [Serie Dsv2](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general#dsv2-series) y [serie Dsv3](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general#dsv3-series-1)
* [Serie Esv3](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory#esv3-series)
* [Serie Fsv2](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-compute#fsv2-series-1)
* [Serie GS](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-previous-gen#gs-series)
* [Serie HB](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc#hb-series)
* [Serie HC](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc#hc-series)
* [Serie Ls](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-previous-gen#ls-series) y [serie Lsv2](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-storage#lsv2-series)
* [Serie Mv2](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory#mv2-series)
* [Serie NCv2](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu#ncv2-series) y [serie NCv3](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu#ncv3-series)
* [Serie ND](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu#nd-series)
* [Serie NVv3](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu#nvv3-series--1)

> [!NOTE]
> El uso de imágenes de máquina virtual de generación 2 en las máquinas virtuales de la serie Mv2 está disponible con carácter general, ya que esta serie funciona exclusivamente con imágenes de máquina virtual de generación 2. Las imágenes de máquina virtual de generación 1 no se admiten en máquinas virtuales de la serie Mv2. 

## <a name="generation-2-vm-images-in-azure-marketplace"></a>Imágenes de VM de generación 2 en Azure Marketplace

Las VM de generación 2 admiten las siguientes imágenes de Marketplace:

* Windows Server 2019, 2016, 2012 R2, 2012
* Windows 10
* SUSE Linux Enterprise Server 15 SP1
* SUSE Linux Enterprise Server 12 SP4
* Ubuntu Server 16.04, 18.04, 19.04, 19.10 
* RHEL 8.0

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
| Arranque             | PCAT         | UEFI |
| Controladoras de disco | IDE          | SCSI |
| Tamaños de VM         | Todos los tamaños de VM | Solo las VM que admiten Premium Storage |

### <a name="generation-1-vs-generation-2-capabilities"></a>Funcionalidades de la generación 1 frente a la generación 2

| Capacidad | Generación 1 | Generación 2 |
|------------|--------------|--------------|
| Disco de SO > 2 TB                    | :x:                | :heavy_check_mark: |
| Disco personalizado/imagen/intercambiar SO         | :heavy_check_mark: | :heavy_check_mark: |
| Compatibilidad con los conjuntos de escalado de máquinas virtuales | :heavy_check_mark: | :heavy_check_mark: |
| Azure Site Recovery               | :heavy_check_mark: | :heavy_check_mark: |
| Copia de seguridad y restauración                    | :heavy_check_mark: | :heavy_check_mark: |
| Galería de imágenes compartidas              | :heavy_check_mark: | :heavy_check_mark: |
| Azure Disk Encryption             | :heavy_check_mark: | :x:                |

## <a name="creating-a-generation-2-vm"></a>Creación de una VM de generación 2

### <a name="marketplace-image"></a>Imagen de Marketplace

En Azure Portal o la CLI de Azure, puede crear VM de generación 2 a partir de una imagen de Marketplace que admita el arranque UEFI.

#### <a name="azure-portal"></a>Portal de Azure

Las imágenes de la generación 2 para Windows y SLES se incluyen en la misma oferta de servidor que las imágenes de Generación 1. Lo que eso significa desde una perspectiva de flujo, es que selecciona la oferta y el SKU del portal para la máquina virtual. Si la SKU admite tanto imágenes de la generación 1 como de la generación 2, puede optar por crear una máquina virtual de la generación 2 desde la pestaña *Avanzado* en el flujo de creación de la máquina virtual.

Actualmente, las SKU siguientes admiten imágenes de la generación 1 y de la generación 2:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016
* Windows Server 2019

Al seleccionar una SKU de Windows Server como oferta, en la pestaña **Avanzado**, hay una opción para crear una máquina virtual **Generación 1** (BIOS) o **Generación 2** (UEFI). Si selecciona **Gen 2**, asegúrese de que el tamaño de la máquina virtual seleccionado en la pestaña **Datos básicos**[se admite para máquinas virtuales de la generación 2](#generation-2-vm-sizes).

![Selección de la máquina virtual Generación 1 o Generación 2](./media/generation-2/gen1-gen2-select.png)

#### <a name="powershell"></a>PowerShell

También puede usar PowerShell para crear una máquina virtual haciendo referencia directamente a la SKU de generación 1 o de generación 2.

Por ejemplo, use el siguiente cmdlet de PowerShell para obtener una lista de las SKU de la oferta `WindowsServer`.

```powershell
Get-AzVMImageSku -Location westus2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```

También puede usar la CLI de Azure para ver las imágenes de generación 2 disponibles, organizadas por **publicador**.

```azurecli
az vm image list --publisher Canonical --sku gen2 --output table --all
```

Si va a crear una máquina virtual con Windows Server 2012 como sistema operativo, seleccione la SKU de máquina virtual de generación 1 (BIOS) o de generación 2 (UEFI), que tienen un aspecto similar al siguiente:

```powershell
2012-Datacenter
2012-datacenter-gensecond
```

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

* **Tengo un archivo .vhd de mi VM de generación 2 local. ¿Puedo usar ese archivo .vhd para crear una VM de generación 2 en Azure?**
  Sí, puede llevar el archivo .vhd de generación 2 a Azure y usarlo para crear una VM de generación 2. Para ello, siga estos pasos:
    1. Cargue el archivo . vhd en una cuenta de almacenamiento en la misma región en la que quiere crear la VM.
    1. Cree un disco administrado a partir del archivo .vhd. Establezca la propiedad de generación de Hyper-V en V2. Los siguientes comandos de PowerShell establecen la propiedad de generación de Hyper-V al crear el disco administrado.

        ```powershell
        $sourceUri = 'https://xyzstorage.blob.core.windows.net/vhd/abcd.vhd'. #<Provide location to your uploaded .vhd file>
        $osDiskName = 'gen2Diskfrmgenvhd'  #<Provide a name for your disk>
        $diskconfig = New-AzDiskConfig -Location '<location>' -DiskSizeGB 127 -AccountType Standard_LRS -OsType Windows -HyperVGeneration "V2" -SourceUri $sourceUri -CreateOption 'Import'
        New-AzDisk -DiskName $osDiskName -ResourceGroupName '<Your Resource Group>' -Disk $diskconfig
        ```

    1. Una vez que el disco esté disponible, cree una VM mediante la conexión de este disco. La VM creada será de generación 2.
    Cuando se crea la VM de generación 2, puede generalizar la imagen de esta VM de forma opcional. Al generalizar la imagen, puede usarla para crear varias VM.

* **¿Cómo se puede aumentar el tamaño del disco del SO?**  
  Los discos del SO mayores de 2 TB son nuevos en las máquinas virtuales de generación 2. De forma predeterminada, los discos del SO son menores que 2 TB para las máquinas virtuales de generación 2. Puede aumentar el tamaño del disco hasta un máximo recomendado de 4 TB. Use la CLI de Azure o Azure Portal para aumentar el tamaño del disco del SO. Para obtener información sobre cómo expandir los discos mediante programación, vea [Cómo ampliar la unidad de sistema operativo de una máquina virtual](expand-disks.md).

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
