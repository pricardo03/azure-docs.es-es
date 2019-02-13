---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/24/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 212506667a56befb4e3926dec7a9e3eb9772ebed
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2019
ms.locfileid: "55736262"
---
# <a name="ultra-ssd-preview-managed-disks-for-azure-virtual-machine-workloads"></a>Managed Disks con Ultra SSD (versión preliminar) para cargas de trabajo de máquinas virtuales de Azure

Azure Ultra SSD (versión preliminar) le ofrece un alto rendimiento, IOPS elevadas y un almacenamiento en disco consistente y de baja latencia para máquinas virtuales IaaS de Azure. En esta nueva oferta se proporciona un rendimiento exclusivo que se encuentra en los mismos niveles de disponibilidad que nuestras ofertas de discos existentes. Los beneficios adicionales de Ultra SSD incluyen la capacidad de cambiar dinámicamente el rendimiento del disco junto con sus cargas de trabajo sin tener que reiniciar las máquinas virtuales. Asimismo, Ultra SSD es adecuado para cargas de trabajo con grandes cantidades de datos, como SAP HANA, bases de datos de primer nivel y cargas de trabajo que admitan muchas transacciones.

## <a name="ultra-ssd-features"></a>Características de Ultra SSD

**Discos administrados**: Los SSD Ultra solo están disponibles como discos administrados. Además, los discos Ultra SSD no se pueden implementar como un disco no administrado o un blob en páginas. Al crear el disco administrado, debe especificar el tipo de SKU del disco como Ultra SSD_LRS e indicar el tamaño del mismo, las unidades IOPS y el rendimiento que necesite; asimismo, Azure creará y administrará el disco automáticamente.  

**Máquinas virtuales**: los discos Ultra SSD están diseñados para funcionar con todas las SKU de Azure Virtual Machine que fueron habilitadas con SSD Premium; sin embargo, ya que está ahora en la versión preliminar, los tamaños de las máquinas virtuales se limitarán a ES/DS v3.

**Configuración del rendimiento dinámico**: los discos Ultra SSD le permiten cambiar dinámicamente el rendimiento (unidades IOPS y capacidad de proceso) del disco junto con sus necesidades de carga de trabajo, sin tener que reiniciar sus máquinas virtuales.

## <a name="scalability-and-performance-targets"></a>Objetivos de escalabilidad y rendimiento

Cuando aprovisione un disco Ultra SSD, tendrá la opción de configurar de forma independiente la capacidad y el rendimiento del disco. Los discos Ultra SSD vienen en varios tamaños fijos que van desde los 4 GiB hasta los 64 TiB, y cuentan con un modelo de configuración de rendimiento flexible que le permite configurar las unidades IOPS y el rendimiento de forma independiente. Asimismo, los discos Ultra SSD solo pueden usarse como discos de datos. Por ello, le recomendamos que use los discos SSD Premium como discos de sistema operativo.

Estas son algunas capacidades clave de los discos Ultra SSD:

- Capacidad de disco: Ultra SSD le ofrece una gama de distintos tamaños de disco, de 4 GiB a 64 TiB.
- IOPS de disco: los dispositivos Ultra SSD admiten límites de IOPS de 300 IOPS/GiB y hasta un máximo de 160 K IOPS por disco. Para recuperar la tasa de unidades IOPS que aprovisionó, asegúrese de que la cantidad de IOPS de disco seleccionadas sea menor que la cantidad de IOPS de la máquina virtual. El mínimo de IOPS de disco es de 100 IOPS.
- Rendimiento del disco: con los discos Ultra SSD, el límite de rendimiento de un solo disco es de 256 KiB/s por cada IOPS aprovisionada, y hasta 2000 MBps como máximo por disco (donde MBps = 10^6 bytes por segundo). El rendimiento de disco mínimo es de 1 MiB.

En la tabla siguiente, se resumen las diferentes configuraciones que se admiten para los diferentes tamaños de disco:  

### <a name="ultra-ssd-managed-disk-offerings"></a>Ofertas de instancias de Managed Disks con Ultra SSD

|Tamaño de disco (GiB)  |Capacidades de IOPS  |Capacidad de rendimiento (MBps)  |
|---------|---------|---------|
|4     |1,200         |300         |
|8     |2,400         |600         |
|16     |4,800         |1,200         |
|32     |9600         |2.000         |
|64     |19 200         |2.000         |
|128     |38 400         |2.000         |
|256     |76 800         |2.000         |
|512     |80 000         |2.000         |
|1024 - 65 536 (los tamaños de este intervalo aumentan en incrementos de 1 TiB)     |160 000         |2.000         |

## <a name="pricing-and-billing"></a>Precios y facturación

Al usar Ultra SSD, se aplicarán las siguientes consideraciones de facturación:

- Tamaño del disco administrado
- IOPS aprovisionada del disco administrado
- Rendimiento aprovisionado del disco administrado
- Precio de reserva de VM de Ultra SSD

