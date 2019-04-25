---
title: 'Información general de implementación: Avere vFXT para Azure'
description: Información general de la implementación de Avere vFXT para Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: v-erkell
ms.openlocfilehash: 88305e5d877f5bc3cc7ebb116f69b0f49f162b79
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60409923"
---
# <a name="avere-vfxt-for-azure---deployment-overview"></a>Avere vFXT para Azure: información general de la implementación

En este artículo se proporciona información general sobre los pasos necesarios para obtener un clúster funcional de Avere vFXT para Azure.

Se necesitan varias tareas antes y después de crear el clúster de vFXT desde Azure Marketplace. Tener una idea clara de los pasos que debe realizar le ayudará a calcular el esfuerzo necesario para completar el proceso de principio a fin. 

## <a name="deployment-steps"></a>Pasos de implementación

Después de [diseñar el sistema](avere-vfxt-deploy-plan.md), puede empezar a crear el clúster de Avere vFXT. 

Una plantilla de Azure Resource Manager en Azure Marketplace recopila la información necesaria e implementa automáticamente todo el clúster. 

Una vez que el clúster de vFXT esté en funcionamiento, le interesará saber cómo conectarlo a los clientes y, si es necesario, cómo mover los datos al nuevo contenedor de Blob Storage.  

Aquí encontrará información general de todos los pasos.

1. Configuración de los requisitos previos 

   Antes de crear una máquina virtual, debe crear una nueva suscripción para el proyecto de Avere vFXT, configurar la propiedad de la suscripción, comprobar las cuotas y solicitar un aumento si fuera necesario y aceptar los términos para usar el software de Avere vFXT. Lea [Prepare to create the Avere vFXT](avere-vfxt-prereqs.md) (Prepararse para crear una instancia de Avere vFXT) para obtener instrucciones detalladas.

1. Cree el clúster de Avere vFXT 

   Use Azure Marketplace para crear el clúster de Avere vFXT for Azure. Una plantilla recopila la información necesaria y ejecuta scripts para crear el producto final.

   La creación del clúster conlleva estos pasos, que se realizan con la plantilla de Marketplace: 

   * Crear una nueva infraestructura de red y grupos de recursos, si fuera necesario
   * Crear un *controlador de clúster*  

     El controlador de clúster es una máquina virtual sencilla que se encuentra en la misma red virtual que el clúster de Avere vFXT y tiene el software personalizado necesario para crear y administrar el clúster. El controlador crea los nodos de vFXT y da forma al clúster; asimismo, también se encarga de proporcionar una interfaz de línea de comandos para administrar el clúster durante su duración.

     Si crea una nueva red virtual durante la implementación, el controlador tendrá una dirección IP pública. Esto significa que el controlador puede actuar como un host de salto para conectarse al clúster desde fuera de la red virtual.

   * Crear máquinas virtuales para nodos de clúster

   * Configurar las máquinas virtuales para formar el clúster de nodo de clúster

   * Opcionalmente, crear un contenedor de blobs y configurarlo como un almacenamiento back-end para el clúster

1. Configuración del clúster 

   Conéctese a la interfaz de configuración de Avere vFXT (panel de control de Avere) para personalizar la configuración del clúster. Puede participar en la opción de supervisión del soporte técnico y agregar el sistema de almacenamiento si usa un centro de datos local.

   * [Obtener acceso al clúster de vFXT](avere-vfxt-cluster-gui.md)
   * [Habilitar el soporte técnico](avere-vfxt-enable-support.md)
   * [Configurar el almacenamiento](avere-vfxt-add-storage.md) (si fuera necesario)

1. Agregar clientes

   Siga las instrucciones para [montar el clúster de Avere vFXT ](avere-vfxt-mount-clients.md) si quiere obtener información sobre el equilibrio de carga y cómo los equipos cliente deben usar el clúster.

1. Agregar datos (si fuera necesario)

   Dado que Avere vFXT es una caché multicliente escalable, la mejor manera de mover datos a un nuevo contenedor de almacenamiento de back-end es usar la estrategia de copia de archivos multicliente y multiproceso. Lea [Moving data to the vFXT cluster](avere-vfxt-data-ingest.md) (Mover datos al clúster de vFXT) para obtener más información.

## <a name="next-steps"></a>Pasos siguientes

Para completar las tareas preliminares para implementar Avere vFXT para Azure, vaya a [Prepare to create the Avere vFXT](avere-vfxt-prereqs.md) (Prepararse para crear Avere vFXT). 