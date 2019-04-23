---
title: Requisitos previos de Avere vFXT - Azure
description: Requisitos previos de Avere vFXT para Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: v-erkell
ms.openlocfilehash: 352833b12c00abbefcf7016d27dfb580ee25e450
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59786446"
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

Un usuario con permisos de propietario de la suscripción debe encargarse de crear el clúster de vFXT. Acepte los términos de servicio y realizar otras acciones, se necesitan permisos de propietario de la suscripción. 

Hay algunos escenarios de solución que permiten que no sea el propietario crear un vFTX Avere para el clúster de Azure. Estos escenarios implican restringir los recursos y la asignación de roles adicionales al creador. En ambos casos, un propietario de la suscripción debe también [acepte los términos de vFXT Avere](#accept-software-terms) antes de tiempo. 

| Escenario | Restricciones | Roles de acceso necesarios para crear el clúster de vFXT Avere | 
|----------|--------|-------|
| Administrador del grupo de recursos | La red virtual, controlador de clústeres y nodos de clúster deben crearse en el grupo de recursos | [Administrador de acceso de usuario](../role-based-access-control/built-in-roles.md#user-access-administrator) y [colaborador](../role-based-access-control/built-in-roles.md#contributor) roles, ambos con ámbito en el grupo de recursos de destino | 
| Red virtual externa | El controlador de clústeres y nodos de clúster se crean dentro del grupo de recursos, pero se usa una red virtual existente en otro grupo de recursos | (1) [Administrador de acceso de usuario](../role-based-access-control/built-in-roles.md#user-access-administrator) y [colaborador](../role-based-access-control/built-in-roles.md#contributor) roles con ámbito en el grupo de recursos vFXT; y (2) [colaborador de máquina Virtual](../role-based-access-control/built-in-roles.md#virtual-machine-contributor), [acceso de usuario Administrador](../role-based-access-control/built-in-roles.md#user-access-administrator), y [Avere colaborador](../role-based-access-control/built-in-roles.md#avere-contributor) roles con ámbito en el grupo de recursos de red virtual. |
 
Una alternativa es crear un rol de acceso personalizado basado en roles (RBAC) de control antes de tiempo y asignar los privilegios al usuario, como se explica en [en este artículo](avere-vfxt-non-owner.md). Este método proporciona permisos significativos a estos usuarios. 

## <a name="quota-for-the-vfxt-cluster"></a>Cuota del clúster de vFXT

Debe tener una cuota suficiente para los siguientes componentes de Azure. Si fuera necesario, [solicite un aumento de la cuota](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

> [!NOTE]
> Las máquinas virtuales y los componentes SSD que se enumeran aquí son para el propio clúster de vFXT. Necesitará una cuota adicional para las máquinas virtuales y la SSD que se va a usar en la granja de servidores de proceso.  Asegúrese de que la cuota está habilitada para la región donde se vaya a ejecutar el flujo de trabajo.

|Componente de Azure|Quota|
|----------|-----------|
|Máquinas virtuales|3 o más E32s_v3|
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

## <a name="create-a-storage-service-endpoint-in-your-virtual-network-if-needed"></a>Crear un punto de conexión de servicio de almacenamiento en la red virtual (si es necesario)

Un [punto de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md) mantiene el tráfico de Azure Blob local en lugar de enrutarlo fuera de la red virtual. Se recomienda para cualquier vFXT Avere para el clúster de Azure que usa Azure Blob para el almacenamiento de datos back-end. 

Si va a proporcionar una red virtual existente y crear un nuevo contenedor de blobs de Azure para su almacenamiento back-end como parte de la creación del clúster, debe tener un extremo de servicio en la red virtual para el almacenamiento de Microsoft. Este punto de conexión debe existir antes de crear el clúster, o se producirá un error en la creación. 

Se recomienda un punto de conexión de servicio de almacenamiento para cualquier vFXT Avere para el clúster de Azure que usa Azure Blob storage, incluso si agrega el almacenamiento más adelante. 

> [!TIP] 
> * Omita este paso si va a crear una nueva red virtual como parte de la creación del clúster. 
> * Este paso es opcional si no va a crear el almacenamiento de Blob durante la creación del clúster. En ese caso, puede crear el punto de conexión de servicio más adelante si decide usar Azure Blob.

Crear el punto de conexión de servicio de almacenamiento desde el portal de Azure. 

1. En el portal, haga clic en **Redes virtuales** a la izquierda.
1. Seleccione la red virtual para el clúster. 
1. Haga clic en **Extremos de servicio** a la izquierda.
1. Haga clic en **Agregar** en la parte superior.
1. Deje el servicio como ``Microsoft.Storage`` y elija la subred del clúster.
1. Haga clic en **Agregar**en la parte inferior.

   ![Captura de pantalla de Azure Portal con anotaciones de los pasos para crear el punto de conexión de servicio](media/avere-vfxt-service-endpoint.png)


## <a name="next-step-create-the-vfxt-cluster"></a>Paso siguiente: Creación del clúster de vFXT

Después de completar estos requisitos previos, puede crear el clúster propiamente dicho. Lea [Deploy the vFXT cluster](avere-vfxt-deploy.md) (Implementar el clúster de vFXT) para obtener más instrucciones.