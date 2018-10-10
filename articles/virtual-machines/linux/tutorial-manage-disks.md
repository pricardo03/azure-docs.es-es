---
title: 'Tutorial: Administración de discos de Azure con la CLI de Azure | Microsoft Docs'
description: En este tutorial, aprenderá a usar la CLI de Azure para crear y administrar discos de Azure para máquinas virtuales
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/30/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 04fad24b17d7f74211deae53c0d044f2049660f2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978325"
---
# <a name="tutorial---manage-azure-disks-with-the-azure-cli"></a>Tutorial: Administración de discos de Azure con la CLI de Azure

Las máquinas virtuales (VM) de Azure usan discos para almacenar el sistema operativo, las aplicaciones y los datos. Cuando se crea una máquina virtual es importante elegir un tamaño de disco y la configuración adecuada para la carga de trabajo esperada. En este tutorial se muestra cómo implementar y administrar los discos de una máquina virtual. Aprenderá sobre los siguientes temas:

> [!div class="checklist"]
> * Discos del SO y temporales
> * Discos de datos
> * Discos Estándar y Premium
> * Rendimiento de disco
> * Conectar y preparar los discos de datos
> * Cambiar el tamaño de los discos
> * Instantáneas de disco

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, en este tutorial es preciso que ejecute la CLI de Azure de la versión 2.0.30, u otra posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="default-azure-disks"></a>Discos de Azure predeterminados

Cuando se crea una máquina virtual de Azure, se conectan dos discos automáticamente a la máquina virtual.

**Disco del sistema operativo**: el tamaño de los discos del sistema operativo puede llegar a los 2 TB y hospedan el sistema operativo de las máquinas virtuales. El disco del sistema operativo lleva de forma predeterminada la etiqueta */dev/sda* . La configuración de almacenamiento en caché del disco del sistema operativo está optimizada para el rendimiento del sistema operativo. Debido a esta configuración, el disco del sistema operativo **no se debe** usar para aplicaciones o datos. Para aplicaciones y datos, use discos de datos, que se explican más adelante en este tutorial.

**Disco temporal**: los discos temporales usan una unidad de estado sólido que se encuentra en el mismo host de Azure que la máquina virtual. Los discos temporales son muy eficiente y se pueden usar para operaciones tales como el procesamiento temporal de los datos. Sin embargo, si la máquina virtual se mueve a un nuevo host, los datos almacenados en un disco temporal se eliminarán. El tamaño del disco temporal se determina por el tamaño de la máquina virtual. Los discos temporales llevan la etiqueta */dev/sdb* y tienen un punto de montaje de */mnt*.

### <a name="temporary-disk-sizes"></a>Tamaños de disco temporal

| Escriba | Tamaños comunes | Tamaño máximo de disco temporal (GiB) |
|----|----|----|
| [Uso general](sizes-general.md) | Series A, B y D | 1600 |
| [Proceso optimizado](sizes-compute.md) | Serie F | 576 |
| [Memoria optimizada](sizes-memory.md) | Series D, E, G y M | 6144 |
| [Almacenamiento optimizado](sizes-storage.md) | Serie L | 5630 |
| [GPU](sizes-gpu.md) | Serie N | 1440 |
| [Alto rendimiento](sizes-hpc.md) | Series A y H | 2000 |

## <a name="azure-data-disks"></a>Discos de datos de Azure

Para instalar aplicaciones y almacenar datos, se pueden agregar más discos de datos. Los discos de datos deben usarse en cualquier situación donde desee un almacenamiento de datos duradero y con capacidad de respuesta. Cada disco de datos tiene una capacidad máxima de 4 TB. El tamaño de la máquina virtual determina cuántos discos de datos se pueden conectar a una máquina virtual. Para cada vCPU de la máquina virtual, se pueden asociar dos discos de datos.

### <a name="max-data-disks-per-vm"></a>Discos de datos máximos por máquina virtual

| Escriba | Tamaño de VM | Discos de datos máximos por máquina virtual |
|----|----|----|
| [Uso general](sizes-general.md) | Series A, B y D | 64 |
| [Proceso optimizado](sizes-compute.md) | Serie F | 64 |
| [Memoria optimizada](../virtual-machines-windows-sizes-memory.md) | Series D, E y G | 64 |
| [Almacenamiento optimizado](../virtual-machines-windows-sizes-storage.md) | Serie L | 64 |
| [GPU](sizes-gpu.md) | Serie N | 64 |
| [Alto rendimiento](sizes-hpc.md) | Series A y H | 64 |

## <a name="vm-disk-types"></a>Tipos de disco de máquina virtual

Azure proporciona dos tipos de disco.

### <a name="standard-disk"></a>Disco estándar

Standard Storage está respaldado por unidades de disco duro y ofrece un almacenamiento rentable al mismo tiempo que tiene un rendimiento superior. Los discos estándar son ideales para cargas de trabajo de desarrollo y prueba rentables.

### <a name="premium-disk"></a>Disco Premium

