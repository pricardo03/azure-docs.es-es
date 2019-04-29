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
ms.openlocfilehash: 4b5d2de2e9ccd44517e083a435e127bd5678f002
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60564347"
---
# <a name="ultra-disks-preview-managed-disks-for-azure-virtual-machine-workloads"></a>Discos Ultra (versión preliminar) de managed disks para las cargas de trabajo de máquina virtual de Azure

Los discos de ultra Azure (versión preliminar) ofrece un alto rendimiento, una IOPS elevada y almacenamiento en disco baja latencia coherente para máquinas virtuales de IaaS de Azure. En esta nueva oferta se proporciona un rendimiento exclusivo que se encuentra en los mismos niveles de disponibilidad que nuestras ofertas de discos existentes. Ventajas adicionales de ultra discos incluyen la capacidad de cambiar dinámicamente el rendimiento del disco junto con las cargas de trabajo sin necesidad de reiniciar las máquinas virtuales. Discos Ultra son adecuados para cargas de trabajo de uso intensivo de datos como SAP HANA, las bases de datos de nivel superior y las cargas de trabajo intensivo de la transacción.

## <a name="ultra-disk-features"></a>Características de disco Ultra

**Managed Disks**: Discos Ultra solo están disponibles como discos administrados. No se puede implementar discos Ultra como un disco no administrado o un blob en páginas. Al crear un disco administrado, especifique la sku de disco escriba como UltraSSD_LRS e indican el tamaño del disco, el número de IOPS y rendimiento que necesita y Azure crea y administra el disco automáticamente.  

**Máquinas virtuales**: Discos Ultra están diseñados para funcionar con todas las Premium SSD SKU de máquina Virtual de Azure habilitada; Sin embargo, ya que está actualmente en versión preliminar, las máquinas virtuales tienen un tamaño como v3 ES/DS.

**Configuración del rendimiento dinámico**: Ultra discos le permiten cambiar dinámicamente el rendimiento (IOPS y rendimiento) del disco junto con sus necesidades de carga de trabajo sin tener que reiniciar las máquinas virtuales.

## <a name="scalability-and-performance-targets"></a>Objetivos de escalabilidad y rendimiento

Cuando aprovisiona un ultra discos, tendrá la opción de configurar la capacidad y el rendimiento del disco de forma independiente. Discos Ultra vienen en varios tamaños fijos de 4 GB hasta 64 TB y un modelo de configuración de rendimiento flexible que le permite configurar de forma independiente la IOPS y rendimiento. Solo se pueden aprovechar Ultra discos como discos de datos. Por ello, le recomendamos que use los discos SSD Premium como discos de sistema operativo.

Algunas funcionalidades de ultra discos principales son:

- Capacidad de disco: Discos Ultra ofrece una gama de diferentes tamaños de discos de 4 GB hasta 64 TB.
- IOPS de disco: Discos Ultra son compatibles con los límites de IOPS de 300 e/s por segundo y GiB, hasta un máximo de 160 KB IOPS por disco. Para recuperar la tasa de unidades IOPS que aprovisionó, asegúrese de que la cantidad de IOPS de disco seleccionadas sea menor que la cantidad de IOPS de la máquina virtual. El mínimo de IOPS de disco es de 100 IOPS.
- Rendimiento del disco: Con los discos ultra, el límite de rendimiento de un solo disco es 256 KiB/s para cada una IOPS aprovisionadas, hasta un máximo de 2000 MBps por disco (donde MBps = 10 ^ 6 Bytes por segundo). El rendimiento de disco mínimo es de 1 MiB.

En la tabla siguiente, se resumen las diferentes configuraciones que se admiten para los diferentes tamaños de disco:  

### <a name="ultra-disks-managed-disk-offerings"></a>Ultra discos administrados las ofertas de disco

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

Al usar discos ultra, se aplicarán las siguientes consideraciones de facturación:

- Tamaño del disco administrado
- IOPS aprovisionada del disco administrado
- Rendimiento aprovisionado del disco administrado
- Cuota de disco Ultra VM reserva

### <a name="managed-disk-size"></a>Tamaño de disco administrado

