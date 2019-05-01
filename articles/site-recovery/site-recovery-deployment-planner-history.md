---
title: Historial de versiones de Azure Site Recovery Deployment Planner
description: Correcciones de Site Recovery Deployment Planner versiones diferentes conocidas y limitaciones conocidas junto con sus fechas de lanzamiento.
services: site-recovery
author: Daya-Patil
manager: carmonm
ms.topic: article
ms.service: site-recovery
ms.date: 04/24/2019
ms.author: dapatil
ms.openlocfilehash: 2edf7ce3be1402a497ceab5b826a89ee43c5c39b
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64927378"
---
# <a name="azure-site-recovery-deployment-planner-version-history"></a>Historial de versiones de Azure Site Recovery Deployment Planner

En este artículo se proporciona el historial de todas las versiones de Azure Site Recovery Deployment Planner junto con las correcciones, conocida las limitaciones de cada uno de ellos y sus fechas de lanzamiento.

## <a name="version-24"></a>Versión 2.4

**Fecha de lanzamiento: 17 de abril de 2019**

**Correcciones:**

- Mejorado la compatibilidad del sistema operativo, específicamente al control de errores de localización.
- (Renovación) de la frecuencia de cambio de máquinas virtuales se ha agregado con un máximo de 20 Mbps de datos a la lista de comprobación de compatibilidad.
- Mensajes de error mejorados
  - Se agregó compatibilidad para vCenter 6.7.
  - Se agregó compatibilidad para la estación de trabajo de Windows Server 2019 y Red Hat Enterprise Linux (RHEL).



## <a name="version-23"></a>Versión 2.3

**Fecha de lanzamiento: 3 de diciembre de 2018**

**Correcciones:**

- Se ha corregido un problema que impedía Deployment Planner genere un informe con la ubicación de destino proporcionado y la suscripción.

## <a name="version-22"></a>Versión 2.2 

**Fecha de lanzamiento: 25 de abril de 2018**

**Correcciones:**

- Operaciones de GetVMList:
  - Se ha corregido un problema que provocaba GetVMList producir un error si no existe la carpeta especificada. Lo ahora crea el directorio predeterminado, o crea el directorio especificado en el parámetro outputfile.
  - Agregar más motivos del error para GetVMList.
- Se ha agregado información de tipo de máquina virtual como una columna en la hoja de máquinas virtuales compatible del informe de Deployment Planner.
- Hyper-V para la recuperación ante desastres de Azure:
  - Discos excluidos de máquinas virtuales con compartido de discos duros virtuales y paso a través de generación de perfiles. La operación Startprofiling muestra la lista de máquinas virtuales excluidas en la consola.
  - Máquinas virtuales se ha agregado con más de 64 discos a la lista de máquinas virtuales incompatibles.
  - Actualizar la replicación inicial (IR) y el factor de compresión delta replication (DR).
  - Se ha agregado compatibilidad limitada para el almacenamiento SMB.

## <a name="version-21"></a>Versión 2.1

**Fecha de lanzamiento: 3 de enero de 2018**

**Correcciones:**

- Actualiza el informe de Excel.
- Errores corregidos en la operación de GetThroughput.
- Agregado la opción para limitar el número de máquinas virtuales para generar perfiles o generar el informe. El límite predeterminado es 1000 máquinas virtuales.
- VMware para la recuperación ante desastres de Azure:
  - Se ha corregido un problema de VM de Windows Server 2016 en la tabla no compatible. 
  - Mensajes de compatibilidad actualizada para máquinas virtuales de Windows Extensible Firmware Interface (EFI).
- Actualiza el VMware a Hyper-V a Azure y Azure, límite por máquina virtual de renovación de datos de la máquina virtual. 
- Confiabilidad mejorada de análisis de archivo de lista de máquinas virtuales.

## <a name="version-201"></a>Versión 2.0.1

**Fecha de lanzamiento: 7 de diciembre de 2017**

**Correcciones:**

- Se ha agregado recomendaciones para optimizar el ancho de banda de red.

## <a name="version-20"></a>Versión 2.0

**Fecha de lanzamiento: 28 de noviembre de 2017**

**Correcciones:**

