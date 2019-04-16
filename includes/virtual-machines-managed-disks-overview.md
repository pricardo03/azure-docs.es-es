---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/11/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 4563bb0de688a8328796badffc8a133a33f64fcc
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2019
ms.locfileid: "59566553"
---
## <a name="benefits-of-managed-disks"></a>Ventajas de los discos administrados

Vamos a examinar algunas de las ventajas que se obtienen al usar discos administrados.

### <a name="highly-durable-and-available"></a>Mayor durabilidad y disponibilidad

Los discos administrados están diseñados para ofrecer una disponibilidad del 99,999 %. Para conseguir esto, proporcionan tres réplicas de los datos, lo que permite una gran durabilidad. Si una o incluso dos réplicas experimentan problemas, las réplicas restantes garantizan la persistencia de los datos y una gran tolerancia a errores. Esta arquitectura ha contribuido a que Azure destaque en el sector por ofrecer, de manera constante, durabilidad de nivel empresarial para discos de infraestructura como servicio (IaaS), con una tasa de error anualizada del 0 %.

### <a name="simple-and-scalable-vm-deployment"></a>La implementación de las máquina virtuales es simple y escalable

Con discos administrados, puede crear hasta 50 000 **discos** de máquina virtual de un tipo en una suscripción por región, lo que le permite crear miles de **máquinas virtuales** en una sola suscripción. Esta característica también aumenta la escalabilidad de los [conjuntos de escalado de máquina virtual](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md), ya que permite crear hasta 1000 máquinas virtuales en un conjunto de escalado de máquina virtual con una imagen de Marketplace.

### <a name="integration-with-availability-sets"></a>Integración con conjuntos de disponibilidad

Los discos administrados se integran con conjuntos de disponibilidad para garantizar que los discos de [máquinas virtuales de un conjunto de disponibilidad](../articles/virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) están suficientemente aislados entre sí para evitar un único punto de error. Los discos se colocan automáticamente en diferentes unidades de escalado de almacenamiento (marcas). Si se produce un error en una marca debido a un error de hardware o software, solo dejarán de funcionar las instancias de máquina virtual con discos de dichas marcas. Por ejemplo, suponga que tiene una aplicación que se ejecuta en cinco máquinas virtuales y estas están en un conjunto de disponibilidad. No todos los discos de dichas máquinas virtuales se almacenarán en la misma marca, por lo que, si se produce un error en una, no se detiene la ejecución de las restantes instancias de la aplicación.

### <a name="integration-with-availability-zones"></a>Integración con las zonas de disponibilidad

Admite discos de Managed [zonas de disponibilidad](../articles/availability-zones/az-overview.md), que es una oferta de alta disponibilidad que protege sus aplicaciones frente a errores de centro de datos. Las zonas de disponibilidad son ubicaciones físicas exclusivas dentro de una región de Azure. Cada zona de disponibilidad consta de uno o varios centros de datos equipados con alimentación, refrigeración y redes independientes. Para garantizar la resistencia, hay tres zonas independientes como mínimo en todas las regiones habilitadas. Con las zonas de disponibilidad, Azure ofrece el mejor Acuerdo de Nivel de Servicio del sector de tiempo de actividad de máquina virtual, con un 99,99 %.

### <a name="azure-backup-support"></a>Soporte técnico de Azure Backup

