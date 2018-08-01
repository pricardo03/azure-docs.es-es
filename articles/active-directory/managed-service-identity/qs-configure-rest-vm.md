---
title: Cómo configurar las identidades asignadas por el usuario y por el sistema en una máquina virtual de Azure mediante REST
description: Instrucciones paso a paso para configurar identidades asignadas por el sistema y por el usuario en una máquina virtual de Azure mediante CURL para llamar a la API REST.
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
ms.date: 06/25/2018
ms.author: daveba
ms.openlocfilehash: 825f34d174c37c2ee5d4187048f7a31fbaeef226
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215936"
---
# <a name="configure-managed-identity-on-an-azure-vm-using-rest-api-calls"></a>Configuración de la identidad administrada en una máquina virtual de Azure mediante llamadas a la API REST

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Managed Identity proporciona a los servicios de Azure una identidad de sistema administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, aprenderá a realizar las siguientes operaciones de Managed Identity en una máquina virtual de Azure mediante CURL para llamar al punto de conexión de Azure Resource Manager:

- Habilitar y deshabilitar la identidad asignada por el sistema en una máquina virtual de Azure
- Agregar y quitar una identidad asignada por el usuario en una máquina virtual de Azure

## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con Managed Service Identity, consulte la [sección de introducción](overview.md). **No olvide revisar la [diferencia entre una identidad asignada por el sistema y una asignada por el usuario](overview.md#how-does-it-work)**.
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para llevar a cabo las operaciones de administración en este artículo, su cuenta debe tener las siguientes asignaciones de roles:
    - [Colaborador de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para crear una máquina virtual y habilitar y quitar la identidad administrada asignada por el usuario o el sistema desde una máquina virtual de Azure.
    - Rol [Colaborador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) para crear una identidad asignada por el usuario.
    - Rol [Operador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-operator) para asignar y quitar una identidad asignada por el usuario en una máquina virtual.
- Si usa Windows, instale el [subsistema de Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about) o [Azure Cloud Shell](../../cloud-shell/overview.md) en Azure Portal.
- [Instale la consola local de la CLI de Azure](/azure/install-azure-cli) si utiliza el [subsistema de Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about) o un [sistema operativo de distribución de Linux](/cli/azure/install-azure-cli-apt?view=azure-cli-latest).
- Si utiliza la consola local de la CLI de Azure, inicie sesión en Azure mediante `az login` con una cuenta que esté asociada a la suscripción de Azure de la que desea administrar las identidades asignadas por el usuario o por el sistema.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Identidad asignada por el sistema

En esta sección, aprenderá a habilitar y deshabilitar una identidad asignada por el sistema en una máquina virtual de Azure mediante CURL para llamar al punto de conexión de REST de Azure Resource Manager.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Habilitación de una identidad asignada por el sistema durante la creación de una máquina virtual de Azure

Para crear una máquina virtual de Azure con la identidad asignada por el sistema habilitada, debe crear una máquina virtual y recuperar un token de acceso para usar CURL y llamar al punto de conexión de Resource Manager con el valor del tipo de identidad asignado por el sistema.

1. Cree un [grupo de recursos](../../azure-resource-manager/resource-group-overview.md#terminology) para contener e implementar la máquina virtual y sus recursos relacionados, con [az group create](/cli/azure/group/#az_group_create). Puede omitir este paso si ya tiene un grupo de recursos que le gustaría usar en su lugar:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Cree una [interfaz de red](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) para la máquina virtual:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Recupere un token de acceso de portador, que utilizará en el siguiente paso en el encabezado de autorización para crear la máquina virtual con una identidad administrada asignada por el sistema.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Cree una máquina virtual con CURL para llamar al punto de conexión REST de Azure Resource Manager. En el ejemplo siguiente, se crea una máquina virtual denominada *myVM* con una identidad asignada por el sistema, como ha identificado en el cuerpo de la solicitud el valor `"identity":{"type":"SystemAssigned"}`. Reemplace `<ACCESS TOKEN>` por el valor que ha recibido en el paso anterior cuando solicitó un token de acceso de portador y el valor `<SUBSCRIPTION ID>` según sea apropiado para su entorno.
 
    ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"SystemAssigned"},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"TestVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
    ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Habilitación de la identidad asignada por el sistema en una máquina virtual de Azure existente

Para habilitar la identidad asignada por el sistema en una máquina virtual existente, debe adquirir un token de acceso y, después, utilizar CURL para llamar al punto de conexión REST de Resource Manager y actualizar el tipo de identidad.

1. Recupere un token de acceso de portador, que utilizará en el siguiente paso en el encabezado de autorización para crear la máquina virtual con una identidad administrada asignada por el sistema.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Use el comando CURL siguiente para llamar al punto de conexión REST de Azure Resource Manager para habilitar la identidad asignada por el sistema en la máquina virtual, como se identifica en el cuerpo de la solicitud por el valor `{"identity":{"type":"SystemAssigned"}` para una máquina virtual denominada *myVM*.  Reemplace `<ACCESS TOKEN>` por el valor que ha recibido en el paso anterior cuando solicitó un token de acceso de portador y el valor `<SUBSCRIPTION ID>` según sea apropiado para su entorno.
   
   > [!IMPORTANT]
   > Para asegurarse de que no elimina ninguna de las identidades administradas asignadas por el usuario que están asignadas a la máquina virtual, necesita enumerar las identidades asignadas por el usuario mediante este comando CURL: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Si tiene asignadas identidades asignadas por el usuario a la máquina virtual como se identifica en el valor `identity` de la respuesta, vaya al paso 3 que muestra cómo conservar las identidades asignadas por el usuario mientras se habilita la identidad asignada por el sistema en la máquina virtual.

   ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Para habilitar la identidad asignada por el sistema en una máquina virtual con identidades existentes asignadas por el usuario, debe agregar `SystemAssigned` al valor `type`.  
   
   Por ejemplo, si la máquina virtual tiene asignadas las identidades de usuario `ID1` y `ID2`, y desea agregar una identidad asignada por el sistema a dicha máquina virtual, utilice la siguiente llamada CURL. Reemplace `<ACCESS TOKEN>` y `<SUBSCRIPTION ID>` por los valores adecuados para su entorno.
   
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="disable-system-assigned-identity-from-an-azure-vm"></a>Deshabilitación de una identidad asignada por el sistema desde una máquina virtual de Azure

Para deshabilitar una identidad asignada por el sistema en una máquina virtual existente, debe adquirir un token de acceso y, después, utilizar CURL para llamar al punto de conexión REST de Resource Manager y actualizar el tipo de identidad a `None`.

1. Recupere un token de acceso de portador, que utilizará en el siguiente paso en el encabezado de autorización para crear la máquina virtual con una identidad administrada asignada por el sistema.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Actualice la máquina virtual mediante CURL para llamar al punto de conexión REST de Azure Resource Manager para deshabilitar la identidad asignada por el sistema.  En el ejemplo siguiente, se deshabilita una identidad asignada por el sistema en el cuerpo de la solicitud por el valor `{"identity":{"type":"None"}}` desde una máquina virtual denominada *myVM*.  Reemplace `<ACCESS TOKEN>` por el valor que ha recibido en el paso anterior cuando solicitó un token de acceso de portador y el valor `<SUBSCRIPTION ID>` según sea apropiado para su entorno.

   > [!IMPORTANT]
   > Para asegurarse de que no elimina ninguna de las identidades administradas asignadas por el usuario que están asignadas a la máquina virtual, necesita enumerar las identidades asignadas por el usuario mediante este comando CURL: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Si tiene asignadas identidades asignadas por el usuario a la máquina virtual como se identifica en el valor `identity` de la respuesta, vaya al paso 3 que muestra cómo conservar las identidades asignadas por el usuario mientras se deshabilita la identidad asignada por el sistema en la máquina virtual.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Para quitar la identidad asignada por el sistema de una máquina virtual que tiene identidades asignadas por el usuario, quite `SystemAssigned` del valor `{"identity":{"type:" "}}` mientras mantiene el valor `UserAssigned` y la matriz `identityIds` que define qué identidades asignadas por el usuario se asignan a la máquina virtual.

## <a name="user-assigned-identity"></a>Identidad asignada por el usuario

En esta sección, aprenderá a agregar y a quitar una identidad asignada por el usuario en una máquina virtual de Azure mediante CURL para llamar al punto de conexión de REST de Azure Resource Manager.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vm"></a>Asignación de una identidad asignada por el usuario durante la creación de una máquina virtual de Azure

1. Recupere un token de acceso de portador, que utilizará en el siguiente paso en el encabezado de autorización para crear la máquina virtual con una identidad administrada asignada por el sistema.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Cree una [interfaz de red](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) para la máquina virtual:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Recupere un token de acceso de portador, que utilizará en el siguiente paso en el encabezado de autorización para crear la máquina virtual con una identidad administrada asignada por el sistema.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Cree una identidad asignada por el usuario mediante las instrucciones que se encuentran aquí: [Creación de una identidad administrada asignada por el usuario](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Cree una máquina virtual con CURL para llamar al punto de conexión REST de Azure Resource Manager. En el ejemplo siguiente se crea una máquina virtual denominad *myVM* en el grupo de recursos *myResourceGroup* con una identidad asignada por el usuario `ID1`, como se identificó en el cuerpo de la solicitud por el valor `"identity":{"type":"UserAssigned"}`. Reemplace `<ACCESS TOKEN>` por el valor que ha recibido en el paso anterior cuando solicitó un token de acceso de portador y el valor `<SUBSCRIPTION ID>` según sea apropiado para su entorno.
 
   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM",{"identity":{"type":"UserAssigned", "identityIds":["/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"TestVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Asignar una identidad asignada por el usuario a una máquina virtual de Azure existente

1. Recupere un token de acceso de portador, que utilizará en el siguiente paso en el encabezado de autorización para crear la máquina virtual con una identidad administrada asignada por el sistema.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Cree una identidad asignada por el usuario mediante las instrucciones que se encuentran aquí: [Creación de una identidad administrada asignada por el usuario](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3.  Para asegurarse de que no elimina las identidades administradas asignadas por el usuario o por el sistema que están asignadas a la máquina virtual, necesita enumerar los tipos de identidades asignados a la máquina virtual mediante el comando CURL siguiente. Si ha administrado identidades asignadas al conjunto de escalado de máquinas virtuales, se enumeran en el valor `identity`.

    ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    Si tiene asignadas identidades asignadas por el usuario o por el sistema a la máquina virtual como se identifica en el valor `identity` de la respuesta, vaya al paso 5 que muestra cómo conservar las identidades asignadas por el usuario mientras se habilita la identidad asignada por el sistema en la máquina virtual.

4. Si no tiene ninguna identidad asignada por el usuario en la máquina virtual, use el siguiente comando CURL para llamar al punto de conexión REST de Azure Resource Manager para asignar la primera identidad asignada por el usuario a la máquina virtual.  Si tiene asignada una identidad o identidades asignadas por el usuario a la máquina virtual, vaya al siguiente paso que muestra cómo agregar varias identidades asignadas por el usuario a una máquina virtual.

   En el ejemplo siguiente se asigna una identidad asignada por el usuario, `ID1`, a una máquina virtual denominada *myVM* en el grupo de recursos *myResourceGroup*.  Reemplace `<ACCESS TOKEN>` por el valor que ha recibido en el paso anterior cuando solicitó un token de acceso de portador y el valor `<SUBSCRIPTION ID>` según sea apropiado para su entorno.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

5. Si tiene asignadas identidades asignadas por el usuario o por el sistema a la máquina virtual, debe agregar la nueva identidad asignada por el usuario a la matriz `identityIDs`, mientras conserva al mismo tiempo las identidades asignadas por el usuario y por el sistema que están actualmente asignadas a la máquina virtual.

   Por ejemplo, si tiene una identidad asignada por el sistema y una identidad asignada por el usuario `ID1` actualmente asignadas a la máquina virtual y desea agregarles la identidad de usuario `ID2`, utilice el siguiente comando CURL. Reemplace `<ACCESS TOKEN>` por el valor que ha recibido en los pasos cuando solicitó un token de acceso de portador y el valor `<SUBSCRIPTION ID>` según sea apropiado para su entorno.

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vm"></a>Eliminación de una identidad asignada por el usuario de una máquina virtual de Azure

1. Recupere un token de acceso de portador, que utilizará en el siguiente paso en el encabezado de autorización para crear la máquina virtual con una identidad administrada asignada por el sistema.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Para asegurarse de que no elimina ninguna identidad administrada asignada por el usuario o por el sistema que está asignada a la máquina virtual y de que no quita la identidad asignada por el sistema, necesita enumerar las identidades administradas mediante el comando CURL siguiente: 
 
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```
 
   Si ha administrado identidades asignadas a la máquina virtual, se enumeran en la respuesta del valor `identity`.

   Por ejemplo, si tiene asignadas identidades asignadas por el usuario `ID1` y `ID2` a la máquina virtual, y solo desea mantener `ID1` asignada y conservar la identidad asignada por el sistema, debe utilizar el mismo comando CURL que si asignara una identidad administrada asignada por el usuario a una máquina virtual solo manteniendo el valor `ID1` y el valor `SystemAssigned`. Esto quita la identidad asignada por el usuario `ID2` de la máquina virtual, mientras conserva al mismo tiempo la identidad asignada por el sistema.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

Si la máquina virtual tiene identidades asignadas tanto por el sistema como por el usuario, puede quitar todas las identidades asignadas por el usuario si cambia para usar solo las asignadas mediante el comando siguiente:

```bash
curl 'https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
    
Si la máquina virtual solo tiene identidades asignadas por el usuario y desea quitarlas todas, utilice el comando siguiente:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo crear, enumerar o eliminar el usuario asignado mediante REST, consulte:

- [Creación, enumeración o eliminación de una identidad asignada por el usuario mediante llamadas a la API REST](how-to-manage-ua-identity-rest.md)