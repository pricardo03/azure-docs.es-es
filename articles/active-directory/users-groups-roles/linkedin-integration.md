---
title: 'Consentimiento del Administrador de conexiones de cuentas de LinkedIn: Azure Active Directory | Microsoft Docs'
description: Explica cómo habilitar o deshabilitar las conexiones de cuentas de integración de LinkedIn en aplicaciones de Microsoft en Azure Active Directory
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/29/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1130885cc3168582935264ffaad9fd7a8ba3c60b
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64920263"
---
# <a name="integrate-linkedin-account-connections-in-azure-active-directory"></a>Integrar las conexiones de cuentas de LinkedIn en Azure Active Directory

Puede permitir que a los usuarios de su organización para tener acceso a sus conexiones de LinkedIn dentro de algunas aplicaciones de Microsoft. No se comparte ningún dato hasta que los usuarios consentir para conectar sus cuentas. Puede integrar su organización en Azure Active Directory (Azure AD) [centro de administración](https://aad.portal.azure.com).

> [!IMPORTANT]
> La configuración de conexiones de LinkedIn cuenta actualmente se está implantando a organizaciones de Azure AD. Cuando se implanta a su organización, está habilitado de forma predeterminada.
> 
> Excepciones:
> * La opción no está disponible para los clientes que usan Microsoft Cloud for US Government, Microsoft Cloud Germany, o bien Azure y Office 365 operado por 21Vianet en China.
> * La opción está desactivada de forma predeterminada para los inquilinos que se aprovisionan en Alemania. Tenga en cuenta que la opción no está disponible para los clientes que usan Microsoft Cloud Germany.
> * La opción está desactivada de forma predeterminada para los inquilinos que se aprovisionan en Francia.
>
> Una vez que se habilitan las conexiones de cuentas de LinkedIn para su organización, las conexiones de cuenta funcionen después los usuarios den su consentimiento a aplicaciones que acceden a datos de la empresa en su nombre. Para obtener información acerca de la configuración de consentimiento de usuario, consulte [cómo quitar el acceso de un usuario a una aplicación](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## <a name="enable-linkedin-account-connections-in-the-azure-portal"></a>Habilitar las conexiones de cuentas de LinkedIn en Azure portal

Puede habilitar las conexiones de cuentas de LinkedIn para solo los usuarios que desea tener acceso a partir de toda la organización a solo los usuarios seleccionados de su organización.

1. Inicie sesión en el [centro de administración de Azure AD](https://aad.portal.azure.com/) con una cuenta que sea un administrador global para la organización de Azure AD.
1. Seleccione **Usuarios**.
1. En la hoja **Usuarios**, seleccione **Configuración de usuario**.
1. En **las conexiones de cuentas de LinkedIn**, permitir que los usuarios se conecten sus cuentas para tener acceso a sus conexiones de LinkedIn dentro de algunas aplicaciones de Microsoft. No se comparte ningún dato hasta que los usuarios consentir para conectar sus cuentas.

    * Seleccione **Sí** para habilitar el servicio para todos los usuarios de su organización
    * Seleccione **seleccionados** para habilitar solo un grupo de usuarios seleccionados de la organización
    * Seleccione **No** para retirar el consentimiento de todos los usuarios de su organización

    ![Integrar las conexiones de cuentas de LinkedIn en la organización](./media/linkedin-integration/linkedin-integration.png)

1. Cuando haya terminado, seleccione **guardar** para guardar la configuración.

> [!Important]
> Integración de LinkedIn no es totalmente habilitada para los usuarios hasta que da su consentimiento para conectarse a sus cuentas. Al habilitar las conexiones de cuentas para los usuarios, no se comparte ningún dato.

### <a name="assign-selected-users-with-a-group"></a>Asigne a los usuarios seleccionados con un grupo
Hemos sustituido la opción 'Seleccionados' que especifica una lista de usuarios con la opción para seleccionar un grupo de usuarios para que se puede habilitar la capacidad para conectarse a cuentas de LinkedIn y Microsoft para un único grupo en lugar de muchos usuarios individuales. Si no dispone de las conexiones de cuentas de LinkedIn habilitadas para usuarios individuales seleccionados, no es necesario hacer nada. Si ha habilitado previamente las conexiones de cuentas de LinkedIn para usuarios individuales seleccionados, debe:

1. Obtener la lista actual de usuarios individuales
1. Mover los usuarios individuales habilitados actualmente a un grupo
1. Use el grupo del anterior como el grupo seleccionado en las conexiones de la cuenta de LinkedIn en el centro de administración de Azure AD.

> [!NOTE]
> Incluso si no mueve los usuarios individuales seleccionados actualmente a un grupo, pueden ver información de LinkedIn en aplicaciones de Microsoft.

### <a name="get-the-current-list-of-selected-users"></a>Obtener la lista actual de los usuarios seleccionados

1. Inicie sesión en Microsoft 365 con su cuenta de administrador.
1. Vaya a https://linkedinselectedusermigration.azurewebsites.net/. Verá la lista de usuarios que están seleccionados para las conexiones de cuentas de LinkedIn.
1. Exportar la lista a un archivo CSV.

### <a name="move-the-currently-selected-individual-users-to-a-group"></a>Mover los usuarios individuales actualmente seleccionados a un grupo

1. Inicie PowerShell
1. Instalar el módulo de Azure AD mediante la ejecución `Install-Module AzureAD`
1. Ejecute el siguiente script:

  ``` PowerShell
  $groupId = "GUID of the target group"
  
  $users = Get-Content 
  Path to the CSV file
  
  $i = 1
  foreach($user in $users} { Add-AzureADGroupMember -ObjectId $groupId -RefObjectId $user ; Write-Host $i Added $user ; $i++ ; Start-Sleep -Milliseconds 10 }
  ```

Para usar el grupo del paso dos como el grupo seleccionado en las conexiones de la cuenta de LinkedIn en el centro de administración de Azure AD, consulte [LinkedIn habilitar las conexiones de cuentas en Azure portal](#enable-linkedin-account-connections-in-the-azure-portal).

## <a name="use-group-policy-to-enable-linkedin-account-connections"></a>Usar Directiva de grupo para habilitar las conexiones de cuentas de LinkedIn

1. Descargue los [archivos de plantilla de administración de Office 2016 (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
1. Extraiga los archivos **ADMX** y cópielos en el almacén central.
1. Abra la Administración de directivas de grupo.
1. Cree un objeto de directiva de grupo con la configuración siguiente: **Configuración de usuario** > **Plantillas administrativas** > **Microsoft Office 2016** > **Varios** > **Mostrar características de LinkedIn en aplicaciones de Office**.
1. Seleccione **Habilitado** o **Deshabilitado**.
  
   Estado | Efecto
   ------ | ------
   **Enabled** | La opción **Mostrar características de LinkedIn en aplicaciones de Office** de Opciones de Office 2016 está habilitada. Los usuarios de su organización pueden usar las características de LinkedIn en sus aplicaciones de Office 2016.
   **Deshabilitada** | La opción **Mostrar características de LinkedIn en aplicaciones de Office** de Opciones de Office 2016 está deshabilitada y los usuarios finales no pueden cambiarla. Los usuarios de la organización no podrán usar las características de LinkedIn en las aplicaciones de Office 2016.

Esta directiva de grupo afecta solo a las aplicaciones de Office 2016 de los equipos locales. Si los usuarios deshabilitan LinkedIn en sus aplicaciones de Office 2016, pueden ver las características de LinkedIn en Office 365.

## <a name="next-steps"></a>Pasos siguientes

* [Consentimiento del usuario y los datos de uso compartido de LinkedIn](linkedin-user-consent.md)

* [Información y características de LinkedIn en las aplicaciones de Microsoft](https://go.microsoft.com/fwlink/?linkid=850740)

* [Centro de ayuda de LinkedIn](https://www.linkedin.com/help/linkedin)

* [Visualización de la configuración de integración con LinkedIn actual en Azure Portal](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings)
