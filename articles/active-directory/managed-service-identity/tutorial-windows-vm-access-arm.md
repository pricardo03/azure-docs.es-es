---
title: Uso de una identidad de servicio administrada (MSI) de máquina virtual Windows para acceder a Azure Resource Manager
description: Tutorial que recorre el proceso del uso de la característica Managed Service Identity de una máquina virtual Windows para acceder a Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: 5dc4f498c416142977c5570cddf8b380a8c02ab4
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42885148"
---
# <a name="use-a-windows-vm-managed-service-identity-to-access-resource-manager"></a>Uso de la característica Managed Service Identity de una máquina virtual Windows para acceder a Resource Manager

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

En esta guía de inicio rápido se muestra cómo acceder a la API de Azure Resource Manager con una máquina virtual Windows y una identidad asignada por el sistema habilitada. Las identidades de MSI son administradas automáticamente por Azure y le permiten autenticar los servicios que admiten la autenticación de Azure AD sin necesidad de insertar credenciales en el código. Aprenderá a:

> [!div class="checklist"] 
> * Concesión a una máquina virtual de acceso a un grupo de recursos en Azure Resource Manager 
> * Obtención de un token de acceso mediante la identidad de máquina virtual y su uso para llamar a Azure Resource Manager

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- [Iniciar sesión en Azure Portal](https://portal.azure.com)

- [Crear una máquina virtual Windows](/azure/virtual-machines/windows/quick-create-portal)

- [Habilitar la identidad asignada por el sistema en la máquina virtual](/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm#enable-system-assigned-identity-on-an-existing-vm)

## <a name="grant-your-vm-access-to-a-resource-group-in-resource-manager"></a>Concesión de acceso a la máquina virtual a un grupo de recursos en Resource Manager
Mediante el uso de Managed Service Identity, el código puede obtener tokens de acceso para autenticarse en aquellos recursos que admitan la autenticación de Azure AD.  Azure Resource Manager admite la autenticación de Azure AD.  En primer lugar, es necesario conceder acceso a la identidad de esta máquina virtual a un recurso de Resource Manager, en este caso, al grupo de recursos que contiene la máquina virtual.  

1.  Vaya a la pestaña de **Grupos de recursos**. 
2.  Seleccione el **grupo de recursos** específico que creó para su **máquina virtual Windows**. 
3.  Vaya a **Control de acceso (IAM)** en el panel izquierdo. 
4.  A continuación, **agregue** una nueva asignación de rol para su **máquina virtual Windows**.  En **Rol**, elija **Lector**. 
5.  En la lista desplegable siguiente, **asigne acceso** al recurso **Máquina virtual**. 
6.  A continuación, asegúrese de que la suscripción adecuada aparece en la lista desplegable **Suscripción**. Y en **Grupo de recursos**, seleccione **Todos los grupos de recursos**. 
7.  Por último, en **Seleccionar**, elija la máquina virtual Windows en la lista desplegable y haga clic en **Guardar**.

    ![Texto alternativo de imagen](media/msi-tutorial-windows-vm-access-arm/msi-windows-permissions.png)

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-resource-manager"></a>Obtención de un token de acceso mediante la identidad de máquina virtual y su uso para llamar a Azure Resource Manager 

Deberá usar **PowerShell** en esta parte.  Si no lo ha instalado, descárguelo [aquí](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). 

1.  En el portal, vaya a **Virtual Machines** y diríjase a la máquina virtual Windows. A continuación, en **Introducción**, haga clic en **Conectar**. 
2.  Escriba su **nombre de usuario** y **contraseña** que agregó cuando creó la máquina virtual Windows. 
3.  Ahora que ha creado una **conexión a Escritorio remoto** con la máquina virtual, abra **PowerShell** en la sesión remota. 
4.  Mediante Invoke-WebRequest de PowerShell, realice una solicitud al punto de conexión de Managed Service Identity local para obtener un token de acceso para Azure Resource Manager.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > El valor del parámetro "resource" debe coincidir exactamente con el que Azure AD espera. Al usar el id. de recurso de Azure Resource Manager, debe incluir la barra diagonal final en el URI.
    
    A continuación, extraiga la respuesta completa, que se almacena como una cadena con formato de notación de objetos JavaScript (JSON) en el objeto $response. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Luego, extraiga el token de acceso de la respuesta.
    
    ```powershell
    $ArmToken = $content.access_token
    ```
    
    Por último, llame a Azure Resource Manager mediante el token de acceso. En este ejemplo, se usará también Invoke-WebRequest de PowerShell para realizar la llamada a Azure Resource Manager y se incluirá el token de acceso en el encabezado de autorización.
    
    ```powershell
    (Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{ Authorization ="Bearer $ArmToken"}).content
    ```
    > [!NOTE] 
    > La dirección URL distingue mayúsculas de minúsculas, por lo tanto, asegúrese de que usa las mismas mayúsculas y minúsculas que al asignar el nombre al grupo de recursos, así como la "G" mayúscula de "resourceGroups".
        
    El siguiente comando devuelve los detalles del grupo de recursos:

    ```powershell
    {"id":"/subscriptions/98f51385-2edc-4b79-bed9-7718de4cb861/resourceGroups/DevTest","name":"DevTest","location":"westus","properties":{"provisioningState":"Succeeded"}}
    ```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información sobre cómo crear una identidad asignada por el usuario y a adjuntarla a una instancia de Azure Virtual Machines para acceder a la API de Azure Resource Manager.  Para obtener más información sobre Azure Resource Manager, vea:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)