### <a name="managed-disk-size"></a>Tamaño del disco administrado

Managed Disks se factura por tamaño de la máquina virtual elegida mientras se aprovisiona una nueva máquina virtual de Azure. Azure asigna el tamaño aprovisionado (redondeado al alza) a la oferta de tamaño de disco más cercana. Para obtener detalles sobre los tamaños de disco que se ofrecen, consulte la tabla en la sección Objetivos de escalabilidad y rendimiento anterior. Cada disco se asigna a un tamaño de disco aprovisionado compatible y se facturará según corresponda y por hora. Por ejemplo, si aprovisiona un disco Ultra SSD de 200 GiB y lo elimina después de 20 horas, se asignará a la oferta de tamaño de disco de 256 GiB y se le cobrará por los 256 GiB durante 20 horas. Esta facturación se basa en el consumo por horas de proceso con independencia del volumen de datos escritos realmente en el disco.

### <a name="managed-disk-provisioned-iops"></a>IOPS aprovisionada del disco administrado

IOPS es el número de solicitudes que la aplicación envía a los discos en un segundo. Una operación de entrada y salida puede ser de lectura o escritura secuencial o aleatoria. Según el tamaño del disco o el número de discos conectados a la máquina virtual, el promedio de IOPS se factura por hora. Para obtener detalles sobre las IOPS de disco que se ofrecen, consulte la tabla en la sección Objetivos de escalabilidad y rendimiento anterior.

### <a name="managed-disk-provisioned-throughput"></a>Rendimiento aprovisionado del disco administrado

El rendimiento es la cantidad de datos que la aplicación envía a los discos en un intervalo especificado, y se mide en bytes/segundo. Si la aplicación está realizando operaciones de entrada y salida, necesitará tener un alto rendimiento.  

Hay una relación entre el rendimiento y la IOPS, tal como se muestra en la siguiente fórmula:  IOPS x tamaño de E/S = rendimiento

Por lo tanto, es importante determinar los valores óptimos de rendimiento e IOPS que requiere su aplicación. Si intenta optimizar uno, el otro también se ve afectado. Le recomendamos comenzar con un rendimiento correspondiente a un tamaño de 16 KiB de E/S y ajustarlo si necesita más rendimiento.

Para obtener detalles sobre el rendimiento de disco que se admite en Ultra SSD, consulte la tabla en la sección Objetivos de escalabilidad y rendimiento anterior. Igual que sucede con el tamaño del disco y las IOPS, el rendimiento aprovisionado se factura cada hora según los MBps aprovisionados.

### <a name="ultra-ssd-vm-reservation-fee"></a>Precio de reserva de VM de Ultra SSD

En breve implementaremos una funcionalidad en la máquina virtual que indicará si su máquina virtual es compatible con Ultra SSD. Una máquina virtual compatible con Ultra SSD asigna capacidad de ancho de banda dedicada entre la instancia de la máquina virtual de cálculo y la unidad de escalado de almacenamiento en bloque, para así poder optimizar el rendimiento y reducir la latencia. Al agregar esta funcionalidad en la máquina virtual, se crea un cargo por reserva que solo se cobra si habilita la funcionalidad Ultra SSD en la máquina virtual sin adjuntar un disco Ultra SSD a la misma. Cuando un disco Ultra SSD está conectado a la máquina virtual que es compatible con Ultra SSD, este cargo no se aplicará. Este cargo se calcula según la vCPU aprovisionada en la máquina virtual.

Consulte la página de [precios de discos de Azure](https://azure.microsoft.com/pricing/details/managed-disks/) para ver los detalles de los precios referentes a los nuevos discos Ultra SSD que están disponibles en la versión preliminar limitada.

### <a name="ultra-ssd-preview-scope-and-limitations"></a>Ámbito y limitaciones de la versión preliminar de Ultra SSD

En la versión preliminar, los discos Ultra SSD:

- Se admitirán inicialmente en el Este de EE. UU. 2, en una única zona de disponibilidad.  
- Solo se podrán usar con las zonas de disponibilidad (los conjuntos de disponibilidad y las implementaciones de máquinas virtuales únicas fuera de las zonas no tendrán la capacidad de adjuntar un disco Ultra SSD).
- Solo son compatibles con las máquinas virtuales ES/DS v3.
- Solo están disponibles como discos de datos y solo admiten el tamaño de sector físico 4k.  
- Solo pueden crearse como discos vacíos.  
- Actualmente solo se pueden implementar mediante plantillas de Resource Manager, CLI y SDK de Python.
- No admitirán (todavía) instantáneas de disco, imágenes de máquinas virtuales, conjuntos de disponibilidad, Virtual Machine Scale Sets ni Azure Disk Encryption.
- No admitirán (todavía) la integración con Azure Backup o Azure Site Recovery.
- Tal como sucede con la  [mayoría de las versiones preliminares](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), esta característica no debe usarse para cargas de trabajo de producción hasta que se lance la disponibilidad general (GA).
