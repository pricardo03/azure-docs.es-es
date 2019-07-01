---
title: 'Delegación de roles de administrador de aplicaciones: Azure Active Directory | Microsoft Docs'
description: Administración del acceso a las aplicaciones mediante la delegación de roles para conceder derechos de permiso en Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 58ca814551d8c7d309328f236052e1d07ac6f035
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60469135"
---
# <a name="delegate-app-administrator-roles-in-azure-active-directory"></a>Delegación de roles de administrador de aplicaciones en Azure Active Directory

 Azure AD le permite delegar la administración del acceso a las aplicaciones en un conjunto de roles administrativos integrados. Además de reducir la sobrecarga del administrador global, la delegación de privilegios especializados para administrar las tareas de acceso a las aplicaciones puede mejorar su posición en cuanto a la seguridad y reducir la posibilidad de acceso no autorizado. En [Delegación de la administración en Azure Active Directory](roles-concept-delegation.md), se describen problemas de delegación y criterios generales.

## <a name="delegate-app-administration"></a>Delegación de la administración de aplicaciones

Los siguientes roles conceden permisos para administrar los registros de aplicaciones, la configuración de inicio de sesión único y las asignaciones de usuarios y grupos, y para dar el consentimiento a los permisos delegados y los permisos de aplicación (excluyendo Microsoft Graph y Azure AD Graph). La única diferencia es que el rol de administrador de aplicaciones también concede permisos para administrar la configuración del proxy de aplicación. Ningún rol concede la capacidad para administrar la configuración de acceso condicional.
> [!IMPORTANT]
> Los usuarios asignados a este rol pueden agregar credenciales a una aplicación y usarlas para suplantar la identidad de la aplicación. Esta suplantación de la identidad de la aplicación puede ser una elevación de privilegios sobre lo que puede hacer el usuario con sus otras asignaciones de roles en Azure AD. Un usuario asignado a este rol podría crear o actualizar usuarios u otros objetos durante la suplantación de la aplicación.

Para conceder la capacidad para administrar el acceso a las aplicaciones en Azure Portal:

1. Inicie sesión en su [inquilino de Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) con una cuenta que sea válida para el rol de administrador global del inquilino.
2. Cuando tenga los permisos necesarios, abra la página [Roles y administradores](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators).
3. Abra uno de los roles siguientes para ver sus asignaciones de miembros:
   * **Administrador de aplicaciones**
   * **Administrador de aplicaciones en la nube**
4. En la página **Miembros** del rol, seleccione **Agregar miembro**.
5. Seleccione uno o más miembros para agregar al rol. <!--Members can be users or groups.-->

Puede ver la descripción de estos roles en [Roles disponibles](directory-assign-admin-roles.md#available-roles).

## <a name="delegate-app-registration"></a>Delegación del registro de aplicaciones

De forma predeterminada, todos los usuarios pueden crear registros de aplicaciones, pero se pueden conceder permisos de forma selectiva para crear registros de aplicaciones o permisos para dar el consentimiento a la autorización de una aplicación.

1. Inicie sesión en su [inquilino de Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) con una cuenta que sea válida para el rol de administrador global del inquilino.
2. Cuando haya obtenido permisos suficientes, establezca una o ambas de las siguientes opciones:
   * En la [página Configuración de usuario de su inquilino](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UserSettings), establezca **Los usuarios pueden registrar aplicaciones** en No.
   * En la [configuración de usuario de las aplicaciones empresariales](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/), establezca **Users can consent to applications accessing company data on their behalf** (Los usuarios pueden consentir que las aplicaciones accedan a los datos de la empresa en su nombre), en No.
3. A continuación, asigne a los usuarios que necesiten este permiso como miembros del rol de desarrollador de aplicaciones, según sea necesario.

Cuando un usuario registra una aplicación, se agrega automáticamente como primer propietario de esta.

## <a name="delegate-app-ownership"></a>Delegación de la propiedad de las aplicaciones

Los propietarios de aplicaciones y los propietarios de registros de aplicaciones solo pueden administrar las aplicaciones o los registros de aplicaciones de los que son propietarios. Por ejemplo, cuando agrega un propietario a la aplicación Salesforce, ese propietario puede administrar el acceso a Salesforce y la configuración de esta aplicación, pero no de otras aplicaciones. Una aplicación puede tener muchos propietarios y un usuario puede ser el propietario de muchas aplicaciones.

Un propietario de la aplicación puede:

* Cambiar las propiedades de la aplicación, como el nombre y los permisos de las solicitudes de aplicación
* Administración de credenciales
* Configurar inicio de sesión único
* Asignar acceso de usuario
* Agregar o quitar otros propietarios
* Editar el manifiesto de la aplicación
* Publicar la aplicación en la galería de aplicaciones

> [!IMPORTANT]
> Los usuarios asignados a este rol pueden agregar credenciales a una aplicación y usarlas para suplantar la identidad de la aplicación. Esta suplantación de la identidad de la aplicación puede ser una elevación de privilegios sobre lo que puede hacer el usuario con sus otras asignaciones de roles en Azure AD. Un usuario asignado a este rol podría crear o actualizar usuarios u otros objetos durante la suplantación de la aplicación.

El propietario de un registro de aplicación puede ver y editar el registro de aplicación.

<!-- ### To assign an enterprise app ownership role to a user

1. Sign in to your [Azure AD tenant](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) with an account that is the Global Administrator for the tenant.
2. On the [Roles and administrators page](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators), open one of the following roles to see its member assignments:
  * **Enterprise Application Owner**
  * **Application Registration Owner**
3. On the **Members** page for the role, select **Add member**.
4. Select one or more members to add to the role. -->

### <a name="to-assign-an-owner-to-an-application"></a>Para asignar un propietario a una aplicación:

1. Inicie sesión en su [inquilino de Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) con una cuenta que sea válida para el administrador de aplicaciones o el administrador de aplicaciones en la nube del inquilino.
2. En la página [Registros de aplicaciones](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) del inquilino, seleccione una aplicación para abrir su página **Información general**.
3. Seleccione **Propietarios** para ver la lista de los propietarios de la aplicación.
4. Seleccione **Agregar** para seleccionar uno o varios propietarios para agregar a la aplicación.

### <a name="to-assign-an-owner-to-an-application-registration"></a>Para asignar un propietario a un registro de aplicación:

1. Inicie sesión en su [inquilino de Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) con una cuenta que sea válida para el rol de administrador de aplicaciones o administrador de aplicaciones en la nube del inquilino.
2. Cuando tenga los permisos necesarios, en la página [Aplicaciones empresariales](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) del inquilino, seleccione un registro de aplicación para abrirlo.
3. Seleccione **Configuración**.
4. En la página **Configuración**, seleccione **Propietarios** para ver la lista de los propietarios de la aplicación.
5. Seleccione **Agregar propietario** para seleccionar uno o varios propietarios para agregar a la aplicación.

## <a name="next-steps"></a>Pasos siguientes

* [Referencia de rol de administrador de Azure AD](directory-assign-admin-roles.md)
