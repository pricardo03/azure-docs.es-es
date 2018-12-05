---
title: Vinculación de una cuenta de Azure a un Id. de partner | Microsoft Docs
description: Controle las interacciones con clientes de Azure vinculando un Id. de partner a la cuenta de usuario que utiliza para administrar los recursos del cliente.
services: billing
author: dhirajgandhi
manager: dhgandhi
ms.author: cwatson
ms.date: 03/12/2018
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 1e2492d978073f63c1c9494d652ec35a7d6565b7
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "52274186"
---
# <a name="link-partner-id-to-your-azure-accounts"></a>Vinculación de un Id. de partner a cuentas de Azure

Como partner, puede realizar un seguimiento de su impacto en las interacciones con sus clientes mediante la vinculación de su Id. de partner con las cuentas utilizadas para administrar recursos del cliente.

Esta característica está disponible en versión preliminar pública.

## <a name="get-access-from-your-customer"></a>Obtención de acceso del cliente

Antes de vincular su Id. de partner, el cliente debe concederle acceso a sus recursos de Azure mediante una de las siguientes opciones:

- **Usuario invitado:** el cliente puede añadirle como usuario invitado y asignar roles RBAC. Para más información, consulte [Adición de usuarios invitados de otro directorio](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

- **Cuenta de directorio:** el cliente puede crear un nuevo usuario de su organización en el directorio del cliente y asignarle cualquier rol RBAC.

- **Entidad de servicio:** el cliente puede añadir una aplicación o un script de su organización en el directorio del cliente y asignarle cualquier rol RBAC. La identidad de la aplicación o el script se conoce como entidad de servicio.

## <a name="link-partner-id"></a>Vinculación de Id. de partner

Cuando acceda a los recursos del cliente, use Azure Portal, PowerShell o la CLI para vincular su identificador de Microsoft Partner Network (Id. de MPN) a su identificador de usuario o entidad de servicio. Debe vincular el Id. de partner en cada inquilino de cliente.

### <a name="use-azure-portal-to-link-new-partner-id"></a>Uso de Azure Portal para vincular un nuevo identificador de partner

1. Vaya al [vínculo a un identificador de partner](https://portal.azure.com/#blade/Microsoft_Azure_Billing/managementpartnerblade) en Azure Portal.

2. Inicie sesión en el Portal de Azure.

3. Escriba el identificador de partner de Microsoft. El identificador de partner es el identificador de [Microsoft Partner Network (MPN)](https://partner.microsoft.com/) de su organización.

  ![Captura de pantalla que muestra el vínculo del identificador de partner](./media/billing-link-partner-id/link-partner-ID.PNG)

4. Para vincular el identificador de partner con otro cliente, utilice al selector de directorios. En Cambiar directorio, elija su directorio.

  ![Captura de pantalla que muestra el vínculo del identificador de partner](./media/billing-link-partner-id/directory-switcher.png)

### <a name="use-powershell-to-link-new-partner-id"></a>Uso de PowerShell para vincular un nuevo Id. de partner

1. Instale el módulo de PowerShell [AzureRM.ManagementPartner](https://www.powershellgallery.com/packages/AzureRM.ManagementPartner).

2. Inicie sesión en el inquilino de cliente con la cuenta de usuario o la entidad de servicio. Para más información, consulte [Inicio de sesión con Powershell](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-5.2.0).
 
   ```azurepowershell-interactive
    C:\> Connect-AzureRmAccount -TenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 
   ```


3. Vincule el nuevo Id. de partner. El Id. de partner es el identificador de [Microsoft Partner Network (MPN)](https://partner.microsoft.com/) de su organización.

    ```azurepowershell-interactive
    C:\> new-AzureRmManagementPartner -PartnerId 12345 
    ```

#### <a name="get-the-linked-partner-id"></a>Obtención del Id. de partner vinculado
```azurepowershell-interactive
C:\> get-AzureRmManagementPartner 
```

#### <a name="update-the-linked-partner-id"></a>Actualización del Id. de partner vinculado
```azurepowershell-interactive
C:\> Update-AzureRmManagementPartner -PartnerId 12345 
```
#### <a name="delete-the-linked-partner-id"></a>Eliminación del Id. de partner vinculado
```azurepowershell-interactive
C:\> remove-AzureRmManagementPartner -PartnerId 12345 
```

### <a name="use-cli-to-link-new-partner-id"></a>Uso de la CLI para vincular un nuevo Id. de partner
1.  Instale la extensión de la CLI.

    ```azurecli-interactive
    C:\ az extension add --name managementpartner
    ``` 

2.  Inicie sesión en el inquilino de cliente con la cuenta de usuario o la entidad de servicio. Para más información, vea [Inicio de sesión con la CLI de Azure](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    ```azurecli-interactive
    C:\ az login --tenant <tenant>
    ``` 

3.  Vincule el nuevo Id. de partner. El Id. de partner es el identificador de [Microsoft Partner Network (MPN)](https://partner.microsoft.com/) de su organización.

     ```azurecli-interactive
     C:\ az managementpartner create --partner-id 12345
      ```  

#### <a name="get-the-linked-partner-id"></a>Obtención del Id. de partner vinculado
```azurecli-interactive
C:\ az managementpartner show
``` 

#### <a name="update-the-linked-partner-id"></a>Actualización del Id. de partner vinculado
```azurecli-interactive
C:\ az managementpartner update --partner-id 12345
``` 

#### <a name="delete-the-linked-partner-id"></a>Eliminación del Id. de partner vinculado
```azurecli-interactive
C:\ az managementpartner delete --partner-id 12345
``` 

## <a name="next-steps"></a>Pasos siguientes

Únase al debate en la [comunidad de asociados de Microsoft ](https://aka.ms/PALdiscussion) para recibir actualizaciones o enviar comentarios.

## <a name="frequently-asked-questions"></a>Preguntas frecuentes

**¿Quién puede vincular el Id. de partner?**

Cualquier usuario de la organización asociada que administre los recursos de Azure del cliente puede vincular el identificador de partner a la cuenta.

**Una vez vinculado un Id. de partner, ¿se puede cambiar?**

Sí, el Id. de partner vinculado se puede cambiar, añadir o quitar.

**¿Qué ocurre si un usuario tiene una cuenta en varios inquilinos de cliente?**

El vínculo entre el Id. de partner y la cuenta se realiza para cada inquilino de cliente.  Debe vincular el Id. de partner en cada inquilino de cliente.

**¿Puede otro partner o cliente editar o quitar el vínculo con el Id. de partner?**

El vínculo está asociado al nivel de cuenta del usuario. Solo usted puede editar o quitar el vínculo con el Id. de partner. Ni el cliente ni ningún otro partner podrán modificar el vínculo con el Id. de partner. 