Los discos Premium están respaldados por un disco de latencia reducida y alto rendimiento basado en SSD. Es perfecto para máquinas virtuales que ejecutan cargas de trabajo de producción. Premium Storage es compatible con las máquinas virtuales de las series DS, DSv2, GS y FS. Al seleccionar el tamaño de un disco, el valor se redondea al alza al siguiente tipo. Por ejemplo, si el tamaño del disco es inferior a 128 GB, el tipo de disco es P10. Si el tamaño de disco está entre 129 y 512 GB, el tamaño es un P20. Cualquier tamaño por encima de 512 GB equivale a un tipo P30.

### <a name="premium-disk-performance"></a>Rendimiento del disco Premium

|Tipo de disco de Premium Storage | P4 | P6 | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Tamaño del disco (redondeo hacia arriba) | 32 GB | 64 GB | 128 GB | 512 GB | 1024 GB (1 TB) | 2048 GB (2 TB) | 4095 GB (4 TB) |
| Máximo de IOPS por disco | 120 | 240 | 500 | 2,300 | 5.000 | 7500 | 7500 |
Rendimiento de disco | 25 MB/s | 50 MB/s | 100 MB/s | 150 MB/s | 200 MB/s | 250 MB/s | 250 MB/s |

Aunque la tabla anterior identifica las IOPS máximas por disco, se puede obtener un mayor nivel de rendimiento dividiendo varios discos de datos. Por ejemplo, una máquina virtual Standard_GS5 puede conseguir 80 000 IOPS como máximo. Para más información sobre el número máximo de IOPS por máquina virtual, consulte los [tamaños de máquinas virtuales Linux](sizes.md).

## <a name="create-and-attach-disks"></a>Creación y conexión de discos

Los discos de datos se pueden crear y conectar en el momento de creación de la máquina virtual o a una máquina virtual existente.

### <a name="attach-disk-at-vm-creation"></a>Conexión del disco en el momento de creación de la máquina virtual

