---
title: Pruebas comparativas de rendimiento para Azure Files de NetApp | Microsoft Docs
description: Describe los resultados de las pruebas comparativas de rendimiento para Azure Files de NetApp en el nivel de volumen.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: b-juche
ms.openlocfilehash: aca0668fc364518fe45d9fe94d089ee366b25676
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870888"
---
# <a name="performance-benchmarks-for-azure-netapp-files"></a>Pruebas comparativas de rendimiento para Azure Files de NetApp

Este artículo describe los resultados de las pruebas comparativas de rendimiento de Azure Files de NetApp en el nivel de volumen. 

## <a name="sample-application-used-for-the-tests"></a>Aplicación de ejemplo usada para las pruebas

Se ejecutaron las pruebas de rendimiento con una aplicación de ejemplo con Azure Files de NetApp. La aplicación tiene las siguientes características: 

* Una aplicación basada en Linux creada para la nube
* Puede escalar linealmente con máquinas de virtuales (VM) se ha agregado para aumentar la capacidad de proceso según sea necesario
* Requiere una accesibilidad rápida de data lake
* Tiene los patrones de E/S que a veces son aleatorios y a veces secuenciales 
    * Un patrón aleatorio requiere una latencia baja para grandes cantidades de E/S. 
    * Un patrón secuencial requiere grandes cantidades de ancho de banda. 

## <a name="about-the-workload-generator"></a>Sobre el generador de carga de trabajo

Los resultados se obtienen Vdbench archivos de resumen. [Vdbench](https://www.oracle.com/technetwork/server-storage/vdbench-downloads-1901681.html) es una utilidad de línea de comandos que genera cargas de trabajo de E/S de disco para validar el rendimiento del almacenamiento. La configuración de cliente / servidor utilizada es escalable.  Incluye un solo maestro/cliente mixto y cliente dedicados 14 máquinas virtuales.

## <a name="about-the-tests"></a>Acerca de las pruebas

Las pruebas se diseñaron para identificar los límites que podría tener la aplicación de ejemplo y el tiempo de respuesta que curvas hasta los límites.  

Se ejecutaron las pruebas siguientes: 

* Lectura del 100% 8 KiB aleatorio
* 100% 8 escritura aleatoria de KiB
* Lectura del 100% secuencial de 64 KiB
* 100% 64 KiB escritura secuencial
* 50% 64 KiB lectura secuencial, 50% 64 KiB escritura secuencial
* 8 KiB aleatorias de lectura de 50%, 50% 8 escritura aleatoria de KiB

## <a name="bandwidth"></a>Ancho de banda

Azure Files de NetApp ofrece varios [niveles de servicio](azure-netapp-files-service-levels.md). Cada nivel de servicio ofrece una cantidad de ancho de banda por cada TB de capacidad aprovisionada (cuota de volumen) diferente. El límite de ancho de banda para un volumen se aprovisiona en función de la combinación de la cuota de volumen y el nivel de servicio. Tenga en cuenta que el límite de ancho de banda es solo un factor para determinar la cantidad real de rendimiento que se alcanzará.  

Actualmente, la MiB 4.500 es el mayor rendimiento que se ha logrado una carga de trabajo en un único volumen en las pruebas.  Con el nivel de servicio Premium, una cuota de volumen de TiB 70.31 aprovisionará suficiente ancho de banda para obtener este rendimiento por el siguiente cálculo: 

![Fórmula de ancho de banda](../media/azure-netapp-files/azure-netapp-files-bandwidth-formula.png)

![Nivel de servicio y de cuota](../media/azure-netapp-files/azure-netapp-files-quota-service-level.png)

## <a name="throughput-intensive-workloads"></a>Cargas de trabajo intensivas de rendimiento

La prueba de rendimiento utiliza Vdbench y una combinación de máquinas virtuales de almacenamiento 12xD32s V3. El volumen de ejemplo en la prueba ha conseguido las cifras de rendimiento siguientes:

![Prueba de rendimiento](../media/azure-netapp-files/azure-netapp-files-throughput-test.png)

## <a name="io-intensive-workloads"></a>Cargas de trabajo de E/s intensivas

La prueba de E/S usa Vdbench y una combinación de máquinas virtuales de almacenamiento 12xD32s V3. El volumen de ejemplo en la prueba de logra los siguientes números de E/S:

![Prueba de E/S](../media/azure-netapp-files/azure-netapp-files-io-test.png)

## <a name="latency"></a>Latencia

La distancia entre las máquinas virtuales de prueba y el volumen de archivos de NetApp Azure tiene un impacto en el rendimiento de E/S.  El gráfico siguiente compara la e/s por segundo frente a las curvas de respuesta de latencia para dos conjuntos diferentes de las máquinas virtuales.  Es un conjunto de máquinas virtuales cerca de los archivos de NetApp de Azure y el otro conjunto es aún más lejos.  Tenga en cuenta que la mayor latencia para el conjunto de máquinas virtuales más tiene un impacto en la cantidad de IOPS se consigue en un nivel determinado de paralelismo.  En cualquier caso, lecturas con relación a un volumen pueden superar los 300 000 IOPS, tal como se muestra a continuación: 

![Estudio de latencia](../media/azure-netapp-files/azure-netapp-files-latency-study.png)

## <a name="summary"></a>Resumen

Las cargas de trabajo sensibles a la latencia (bases de datos) pueden tener un tiempo de respuesta un milisegundo. El rendimiento transaccional puede ser mayor de 300 KB IOPS para un único volumen.

Las aplicaciones sensibles al rendimiento (para streaming y creación de imágenes) pueden tener 4.5GiB / s de rendimiento de.
