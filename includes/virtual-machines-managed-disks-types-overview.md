---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 8a067474fb172d4ff7a7fdf7eb6d24536bd2d017
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56443432"
---
# <a name="what-disk-types-are-available-in-azure"></a>¿Qué tipos de disco están disponibles en Azure?

Actualmente, Azure Managed Disks ofrece cuatro tipos de discos, tres de los cuales están disponibles con carácter general (GA) y otro que está actualmente en versión preliminar. Cada uno de estos cuatro tipos de disco tiene sus propios escenarios de clientes de destino apropiados.

## <a name="disk-comparison"></a>Comparación de discos

En la tabla siguiente se ofrece una comparación entre las unidades de estado sólido (SSD) (versión preliminar), SSD premium, SSD estándar y unidades de disco duro (HDD) estándar para Managed Disks, a fin de ayudar a decidir qué opción usar.

|   | SSD Ultra (versión preliminar)   | SSD Premium   | SSD estándar   | HDD estándar   |
|---------|---------|---------|---------|---------|
|Tipo de disco   |SSD   |SSD   |SSD   |HDD   |
|Escenario   |Cargas de trabajo con uso intensivo de E/S, como SAP HANA, bases de dato de capa superior (por ejemplo, SQL y Oracle) y otras cargas de trabajo con muchas transacciones.   |Cargas de trabajo confidenciales de producción y rendimiento   |Servidores web, aplicaciones empresariales poco utilizadas y desarrollo y pruebas   |Copia de seguridad, no crítico, acceso poco frecuente   |
|Tamaño del disco   |65 536 gibibytes (GiB) (versión preliminar)   |4095 GiB (GA), 32 767 GiB (versión preliminar)    |4095 (GA) GiB, 32 767 GiB (versión preliminar)   |4095 GiB (GA), 32 767 GiB (versión preliminar)   |
|Rendimiento máx.   |2000 MiB/s (versión preliminar)   |250 (GA) MiB/s, 750 MiB/s (versión preliminar)   |60 MiB/s (GA), 500 MiB/s (versión preliminar)   |60 Mib/s (GA), 500 MiB/s (versión preliminar)   |
|IOPS máx.   |160 000 (versión preliminar)   |7500 (GA), 20 000 (versión preliminar)   |500 (GA), 2000 (versión preliminar)   |500 (GA), 2000 (versión preliminar)   |

## <a name="ultra-ssd-preview"></a>SSD Ultra (versión preliminar)

Azure SSD Ultra (versión preliminar) le ofrece un alto rendimiento, IOPS elevadas y un almacenamiento en disco consistente y de baja latencia para máquinas virtuales IaaS de Azure. Algunas ventajas adicionales de SSD Ultra incluyen la capacidad de cambiar dinámicamente el rendimiento del disco junto con sus cargas de trabajo sin tener que reiniciar las máquinas virtuales. Asimismo, SSD Ultra es adecuado para cargas de trabajo con grandes cantidades de datos, como SAP HANA, bases de datos de nivel superior y cargas de trabajo que admitan muchas transacciones. SSD Ultra solo puede utilizarse como discos de datos. Por ello, recomendamos usar los discos SSD Premium como discos de sistema operativo.

### <a name="performance"></a>Rendimiento

Cuando aprovisione un disco ultra, puede configurar de forma independiente la capacidad y el rendimiento del disco. Los discos SSD Ultra vienen en varios tamaños fijos que van desde los 4 GiB hasta los 64 TiB, y cuentan con un modelo de configuración de rendimiento flexible que le permite configurar las unidades IOPS y el rendimiento de forma independiente.

Estas son algunas capacidades clave de los discos Ultra SSD:

