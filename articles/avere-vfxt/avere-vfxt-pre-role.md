---
title: 'Creación del rol Avere sin controlador: Avere vFXT for Azure'
description: Método para crear el rol RBAC necesario sin una VM de controlador del clúster
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 547a0e0ac5254408a4064d627ec4c1e7c354a433
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50670032"
---
# <a name="create-the-avere-vfxt-cluster-runtime-access-role-without-a-controller"></a>Creación del rol de acceso en tiempo de ejecución del clúster Avere vFXT sin un controlador

En este documento se muestra un método para crear el rol de acceso del nodo de clúster desde la línea de comandos antes de crear la VM de controlador del clúster. 

Para crearlo desde el controlador del clúster, lea [Creación del rol de acceso del nodo de clúster](avere-vfxt-deploy.md#create-the-cluster-node-access-role). La imagen del controlador incluye un archivo de prototipo de rol. Puede actualizar el archivo con el identificador de suscripción y usarlo para definir el rol localmente en la VM de controlador.

## <a name="create-an-azure-rbac-role"></a>Creación de un rol RBAC de Azure

El sistema Avere vFXT usa el [control de acceso basado en roles](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC) para autorizar a los nodos del clúster vFXT realizar las tareas necesarias.  

Como parte de la operación normal del clúster vFXT, los nodos de vFXT individuales deben hacer cosas como leer las propiedades de los recursos de Azure, administrar el almacenamiento y controlar la configuración de la interfaz de red de otros nodos. 

Este rol se usa para los nodos vFXT únicamente, no para la VM de controlador del clúster.  

Si quiere crear el rol antes del controlador, siga estos pasos: 

1. Abra Azure Cloud Shell en Azure Portal o vaya a [https://shell.azure.com](https://shell.azure.com).

1. Use el comando de la CLI de Azure para cambiar a su suscripción de vFXT:

   ```az account set --subscription YOUR_SUBSCRIPTION_ID```

1. Use estos comandos para descargar la definición de roles de la imagen de Marketplace y editarla. 

   ```bash
   wget -O- https://averedistribution.blob.core.windows.net/public/vfxtdistdoc.tgz | tar zxf - avere-cluster.json
   vi avere-cluster.json
   ``` 

4. Edite el archivo para incluir el identificador de suscripción y elimine la línea encima de él. Guarde el archivo como ``avere-cluster.json``.

   ![Editor de texto de la consola que muestra el identificador de suscripción y "eliminar esta línea" seleccionado para su eliminación](media/avere-vfxt-edit-role.png)

5. Cree el rol:  

   ```bash
   az role definition create --role-definition /avere-cluster.json
   ```

Al crear el clúster, proporcione el nombre del rol (en este caso, `avere-cluster`). El script de creación del clúster asigna el rol a los nodos del clúster recién creados. 

## <a name="sample-cluster-node-runtime-role-definition"></a>Ejemplo de definición de roles en tiempo de ejecución de nodos del clúster

> [!IMPORTANT] 
> Esta definición de ejemplo se tomó de una versión anterior a la disponibilidad general del producto. Si la versión que obtiene con la distribución actual del producto es diferente, use esa versión en su lugar.

```json

{
    "AssignableScopes": [
        "/subscriptions/YOUR_SUBSCRIPTION_ID_GOES_HERE"
    ],
    "Name": "avere-cluster",
    "IsCustom": "true",
    "Description": "Avere cluster runtime role",
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