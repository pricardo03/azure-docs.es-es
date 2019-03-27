---
title: Avere vFXT for Azure
description: Introducción a Avere vFXT for Azure, una capa de caché en la nube para la informática de alto rendimiento
author: ekpgh
ms.service: avere-vfxt
ms.topic: overview
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 724352ae9f7c66dfeab293473ea79438e3b36254
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2019
ms.locfileid: "56989326"
---
# <a name="what-is-avere-vfxt-for-azure"></a>¿Qué es Avere vFXT for Azure? 

Avere vFXT for Azure proporciona una solución de almacenamiento en caché del sistema de archivos para tareas de informática de alto rendimiento (HPC) en las que se usan muchos datos. Le permite aprovechar la escalabilidad de la informática en la nube para hacer que los datos sean accesibles cuando y donde se necesiten, incluso para los datos que están almacenados en su propio hardware local.

Avere vFXT admite estos escenarios de informática comunes: 

* Arquitectura de nube híbrida: Avere vFXT for Azure puede funcionar con un sistema de almacenamiento de hardware, que proporciona la ventaja de la informática en la nube sin tener que mover los archivos. 
* Ampliación en la nube: Avere vFXT for Azure puede ayudarle a mover los datos a la nube para un solo proyecto, o a migrar mediante lift-and-shift todo el flujo de trabajo de manera permanente. 

![diagrama que muestra los detalles del sistema Avere vFXT dentro de una suscripción de Azure conectada al almacenamiento de blobs y a un centro de datos local](media/avere-vfxt-hybrid.png)

Avere vFXT for Azure es perfecto para estas situaciones: 

* Operaciones de lectura intensiva para cargas de trabajo de informática de alto rendimiento
* Aplicaciones que utilizan el protocolo NFS común
* Granjas de servidores de proceso de 1000 a 40 000 núcleos de CPU
* Integración con el hardware local NAS, con Azure Blob Storage o ambos

Para más información, visite <https://azure.microsoft.com/services/storage/avere-vfxt/>.

## <a name="who-uses-avere-vfxt-for-azure"></a>¿Quién utiliza Avere vFXT for Azure? 

Avere vFXT puede ayudarle con todo tipo de tareas informáticas de lectura intensiva:

### <a name="visual-effects-rendering"></a>Representación de efectos visuales 

En medios y entretenimiento, el clúster de Avere vFXT puede acelerar el acceso a los datos para los proyectos de representación en los que el tiempo es un factor crítico. Debido a que puede agregar más espacio de caché así como más nodos de proceso en Azure, tiene la flexibilidad de administrar proyectos grandes de forma eficiente. 

### <a name="life-sciences"></a>Ciencias biológicas 

Avere vFXT puede permitir a los investigadores ejecutar sus flujos de trabajo de análisis secundarios en Azure Compute, y acceder a datos genómicos sin importar su ubicación.

En investigación farmacéutica, los clústeres de Avere vFXT pueden utilizarse para acelerar el descubrimiento de fármacos al ayudar a los investigadores a predecir las interacciones fármaco-objetivo y a analizar los datos de la investigación.

### <a name="financial-services-analytics"></a>Análisis de servicios financieros

Un clúster de Avere vFXT puede ayudar a acelerar los cálculos de análisis cuantitativo, lo que proporciona a las empresas de servicios financieros una mejor visión para tomar decisiones estratégicas. 

## <a name="features-and-specifications"></a>Características y especificaciones

El sistema Avere vFXT está formado por tres o más nodos del archivador perimetral virtual, configurados en un clúster. Puede estar situado cerca de las máquinas cliente, que montan el clúster en lugar de montar el almacenamiento directamente. 

El clúster de Avere vFXT almacena en caché los archivos a medida que se solicitan. Las solicitudes repetidas se pueden atender desde la caché más de 80 % del tiempo.

### <a name="compatibility"></a>Compatibilidad 

* Compatible con los sistemas NAS de hardware de NetApp o Dell EMC Isilon
* Compatible con blob de Azure
* Usa el protocolo NFSv3 o SMB2

Avere vFXT utiliza los siguientes recursos de Azure: 

|Componente de Azure|   |
|----------|-----------|
|Máquinas virtuales|3 o más E32s_v3|
|Almacenamiento SSD Premium|200 GB de espacio de sistema operativo y de 1 TB a 4 TB de espacio en caché por nodo |
|Cuenta de almacenamiento (opcional) |v2|
|Almacenamiento back-end de datos (opcional) | Un contenedor de blobs LRS vacío |

## <a name="next-steps"></a>Pasos siguientes

Aquí hay algunos vínculos para comenzar a crear su propia implementación de Avere vFXT. 

* [Planeamiento del sistema](avere-vfxt-deploy-plan.md)
* [Descripción general de la implementación](avere-vfxt-deploy-overview.md)
* [Creación de vFXT](avere-vfxt-deploy.md)