- Se ha agregado compatibilidad con Hyper-V para la recuperación ante desastres de Azure.
- Calculadora de costo añadido.
- Se ha agregado del sistema operativo comprobación de la versión de VMware para la recuperación ante desastres de Azure para determinar si la máquina virtual es compatible o incompatible para la protección. La herramienta usa la cadena de versión del sistema operativo devuelto por el servidor vCenter para esa máquina virtual. Es la versión de sistema operativo invitado que el usuario ha seleccionado al crear la máquina virtual en VMware.

**Limitaciones conocidas:**

- Para Hyper-V para la recuperación ante desastres de Azure, la máquina virtual con nombre que contiene los caracteres, como: `,`, `"`, `[`, `]`, y ``` ` ``` no se admiten. Si se generan perfiles, generación de informes se producirá un error o tendrá un resultado incorrecto.
- Para VMware para la recuperación ante desastres de Azure, no se admite la máquina virtual con nombre que contiene comas. Si ha generado perfil, se produce un error de generación de informes o tendrá un resultado incorrecto.

## <a name="version-131"></a>Versión 1.3.1

**Fecha de lanzamiento: 19 de julio de 2017** 

**Correcciones:**

- Se agregó compatibilidad con discos de gran tamaño (> 1 TB) en la generación de informes. Ahora puede usar Deployment Planner para planear la replicación de máquinas virtuales que tengan discos cuyo tamaño supere 1 TB (hasta 4095 GB).
Para más información, consulte [Compatibilidad con discos de gran tamaño en Azure Site Recovery](https://azure.microsoft.com/blog/azure-site-recovery-large-disks/)

## <a name="version-13"></a>Versión 1.3

**Fecha de lanzamiento: 9 de mayo de 2017**

**Correcciones:**

- Se agregó compatibilidad para disco administrado en la generación de informes. El número de máquinas virtuales que se pueden colocar en una cuenta de almacenamiento individual se calcula en función de si el disco administrado está seleccionado para la conmutación por error/conmutación.

## <a name="version-12"></a>versión 1.2

**Fecha de lanzamiento: 7 de abril de 2017**

**Correcciones:**

- Se ha agregado arranque (BIOS o EFI) comprobaciones de tipo para cada máquina virtual determinar si la máquina virtual es compatible o incompatible para la protección.
- Sistema operativo se ha agregado información de tipo para cada máquina virtual en las máquinas virtuales compatibles y las hojas de cálculo de las máquinas virtuales incompatibles.
- Se agregó compatibilidad para la operación de GetThroughput para las regiones US Government y China Microsoft Azure.
- Se han agregado algunas comprobaciones más de requisitos previos para el servidor vCenter y ESXi.
- Se ha corregido un problema de informe incorrecto que se generan cuando la configuración regional se establece en distintos del inglés.

## <a name="version-11"></a>Versión 1.1

**Fecha de lanzamiento: 9 de marzo de 2017**

**Correcciones:**

- Se ha corregido un problema que impedía que las máquinas virtuales de generación de perfiles cuando hay dos o más máquinas virtuales con el mismo nombre o dirección IP entre los distintos hosts ESXi de vCenter.
- Se ha corregido un problema que provocaba la copia y búsqueda en deshabilitado para las máquinas virtuales compatibles y las hojas de cálculo de las máquinas virtuales incompatibles.

## <a name="version-10"></a>Versión 1.0

**Fecha de lanzamiento: 23 de febrero de 2017**

**Limitaciones conocidas:**

- Admite solo para VMware en escenarios de recuperación ante desastres de Azure. Hyper-v en escenarios de recuperación ante desastres de Azure, use el [herramienta capacity planner de Hyper-V](./site-recovery-capacity-planning-for-hyper-v-replication.md).
- No es compatible con la operación de GetThroughput para las regiones US Government y China Microsoft Azure.
- El perfil de cann't herramienta máquinas virtuales, si el servidor vCenter tiene dos o más máquinas virtuales con el mismo nombre o dirección IP entre los distintos hosts ESXi.
En esta versión, la herramienta omite la generación de perfiles de los nombres o direcciones IP de máquinas virtuales duplicados de VMListFile. La solución alternativa consiste en generar perfiles de las máquinas virtuales mediante un host de ESXi, en lugar del servidor vCenter. Asegúrese de ejecutar una instancia para cada host de ESXi.