Cree un grupo de recursos con el comando [az group create](/cli/azure/group#az-group-create).

```azurecli-interactive
az group create --name myResourceGroupDisk --location eastus
```

Cree una máquina virtual mediante el comando [az vm create](/cli/azure/vm#az-vm-create). En el ejemplo siguiente, se crea una máquina virtual denominada *myVM*, se agrega una cuenta de usuario denominada *azureuser* y se generan claves SSH, si no existen. El argumento `--datadisk-sizes-gb` se usa para especificar que se debe crear y conectar un disco adicional a la máquina virtual. Para crear y conectar más de un disco, use una lista delimitada por espacios de valores de tamaño de disco. En el ejemplo siguiente, se crea una máquina virtual con dos discos de datos, ambos de 128 GB. Dado que los tamaños de disco son de 128 GB, estos discos se configuran ambos como P10s, que proporcionan el número máximo de 500 IOPS por disco.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --admin-username azureuser \
  --generate-ssh-keys \
  --data-disk-sizes-gb 128 128
```

### <a name="attach-disk-to-existing-vm"></a>Conexión del disco a máquina virtual existente

Para crear y conectar un nuevo disco a una máquina virtual existente, use el comando [az vm disk attach](/cli/azure/vm/disk#az-vm-disk-attach). En el ejemplo siguiente se crea un disco Premium con un tamaño de 128 gigabytes y se conecta a la máquina virtual que creó en el último paso.

```azurecli-interactive
az vm disk attach \
    --resource-group myResourceGroupDisk \
    --vm-name myVM \
    --disk myDataDisk \
    --size-gb 128 \
    --sku Premium_LRS \
    --new
```

## <a name="prepare-data-disks"></a>Preparación de los discos de datos

Después de que se ha conectado un disco a la máquina virtual, es necesario configurar el sistema operativo para usar el disco. En el ejemplo siguiente se muestra cómo configurar manualmente un disco. Este proceso también se puede automatizar mediante cloud-init, que se trata en un [tutorial posterior](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>Configuración manual

Cree una conexión SSH con la máquina virtual. Reemplace la dirección IP de ejemplo por la dirección IP pública de la máquina virtual:

```azurecli-interactive
ssh azureuser@52.174.34.95
```

Cree particiones del disco con `fdisk`.

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

Escriba un sistema de archivos en la partición con el comando `mkfs`.

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Monte el disco nuevo para que sea accesible en el sistema operativo.

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

Ahora se puede acceder al disco mediante el punto de montaje *datadrive*, que se puede comprobar con el comando `df -h`.

```bash
df -h
```

El resultado muestra la nueva unidad montada en */datadrive*.

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

Para asegurarse de que la unidad se vuelve a montar tras un reinicio, se debe agregar al archivo */etc/fstab*. Para ello, obtenga el UUID del disco con la utilidad `blkid`.

```bash
sudo -i blkid
```

El resultado muestra el UUID de la unidad, en este caso `/dev/sdc1`.

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

Agregue una línea similar a la siguiente al archivo */etc/fstab*.

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail   1  2
```

Ahora que se ha configurado el disco, cierre la sesión de SSH.

```bash
exit
```

## <a name="resize-vm-disk"></a>Cambio del tamaño de disco de una máquina virtual

Una vez que se ha implementado una máquina virtual, se puede aumentar el tamaño del disco del sistema operativo o de los discos de datos conectados. El aumento del tamaño de un disco resulta beneficioso cuando se necesita más espacio de almacenamiento o un nivel mayor de rendimiento (como P10, P20 o P30). No se puede reducir el tamaño de los discos.

Antes de aumentar el tamaño de un disco, se necesita el identificador o el nombre del mismo. Use el comando [az disk list](/cli/azure/disk#az-disk-list) para devolver todos los discos de un grupo de recursos. Anote el nombre del disco que quiere cambiar de tamaño.

```azurecli-interactive
az disk list \
    --resource-group myResourceGroupDisk \
    --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
    --output table
```

La máquina virtual se debe desasignar. Use el comando [az vm deallocate](/cli/azure/vm#az-vm-deallocate) para detener y desasignar la máquina virtual.

```azurecli-interactive
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

Use el comando [az disk update](/cli/azure/vm/disk#az-vm-disk-update) para cambiar el tamaño del disco. En este ejemplo se cambia el tamaño de un disco llamado *myDataDisk* a 1 terabyte.

```azurecli-interactive
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

Cuando se haya completado la operación de cambio de tamaño, inicie la máquina virtual.

```azurecli-interactive
az vm start --resource-group myResourceGroupDisk --name myVM
```

Si cambia el tamaño del disco del sistema operativo, la partición se expande automáticamente. Si cambia el tamaño de un disco de datos, todas las particiones actuales se deben expandir en el sistema operativo de las máquinas virtuales.

## <a name="snapshot-azure-disks"></a>Captura de instantáneas de discos de Azure

Cuando se toma una instantánea de un disco, Azure crea una copia de solo lectura y de un momento dado del disco. Las instantáneas de máquina virtual de Azure resultan útiles para guardar rápidamente el estado de una máquina virtual antes de realizar cambios en la configuración. En caso de que los cambios de configuración demostraran no ser los deseados, se puede restaurar el estado de la máquina virtual mediante una instantánea. Cuando una máquina virtual tiene más de un disco, se toma una instantánea de cada uno con independencia de los demás. Para realizar copias de seguridad coherentes con la aplicación, considere la posibilidad de detener la máquina virtual antes de tomar instantáneas de disco. Como alternativa, use el [servicio Azure Backup](/azure/backup/), que permite realizar copias de seguridad automatizadas mientras se ejecuta la máquina virtual.

### <a name="create-snapshot"></a>Creación de una instantánea

Antes de crear una instantánea de un disco de máquina virtual, se necesitan el identificador o el nombre del disco. Use el comando [az vm show](/cli/azure/vm#az-vm-show) para devolver el identificador del disco. En este ejemplo, el identificador del disco se almacena en una variable para que se pueda usar en un paso posterior.

```azurecli-interactive
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Ahora que tiene el identificador del disco de la máquina virtual, el siguiente comando crea una instantánea del disco.

```azurcli
az snapshot create \
    --resource-group myResourceGroupDisk \
    --source "$osdiskid" \
    --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a>Creación del disco a partir de la instantánea

Esta instantánea se puede convertir en un disco, que se puede usar para volver a crear la máquina virtual.

```azurecli-interactive
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a>Restauración de la máquina virtual a partir de la instantánea

Para demostrar la recuperación de la máquina virtual, elimine la máquina virtual existente.

```azurecli-interactive
az vm delete --resource-group myResourceGroupDisk --name myVM
```

Cree una nueva máquina virtual en el disco de instantáneas.

```azurecli-interactive
az vm create \
    --resource-group myResourceGroupDisk \
    --name myVM \
    --attach-os-disk mySnapshotDisk \
    --os-type linux
```

### <a name="reattach-data-disk"></a>Reconexión de un disco de datos

Todos los discos de datos se deben volver a conectar a la máquina virtual.

En primer lugar, busque el nombre del disco de datos mediante el comando [az disk list](/cli/azure/disk#az-disk-list). En este ejemplo se coloca el nombre del disco en una variable denominada *datadisk*, que se usa en el paso siguiente.

```azurecli-interactive
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

Use el comando [az vm disk attach](/cli/azure/vm/disk#az-vm-disk-attach) para adjuntar el disco.

```azurecli-interactive
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido sobre temas relacionados con los discos de máquina virtual; por ejemplo:

> [!div class="checklist"]
> * Discos del SO y temporales
> * Discos de datos
> * Discos Estándar y Premium
> * Rendimiento de disco
> * Conectar y preparar los discos de datos
> * Cambiar el tamaño de los discos
> * Instantáneas de disco

Siga con el siguiente tutorial para aprender sobre la automatización de la configuración de la máquina virtual.

> [!div class="nextstepaction"]
> [Automatización de la configuración de máquinas virtuales](./tutorial-automate-vm-deployment.md)
