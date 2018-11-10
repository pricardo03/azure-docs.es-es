---
title: Ajuste del clúster de Avere vFXT - Azure
description: Información general sobre la configuración personalizada para optimizar el rendimiento en Avere vFXT para Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 640c550e82c1b883970a3ea7a374a85989cd5e21
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669652"
---
# <a name="cluster-tuning"></a>Ajuste del clúster


La mayoría de los clústeres de vFXT pueden beneficiarse de la configuración de rendimiento personalizada. Estas opciones de configuración permiten que el clúster funcione mejor con un flujo de trabajo, un conjunto de datos y unas herramientas determinados. 

Esta opción de personalización debe realizarse junto con un representante del soporte técnico, ya que normalmente es necesario configurar características que no están disponibles en el panel de control de Avere.

En esta sección se explican algunas de las opciones de optimización personalizada que pueden realizarse.

<!-- 
[ xxx keep or not? \/ research this xxx ]

> [!TIP]
> The VDBench utility can be helfpul in generating I/O workloads to test a vFXT cluster. Read [Measuring vFXT Performance](vdbench.md) to learn more.

-->

## <a name="general-optimizations"></a>Optimizaciones generales

Estos cambios son recomendables según las cualidades del conjunto de datos o el estilo del flujo de trabajo.

* Si la carga de trabajo es pesada en cuanto a la escritura, aumente el tamaño de la memoria caché de escritura a partir de su valor predeterminado del 20 %. 
* Si el conjunto de datos tiene muchos archivos pequeños, aumente el límite de recuento de archivos de la memoria caché del clúster. 
* Si el trabajo implica copiar o mover datos entre dos repositorios, ajuste el número de subprocesos que se usan para mover datos: 
  * Para aumentar la velocidad, puede aumentar el número de subprocesos paralelos que se usan.
  * Si se está sobrecargando el volumen de almacenamiento de back-end, debe reducir el número de subprocesos paralelos que use.
* Si el clúster se almacena en la caché de datos de un archivador principal que usa las ACL de NFSv4, habilite el almacenamiento en caché del modo de acceso para agilizar la autorización de archivos para determinados clientes.

## <a name="cloud-nas-or-cloud-gateway-optimizations"></a>Optimizaciones en la nube de NAS o en la puerta de enlace en la nube

Para aprovechar las velocidades de datos más altas entre el clúster de vFXT y el almacenamiento en la nube en un escenario de NAS o puerta de enlace en la nube (donde el clúster de vFXT proporciona acceso de estilo NAS a un contenedor en la nube), su representante puede recomendarle cambiar opciones de configuración como éstas para insertar datos de forma más agresiva al volumen de almacenamiento desde la caché:

* Aumentar el número de conexiones TCP entre el clúster y el contenedor de almacenamiento.
* Disminuir el valor de tiempo de espera de REST para la comunicación entre el clúster y el almacenamiento para volver a intentar realizar las escrituras antes si el proceso no se realiza correctamente.  
* Aumentar el tamaño del segmento para que cada segmento de escritura de back-end transfiera un fragmento de datos de 8 MB de tamaño en lugar de 1 MB.

## <a name="cloud-bursting-or-hybrid-wan-optimizations"></a>Seguridad en la nube u optimizaciones WAN híbridas

En un escenario de seguridad en la nube o en uno dedicado a la optimización WAN del almacenamiento híbrido (donde el clúster de vFXT proporciona integración entre la nube y el almacenamiento de hardware local), estos cambios pueden serle útiles:

* Aumentar el número de conexiones TCP entre el clúster y el archivador principal.
* Habilitar la configuración de optimización WAN para el archivador principal remoto (esta configuración puede usarse para un archivador remoto local o para un archivador principal en la nube que se encuentre en otra región de Azure).
* Aumentar el tamaño de búfer del socket TCP (según las necesidades de rendimiento y carga de trabajo).
* Habilitar la configuración "Reenviar siempre" para reducir los archivos que están en la caché de forma redundante (según las necesidades del rendimiento y de la carga de trabajo).

## <a name="help-optimizing-your-avere-vfxt-for-azure"></a>Ayuda para optimizar Avere vFXT para Azure

Use el procedimiento descrito en [Get help with your system](avere-vfxt-open-ticket.md) (Obtener ayuda con el sistema) para ponerse en contacto con el personal de soporte técnico acerca de estas optimizaciones. 