- Capacidad de disco: Intervalos de capacidad de SSD Ultra desde 4 GiB hasta 64 TiB.
- IOPS de disco: los dispositivos SSD Ultra admiten límites de IOPS de 300 IOPS/GiB y hasta un máximo de 160 K IOPS por disco. Para recuperar la tasa de unidades IOPS que aprovisionó, asegúrese de que la cantidad de IOPS de disco seleccionadas sea menor que la cantidad de IOPS de la máquina virtual. El mínimo de IOPS de disco es de 100 IOPS.
- Rendimiento del disco: con los discos SSD Ultra, el límite de rendimiento de un solo disco es de 256 KiB/s por cada IOPS aprovisionada, y hasta 2000 MBps como máximo por disco (donde MBps = 10^6 bytes por segundo). El rendimiento de disco mínimo es de 1 MiB.

### <a name="disk-size"></a>Tamaño del disco

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

### <a name="preview-scope-and-limitations"></a>Ámbito y limitaciones de la versión preliminar

Durante la versión preliminar, los discos SSD Ultra:

- Se admiten en el Este de EE. UU. 2, en una única zona de disponibilidad.  
- Solo se podrán usar con las zonas de disponibilidad (los conjuntos de disponibilidad y las implementaciones de máquinas virtuales únicas fuera de las zonas no tendrán la capacidad de adjuntar un disco Ultra).
- Solo son compatibles con las máquinas virtuales ES/DS v3.
- Solo están disponibles como discos de datos y solo admiten el tamaño de sector físico 4k.  
- Solo pueden crearse como discos vacíos.  
- Actualmente solo se pueden implementar mediante plantillas de Azure Resource Manager, CLI y SDK de Python.
- No admitirán todavía instantáneas de disco, imágenes de máquinas virtuales, conjuntos de disponibilidad, Virtual Machine Scale Sets ni Azure Disk Encryption.
- No admitirán todavía la integración con Azure Backup o Azure Site Recovery.
- Tal como sucede con la  [mayoría de las versiones preliminares](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), esta característica no debe usarse para cargas de trabajo de producción hasta que se lance la disponibilidad general (GA).

## <a name="premium-ssd"></a>SSD Premium

Los discos SSD Premium de Azure ofrecen soporte de disco de alto rendimiento y latencia baja para máquinas virtuales (VM) con cargas de trabajo con uso intensivo de entrada/salida (E/S). Para aprovechar la ventaja de la velocidad y el rendimiento de discos de Premium Storage, puede migrar los discos de VM existentes a SSD Premium. Los discos SSD Premium son adecuados para aplicaciones de producción críticas.

### <a name="disk-size"></a>Tamaño del disco

Los tamaños marcados con un asterisco están actualmente en versión preliminar.

| Tamaños de SSD Premium  | P4               | P6               | P10             | P15 | P20              | P30              | P40              | P50              | P60*              | P70*              | P80*              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Tamaño de disco en GiB           | 32             | 64             | 128            | 256  | 512            | 1024    | 2 048     | 4095    | 8192     | 16 384     | 32 767     |
| IOPS por disco       | Hasta 120 | Hasta 240              | Hasta 500              | Hasta 1100 | Hasta 2300              | Hasta 5000              | Hasta 7500             | Hasta 7500              | Hasta 12 500              | Hasta 15 000              | Hasta 20 000              |
| Rendimiento de disco | Hasta 25 MiB/s | Hasta 50 MiB/s | Hasta 100 MiB/s | Hasta 125 MiB/s | Hasta 150 MiB/s | Hasta 200 MiB/s | Hasta 250 MiB/s | Hasta 250 MiB/s| Hasta 480 MiB/s | Hasta 750 MiB/s | Hasta 750 MiB/s |

## <a name="standard-ssd"></a>SSD estándar

Los discos SSD estándar de Azure son una opción de almacenamiento rentable, optimizada para cargas de trabajo que necesitan un rendimiento constante en niveles inferiores de IOPS. Los discos SSD estándar ofrecen una buena experiencia de nivel de entrada para aquellos que desean pasarse a la nube, en especial si tiene problemas con la variación de las cargas de trabajo que se ejecutan en las soluciones de disco duro locales. Los discos SSD estándar ofrecen mayor disponibilidad, coherencia, confiabilidad y latencia en comparación con los discos HDD. Los discos SSD estándar son convenientes para servidores web, servidores de aplicaciones con IOPS bajas, aplicaciones empresariales de poco uso y cargas de trabajo de desarrollo/pruebas.

