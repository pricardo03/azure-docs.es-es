---
title: Configuración de la administración de grupos de autoservicio en Azure Active Directory | Microsoft Docs
description: Creación y administración de grupos de seguridad o de grupos de Office 365 en Azure Active Directory y solicitud de la pertenencia a grupos de Office 365 o de seguridad
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: b860257fd1b3f0897152dc3d48bff0c7e1d3d994
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60469868"
---
# <a name="set-up-self-service-group-management-in-azure-active-directory"></a>Configuración de la administración de grupos de autoservicio en Azure Active Directory 

Puede permitir que los usuarios creen y administren sus propios grupos de seguridad o grupos de Office 365 en Azure Active Directory (Azure AD). El propietario del grupo puede aprobar o rechazar solicitudes de pertenencia y puede delegar el control de la pertenencia a grupos. Las características de administración de grupos de autoservicio no están disponibles para grupos de seguridad habilitados para correo electrónico o listas de distribución.

## <a name="self-service-group-membership-defaults"></a>Valores predeterminados de pertenencia a grupos de autoservicio

Cuando se crean grupos de seguridad en Azure Portal o con Azure AD PowerShell, solo los propietarios del grupo pueden actualizar pertenencia. Los grupos de seguridad creados en el [Panel de acceso](https://account.activedirectory.windowsazure.com/r#/joinGroups) y todos los grupos de Office 365 están disponibles para que se unan todos los usuarios, tanto los aprobados por el propietario como los aprobados automáticamente. En el Panel de acceso, puede cambiar las opciones de pertenencia al crear el grupo.

Grupos creados en | Comportamiento predeterminado del grupo de seguridad | Comportamiento predeterminado del grupo de Office 365
------------------ | ------------------------------- | ---------------------------------
[Azure AD PowerShell](groups-settings-cmdlets.md) | Solo los propietarios pueden agregar miembros<br>Visible pero no está disponible para unión en el Panel de acceso | Abierto para unirse todos los usuarios
[Azure Portal](https://portal.azure.com) | Solo los propietarios pueden agregar miembros<br>Visible pero no está disponible para unión en el Panel de acceso<br>El propietario no se asigna automáticamente durante la creación del grupo | Abierto para unirse todos los usuarios
[Panel de acceso](https://account.activedirectory.windowsazure.com/r#/joinGroups) | Abierto para unirse todos los usuarios<br>Las opciones de pertenencia se pueden cambiar cuando se crea el grupo | Abierto para unirse todos los usuarios<br>Las opciones de pertenencia se pueden cambiar cuando se crea el grupo

## <a name="self-service-group-management-scenarios"></a>Escenarios de administración de grupos de autoservicio

* **Administración de grupos delegados** Por ejemplo, un administrador que administra el acceso a una aplicación SaaS que su compañía usa. La administración de estos derechos de acceso se está volviendo compleja, por lo que este administrador solicita al propietario de la empresa la creación de un nuevo grupo. El administrador asigna acceso a la aplicación al grupo nuevo y agrega a dicho grupo todas las personas que ya acceden a la aplicación. Luego, el propietario de la empresa puede agregar más usuarios, y dichos usuarios se aprovisionan automáticamente en la aplicación. No es preciso que el propietario espere a que el administrador administre el acceso de los usuarios. Si el administrador concede el mismo permiso a un administrador de otro grupo de negocios, dicha persona también puede administrar el acceso de sus propios usuarios. Ni el propietario de una empresa ni el administrador pueden ver o administrar las pertenencias al grupo del otro. El administrador podrá seguir viendo todos los usuarios que tienen acceso a la aplicación y bloquear los derechos de acceso si fuera necesario.
* **Administración de grupos de autoservicio** Por ejemplo, dos usuarios tienen sitios de SharePoint Online configurados de forma independiente. Ellos desean que los equipos del otro puedan acceder a sus sitios. Para ello, pueden crear un grupo en Azure AD, y en SharePoint Online cada uno de ellos selecciona dicho grupo para proporcionar acceso a sus sitios. Cuando alguien desea tener acceso, lo solicita en el Panel de acceso, y tras la aprobación obtiene acceso a ambos sitios de SharePoint Online automáticamente. Posteriormente, uno de ellos decide que todas las personas que accedan a su sitio también deben obtener acceso a una aplicación SaaS concreta. El administrador de la aplicación SaaS puede agregar derechos de acceso a la aplicación en el sitio de SharePoint Online. Desde ese momento, todas las solicitudes aprobadas darán acceso tanto a los dos sitios de SharePoint Online como a la aplicación SaaS.

## <a name="make-a-group-available-for-user-self-service"></a>Puesta a disposición de un grupo para el autoservicio del usuario

1. Inicie sesión en el [Centro de administración de Azure AD](https://aad.portal.azure.com) con una cuenta que tenga el rol de administrador global en el directorio.
2. Seleccione **Usuarios y grupos** y, luego, **Todos los usuarios**.
3. Establezca **Administración de grupos de autoservicio habilitada** en **Sí**.
4. Establezca **Los usuarios pueden crear grupos de seguridad** o **Los usuarios pueden crear grupos de Office 365** en **Sí**.
   * Si estas opciones están habilitadas, todos los usuarios del directorio pueden crear nuevos grupos de seguridad y agregarles miembros. Estos grupos nuevos también se muestran en el Panel de acceso para los restantes usuarios. Si la configuración de la directiva en el grupo lo permite, otros usuarios pueden crear solicitudes para unirse a dichos grupos. 
   * Si están deshabilitadas, los usuarios no pueden crear grupos ni pueden cambiar los grupos existentes de los que sean propietarios. Sin embargo, pueden administrar la pertenencia a dichos grupos y aprobar las solicitudes de otros usuarios para unirse a ellos.

También puede utilizar la opción **Usuarios que pueden administrar los grupos de seguridad** y **Usuarios que pueden administrar los grupos de Office 365** para conseguir un control de acceso más específico sobre la administración de grupos de autoservicio para los usuarios. Cuando la opción **Los usuarios pueden crear grupos** esté habilitada, todos los usuarios de su inquilino podrán crear nuevos grupos y agregarles miembros. No puede especificar a personas que puedan crear sus propios grupos. Solo puede especificar a personas para convertir a otro miembro del grupo en propietario del grupo.

Si en **Usuarios que pueden utilizar el autoservicio para grupos de seguridad** y **Usuarios que pueden administrar grupos de Office 365** se selecciona **Sí**, se permite que todos los usuarios del inquilino creen grupos.

También puede usar **Grupo que puede administrar los grupos de seguridad** o **Grupo que puede administrar los grupos de Office 365** para especificar un único grupo cuyos miembros pueden utilizar el autoservicio.

## <a name="next-steps"></a>Pasos siguientes

Estos artículos proporcionan información adicional sobre Azure Active Directory.

* [Administración del acceso a recursos con grupos de Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Cmdlets de Azure Active Directory para configurar las opciones de grupo](groups-settings-cmdlets.md)
* [Administración de aplicaciones en Azure Active Directory](../manage-apps/what-is-application-management.md)
* [¿Qué es Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Integre las identidades locales con Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
