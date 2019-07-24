---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 05/14/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 2bc5602011ed64b11b1b8c96b7e69a8d5ee9bf32
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67132981"
---
## <a name="premium-ssd"></a>SSD Premium

Los discos SSD Premium de Azure ofrecen soporte de disco de alto rendimiento y latencia baja para máquinas virtuales (VM) con cargas de trabajo con uso intensivo de entrada/salida (E/S). Para aprovechar la ventaja de la velocidad y el rendimiento de discos de Premium Storage, puede migrar los discos de VM existentes a SSD Premium. Los discos SSD Premium son adecuados para aplicaciones de producción críticas. Los discos SSD Premium solo se pueden usar con series de máquinas virtuales que sean compatibles con el almacenamiento premium.

Para más información sobre los tipos individuales de máquinas virtuales y los tamaños de Azure para Windows, incluidos los tamaños que son compatibles con el almacenamiento premium, consulte [Tamaños de máquina virtual Windows](../articles/virtual-machines/windows/sizes.md). Para más información sobre los tipos individuales de máquinas virtuales y los tamaños de Azure para Linux, incluidos los tamaños que son compatibles con el almacenamiento premium, consulte [Tamaños de máquina virtual Linux](../articles/virtual-machines/linux/sizes.md).

### <a name="disk-size"></a>Tamaño del disco
[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

Cuando se aprovisiona un disco de Premium Storage, a diferencia de Standard Storage, se garantizan la capacidad, las E/S por segundo y el rendimiento del mismo. Por ejemplo, si crea un disco P50, Azure aprovisiona una capacidad de almacenamiento de 4095 GB, 7500 E/S por segundo y un rendimiento de 250 MB/s para él. La aplicación puede usar toda la capacidad y el rendimiento o parte de ellos. Los discos SSD Premium están diseñados para proporcionar bajas latencias inferiores a 10 milisegundos y un IOPS y rendimiento que se describen en la tabla anterior como del 99,9 % del tiempo.

### <a name="transactions"></a>Transacciones

Para los discos SSD Premium, cada operación de E/S menor o igual a 256 KiB de rendimiento se considera una sola operación de E/S. Las operaciones de E/S mayores de 256 KiB de rendimiento se consideran múltiplos de operaciones de E/S con un tamaño de 256 KiB.

## <a name="standard-ssd"></a>SSD estándar

Los discos SSD estándar de Azure son una opción de almacenamiento rentable, optimizada para cargas de trabajo que necesitan un rendimiento constante en niveles inferiores de IOPS. Los discos SSD estándar ofrecen una buena experiencia de nivel de entrada para aquellos que desean pasarse a la nube, en especial si tiene problemas con la variación de las cargas de trabajo que se ejecutan en las soluciones de disco duro locales. En comparación con los discos HDD estándar, los discos SSD estándar ofrecen mayor disponibilidad, coherencia, confiabilidad y latencia. Los discos SSD estándar son convenientes para servidores web, servidores de aplicaciones con IOPS bajas, aplicaciones empresariales de poco uso y cargas de trabajo de desarrollo/pruebas. Al igual que los discos HDD estándar, los SSD están disponibles en todas las máquinas virtuales de Azure.

### <a name="disk-size"></a>Tamaño del disco
[!INCLUDE [disk-storage-standard-ssd-sizes](disk-storage-standard-ssd-sizes.md)]

Los discos SSD estándar están diseñados para proporcionar latencias de menos de 10 milisegundos y un nivel de IOPS y de rendimiento hasta los límites descritos en la tabla anterior durante el 99 % del tiempo. Las IOPS y el rendimiento reales pueden variar a veces, según los patrones de tráfico. Los discos SSD estándar proporcionarán un rendimiento más coherente que los discos HDD, con una latencia menor.

### <a name="transactions"></a>Transacciones

Para los discos SSD estándar, cada operación de E/S menor o igual a 256 KiB de rendimiento se considera una sola operación de E/S. Las operaciones de E/S mayores de 256 KiB de rendimiento se consideran múltiplos de operaciones de E/S con un tamaño de 256 KiB. Estas transacciones tienen un impacto en la facturación.

## <a name="standard-hdd"></a>HDD estándar

Los discos HDD estándar de Azure ofrecen compatibilidad de discos confiable y de bajo coste para las máquinas virtuales que ejecutan cargas de trabajo que no tienen en cuenta la latencia. Con Standard Storage, los datos se almacenan en unidades de disco duro (HDD). La latencia, las IOPS y el rendimiento de los discos HDD estándar pueden variar más en comparación con los discos basados en SSD. Los discos HDD estándar están diseñados para ofrecer latencias de escritura inferiores a 10 ms y latencias de lectura inferiores a 20 ms para la mayoría de las operaciones de E/S, aunque el rendimiento real puede variar según el tamaño de E/S y el patrón de carga de trabajo. Cuando se trabaja con máquinas virtuales, se pueden usar discos HDD estándar para escenarios de desarrollo/pruebas y para cargas de trabajo menos críticas. Los discos HDD estándar están disponibles en todas las regiones de Azure y se pueden utilizar con todas las máquinas virtuales de Azure.

### <a name="disk-size"></a>Tamaño del disco
[!INCLUDE [disk-storage-standard-hdd-sizes](disk-storage-standard-hdd-sizes.md)]

### <a name="transactions"></a>Transacciones

En el caso de los discos HDD estándar, cada operación de E/S se considera como una única transacción, independientemente del tamaño de esta. Estas transacciones tienen un impacto en la facturación.

## <a name="billing"></a>Facturación

Al usar Managed Disks, se aplican las siguientes consideraciones de facturación:

- Tipo de disco
- Tamaño del disco administrado
- Instantáneas
- Transferencias de datos de salida
- Número de transacciones

**Tamaño del disco administrado**: los discos administrados se facturan por el tamaño aprovisionado. Azure asigna el tamaño aprovisionado (redondeado al alza) a la oferta de tamaño de disco más cercana. Para obtener detalles sobre los tamaños de disco ofrecidos, consulte las tablas anteriores. Cada disco se asigna a una oferta de tamaño de disco aprovisionado compatible y se factura según corresponda. Por ejemplo, si ha aprovisionado un disco SSD estándar de 200 GiB, se asigna a la oferta de tamaño de disco E15 (256 GiB). La facturación de cualquier disco aprovisionado se prorratea cada hora con el precio mensual de la oferta de Premium Storage. Por ejemplo, si ha aprovisionado un disco E10 y lo ha eliminado después de 20 horas, se factura la oferta E10 prorrateada a 20 horas. Esto es así independientemente de la cantidad de datos que se escriba en el disco.

**Instantáneas**: Las instantáneas se facturan según el tamaño utilizado. Por ejemplo, si crea una instantánea de un disco administrado con capacidad aprovisionada de 64 GiB y el tamaño de datos usado real es de 10 GiB, solo se le cobra por el tamaño de datos usado de 10 GiB.
