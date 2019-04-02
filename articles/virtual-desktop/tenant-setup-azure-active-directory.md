---
title: 'Creación de un inquilino en Windows Virtual Desktop (versión preliminar): Azure'
description: Se describe cómo configurar los inquilinos de la versión preliminar de Windows Virtual Desktop en Azure Active Directory.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: b8b5c2cef1db5018ce0d61e1950f49a3bd215ac2
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58402905"
---
# <a name="tutorial-create-a-tenant-in-windows-virtual-desktop-preview"></a>Tutorial: Creación de un inquilino en Windows Virtual Desktop (versión preliminar)

Crear un inquilino en la versión preliminar de Windows Virtual Desktop es el primer paso hacia la compilación de su primera solución de virtualización de escritorio. Un inquilino es un grupo de uno o varios grupos host. Cada grupo host se compone de varios hosts de sesión, que se ejecutan como máquinas virtuales en Azure y que están registrados en el servicio Windows Virtual Desktop. Cada grupo host consta también de uno o varios grupos de aplicaciones que se usan para publicar recursos del escritorio remoto y de la aplicación remota para los usuarios. Con un inquilino, puede crear grupos host, crear grupos de aplicaciones, asignar usuarios y realizar conexiones a través del servicio.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Conceder permisos de Azure Active Directory en el servicio Windows Virtual Desktop.
> * Asignar el rol de aplicación TenantCreator a un usuario en el inquilino de Azure Active Directory.
> * Crear un inquilino de Windows Virtual Desktop.

Esto es lo que necesita para configurar el inquilino de Windows Virtual Desktop:

* El identificador de inquilino de [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) para los usuarios de Windows Virtual Desktop.
* Una cuenta de administrador global en el inquilino de Azure Active Directory.
   * Esto también es aplicable a las organizaciones de proveedores de soluciones en la nube (CSP) que crean un inquilino de Windows Virtual Desktop para sus clientes. Si este es su caso, debe poder iniciar sesión como administrador global de la instancia de Azure Active Directory del cliente.
* Un identificador de suscripción de Azure

## <a name="grant-azure-active-directory-permissions-to-the-windows-virtual-desktop-preview-service"></a>Conceder permisos de Azure Active Directory en el servicio Windows Virtual Desktop (versión preliminar)

Si ya ha concedido permisos a Windows Virtual Desktop para esta instancia de Azure Active Directory, omita esta sección.

Conceder permisos al servicio Windows Virtual Desktop le permite consultar Azure Active Directory para buscar tareas administrativas y de usuario final.

Para conceder los permisos de servicio:

1. Abra un explorador y conéctese a la [página de consentimiento de Windows Virtual Desktop](https://rdweb.wvd.microsoft.com).
2. En **Consent Option** (Opción de consentimiento) > **Server App**, escriba el nombre del inquilino de Azure Active Directory o el identificador de directorio y, después, seleccione **Submit** (Enviar).
        Para clientes de proveedores de soluciones en la nube, el identificador es el identificador de Microsoft del cliente de Partner Portal. Para los clientes empresariales, el identificador se encuentra en **Azure Active Directory** > **Propiedades** > **Id. de directorio**.
3. Inicie sesión en la página de consentimiento de Windows Virtual Desktop con una cuenta de administrador global. Por ejemplo, si pertenecía a la organización Contoso, su cuenta podría ser admin@contoso.com o admin@contoso.onmicrosoft.com.  
4. Seleccione **Aceptar**.
5. Espere un minuto.
6. Vuelva a la [página de consentimiento de Windows Virtual Desktop](https://rdweb.wvd.microsoft.com).
7. Vaya a **Consent Option** (Opción de consentimiento) > **Client App** y escriba el mismo nombre de inquilino de Azure Active Directory o identificador de directorio y, después, seleccione **Submit** (Enviar).
8. Inicie sesión en la página de consentimiento de Windows Virtual Desktop como administrador global como hizo en el paso 3 anterior.
9. Seleccione **Aceptar**.

## <a name="assign-the-tenantcreator-application-role-to-a-user-in-your-azure-active-directory-tenant"></a>Asignar el rol de aplicación TenantCreator a un usuario en el inquilino de Azure Active Directory

Asignar a un usuario de Azure Active Directory el rol de aplicación TenantCreator permite a ese usuario crear un inquilino de Windows Virtual Desktop asociado con la instancia de Azure Active Directory. Deberá usar su cuenta de administrador global para asignar el rol TenantCreator.

Para asignar el rol de aplicación TenantCreator con su cuenta de administrador global:

1. Abra un explorador y conéctese al [portal de Azure Active Directory](https://aad.portal.azure.com) con su cuenta de administrador global.
   - Si está trabajando con varios inquilinos de Azure AD, es recomendable abrir una sesión privada del explorador y copiar y pegar las direcciones URL en la dirección.
2. Seleccione **aplicaciones empresariales** y busque **Windows Virtual Desktop**. Verá las dos aplicaciones para las que ha dado su consentimiento en la sección anterior. De estas dos aplicaciones, seleccione **Windows Virtual Desktop**.
3. Seleccione **Usuarios y grupos** y, luego, **Agregar usuario**.
4. En la hoja **Agregar asignación**, seleccione Usuarios y grupos.
5. Busque una cuenta de usuario que cree el inquilino de Windows Virtual Desktop.
   - Para mayor sencillez, esta puede ser la cuenta de administrador global.
6. Seleccione la cuenta de usuario, haga clic en el botón **Seleccionar** y, a continuación, seleccione **Asignar**.

## <a name="create-a-windows-virtual-desktop-preview-tenant"></a>Creación de un inquilino en Windows Virtual Desktop (versión preliminar)

Ahora que ha concedido los permisos al servicio Windows Virtual Desktop para consultar Azure Active Directory y le ha asignado el rol TenantCreator a una cuenta de usuario, puede crear un inquilino de Windows Virtual Desktop.

En primer lugar y, si aún no lo ha hecho, [descargue e importe el módulo de Windows Virtual Desktop](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) que se usará en la sesión de PowerShell.

Inicie sesión en Windows Virtual Desktop mediante la cuenta de usuario de TenantCreator con este cmdlet:

```powershell
Add-RdsAccount -DeploymentUrl “https://rdbroker.wvd.microsoft.com”
```

Después de eso, cree un nuevo inquilino de Windows Virtual Desktop asociado al inquilino de Azure Active Directory:

```powershell
New-RdsTenant -Name <TenantName> -AadTenantId <DirectoryID> -AzureSubscriptionId <SubscriptionID>
```

Los valores entre corchetes deben reemplazarse por los valores pertinentes para la organización y el inquilino. Por ejemplo, supongamos que usted tiene el rol TenantCreator en Windows Virtual Desktop para la organización Contoso. El cmdlet que ejecutaría tendría este aspecto:

```powershell
New-RdsTenant -Name Contoso -AadTenantId 00000000-1111-2222-3333-444444444444 -AzureSubscriptionId 55555555-6666-7777-8888-999999999999
```

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya creado su inquilino, deberá crear un grupo host. Para más información acerca de los grupos host, continúe con el tutorial de creación de un grupo host en Windows Virtual Desktop.

> [!div class="nextstepaction"]
> [Tutorial sobre grupos host de Windows Virtual Desktop](./create-host-pools-azure-marketplace.md)