### <a name="disk-size"></a>Tamaño del disco

Los tamaños marcados con un asterisco están actualmente en versión preliminar.

| Tamaños de SSD estándar  | E10               | E15               | E20             | E30 | E40              | E50              | E60*              | E70*              | E80*              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Tamaño de disco en GiB           | 128             | 256             | 512            | 1024  | 2 048            | 4095     | 8192     | 16 384     | 32 767    |
| IOPS por disco       | Hasta 500              | Hasta 500              | Hasta 500              | Hasta 500 | Hasta 500              | Hasta 500              | Hasta 500             | Hasta 500              | Hasta 1300              | Hasta 2000              | Hasta 2000              |
| Rendimiento de disco | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s| Hasta 300 MiB/s |  Hasta 500 MiB/s | Hasta 500 MiB/s |

## <a name="standard-hdd"></a>HDD estándar

Los discos HDD estándar de Azure ofrecen compatibilidad de discos confiable y de bajo coste para las máquinas virtuales que ejecutan cargas de trabajo que no tienen en cuenta la latencia. También admite blobs, tablas, colas y archivos. Con Standard Storage, los datos se almacenan en unidades de disco duro (HDD). Cuando se trabaja con máquinas virtuales, se pueden usar discos SSD y HDD estándar para escenarios de desarrollo/pruebas y para cargas de trabajo menos críticas. Standard Storage está disponible en todas las regiones de Azure.

### <a name="disk-size"></a>Tamaño del disco

Los tamaños marcados con un asterisco están actualmente en versión preliminar.

| Tipo de disco Estándar  | S4               | S6               | S10             | S15 | S20              | S30              | S40              | S50              | S60*              | S70*              | S80*              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Tamaño de disco en GiB          | 32             | 64             | 128            | 256  | 512            | 1024    | 2 048     | 4095    | 8192     | 16 384     | 32 767     |
| IOPS por disco       | Hasta 500              | Hasta 500              | Hasta 500              | Hasta 500 | Hasta 500              | Hasta 500              | Hasta 500             | Hasta 500              | Hasta 1300              | Hasta 2000              | Hasta 2000              |
| Rendimiento de disco | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s | Hasta 60 MiB/s| Hasta 300 MiB/s | Hasta 500 MiB/s | Hasta 500 MiB/s |

## <a name="billing"></a>Facturación

Al usar Managed Disks, se aplican las siguientes consideraciones de facturación:

- Tipo de disco
- Tamaño del disco administrado
- Instantáneas
- Transferencias de datos de salida
- Número de transacciones

**Tamaño del disco administrado**: los discos administrados se facturan por el tamaño aprovisionado. Azure asigna el tamaño aprovisionado (redondeado al alza) a la oferta de tamaño de disco más cercana. Para obtener detalles sobre los tamaños de disco ofrecidos, consulte las tablas anteriores. Cada disco se asigna a una oferta de tamaño de disco aprovisionado compatible y se factura según corresponda. Por ejemplo, si ha aprovisionado un disco SSD estándar de 200 GiB, se asigna a la oferta de tamaño de disco E15 (256 GiB). La facturación de cualquier disco aprovisionado se prorratea cada hora con el precio mensual de la oferta de Premium Storage. Por ejemplo, si ha aprovisionado un disco E10 y lo ha eliminado después de 20 horas, se factura la oferta E10 prorrateada a 20 horas. Esto es así independientemente de la cantidad de datos que se escriba en el disco.

**Instantáneas**: Las instantáneas se facturan según el tamaño utilizado. Por ejemplo, si crea una instantánea de un disco administrado con capacidad aprovisionada de 64 GiB y el tamaño de datos usado real es de 10 GiB, solo se le cobra por el tamaño de datos usado de 10 GiB.
