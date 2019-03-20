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
ms.openlocfilehash: 0201776914610ddaca50a670fc500156a65cd734
ms.sourcegitcommit: f596d88d776a3699f8c8cf98415eb874187e2a48
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2019
ms.locfileid: "58114390"
---
## <a name="premium-ssd"></a>SSD Premium

Los discos SSD Premium de Azure ofrecen soporte de disco de alto rendimiento y latencia baja para máquinas virtuales (VM) con cargas de trabajo con uso intensivo de entrada/salida (E/S). Para aprovechar la ventaja de la velocidad y el rendimiento de discos de Premium Storage, puede migrar los discos de VM existentes a SSD Premium. Los discos SSD Premium son adecuados para aplicaciones de producción críticas.

### <a name="disk-size"></a>Tamaño del disco

Los tamaños marcados con un asterisco están actualmente en versión preliminar.

| Tamaños de SSD Premium | P4 | P6 | P10 | P15 | P20 | P30 | P40 | P50 | P60* | P70* | P80* |
|-------------------|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| Tamaño de disco en GiB | 32 | 64 | 128 | 256 | 512 | 1024 | 2 048 | 4095 | 8192 | 16 384 | 32 767 |
| IOPS por disco | Hasta 120 | Hasta 240 | Hasta 500 | Hasta 1100 | Hasta 2300 | Hasta 5000 | Hasta 7500 | Hasta 7500 | Hasta 12 500 | Hasta 15 000 | Hasta 20 000 |
| Rendimiento de disco. | Hasta 25 MiB/s | Hasta 50 MiB/s | Hasta 100 MiB/s | Hasta 125 MiB/s | Hasta 150 MiB/s | Hasta 200 MiB/s | Hasta 250 MiB/s | Hasta 250 MiB/s| Hasta 480 MiB/s | Hasta 750 MiB/s | Hasta 750 MiB/s |

## <a name="standard-ssd"></a>SSD estándar

Los discos SSD estándar de Azure son una opción de almacenamiento rentable, optimizada para cargas de trabajo que necesitan un rendimiento constante en niveles inferiores de IOPS. Los discos SSD estándar ofrecen una buena experiencia de nivel de entrada para aquellos que desean pasarse a la nube, en especial si tiene problemas con la variación de las cargas de trabajo que se ejecutan en las soluciones de disco duro locales. Los discos SSD estándar ofrecen mayor disponibilidad, coherencia, confiabilidad y latencia en comparación con los discos HDD. Los discos SSD estándar son convenientes para servidores web, servidores de aplicaciones con IOPS bajas, aplicaciones empresariales de poco uso y cargas de trabajo de desarrollo/pruebas.

### <a name="disk-size"></a>Tamaño del disco

Los tamaños marcados con un asterisco están actualmente en versión preliminar.

| Tamaños de SSD estándar | E4 | E6 | E10 | E15 | E20 | E30 | E40 | E50 | E60* | E70* | E80* |
|--------------------|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| Tamaño de disco en GiB | 32 | 64 | 128 | 256 | 512 | 1024 | 2 048 | 4095 | 8192 | 16 384 | 32 767 |
| IOPS por disco | Hasta 120 | Hasta 240 | Hasta 500 | Hasta 500 | Hasta 500 | Hasta 500 | Hasta 500 | Hasta 500 | Hasta 1300 | Hasta 2000 | Hasta 2000 |
| Rendimiento de disco. |  Hasta 25 MiB/s |  Hasta 50 MiB/s  |  Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s| Hasta 300 MiB/s |  Hasta 500 MiB/s | Hasta 500 MiB/s |

## <a name="standard-hdd"></a>HDD estándar

Los discos HDD estándar de Azure ofrecen compatibilidad de discos confiable y de bajo coste para las máquinas virtuales que ejecutan cargas de trabajo que no tienen en cuenta la latencia. También admite blobs, tablas, colas y archivos. Con Standard Storage, los datos se almacenan en unidades de disco duro (HDD). Cuando se trabaja con máquinas virtuales, se pueden usar discos SSD y HDD estándar para escenarios de desarrollo/pruebas y para cargas de trabajo menos críticas. Standard Storage está disponible en todas las regiones de Azure.

### <a name="disk-size"></a>Tamaño del disco

Los tamaños marcados con un asterisco están actualmente en versión preliminar.

| Tipo de disco Estándar | S4 | S6 | S10 | S15 | S20 | S30 | S40 | S50 | S60* | S70* | S80* |
|--------------------|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| Tamaño de disco en GiB | 32 | 64 | 128 | 256 | 512 | 1024 | 2 048 | 4095 | 8192 | 16 384 | 32 767 |
| IOPS por disco | Hasta 500 | Hasta 500 | Hasta 500 | Hasta 500 | Hasta 500 | Hasta 500 | Hasta 500 | Hasta 500 | Hasta 1300 | Hasta 2000 | Hasta 2000 |
| Rendimiento de disco. | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s| Hasta 300 MiB/s | Hasta 500 MiB/s | Hasta 500 MiB/s |

## <a name="billing"></a>Facturación

Al usar Managed Disks, se aplican las siguientes consideraciones de facturación:

- Tipo de disco
- Tamaño del disco administrado
- Instantáneas
- Transferencias de datos de salida
- Número de transacciones

**Tamaño del disco administrado**: los discos administrados se facturan por el tamaño aprovisionado. Azure asigna el tamaño aprovisionado (redondeado al alza) a la oferta de tamaño de disco más cercana. Para obtener detalles sobre los tamaños de disco ofrecidos, consulte las tablas anteriores. Cada disco se asigna a una oferta de tamaño de disco aprovisionado compatible y se factura según corresponda. Por ejemplo, si ha aprovisionado un disco SSD estándar de 200 GiB, se asigna a la oferta de tamaño de disco E15 (256 GiB). La facturación de cualquier disco aprovisionado se prorratea cada hora con el precio mensual de la oferta de Premium Storage. Por ejemplo, si ha aprovisionado un disco E10 y lo ha eliminado después de 20 horas, se factura la oferta E10 prorrateada a 20 horas. Esto es así independientemente de la cantidad de datos que se escriba en el disco.

**Instantáneas**: Las instantáneas se facturan según el tamaño utilizado. Por ejemplo, si crea una instantánea de un disco administrado con capacidad aprovisionada de 64 GiB y el tamaño de datos usado real es de 10 GiB, solo se le cobra por el tamaño de datos usado de 10 GiB.