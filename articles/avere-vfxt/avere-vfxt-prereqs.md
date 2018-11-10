---
title: Requisitos previos de Avere vFXT - Azure
description: Requisitos previos de Avere vFXT para Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 823bf50a54ff43fa95f7136c137e3d8f3303c3e0
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669532"
---
# <a name="prepare-to-create-the-avere-vfxt"></a>Preparación para la creación de Avere vFXT

En este artículo se explican las tareas previas necesarias para crear un clúster de Avere vFXT.

## <a name="create-a-new-subscription"></a>Crear una nueva suscripción

Comience creando nueva suscripción de Azure. Use una suscripción independiente para cada proyecto de Avere vFXT y así poder realizar un seguimiento sencillo de todos los recursos y gastos del proyecto, proteger otros proyectos ante una posible limitación de recursos durante el aprovisionamiento y simplificar la limpieza.  

Para crear una nueva suscripción de Azure en Azure Portal:

* Vaya a la [hoja de suscripciones](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
* Haga clic en el botón **+ Agregar** de la parte superior.
* Inicie sesión si es necesario.
* Seleccione una oferta y siga los pasos para crear una nueva suscripción.

## <a name="configure-subscription-owner-permissions"></a>Configurar los permisos de propietario de la suscripción

Un usuario con permisos de propietario de la suscripción debe encargarse de crear el clúster de vFXT. Es necesario tener permisos de propietario de la suscripción para realizar estas y otras acciones:

* Acepte los términos para el software de Avere vFXT.
* Cree el rol de acceso del nodo de clúster.
* Permita que el nodo del controlador de clúster administre los grupos de recursos y las redes virtuales. 
* Permita que el controlador de clúster cree y modifique los nodos del clúster. 

Si no quiere conceder acceso de propietario a los usuarios que vayan a crear la instancia de vFXT, tiene dos soluciones alternativas:

* El propietario de un grupo de recursos puede crear un clúster si se cumplen estas condiciones:

  * El propietario de la suscripción debe [aceptar los términos de software correspondientes a Avere vFXT](#accept-software-terms-in-advance) y [crear el rol de acceso del nodo de clúster](avere-vfxt-deploy.md#create-the-cluster-node-access-role).
  * Todos los recursos de Avere vFXT deben implementarse en el grupo de recursos, incluyendo los siguientes:
    * Controlador de clúster
    * Nodos de clúster
    * Almacenamiento de blobs
    * Elementos de la red
 
* Un usuario sin privilegios de propietario también puede crear clústeres de vFXT si se crea un rol de acceso adicional y se le asigna al usuario antes de realizar cualquier operación. Sin embargo, tenga en cuenta que este rol proporciona permisos significativos a estos usuarios. [En este artículo](avere-vfxt-non-owner.md) se explica cómo permitir que los usuarios no propietarios creen clústeres.

## <a name="quota-for-the-vfxt-cluster"></a>Cuota del clúster de vFXT

Debe tener una cuota suficiente para los siguientes componentes de Azure. Si fuera necesario, [solicite un aumento de la cuota](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

> [!NOTE]
> Las máquinas virtuales y los componentes SSD que se enumeran aquí son para el propio clúster de vFXT. Necesitará una cuota adicional para las máquinas virtuales y la SSD que se va a usar en la granja de servidores de proceso.  Asegúrese de que la cuota está habilitada para la región donde se vaya a ejecutar el flujo de trabajo.

|Componente de Azure|Quota|
|----------|-----------|
|Máquinas virtuales|3 o más D16s_v3 o E32s_v3|
|Almacenamiento SSD Premium|200 GB de espacio de sistema operativo y de 1 TB a 4 TB de espacio en caché por nodo |
|Cuenta de almacenamiento (opcional) |v2|
|Almacenamiento back-end de datos (opcional) |Un contenedor de blobs LRS nuevo |

## <a name="accept-software-terms-in-advance"></a>Aceptar los términos de software de antemano

> [!NOTE] 
> Este paso no es necesario si un propietario de la suscripción crea el clúster de Avere vFXT.

Para poder crear un clúster, debe aceptar los términos del servicio para el software de Avere vFXT. Si no es el propietario de la suscripción, tiene que pedir a un propietario de la suscripción que acepte los términos antes de realizar cualquier operación. Este paso solo se debe hacer una vez por suscripción.

Para aceptar los términos de software de antemano: 

1. Abra una instancia de Cloud Shell en Azure Portal o vaya a <https://shell.azure.com>. Inicie sesión con el id. de la suscripción.

   ```azurecli
    az login
    az account set --subscription abc123de-f456-abc7-89de-f01234567890
   ```

1. Use este comando para aceptar los términos de servicio y habilitar el acceso mediante programación para Avere vFXT para así obtener las imágenes de software de Azure: 

   ```azurecli
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-controller:latest
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-node:latest
   ```

## <a name="next-step-create-the-vfxt-cluster"></a>Siguiente paso: crear el clúster de vFXT

Después de completar estos requisitos previos, puede crear el clúster propiamente dicho. Lea [Deploy the vFXT cluster](avere-vfxt-deploy.md) (Implementar el clúster de vFXT) para obtener más instrucciones.