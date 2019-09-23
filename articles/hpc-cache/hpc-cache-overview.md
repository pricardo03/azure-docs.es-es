---
title: Introducción a Azure HPC Cache
description: Descripción de Azure HPC Cache, una solución de aceleración del acceso a los archivos para la informática de alto rendimiento
author: ekpgh
ms.service: hpc-cache
ms.topic: overview
ms.date: 09/06/2019
ms.author: v-erkell
ms.openlocfilehash: 77eca3ef58733a616705fdaac1d2880b5a990e8a
ms.sourcegitcommit: 0fab4c4f2940e4c7b2ac5a93fcc52d2d5f7ff367
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71036882"
---
# <a name="what-is-azure-hpc-cache"></a>¿Qué es Azure HPC Cache?

Azure HPC Cache acelera el acceso a los datos para las tareas de informática de alto rendimiento (HPC). El almacenamiento en caché de archivos en Azure permite que la escalabilidad de la informática en la nube esté disponible incluso para los flujos de trabajo en los que se almacenan los datos mediante vínculos WAN, como por ejemplo el entorno de almacenamiento conectado a la red (NAS) local del centro de datos.

Azure HPC Cache es fácil de iniciar y supervisar desde el Azure Portal. El almacenamiento de NFS existente o los nuevos contenedores de blobs pueden formar parte de su espacio de nombres agregado, lo que hace que el acceso del cliente sea sencillo aunque cambie el destino de almacenamiento de back-end.

## <a name="use-cases"></a>Casos de uso

Azure HPC Cache mejora la productividad en flujos de trabajo como los siguientes:

* Flujo de trabajo de acceso a archivos de lectura intensiva
* Datos almacenados en almacenamiento accesible desde NFS, blobs de Azure o ambos
* Granjas de servidores de hasta 75 000 núcleos de CPU

Azure HPC Cache se puede agregar a una amplia variedad de flujos de trabajo de muchos sectores. Cualquier sistema en el que un gran número de máquinas necesite acceder a un conjunto de archivos a escala y con latencia baja se beneficiará de este servicio. En las secciones siguientes se proporcionan ejemplos específicos.

### <a name="visual-effects-vfx-rendering"></a>Representación de efectos visuales

En medios y entretenimiento, Azure HPC Cache acelera el acceso a los datos para los proyectos de representación en los que el tiempo es un factor crítico. Los flujos de trabajo de representación de efectos visuales suelen requerir procesamiento de última hora en un gran número de nodos de proceso. Los datos de estos flujos de trabajo suelen encontrarse en un entorno NAS local. Azure HPC Cache almacena en caché los datos de archivos en la nube para reducir la latencia y mejorar la flexibilidad para la representación a petición.

### <a name="life-sciences"></a>Ciencias biológicas

Muchos flujos de trabajo de ciencias biológicas pueden beneficiarse del almacenamiento en caché de archivos de escalabilidad horizontal.

Un instituto de investigación que quiera migrar sus flujos de trabajo de análisis genómico a Azure puede desplazarlos fácilmente mediante Azure HPC Cache. Dado que la memoria caché proporciona acceso a los archivos POSIX, puede ejecutar su flujo de trabajo de cliente existente en la nube sin cambios.

También se puede aprovechar Azure HPC Cache para mejorar la eficacia de tareas como el análisis secundario, la simulación farmacológica o el análisis de imágenes controlado por IA.

### <a name="financial-services-analytics"></a>Análisis de servicios financieros

Azure HPC Cache puede ayudar a acelerar los cálculos en los análisis cuantitativos, las cargas de trabajo en los análisis de riesgos y las simulaciones de Monte Carlo para ofrecer a las empresas de servicios financieros una mejor perspectiva para la toma de decisiones estratégicas.

## <a name="region-availability"></a>Disponibilidad en regiones

Azure HPC Cache está disponible en estas regiones de Azure:

* East US
* Este de EE. UU. 2
* Europa del Norte
* Europa occidental
* Sudeste asiático
* Oeste de EE. UU. 2

Consulte la [página del producto Azure HPC Cache](https://azure.microsoft.com/services/hpc-cache) para la información de disponibilidad más reciente.

## <a name="next-steps"></a>Pasos siguientes

* Lea la [página del producto Azure HPC Cache](https://azure.microsoft.com/services/hpc-cache) para más información sobre sus funcionalidades
* Información sobre los [requisitos previos](hpc-cache-prereqs.md) del producto
* [Creación de una instancia de Azure HPC Cache](hpc-cache-create.md) desde Azure Portal
