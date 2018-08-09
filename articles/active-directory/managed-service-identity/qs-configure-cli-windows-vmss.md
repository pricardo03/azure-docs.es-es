---
title: Cómo configurar las identidades asignadas por el usuario y por el sistema en un VMSS de Azure mediante la CLI de Azure
description: Instrucciones paso a paso para configurar identidades asignadas por el sistema y el usuario en un VMSS de Azure, mediante la CLI de Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2018
ms.author: daveba
ms.openlocfilehash: 6474b34abeceb58c2eff9e7a2d2237ec47e61933
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447530"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-azure-cli"></a>Configuración de una identidad de servicio administrada (MSI) en un conjunto de escalado de máquinas virtuales mediante la CLI de Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Managed Service Identity proporciona a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo obtendrá más información sobre cómo realizar las siguientes operaciones de Managed Service Identity en un conjunto de escalado de máquinas virtuales (VMSS) de Azure mediante la CLI de Azure:
- Habilitar y deshabilitar la identidad asignada por el sistema en un VMSS de Azure
- Agregar y quitar una identidad asignada por el usuario en un VMSS de Azure


## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con Managed Service Identity, consulte la [sección de introducción](overview.md). **No olvide revisar la [diferencia entre una identidad asignada por el sistema y una asignada por el usuario](overview.md#how-does-it-work)**.
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para llevar a cabo las operaciones de administración de este artículo, su cuenta debe tener las siguientes asignaciones de roles:
    - [Colaborador de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para crear un conjunto de escalado de máquinas virtuales y habilitar y quitar la identidad administrada asignada por el usuario o por el sistema desde un conjunto de escalado de máquinas virtuales.
    - Rol [Colaborador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) para crear una identidad asignada por el usuario.
    - Rol [Operador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-operator) para asignar y quitar una identidad asignada por el usuario en un conjunto de escalado de máquinas virtuales.
- Para ejecutar los ejemplos de script de la CLI, tiene tres opciones:
    - Usar [Azure Cloud Shell](../../cloud-shell/overview.md) desde Azure Portal (consulte la sección siguiente).
    - Usar Azure Cloud Shell integrado a través del botón "Pruébelo", situado en la esquina superior derecha de cada bloque de código.
    - [Instalar la versión más reciente de CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 o posterior), si prefiere usar una consola local de CLI. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Identidad asignada por el sistema

En esta sección, obtendrá información sobre cómo habilitar y deshabilitar la identidad asignada por el sistema para un VMSS mediante la CLI de Azure.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Habilitar la identidad asignada durante la creación de un conjunto de escalado de máquinas virtuales de Azure

Para crear un conjunto de escalado de máquinas virtuales con la identidad asignada por el sistema habilitada:

1. Si usa la CLI de Azure en una consola local, lo primero que debe hacer es iniciar sesión en Azure mediante el [inicio de sesión de az](/cli/azure/reference-index#az-login). Use una cuenta asociada a la suscripción de Azure en la que desearía implementar el conjunto de escalado de máquinas virtuales:

   ```azurecli-interactive
   az login
   ```

2. Cree un [grupo de recursos](../../azure-resource-manager/resource-group-overview.md#terminology) para contener e implementar el conjunto de escalado de máquinas virtuales y sus recursos relacionados, con [az group create](/cli/azure/group/#az-group-create). Puede omitir este paso si ya tiene un grupo de recursos que le gustaría usar en su lugar:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Cree un conjunto de escalado de máquinas virtuales con [az vmss create](/cli/azure/vmss/#az-vmss-create). En el ejemplo siguiente se crea un conjunto de escalado de máquinas virtuales denominado *myVMSS* con una identidad asignada por el sistema, como solicitó el parámetro `--assign-identity`. Los parámetros `--admin-username` y `--admin-password` especifican el nombre de usuario administrativo y la contraseña de la cuenta para el inicio de sesión en la máquina virtual. Actualice estos valores según convenga para su entorno: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Habilitar la identidad asignada en un conjunto de escalado de máquinas virtuales de Azure existente

Si necesita habilitar la identidad asignada en un conjunto de escalado de máquinas virtuales de Azure existente:

1. Si usa la CLI de Azure en una consola local, lo primero que debe hacer es iniciar sesión en Azure mediante el [inicio de sesión de az](/cli/azure/reference-index#az-login). Use una cuenta asociada a la suscripción de Azure que contenga el conjunto de escalado de máquinas virtuales.

   ```azurecli-interactive
   az login
   ```

2. Use el comando [aaz vmss identity assign](/cli/azure/vmss/identity/#az-vmss-identity-assign) para habilitar una identidad asignada por el sistema para una máquina virtual existente:

   ```azurecli-interactive
   az vmss identity assign -g myResourceGroup -n myVMSS
   ```

### <a name="disable-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Deshabilitar la identidad asignada de un conjunto de escalado de máquinas virtuales de Azure

Si tiene un conjunto de escalado de máquinas virtuales que ya no necesita la identidad asignada por el sistema, pero aún necesita identidades asignadas por el usuario, use el siguiente comando:

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```

Si tiene una máquina virtual que ya no necesita la identidad asignada por el sistema y no tiene identidades asignadas por el usuario, use el siguiente comando:

> [!NOTE]
> El valor `none` no distingue mayúsculas de minúsculas. Debe estar en minúscula. 

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type="none"
```

Para quitar la extensión de VM de MSI, use el comando [az vmss identity remove](/cli/azure/vmss/identity/#az-vmss-remove-identity) para quitar la identidad asignada por el sistema de un VMSS:

```azurecli-interactive
az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
```

## <a name="user-assigned-identity"></a>Identidad asignada por el usuario

En esta sección obtendrá más información sobre cómo habilitar y quitar una identidad asignada por el usuario mediante la CLI de Azure.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Asignación de una identidad asignada por el usuario durante la creación de un VMSS de Azure

En esta sección se explica el proceso de creación de un VMSS y de asignación de una identidad asignada por el usuario al VMSS. Si ya tiene un VMSS que quiera usar, omita esta sección y vaya a la siguiente.

1. Puede omitir este paso si ya tiene un grupo de recursos que le gustaría usar. Cree un [grupo de recursos](~/articles/azure-resource-manager/resource-group-overview.md#terminology) para contener e implementar la identidad asignada por el usuario mediante [az group create](/cli/azure/group/#az-group-create). Asegúrese de reemplazar los valores de los parámetros `<RESOURCE GROUP>` y `<LOCATION>` con sus propios valores. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Cree una identidad asignada por el usuario mediante [az identity create](/cli/azure/identity#az-identity-create).  El parámetro `-g` especifica el grupo de recursos donde se crea la identidad asignada por el usuario, mientras que el parámetro `-n` especifica su nombre. Asegúrese de reemplazar los valores de los parámetros `<RESOURCE GROUP>` y `<USER ASSIGNED IDENTITY NAME>` por sus propios valores:

   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```azurecli-interactive
   az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
   ```
   La respuesta contiene detalles de la identidad asignada por el usuario que se ha creado, de forma similar a lo siguiente. El valor del recurso `id` asignado a la identidad asignada por el usuario se usa en el paso siguiente.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

3. Cree un VMSS con [az vmss create](/cli/azure/vmss/#az-vmss-create). En el ejemplo siguiente se crea un VMSS asociado a la nueva identidad asignada por el usuario, según lo especificado por el parámetro `--assign-identity`. Asegúrese de reemplazar los valores de los parámetros `<RESOURCE GROUP>`, `<VMSS NAME>`, `<USER NAME>`, `<PASSWORD>` y `<USER ASSIGNED IDENTITY ID>` por sus propios valores. Para `<USER ASSIGNED IDENTITY ID>`, use la propiedad `id` del recurso de la identidad asignada por el usuario creada en el paso anterior: 

   ```azurecli-interactive 
   az vmss create --resource-group <RESOURCE GROUP> --name <VMSS NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY ID>
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Asignar una identidad asignada por el usuario a una máquina virtual de Azure existente

1. Cree una identidad asignada por el usuario mediante [az identity create](/cli/azure/identity#az-identity-create).  El parámetro `-g` especifica el grupo de recursos donde se crea la identidad asignada por el usuario, mientras que el parámetro `-n` especifica su nombre. Asegúrese de reemplazar los valores de los parámetros `<RESOURCE GROUP>` y `<USER ASSIGNED IDENTITY NAME>` por sus propios valores:

    > [!IMPORTANT]
    > Actualmente no se admite la creación de identidades asignadas por el usuario con caracteres especiales (por ejemplo, guion bajo) en el nombre. Use caracteres alfanuméricos. Compruebe si hay actualizaciones.  Para obtener más información, consulte [Preguntas más frecuentes y problemas conocidos](known-issues.md).

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
La respuesta contiene detalles de la identidad asignada por el usuario que se ha creado, de forma similar a lo siguiente. El valor del recurso `id` asignado a la identidad asignada por el usuario se usa en el paso siguiente.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY >/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. Asigne al VMSS la identidad asignada por el usuario mediante [az vmss identity assign](/cli/azure/vmss/identity#az-vm-assign-identity). Asegúrese de reemplazar los valores de los parámetros `<RESOURCE GROUP>` y `<VMSS NAME>` con sus propios valores. El parámetro `<USER ASSIGNED IDENTITY ID>` será la propiedad `id` del recurso de la identidad asignada por el usuario, como se ha creado en el paso anterior:

    ```azurecli-interactive
    az vmss identity assign -g <RESOURCE GROUP> -n <VMSS NAME> --identities <USER ASSIGNED IDENTITY ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Eliminación de una identidad asignada por el usuario de un conjunto de escalado de máquinas virtuales de Azure

Para eliminar una identidad asignada por un usuario desde un conjunto de escalado de máquinas virtuales, use [az vmss identity remove](/cli/azure/vmss/identity#az-vmss-identity-remove). Asegúrese de reemplazar los valores de los parámetros `<RESOURCE GROUP>` y `<VMSS NAME>` con sus propios valores. El parámetro `<MSI NAME>` será la propiedad `name` de la identidad asignada por el usuario, que se puede encontrar en la sección de identidad de la máquina virtual mediante `az vmss identity show`:

```azurecli-interactive
az vmss identity remove -g <RESOURCE GROUP> -n <VMSS NAME> --identities <MSI NAME>
```

Si el conjunto de escalado de máquinas virtuales no tiene una identidad asignada por el sistema y desea quitar de ella todas las identidades asignadas por el usuario, utilice el siguiente comando:

> [!NOTE]
> El valor `none` no distingue mayúsculas de minúsculas. Debe estar en minúscula.

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type="none" identity.identityIds=null
```

Si el conjunto de escalado de máquinas virtuales tiene identidades asignadas tanto por el sistema como por el usuario, puede quitar todas las identidades asignadas por el usuario si cambia para usar solo las asignadas por el sistema. Use el comando siguiente:

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type='SystemAssigned' identity.identityIds=null 
```

## <a name="next-steps"></a>Pasos siguientes

- [Información general de Managed Service Identity](overview.md)
- Si desea consultar la guía completa de inicio rápido sobre creación de un conjunto de escalado de máquinas virtuales de Azure, consulte: 

  - [Creación de un conjunto de escalado de máquinas virtuales con la CLI](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

















