---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/13/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 885bc1c627626ee7ba4f391be31131b18fa1ab39
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65212038"
---
## <a name="premium-ssd"></a>SSD Premium

Los discos SSD Premium de Azure ofrecen soporte de disco de alto rendimiento y latencia baja para máquinas virtuales (VM) con cargas de trabajo con uso intensivo de entrada/salida (E/S). Para aprovechar la ventaja de la velocidad y el rendimiento de discos de Premium Storage, puede migrar los discos de VM existentes a SSD Premium. Los discos SSD Premium son adecuados para aplicaciones de producción críticas.

### <a name="disk-size"></a>Tamaño del disco
[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

Cuando se aprovisiona un disco de Premium Storage, a diferencia de Standard Storage, se garantizan la capacidad, las E/S por segundo y el rendimiento del mismo. Por ejemplo, si crea un disco P50, Azure aprovisiona una capacidad de almacenamiento de 4095 GB, 7500 E/S por segundo y un rendimiento de 250 MB/s para él. La aplicación puede usar toda la capacidad y el rendimiento o parte de ellos. Los discos SSD premium están diseñados para proporcionar rendimiento de destino el 99,9 % del tiempo.

## <a name="standard-ssd"></a>SSD estándar

Los discos SSD estándar de Azure son una opción de almacenamiento rentable, optimizada para cargas de trabajo que necesitan un rendimiento constante en niveles inferiores de IOPS. Los discos SSD estándar ofrecen una buena experiencia de nivel de entrada para aquellos que desean pasarse a la nube, en especial si tiene problemas con la variación de las cargas de trabajo que se ejecutan en las soluciones de disco duro locales. Los discos SSD estándar ofrecen mayor disponibilidad, coherencia, confiabilidad y latencia en comparación con los discos HDD. Los discos SSD estándar son convenientes para servidores web, servidores de aplicaciones con IOPS bajas, aplicaciones empresariales de poco uso y cargas de trabajo de desarrollo/pruebas.

### <a name="disk-size"></a>Tamaño del disco
[!INCLUDE [disk-storage-standard-ssd-sizes](disk-storage-standard-ssd-sizes.md)]

SSDs estándares están diseñadas para proporcionar las latencias de milisegundos para la mayoría de las operaciones de E/S y para entregar la IOPS y el rendimiento hasta los límites descritos en la anterior tabla 99% del tiempo. IOPS y el rendimiento real pueden variar a veces, según los patrones de tráfico. Los discos SSD estándar proporcionarán un rendimiento más coherente que los discos HDD, con una latencia menor.

## <a name="standard-hdd"></a>HDD estándar

Los discos HDD estándar de Azure ofrecen compatibilidad de discos confiable y de bajo coste para las máquinas virtuales que ejecutan cargas de trabajo que no tienen en cuenta la latencia. También admite blobs, tablas, colas y archivos. Con Standard Storage, los datos se almacenan en unidades de disco duro (HDD). Cuando se trabaja con máquinas virtuales, se pueden usar discos SSD y HDD estándar para escenarios de desarrollo/pruebas y para cargas de trabajo menos críticas. Standard Storage está disponible en todas las regiones de Azure.

### <a name="disk-size"></a>Tamaño del disco
[!INCLUDE [disk-storage-standard-hdd-sizes](disk-storage-standard-hdd-sizes.md)]

## <a name="billing"></a>Facturación

Al usar Managed Disks, se aplican las siguientes consideraciones de facturación:

- Tipo de disco
- Tamaño del disco administrado
- Instantáneas
- Transferencias de datos de salida
- Número de transacciones

**Tamaño del disco administrado**: los discos administrados se facturan por el tamaño aprovisionado. Azure asigna el tamaño aprovisionado (redondeado al alza) a la oferta de tamaño de disco más cercana. Para obtener detalles sobre los tamaños de disco ofrecidos, consulte las tablas anteriores. Cada disco se asigna a una oferta de tamaño de disco aprovisionado compatible y se factura según corresponda. Por ejemplo, si ha aprovisionado un disco SSD estándar de 200 GiB, se asigna a la oferta de tamaño de disco E15 (256 GiB). La facturación de cualquier disco aprovisionado se prorratea cada hora con el precio mensual de la oferta de Premium Storage. Por ejemplo, si ha aprovisionado un disco E10 y lo ha eliminado después de 20 horas, se factura la oferta E10 prorrateada a 20 horas. Esto es así independientemente de la cantidad de datos que se escriba en el disco.

**Instantáneas**: Las instantáneas se facturan según el tamaño utilizado. Por ejemplo, si crea una instantánea de un disco administrado con capacidad aprovisionada de 64 GiB y el tamaño de datos usado real es de 10 GiB, solo se le cobra por el tamaño de datos usado de 10 GiB.