Managed Disks se factura por tamaño de la máquina virtual elegida mientras se aprovisiona una nueva máquina virtual de Azure. Azure asigna el tamaño aprovisionado (redondeado al alza) a la oferta de tamaño de disco más cercana. Para obtener detalles sobre los tamaños de disco que se ofrecen, consulte la tabla en la sección Objetivos de escalabilidad y rendimiento anterior. Cada disco se asigna a un tamaño de disco aprovisionado compatible y se facturará según corresponda y por hora. Por ejemplo, si se aprovisiona un disco de ultra GiB 200 y se elimina después de 20 horas, asignará a la oferta de tamaño de disco de 256 GiB y se le facturará de las 256 GiB de 20 horas. Esta facturación se basa en el consumo por horas de proceso con independencia del volumen de datos escritos realmente en el disco.

### <a name="managed-disk-provisioned-iops"></a>Disco administrado aprovisionado e/s por segundo

IOPS es el número de solicitudes que la aplicación envía a los discos en un segundo. Una operación de entrada y salida puede ser de lectura o escritura secuencial o aleatoria. Según el tamaño del disco o el número de discos conectados a la máquina virtual, el promedio de IOPS se factura por hora. Para obtener detalles sobre las IOPS de disco que se ofrecen, consulte la tabla en la sección Objetivos de escalabilidad y rendimiento anterior.

### <a name="managed-disk-provisioned-throughput"></a>Administra el procesamiento aprovisionado del disco

El rendimiento es la cantidad de datos que la aplicación envía a los discos en un intervalo especificado, y se mide en bytes/segundo. Si la aplicación está realizando operaciones de entrada y salida, necesitará tener un alto rendimiento.  

Hay una relación entre el rendimiento y la IOPS, tal como se muestra en la siguiente fórmula:  IOPS x tamaño de E/S = rendimiento

Por lo tanto, es importante determinar los valores óptimos de rendimiento e IOPS que requiere su aplicación. Si intenta optimizar uno, el otro también se ve afectado. Le recomendamos comenzar con un rendimiento correspondiente a un tamaño de 16 KiB de E/S y ajustarlo si necesita más rendimiento.

Para obtener más información sobre el rendimiento de disco compatibles en ultra discos, consulte la tabla en la sección anterior de objetivos de escalabilidad y rendimiento. Igual que sucede con el tamaño del disco y las IOPS, el rendimiento aprovisionado se factura cada hora según los MBps aprovisionados.

### <a name="ultra-disk-vm-reservation-fee"></a>Cuota de disco Ultra VM reserva

Estamos introduciendo una capacidad en la máquina virtual que indica que la máquina virtual es ultra disco compatible. Una máquina virtual compatible con SSD Ultra asigna capacidad dedicada de ancho de banda entre la instancia de la máquina virtual de proceso y la unidad de escalado de almacenamiento en bloque, para así poder optimizar el rendimiento y reducir la latencia. Al agregar esta funcionalidad en la máquina virtual, se crea un cargo por reserva que solo se cobra si habilita la funcionalidad de discos Ultra en la máquina virtual sin asociarle uno de estos discos. Cuando un disco ultra está conectado a la máquina virtual compatible ultra, no se aplicaría este cargo. Este cargo se calcula según la vCPU aprovisionada en la máquina virtual.

Hacer referencia a la [página de precios de Azure Disks](https://azure.microsoft.com/pricing/details/managed-disks/) para el nuevo ultra discos Detalles de precios disponibles en versión preliminar limitada.

### <a name="ultra-disk-preview-scope-and-limitations"></a>Limitaciones y el ámbito de la versión preliminar de disco ultra

En la versión preliminar, ultra discos:

- Se admitirán inicialmente en el Este de EE. UU. 2, en una única zona de disponibilidad.  
- Solo se puede usar con las zonas de disponibilidad (conjuntos de disponibilidad y las implementaciones de VM único fuera zonas no tienen la capacidad para conectar un disco ultra)
- Solo son compatibles con las máquinas virtuales ES/DS v3.
- Solo están disponibles como discos de datos y solo admiten el tamaño de sector físico 4k.  
- Solo pueden crearse como discos vacíos.  
- Actualmente solo se pueden implementar mediante plantillas de Resource Manager, CLI y SDK de Python.
- No admitirán (todavía) instantáneas de disco, imágenes de máquinas virtuales, conjuntos de disponibilidad, Virtual Machine Scale Sets ni Azure Disk Encryption.
- No admitirán (todavía) la integración con Azure Backup o Azure Site Recovery.
- Tal como sucede con la  [mayoría de las versiones preliminares](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), esta característica no debe usarse para cargas de trabajo de producción hasta que se lance la disponibilidad general (GA).
