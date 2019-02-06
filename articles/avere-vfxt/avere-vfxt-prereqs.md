---
title: Requisitos previos de Avere vFXT - Azure
description: Requisitos previos de Avere vFXT para Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: 9c3301ba16bfaeb7014658a380e287a36a505be8
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55299214"
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

Si no quiere conceder acceso de propietario a los usuarios que vayan a crear la instancia de vFXT, tiene dos soluciones alternativas:

* El propietario de un grupo de recursos puede crear un clúster si se cumplen estas condiciones:

  * El propietario de la suscripción debe [aceptar los términos de software correspondientes a Avere vFXT](#accept-software-terms) y [crear el rol de acceso del nodo de clúster](#create-the-cluster-node-access-role). 
  * Todos los recursos de Avere vFXT deben implementarse en el grupo de recursos, incluyendo los siguientes:
    * Controlador de clúster
    * Nodos de clúster
    * Almacenamiento de blobs
    * Elementos de la red
 
* Un usuario sin privilegios de propietario puede crear clústeres de vFXT con control de acceso basado en rol (RBAC) antes de tiempo para asignar privilegios al usuario. Este método proporciona permisos significativos a estos usuarios. En [este artículo](avere-vfxt-non-owner.md) se explica cómo crear un rol de acceso para autorizar a los usuarios no propietarios a crear clústeres.

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

## <a name="accept-software-terms"></a>Aceptar los términos de software

> [!NOTE] 
> Este paso no es necesario si un propietario de la suscripción crea el clúster de Avere vFXT.

Durante la creación del clúster, debe aceptar los términos del servicio para el software de Avere vFXT. Si no es el propietario de la suscripción, tiene que pedir a un propietario de la suscripción que acepte los términos antes de realizar cualquier operación. Este paso solo se debe hacer una vez por suscripción.

Para aceptar los términos de software de antemano: 

1. Abra una instancia de Cloud Shell en Azure Portal o vaya a <https://shell.azure.com>. Inicie sesión con el id. de la suscripción.

   ```azurecli
    az login
    az account set --subscription abc123de-f456-abc7-89de-f01234567890
   ```

1. Use este comando para aceptar los términos de servicio y habilitar el acceso mediante programación para Avere vFXT y así obtener la imagen de software de Azure: 

   ```azurecli
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-controller:latest
   ```

## <a name="create-access-roles"></a>Crear reglas de acceso 

El [control de acceso basado en rol](../role-based-access-control/index.yml) (RBAC) da a los nodos de clúster y al controlador de clúster de vFXT la autorización para realizar las tareas necesarias.

* El controlador de clúster necesita permiso para crear y modificar máquinas virtuales para poder crear el clúster. 

* Los nodos de vFXT individuales deben hacer cosas como leer las propiedades de los recursos de Azure, administrar el almacenamiento y controlar la configuración de la interfaz de red de otros nodos como parte de la operación normal del clúster vFXT.

Para poder crear el clúster de Avere vFXT, primero debe definir un rol personalizado para usarlo con los nodos de clúster. 

En el caso del controlador de clúster, puede aceptar el rol predeterminado de la plantilla. El predeterminado proporciona al controlador de clúster privilegios de propietario para el grupo de recursos. Si prefiere crear un rol personalizado para el controlador, vea [Rol de acceso al controlador personalizado](avere-vfxt-controller-role.md).

> [!NOTE] 
> Solo el propietario de una suscripción, un usuario con el rol de propietario o un administrador de acceso de usuario pueden crear roles. Los roles se pueden crear con antelación.  

### <a name="create-the-cluster-node-access-role"></a>Cree el rol de acceso del nodo de clúster.

<!-- caution - this header is linked to in the template so don't change it unless you can change that -->

Puede crear el rol del nodo de clúster antes de crear el clúster de Avere vFXT for Azure.

> [!TIP] 
> Los usuarios internos de Microsoft deben usar el rol existente denominado "Avere Cluster Runtime Operator" (Operador de tiempo de ejecución de clúster Avere) en lugar de intentar crear uno. 

1. Copie este archivo. Agregue el identificador de suscripción en la línea AssignableScopes.

   (La versión actual de este archivo se almacena en el repositorio github.com/Azure/Avere como [AvereOperator.txt](https://github.com/Azure/Avere/blob/master/src/vfxt/src/roles/AvereOperator.txt)).  

   ```json
   {
      "AssignableScopes": [
          "/subscriptions/PUT_YOUR_SUBSCRIPTION_ID_HERE"
      ],
      "Name": "Avere Operator",
      "IsCustom": "true",
      "Description": "Used by the Avere vFXT cluster to manage the cluster",
      "NotActions": [],
      "Actions": [
          "Microsoft.Compute/virtualMachines/read",
          "Microsoft.Network/networkInterfaces/read",
          "Microsoft.Network/networkInterfaces/write",
          "Microsoft.Network/virtualNetworks/subnets/read",
          "Microsoft.Network/virtualNetworks/subnets/join/action",
          "Microsoft.Network/networkSecurityGroups/join/action",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
          "Microsoft.Storage/storageAccounts/blobServices/containers/read",
          "Microsoft.Storage/storageAccounts/blobServices/containers/write"
      ],
      "DataActions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ]
   }
   ```

1. Guarde el archivo como ``avere-operator.json`` o con un nombre de archivo similar que sea fácil de recordar. 


1. Abra una instancia de Azure Cloud Shell e inicie sesión con el identificador de la suscripción (según se ha descrito [anteriormente en este documento](#accept-software-terms)). Use este comando para crear el rol:

   ```bash
   az role definition create --role-definition /avere-operator.json
   ```

El nombre del rol se usa al crear el clúster. En este ejemplo, el nombre es ``avere-operator``.

## <a name="next-step-create-the-vfxt-cluster"></a>Paso siguiente: Creación del clúster de vFXT

Después de completar estos requisitos previos, puede crear el clúster propiamente dicho. Lea [Deploy the vFXT cluster](avere-vfxt-deploy.md) (Implementar el clúster de vFXT) para obtener más instrucciones.