Para protegerse frente a desastres regionales, se puede usar [Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md) para crear un trabajo de copia de seguridad con copias de seguridad basadas en el tiempo y directivas de retención de copia de seguridad. Esto le permite realizar restauraciones sencillas de máquinas virtuales a voluntad. Actualmente, Azure Backup admite tamaños de disco de hasta cuatro tebibytes (TiB). Para más información, consulte [Uso de máquinas virtuales de disco administrado con Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

### <a name="granular-access-control"></a>Control de acceso pormenorizado

Puede usar el [control de acceso basado en rol de Azure (RBAC)](../articles/role-based-access-control/overview.md) para asignar a uno o varios usuarios permisos concretos a un disco administrado. Los discos administrados exponen varias operaciones, entre las que se incluyen la lectura, la escritura (creación o actualización), la eliminación y la recuperación de un [identificador URI de la firma de acceso compartido (SAS)](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md) para el disco. Puede conceder acceso solo a las operaciones necesarias para que una persona pueda realizar su trabajo. Por ejemplo, si no desea que una persona copie un disco administrado a una cuenta de almacenamiento, puede decidir no conceder acceso a la acción de exportación de dicho disco administrado. De igual forma, si no desea que una persona use URI de SAS para copiar un disco administrado, puede elegir no conceder dicho permiso al disco administrado.

## <a name="disk-roles"></a>Roles de disco

Hay tres funciones principales de disco en Azure: el disco de datos, el disco del sistema operativo y el disco temporal. Estos roles se asignan a los discos que están conectados a la máquina virtual.

![Roles de disco en acción](media/virtual-machines-managed-disks-overview/disk-types.png)

### <a name="data-disk"></a>Disco de datos

Un disco de datos es un disco administrado que se asocia a una máquina virtual para almacenar datos de aplicaciones u otros datos que necesita mantener. Los discos de datos se registran como unidades SCSI y se etiquetan con una letra elegida por usted. Cada disco de datos tiene una capacidad máxima de 32.767 gibibytes (GiB). El tamaño de la máquina virtual determina cuántos discos de datos puede conectar y el tipo de almacenamiento que puede usar para hospedar los discos.

### <a name="os-disk"></a>Disco del sistema operativo

Cada máquina virtual tiene un disco de sistema operativo acoplado. Ese disco del sistema operativo tiene un sistema operativo instalado previamente, que se seleccionó cuando se creó la máquina virtual.

Su capacidad máxima es de 2048 GiB.

### <a name="temporary-disk"></a>Disco temporal

Cada máquina virtual contiene un disco temporal, que no es un disco administrado. El disco temporal proporciona almacenamiento a corto plazo para aplicaciones y procesos, y está destinado únicamente a almacenar datos como archivos de paginación o de intercambio. Datos en el disco temporal pueden perderse durante un [evento de mantenimiento](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) eventos o cuando se [volver a implementar una máquina virtual](../articles/virtual-machines/troubleshooting/redeploy-to-new-node-windows.md?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json). Durante un reinicio estándar correcto de la máquina virtual, se conservarán los datos de la unidad temporal.

## <a name="managed-disk-snapshots"></a>Instantáneas de disco administrado

Una instantánea de disco administrado es una copia completa de solo lectura de un disco administrado que, de forma predeterminada, se almacena como disco administrado estándar. Con las instantáneas, puede realizar una copia de seguridad de sus discos administrados en cualquier momento. Estas instantáneas existen independientemente del disco de origen y se pueden usar para crear discos administrados. Se facturan según el tamaño usado. Por ejemplo, si crea una instantánea de un disco administrado con capacidad aprovisionada de 64 GiB y el tamaño de datos usado real es de 10 GiB, solo se le cobra por el tamaño de datos usado de 10 GiB.  

Para más información sobre cómo crear instantáneas con discos administrados, consulte los siguientes recursos:

* [Creación de una copia del disco duro virtual que se almacene como un disco administrado mediante instantáneas en Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Creación de una copia del disco duro virtual que se almacene como un disco administrado mediante instantáneas en Linux](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)

### <a name="images"></a>Imágenes

Los discos administrados también admiten la creación de una imagen personalizada administrada. Puede crear una imagen desde un disco duro virtual personalizado en una cuenta de almacenamiento, o bien directamente desde una máquina virtual generalizada (con Sysprep). Este proceso captura una imagen única. Esta imagen contiene todos los discos administrados asociados con una máquina virtual, lo que incluye tanto el disco de datos como el del sistema operativo. Esta imagen personalizada administrada permite crear cientos de máquinas virtuales con la imagen personalizada sin necesidad de copiar o administrar cuentas de almacenamiento.

Para más información sobre la creación de imágenes, consulte los artículos siguientes:

* [How to capture a managed image of a generalized VM in Azure](../articles/virtual-machines/windows/capture-image-resource.md) (Captura de una imagen administrada de una máquina virtual generalizada en Azure)
* [How to generalize and capture a Linux virtual machine using the Azure CLI 2.0](../articles/virtual-machines/linux/capture-image.md) (Procedimiento de generalización y captura de una máquina virtual Linux con la CLI de Azure 2.0)

#### <a name="images-versus-snapshots"></a>Imágenes frente a instantáneas

Es importante comprender la diferencia entre imágenes e instantáneas. Con los discos administrados, es posible tomar una imagen de una máquina virtual generalizada que se ha desasignado. Esta imagen incluye todos los discos asociados a la máquina virtual. La imagen se puede usar para crear una máquina virtual e incluye todos los discos.

Una instantánea es una copia de un disco en el momento dado en que se toma. Se aplica solo a un disco. Si tiene una máquina virtual con un solo disco (el del sistema operativo), puede tomar una instantánea o una imagen de él y crear una máquina virtual a partir de cualquiera de ellas.

Una instantánea no tiene información sobre ningún disco, excepto el que contiene. Como consecuencia, resulta problemático usarla en escenarios que requieren la coordinación de varios discos, como la creación de bandas. Las instantáneas deben poderse coordinar entre sí y esta funcionalidad no se admite actualmente.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los tipos de disco individuales que ofrece Azure y cuál es una buena elección para sus necesidades en nuestro artículo sobre los tipos de disco.
