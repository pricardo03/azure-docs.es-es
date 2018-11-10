---
title: 'Información general de implementación: Avere vFXT para Azure'
description: Información general de la implementación de Avere vFXT para Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: aa5737d67ea2c9cb8cc7c7098764ae67fc91137d
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669822"
---
# <a name="avere-vfxt-for-azure---deployment-overview"></a>Avere vFXT para Azure: información general de la implementación

En este artículo se proporciona información general sobre los pasos necesarios para obtener un clúster funcional de Avere vFXT para Azure.

La primera vez que implemente un sistema de Avere vFXT, observará que tiene que realizar más pasos que la implementación de la mayoría de las herramientas de Azure. Tener una idea clara de los pasos que debe realizar le ayudará a calcular el esfuerzo necesario para completar el proceso de principio a fin. Una vez que el sistema esté en funcionamiento, su capacidad para agilizar las tareas de procesamiento basadas en la nube hará que valga la pena ese esfuerzo.

## <a name="deployment-steps"></a>Pasos de implementación

Después de [diseñar el sistema](avere-vfxt-deploy-plan.md), puede empezar a crear el clúster de Avere vFXT. 

Comience creando un controlador de clústeres de máquina virtual, que se usará para crear el clúster de vFXT.

Una vez que el clúster de vFXT esté en funcionamiento, le interesará saber cómo conectarlo a los clientes y, si es necesario, cómo mover los datos al nuevo contenedor de Blob Storage.  

Aquí encontrará información general de todos los pasos.

1. Configuración de los requisitos previos 

   Antes de crear una máquina virtual, debe crear una nueva suscripción para el proyecto de Avere vFXT, configurar la propiedad de la suscripción, comprobar las cuotas y solicitar un aumento si fuera necesario y aceptar los términos para usar el software de Avere vFXT. Lea [Prepare to create the Avere vFXT](avere-vfxt-prereqs.md) (Prepararse para crear una instancia de Avere vFXT) para obtener instrucciones detalladas.

1. Cree el controlador del clúster

   El *controlador de clúster* es una máquina virtual sencilla que se encuentra en la misma red virtual que el clúster de Avere vFXT. El controlador crea los nodos de vFXT y da forma al clúster; asimismo, también se encarga de proporcionar una interfaz de línea de comandos para administrar el clúster durante su duración.

   Si configura el controlador con una dirección IP pública, también puede servir como un host de salto para conectarse al clúster desde fuera de la red virtual.

   Todo el software necesario para crear el clúster de vFXT y administrar sus nodos está preinstalado en el controlador de clúster.

   Lea [Create the cluster controller VM](avere-vfxt-deploy.md#create-the-cluster-controller-vm) (Crear la máquina virtual del controlador de clúster) para obtener más detalles.

1. Cree el rol de runtime para los nodos del clúster 

   Azure usa el [control de acceso basado en rol](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC) para autorizar el nodo de clúster de las máquinas virtuales y así poder realizar ciertas tareas. Por ejemplo, los nodos de clúster deben ser capaces de asignar o reasignar las direcciones IP a otros nodos de clúster. Antes de crear el clúster, debe definir un rol que les proporcione los permisos adecuados.

   El software preinstalado del controlador de clúster incluye un rol de prototipo para que pueda personalizarlo. Lea [Create the cluster node access role](avere-vfxt-deploy.md#create-the-cluster-node-access-role) (Crear el rol de acceso del nodo de clúster) para obtener instrucciones.

1. Cree el clúster de Avere vFXT 

   En el controlador, edite el script de creación de clúster adecuado y ejecútelo para crear el clúster. En la sección [Edit the deployment script](avere-vfxt-deploy.md#edit-the-deployment-script) (Editar el script de implementación) obtendrá instrucciones detalladas. 

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