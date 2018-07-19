---
title: Asignación de roles de administrador en Azure Active Directory | Microsoft Docs
description: Un rol de administrador puede agregar usuarios, asignar roles administrativos, restablecer contraseñas de usuario, administrar licencias de usuario o administrar dominios. Un usuario que tiene asignado el rol de administrador tiene los mismos permisos en todos los servicios en la nube a la que se ha suscrito su organización.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 06/07/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 33facedd1f2b3e2330a2a89a38084d99234fbf2e
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2018
ms.locfileid: "37346707"
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Asignación de roles de administrador en Azure Active Directory

Con Azure Active Directory (Azure AD), puede designar administradores independientes que desempeñen distintas funciones. Los administradores se pueden designar en el portal de Azure AD para realizar tareas como agregar usuarios o cambiarlos, asignar roles administrativos, restablecer contraseñas de usuario, administrar licencias de usuario y administración de nombres de dominio.

## <a name="details-about-the-global-administrator-role"></a>Detalles acerca del rol de administrador global
El administrador global tiene acceso a todos los roles administrativos. De forma predeterminada, a la persona que se suscribe a una suscripción de Azure se le asigna el rol de administrador global para el directorio. Los administradores globales son los únicos que pueden asignar otros roles de administrador.

## <a name="assign-or-remove-administrator-roles"></a>Asignación o eliminación de roles de administrador
Para obtener información sobre cómo asignar roles administrativos a un usuario en Azure Active Directory, consulte [Asignación de un usuario a roles de administrador en Azure Active Directory](active-directory-users-assign-role-azure-portal.md).

## <a name="available-roles"></a>Roles disponibles
Los roles de administrador disponibles son los siguientes:

* **[Administrador de aplicaciones](#application-administrator)**: los usuarios con este rol pueden crear y administrar todos los aspectos de las aplicaciones empresariales, los registros de aplicaciones y la configuración del proxy de aplicación. Este rol proporciona igualmente la capacidad de dar el consentimiento para permisos delegados y permisos de aplicaciones, excepto Microsoft Graph y Azure AD Graph. Los miembros de este rol no se agregan como propietarios al crear nuevos registros de aplicaciones o aplicaciones empresariales.

* **[Desarrollador de aplicaciones](#application-developer)**: los usuarios con este rol pueden crear registros de aplicaciones cuando la opción "Users can register applications" (Los usuarios pueden registrar aplicaciones) está establecida en No. Esta función también permite a los miembros dar su consentimiento en su propio nombre cuando la opción "Los usuarios pueden permitir que las aplicaciones accedan a los datos de la compañía en su nombre" está establecida en No. Los miembros de este rol se agregan como propietarios al crear nuevos registros de aplicaciones o aplicaciones empresariales.

* **[Administrador de facturación](#billing-administrator)**: hace compras, administra suscripciones, administra incidencias de soporte técnico y supervisa el estado del servicio.

* **[Administrador de aplicaciones en la nube](#cloud-application-administrator)**: los usuarios con este rol tienen los mismos permisos que el rol de administrador de la aplicación, excluida la capacidad de administrar el proxy de aplicación. Este rol concede la capacidad de crear y administrar todos los aspectos de las aplicaciones empresariales y los registros de aplicaciones. Este rol proporciona igualmente la capacidad de dar el consentimiento para permisos delegados y permisos de aplicaciones, excepto Microsoft Graph y Azure AD Graph. Los miembros de este rol no se agregan como propietarios al crear nuevos registros de aplicaciones o aplicaciones empresariales.

* **[Administrador de cumplimiento](#compliance-administrator)**: los usuarios con este rol tienen permisos de administración en el Centro de seguridad y cumplimiento de Office 365 y el Centro de administración de Exchange. Más información en [Acerca de los roles de administrador de Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Administrador de acceso condicional](#conditional-access-administrator)**: los usuarios con este rol tienen la capacidad de administrar la configuración de acceso condicional de Azure Active Directory.
  > [!NOTE]
  > Para implementar la directiva de acceso condicional de Exchange ActiveSync en Azure, el usuario también debe ser administrador global.
  
* **[Administrador del servicio Dynamics 365 o Administrador de servicios de CRM](#crm-service-administrator)**: los usuarios con este rol tienen permisos globales en Microsoft CRM Online cuando el servicio está presente, así como también la posibilidad de administrar incidencias de soporte técnico y supervisar el estado del servicio. Puede encontrar más información en [Uso del rol de administrador de servicios para administrar inquilinos](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).

* **[Administradores de dispositivos](#device-administrators)**: los usuarios con este rol se convierten en administradores del equipo local en todos los dispositivos Windows 10 que estén unidos a Azure Active Directory. No tienen la capacidad de administrar objetos de dispositivos en Azure Active Directory.

* **[Lectores de directorio](#directory-readers)**: se trata de un rol heredado que se va a asignar a las aplicaciones que no admitan el [marco de consentimiento](active-directory-integrating-applications.md). No se debe asignar a ningún usuario.

* **[Cuentas de sincronización de directorios](#directory-synchronization-accounts)**: no las use. Este rol se asigna automáticamente al servicio de Azure AD Connect y no está previsto ni se admite para ningún otro uso.

* **[Lectores de directorio](#directory-writers)**: es un rol heredado que se asigna a las aplicaciones que no admiten el [marco de consentimiento](active-directory-integrating-applications.md). No se debe asignar a ningún usuario.

* **[Administrador de servicios de Exchange](#exchange-service-administrator)**: los usuarios con este rol tienen permisos globales en Microsoft Exchange Online, cuando el servicio está presente. Más información en [Acerca de los roles de administrador de Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Administrador global/administrador de la compañía](#company-administrator)**: los usuarios con este rol tienen acceso a todas las características administrativas en Azure Active Directory, así como a los servicios que usan identidades de Azure Active Directory, como Exchange Online, SharePoint Online y Skype Empresarial Online. La persona que se suscribe al inquilino de Azure Active Directory se convierte en un administrador global. Los administradores globales son los únicos que pueden asignar otros roles de administrador. Puede haber más de un administrador global en su empresa. Los administradores globales pueden restablecer la contraseña de todos los usuarios y de todos los demás administradores.

  > [!NOTE]
  > En Microsoft Graph API, Graph API de Azure AD, y Azure AD PowerShell, este rol se identifica como "administrador de la compañía". Es "administrador Global" en [Azure Portal](https://portal.azure.com).
  >
  >

* **[Invitador de usuarios](#guest-inviter)**: los usuarios con este rol pueden administrar las invitaciones de usuarios invitados de Azure Active Directory B2B cuando la configuración de usuario **Members can invite** (Los miembros pueden invitar) está establecida en No. Más información sobre la colaboración B2B en [¿Qué es la colaboración B2B de Azure AD?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) No incluye otros permisos.

* **[Administrador de Information Protection](#information-protection-administrator)**: los usuarios con este rol tienen todos los permisos en el servicio Azure Information Protection. Este rol permite configurar las etiquetas de la directiva de Azure Information Protection, administrar plantillas de protección y activar la protección. Este rol no concede ningún permiso de Identity Protection Center, Privileged Identity Management, Supervisión del estado de mantenimiento del servicio Office 365 y Centro de seguridad y cumplimiento de Office 365.

* **[Administrador de servicios de Intune](#intune-service-administrator)**: los usuarios con este rol tienen permisos globales en Microsoft Intune Online, cuando el servicio está presente. Además, este rol contiene la capacidad de administrar usuarios y dispositivos para asociar una directiva, así como también para crear y administrar grupos. Para más información, consulte [Control de administración basado en rol (RBAC) con Microsoft Intune](https://docs.microsoft.com/en-us/intune/role-based-access-control)

* **[Lector del centro de mensajes](#message-center-reader)**: los usuarios con este rol pueden supervisar las notificaciones y las actualizaciones de mantenimiento de aviso en el [Centro de mensajes de Office 365](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) para su organización en los servicios configurados como Exchange, Intune y de Microsoft Teams. Los lectores del centro de mensajes reciben semanal resúmenes de correo electrónico de publicaciones y actualizaciones y pueden compartir entradas del centro de mensajes en Office 365. En Azure AD, los usuarios asignados a este rol solo tendrán acceso de solo lectura en los servicios de Azure AD, como usuarios y grupos. 

* **[Soporte para asociados de nivel 1](#partner-tier1-support)**: no se usa. Este rol está en desuso y se quitará de Azure AD en el futuro. Este rol está diseñado para que lo usen un pequeño número de asociados de reventa de Microsoft, no para un uso general.

* **[Soporte para asociados de nivel 2](#partner-tier2-support)**: no se usa. Este rol está en desuso y se quitará de Azure AD en el futuro. Este rol está diseñado para que lo usen un pequeño número de asociados de reventa de Microsoft, no para un uso general.

* **[Administrador de contraseñas y Administrador del departamento de soporte técnico](#helpdesk-administrator)**: los usuarios con estos roles pueden cambiar contraseñas, administrar solicitudes de servicio y supervisar el estado del servicio. Los administradores del departamento de soporte técnico pueden cambiar contraseñas solo para los usuarios y otros administradores del mismo departamento. 

  > [!NOTE]
  > En Microsoft Graph API, Azure AD Graph API y Azure AD PowerShell, este rol se identifica como "Administrador del departamento de soporte técnico". En "Administrador de contraseñas" en [Azure Portal](https://portal.azure.com/).
  >
  >
  
* **[Administrador de servicios de Power BI](#power-bi-service-administrator)**: los usuarios con este rol tienen permisos globales en Microsoft Power BI, cuando el servicio está presente, así como también la capacidad de administrar incidencias de soporte técnico y supervisar el estado del servicio. Puede encontrar más información en [Descripción del rol de administrador de Power BI](https://docs.microsoft.com/en-us/power-bi/service-admin-role).

* **[Administrador de roles con privilegios](#privileged-role-administrator)**: los usuarios con este rol pueden administrar asignaciones de roles en Azure Active Directory, así como en Azure AD Privileged Identity Management. Además, este rol permite administrar todos los aspectos de Privileged Identity Management.

* **[Lector de informes](#reports-reader)**: los usuarios con este rol pueden ver datos de informes de uso y el panel de informes en el centro de administración de Office 365 y el paquete del contexto de adopción en Power BI. Además, el rol proporciona acceso a informes de inicio de sesión y a actividad de Azure AD y a los datos devueltos por la API de informes de Microsoft Graph. Un usuario asignado al rol de lector de informes puede acceder solo a métricas de uso y adopción pertinentes. No tienen permisos de administrador para configurar valores ni acceder a los centros de administración específicos de productos como Exchange. 

* **[Administrador de seguridad](#security-administrator)**: los usuarios con este rol tienen todos los permisos de solo lectura del rol Lector de seguridad más la capacidad de administrar la configuración de servicios relacionados con la seguridad: Azure Active Directory Identity Protection, Azure Information Protection, Privileged Identity Management y Centro de seguridad y cumplimiento de Office 365. Encuentre más información disponible sobre los permisos de Office 365 en [Permisos del Centro de seguridad y cumplimiento de Office 365](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
  | En el | Puede hacer |
  | --- | --- |
  | Identity Protection Center |<ul><li>Todos los permisos del rol Lector de seguridad.<li>Además, la posibilidad de realizar todas las operaciones de IPC, excepto la de restablecer contraseñas. |
  | Privileged Identity Management |<ul><li>Todos los permisos del rol Lector de seguridad.<li>**No puede** administrar la configuración de roles de Azure AD o la pertenencia a ellos. |
  | <p>Supervisión de estado del servicio de Office 365</p><p>Centro de cumplimiento y seguridad de Office 365 |<ul><li>Todos los permisos del rol Lector de seguridad.<li>Puede configurar todos los valores de la característica Protección frente a amenazas avanzada (malware y protección antivirus, configuración de direcciones URL malintencionadas, seguimiento de URL, etc.). |
  
* **[Lector de seguridad](#security-reader)**: los usuarios con este rol tienen acceso global de solo lectura, incluida toda información en Azure Active Directory, Identity Protection, Privileged Identity Management, así como la capacidad de leer registros de auditoría e informes de inicio de sesión de Azure Active Directory. El rol también concede permiso de solo lectura en el Centro de seguridad y cumplimiento de Office 365. Encuentre más información disponible sobre los permisos de Office 365 en [Permisos del Centro de seguridad y cumplimiento de Office 365](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

  | En | Puede hacer |
  | --- | --- |
  | Identity Protection Center |Leer todos los informes de seguridad y la información de configuración de las características de seguridad<ul><li>Filtro de correo no deseado<li>Cifrado<li>Prevención de la pérdida de datos<li>Antimalware<li>Protección contra amenazas avanzada<li>Protección contra suplantación de identidad (anti-phishing)<li>Reglas de flujo de correo |
  | Privileged Identity Management |<p>Tiene acceso de solo lectura a toda la información que aparece en PIM de Azure AD: directivas e informes para las asignaciones de roles de Azure AD, revisiones de seguridad y, en el futuro, acceso de lectura a datos e informes de directivas en escenarios distintos a la asignación de roles de Azure AD.<p>**No puede** registrarse para PIM de Azure AD ni realizar ningún cambio en esta característica. En el portal de PIM o a través de PowerShell, alguien con este rol puede activar roles adicionales (por ejemplo, Administrador Global o Administrador de rol con privilegios), si el usuario es un candidato para ellos. |
  | <p>Supervisión de estado de mantenimiento del servicio de Office 365</p><p>Centro de seguridad y cumplimiento de Office 365</p> |<ul><li>Leer y administrar alertas<li>Leer directivas de seguridad<li>Leer inteligencia de amenazas, Cloud App Discovery y cuarentena en búsqueda e investigación<li>Leer todos los informes |

* **[Administrador de soporte técnico de servicio](#service-support-administrator)**: los usuarios con este rol pueden abrir solicitudes de soporte técnico con Microsoft para servicios de Azure y Office 365 y visualizaciones del centro de mensajes y el panel de servicios en Azure Portal y el portal de administración de Office 365. Más información en [Acerca de los roles de administrador de Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Administrador de servicios de SharePoint](#sharepoint-service-administrator)**: los usuarios con este rol tienen permisos globales en Microsoft SharePoint Online, cuando el servicio está presente, así como también la capacidad de administrar incidencias de soporte técnico y supervisar el estado del servicio. Más información en [Acerca de los roles de administrador de Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Skype Empresarial/Administrador de servicios de Lync](#lync-service-administrator)**: los usuarios con este rol tienen permisos globales en Microsoft Skype Empresarial, cuando el servicio está presente, así como administrar los atributos de usuario específicos de Skype en Azure Active Directory. Además, este rol concede la capacidad de administrar incidencias de soporte técnico y supervisar el estado del servicio. Puede obtener más información en la sección sobre [información del rol de administrador de Skype Empresarial](https://support.office.com/en-us/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5).

  > [!NOTE]
  > En Microsoft Graph API, Graph API de Azure AD y Azure AD PowerShell, este rol se identifica como "Administrador de servicios de Lync". Es "Administrador del servicio Skype Empresarial" en [Azure Portal](https://portal.azure.com/).
  >
  >

* **[Administrador de cuenta de usuario](#user-account-administrator)**: los usuarios con este rol pueden crear y administrar todos los aspectos de los usuarios y grupos. Además, este rol incluye la capacidad de administrar incidencias de soporte técnico y supervisar el estado del servicio. Se aplican algunas restricciones. Por ejemplo, este rol no permite eliminar un administrador global. Los administradores de cuentas de usuario solo pueden cambiar las contraseñas de los usuarios, de los administradores del departamento de soporte técnico y de otros administradores de cuentas de usuario.

| Puede hacer | No puede hacer |
| --- | --- |
| <p>Ver información de usuario y de la compañía</p><p>Administrar incidencias de soporte técnico de Office</p><p>Cambiar solo las contraseñas de los usuarios, de los administradores del departamento de soporte técnico y de otros administradores de cuentas de usuario</p><p>Crear y administrar vistas de usuario</p><p>Crear, editar y eliminar usuarios y grupos, y administrar licencias de usuarios, con limitaciones. No puede eliminar un administrador global ni crear otros administradores.</p> |<p>Realizar operaciones de facturación y compra productos de Office</p><p>Administrar dominios</p><p>Administrar información de la compañía</p><p>Delegar roles administrativos a otros usuarios</p><p>Usar la sincronización de directorios</p><p>Habilitar o deshabilitar Multi-Factor Authentication</p><p>Visualización de registros de auditoría</p> |

## <a name="deprecated-roles"></a>Roles en desuso

Los siguientes roles no deben usarse. Están en desuso y se eliminarán de Azure AD más adelante.

* Administrador de licencias ad hoc
* Combinación de dispositivos
* Administradores de dispositivos
* Usuarios de dispositivos
* Creador de usuarios comprobados de correo electrónico
* Administrador de buzones de correo
* Combinación de dispositivos de área de trabajo


### <a name="to-add-a-colleague-as-a-global-administrator"></a>Para agregar un compañero como administrador global

1. Inicie sesión en el [Centro de administración de Azure Active Directory](https://aad.portal.azure.com) con una cuenta que sea del administrador global o del administrador de rol con privilegios en el directorio de inquilinos.

   ![Abrir el Centro de administración de Azure AD](./media/active-directory-assign-admin-roles-azure-portal/active-directory-admin-center.png)

2. Seleccione **Usuarios**.

3. Busque el usuario al que quiera designar como administrador global y abra la hoja de ese usuario.

4. En la hoja del usuario, seleccione **Rol de directorio**.
 
5. En la hoja de rol de directorio, seleccione el rol **Administrador global** y guárdelo.

## <a name="detailed-azure-active-directory-permissions"></a>Permisos detallados de Azure Active Directory
Las tablas siguientes describen los permisos específicos concedidos a cada rol en Azure Active Directory. Algunos roles, como el de Administrador Global, pueden tener permisos adicionales en los servicios de Microsoft fuera de Azure Active Directory.


### <a name="application-administrator"></a>Administrador de aplicaciones
Puede crear y administrar todos los aspectos de los registros de aplicaciones y de las aplicaciones empresariales.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/Application/Create | Crea aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/Application/Delete | Elimina aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/Application/Update | Actualiza las propiedades estándar de las aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/Application/Update/DefaultPolicy | Actualiza la propiedad Applications.DefaultPolicy en Azure Active Directory. |
| microsoft.aad.directory/Application/Update/Owners | Actualiza la propiedad Applications.Owners en Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Create | Crea el elemento AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Delete | Elimina AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Update | Actualiza las propiedades estándar de AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/Policy/Create | Crea directivas en Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Elimina directivas en Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Actualiza las propiedades estándar de las directivas en Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Actualiza la propiedad Policies.Owners en Azure Active Directory. |
| microsoft.aad.directory/ServiceAction/ConsentOnBehalfOfAllNoDirectory | Puede realizar autorizaciones en nombre de todos los usuarios para todos los recursos excepto en Azure Active Directory (Azure AD Graph y Microsoft Graph). |
| microsoft.aad.directory/ServicePrincipal/Create | Crea ServicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Delete | Elimina ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update | Actualiza las propiedades estándar de ServicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignedTo | Actualiza la propiedad ServicePrincipals.AppRoleAssignedTo en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignments | Actualiza ServicePrincipals.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Actualiza ServicePrincipals.DefaultPolicy en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/Owners | Actualiza ServicePrincipals.Owners en Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Administra licencias de los usuarios en Azure Active Directory. |
| microsoft.aad.reports/AllEntities/Read | Lee informes de Azure AD. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="application-developer"></a>Desarrollador de aplicaciones
Puede crear registros de aplicación independientemente de la opción de configuración **Los usuarios pueden registrar aplicaciones**.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/Application/CreateAsOwner | Crea aplicaciones en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos creados del creador. |
| microsoft.aad.directory/AppRoleAssignment/CreateAsOwner | Crea el elemento AppRoleAssignments en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos creados del creador. |
| microsoft.aad.directory/OAuth2PermissionGrant/CreateAsOwner | Crea el elemento OAuth2PermissionGrants en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos creados del creador. |
| microsoft.aad.directory/ServicePrincipal/CreateAsOwner | Crea ServicePrincipals en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos creados del creador. |

### <a name="billing-administrator"></a>Administrador de facturación
Puede realizar tareas comunes relacionadas con la facturación como actualizar la información de pago.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol descrita anteriormente.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lee Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.directory/Organization/Update | Actualiza las propiedades estándar del Organizations en Azure Active Directory. |
| microsoft.aad.directory/Organization/Update/TrustedCAsForPasswordlessAuth | Actualiza Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |
| microsoft.aad.billing/AllEntities/AllActions | Administra todos los aspectos de la facturación de Office 365. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="cloud-application-administrator"></a>Administrador de aplicaciones en la nube
Puede crear y administrar todos los aspectos de los registros de aplicaciones y de las aplicaciones empresariales, excepto el proxy de aplicación.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/Application/Create | Crea aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/Application/Delete | Elimina aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/Application/Update | Actualiza las propiedades estándar de las aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/Application/Update/DefaultPolicy | Actualiza la propiedad Applications.DefaultPolicy en Azure Active Directory. |
| microsoft.aad.directory/Application/Update/Owners | Actualiza la propiedad Applications.Owners en Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Create | Crea el elemento AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Delete | Elimina AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Update | Actualiza las propiedades estándar de AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/Policy/Create | Crea directivas en Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Elimina directivas en Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Actualiza las propiedades estándar de las directivas en Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Actualiza la propiedad Policies.Owners en Azure Active Directory. |
| microsoft.aad.directory/ServiceAction/ConsentOnBehalfOfAllNoDirectory | Puede realizar autorizaciones en nombre de todos los usuarios para todos los recursos excepto en Azure Active Directory (Azure AD Graph y Microsoft Graph). |
| microsoft.aad.directory/ServicePrincipal/Create | Crea ServicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Delete | Elimina ServicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update | Actualiza las propiedades estándar de ServicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignedTo | Actualiza la propiedad ServicePrincipals.AppRoleAssignedTo en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignments | Actualiza ServicePrincipals.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Actualiza ServicePrincipals.DefaultPolicy en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/Owners | Actualiza ServicePrincipals.Owners en Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Administra licencias de los usuarios en Azure Active Directory. |
| microsoft.aad.reports/AllEntities/Read | Lee informes de Azure AD. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="company-administrator"></a>Administrador de la compañía
Puede administrar todos los aspectos de los servicios de Azure AD y Microsoft que usan identidades de Azure AD. En Microsoft Graph API, Graph API de Azure AD, y Azure AD PowerShell, este rol se identifica como "administrador de la compañía". Es "administrador Global" en [Azure Portal](https://portal.azure.com).

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/AdministrativeUnit/AllActions/AllProperties | Crea y elimina AdministrativeUnits, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/Application/AllActions/AllProperties | Crea y elimina aplicaciones, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/AllActions/AllProperties | Crea y elimina AppRoleAssignments, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/CollaborationSpace/AllActions/AllProperties | Crea y elimina CollaborationSpaces, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/Contact/AllActions/AllProperties | Crea y elimina contactos, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/Device/AllActions/AllProperties | Crea y elimina dispositivos, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/DirectoryRole/AllActions/AllProperties | Crea y elimina el elemento DirectoryRoles, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/DirectoryRoleTemplate/AllActions/AllProperties | Crea y elimina el elemento DirectoryRoleTemplates, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/DirectorySetting/AllActions/AllProperties | Crea y elimina DirectorySettings, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/DirectorySettingTemplate/AllActions/AllProperties | Crea y elimina DirectorySettingTemplates, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/Domain/AllActions/AllProperties | Crea y elimina Domains, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/Group/AllActions/AllProperties | Crea y elimina Groups, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/LoginTenantBranding/AllActions/AllProperties | Crea y elimina LoginTenantBrandings, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/OAuth2PermissionGrant/AllActions/AllProperties | Crea y elimina OAuth2PermissionGrants, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/Policy/AllActions/AllProperties | Crea y elimina Policies, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/ServiceAction/ConsentOnBehalfOfAllWithDirectory | Puede realizar autorizaciones en nombre de todos los usuarios para todos los recursos, incluido Azure Active Directory (Azure AD Graph y Microsoft Graph). |
| microsoft.aad.directory/ServicePrincipal/AllActions/AllProperties | Crea y elimina ServicePrincipals, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/Organization/AllActions/AllProperties | Crea y elimina el elemento Organizations, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/User/AllActions/AllProperties | Crea y elimina Users, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.aadconnect/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en microsoft.aad.aadconnect. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |
| microsoft.aad.billing/AllEntities/AllActions | Administra todos los aspectos de la facturación de Office 365. |
| microsoft.aad.compliance/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en el Centro de cumplimiento. |
| microsoft.aad.directorysync/AllEntities/AllActions | Realiza todas las acciones en Azure AD Connect. |
| microsoft.aad.lockbox/AllEntities/AllActions | Administra todos los aspectos del servicio Lockbox. |
| microsoft.aad.privilegedrolemanagement/AllEntities/AllActions | Administra todos los aspectos del servicio Privileged Role Management. |
| microsoft.aad.reports/AllEntities/AllActions | Lee y configura informes de Azure AD. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.crm/AllEntities/AllActions | Administra todos los aspectos de Dynamics 365. |
| microsoft.exchange/AllEntities/AllActions | Administra todos los aspectos de Exchange Online. |
| microsoft.aad.informationprotection/AllEntities/AllActions | Administra todos los aspectos de Information Protection. |
| microsoft.intune/AllEntities/AllActions | Administra todos los aspectos de Intune. |
| microsoft.powerbi/AllEntities/AllActions | Administra todos los aspectos de Power BI. |
| microsoft.protectioncenter/AllEntities/AllActions | Administra el Centro de protección de Office 365. |
| microsoft.sharepoint/AllEntities/AllActions | Administra SharePoint Online. |
| microsoft.skypeforbusiness/AllEntities/AllActions | Administra Skype Empresarial Online. |

### <a name="compliance-administrator"></a>Administrador de cumplimiento
Puede leer y administrar los informes y la configuración de cumplimiento en Azure AD y Office 365.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |
| microsoft.aad.compliance/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en el Centro de cumplimiento. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.exchange/Compliance/AllActions | Administra el cumplimiento en Exchange Online. |
| microsoft.sharepoint/Compliance/AllActions | Administra el cumplimiento en SharePoint Online. |
| microsoft.skypeforbusiness/Compliance/AllActions | Administra el cumplimiento en Skype Empresarial Online. |

### <a name="conditional-access-administrator"></a>Administrador de acceso condicional
Puede administrar las funcionalidades de acceso condicional.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/ConditionalAccessPolicy/Create | Crea elementos ConditionalAccessPolicys en Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Delete | Eliminar elementos ConditionalAccessPolicys en Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Read | Lee las propiedades estándar de los elementos ConditionalAccessPolicys en Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Read/Owners | Lee la propiedad ConditionalAccessPolicys.Owners en Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Read/PolicyAppliedTo | Lee la propiedad ConditionalAccessPolicys.PolicyAppliedTo en Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Update | Actualiza las propiedades estándar de ConditionalAccessPolicys en Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Update/Owners | Actualiza la propiedad ConditionalAccessPolicys.Owners en Azure Active Directory. |

### <a name="crm-service-administrator"></a>Administrador de servicios de CRM
Puede administrar todos los aspectos del producto Dynamics 365.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lee Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.crm/AllEntities/AllActions | Administra todos los aspectos de Dynamics 365. |

### <a name="device-administrators"></a>Administradores de dispositivos
Los miembros de este rol se agregan al grupo de administradores locales en dispositivos unidos Azure AD.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |

### <a name="directory-reader"></a>Lector del directorio
Puede leer la información básica del directorio. Para conceder acceso a aplicaciones

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/AdministrativeUnit/Read | Lee las propiedades estándar de AdministrativeUnits en Azure Active Directory. |
| microsoft.aad.directory/AdministrativeUnit/Read/Members | Lee la propiedad AdministrativeUnits.Members en Azure Active Directory. |
| microsoft.aad.directory/Application/Read | Leer las propiedades estándar de Applications en Azure Active Directory. |
| microsoft.aad.directory/Application/Read/Owners | Lee la propiedad Applications.Owners en Azure Active Directory. |
| microsoft.aad.directory/CollaborationSpace/Read | Lee las propiedades estándar de CollaborationSpaces en Azure Active Directory. |
| microsoft.aad.directory/CollaborationSpace/Read/Owners | Lee la propiedad CollaborationSpaces.Owners en Azure Active Directory. |
| microsoft.aad.directory/Contact/Read | Lee las propiedades estándar de los contactos en Azure Active Directory. |
| microsoft.aad.directory/Contact/Read/MemberOf | Lee la propiedad Contacts.MemberOf en Azure Active Directory. |
| microsoft.aad.directory/Device/Read | Lee las propiedades estándar de Devices en Azure Active Directory. |
| microsoft.aad.directory/Device/Read/MemberOf | Lee la propiedad Devices.MemberOf en Azure Active Directory. |
| microsoft.aad.directory/Device/Read/RegisteredOwners | Lee la propiedad Devices.RegisteredOwners en Azure Active Directory. |
| microsoft.aad.directory/Device/Read/RegisteredUsers | Lee la propiedad Devices.RegisteredUsers en Azure Active Directory. |
| microsoft.aad.directory/DirectoryRole/Read | Lee las propiedades estándar de DirectoryRoles en Azure Active Directory. |
| microsoft.aad.directory/DirectoryRole/Read/EligibleMembers | Lee la propiedad DirectoryRoles.EligibleMembers en Azure Active Directory. |
| microsoft.aad.directory/DirectoryRole/Read/Members | Lee la propiedad DirectoryRoles.Members en Azure Active Directory. |
| microsoft.aad.directory/DirectorySetting/Read | Lee las propiedades estándar de DirectorySettings en Azure Active Directory. |
| microsoft.aad.directory/DirectorySettingTemplate/Read | Lee las propiedades estándar de DirectorySettingTemplates en Azure Active Directory. |
| microsoft.aad.directory/Domain/Read | Lee las propiedades estándar de Domains en Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Lee las propiedades estándares del elemento Groups en Azure Active Directory. |
| microsoft.aad.directory/Group/Read/AppRoleAssignments | Lee la propiedad Groups.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/Group/Read/MemberOf | Lee la propiedad Groups.MemberOf en Azure Active Directory. |
| microsoft.aad.directory/Group/Read/Members | Lee la propiedad Groups.Members en Azure Active Directory. |
| microsoft.aad.directory/Group/Read/Owners | Lee la propiedad Groups.Owners en Azure Active Directory. |
| microsoft.aad.directory/Group/Read/Settings | Lee la propiedad Groups.Settings en Azure Active Directory. |
| microsoft.aad.directory/OAuth2PermissionGrant/Read | Lee las propiedades estándar de OAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read | Lee las propiedades estándar de ServicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignedTo | Lee la propiedad ServicePrincipals.AppRoleAssignedTo en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignments | Lee la propiedad ServicePrincipals.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/DefaultPolicy | Lee la propiedad ServicePrincipals.DefaultPolicy en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/MemberOf | Lee la propiedad ServicePrincipals.MemberOf en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OAuth2PermissionGrants | Lee la propiedad ServicePrincipals.OAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/Owners | Lee la propiedad ServicePrincipals.Owners en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OwnedObjects | Lee la propiedad ServicePrincipals.OwnedObjects en Azure Active Directory. |
| microsoft.aad.directory/Organization/Read | Lee las propiedades estándar de Organizations en Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lee Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.directory/User/Read | Lee las propiedades estándar de Users en Azure Active Directory. |
| microsoft.aad.directory/User/Read/AppRoleAssignments | Lee la propiedad Users.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/User/Read/DirectReports | Lee la propiedad Users.DirectReports en Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedBy | Lee la propiedad Users.InvitedBy en Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedUsers | Lee la propiedad Users.InvitedUsers en Azure Active Directory. |
| microsoft.aad.directory/User/Read/Manager | Lee la propiedad Users.Manager en Azure Active Directory. |
| microsoft.aad.directory/User/Read/MemberOf | Lee la propiedad Users.MemberOf en Azure Active Directory. |
| microsoft.aad.directory/User/Read/OAuth2PermissionGrants | Lee la propiedad Users.OAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedDevices | Lee la propiedad Users.OwnedDevices en Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedObjects | Lee la propiedad Users.OwnedObjects en Azure Active Directory. |
| microsoft.aad.directory/User/Read/RegisteredDevices | Lee la propiedad Users.RegisteredDevices en Azure Active Directory. |

### <a name="directory-synchronization-accounts"></a>Cuentas de sincronización de directorios
Solo lo usa el servicio de Azure AD Connect.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/Policy/Create | Crea directivas en Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Elimina directivas en Azure Active Directory. |
| microsoft.aad.directory/Policy/Read | Lee las propiedades estándar de Policies en Azure Active Directory. |
| microsoft.aad.directory/Policy/Read/Owners | Lee la propiedad Policies.Owners en Azure Active Directory. |
| microsoft.aad.directory/Policy/Read/PolicyAppliedTo | Lee la propiedad Policies.PolicyAppliedTo en Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Actualiza las propiedades estándar de las directivas en Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Actualiza la propiedad Policies.Owners en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Create | Crea ServicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read | Lee las propiedades estándar de ServicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignedTo | Lee la propiedad ServicePrincipals.AppRoleAssignedTo en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignments | Lee la propiedad ServicePrincipals.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/DefaultPolicy | Lee la propiedad ServicePrincipals.DefaultPolicy en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/MemberOf | Lee la propiedad ServicePrincipals.MemberOf en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OAuth2PermissionGrants | Lee la propiedad ServicePrincipals.OAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/Owners | Lee la propiedad ServicePrincipals.Owners en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OwnedObjects | Lee la propiedad ServicePrincipals.OwnedObjects en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update | Actualiza las propiedades estándar de ServicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignedTo | Actualiza la propiedad ServicePrincipals.AppRoleAssignedTo en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignments | Actualiza ServicePrincipals.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Actualiza ServicePrincipals.DefaultPolicy en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/Owners | Actualiza ServicePrincipals.Owners en Azure Active Directory. |
| microsoft.aad.directory/Organization/Update/DirSync | Actualiza la propiedad Organizations.DirSync en Azure Active Directory. |
| microsoft.aad.directorysync/AllEntities/AllActions | Realiza todas las acciones en Azure AD Connect. |

### <a name="directory-writer"></a>Escritor de directorios
Puede leer y escribir información básica del directorio. Para conceder acceso a aplicaciones

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/DirectorySetting/Create | Crea DirectorySettings en Azure Active Directory. |
| microsoft.aad.directory/DirectorySetting/Delete | Elimina el elemento DirectorySettings en Azure Active Directory. |
| microsoft.aad.directory/DirectorySetting/Update | Actualiza las propiedades estándar de DirectorySettings en Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Crea grupos en Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Crea grupos en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos creados del creador. |
| microsoft.aad.directory/Group/Read | Lee las propiedades estándares del elemento Groups en Azure Active Directory. |
| microsoft.aad.directory/Group/Update | Actualiza las propiedades estándar de Groups en Azure Active Directory. |
| microsoft.aad.directory/Group/Update/AppRoleAssignments | Actualiza la propiedad Groups.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Actualiza la propiedad Groups.Members en Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Actualiza la propiedad Groups.Owners en Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Settings | Actualiza la propiedad Groups.Settings en Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lee Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Administra licencias de los usuarios en Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.aad.directory/User/Update | Actualiza las propiedades estándar del elemento Users en Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Actualiza la propiedad Users.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Actualiza la propiedad Users.Manager en Azure Active Directory. |

### <a name="exchange-service-administrator"></a>Administrador de servicios de Exchange
Puede administrar todos los aspectos del producto Exchange.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.exchange/AllEntities/AllActions | Administra todos los aspectos de Exchange Online. |

### <a name="guest-inviter"></a>Invitador de usuarios
Los usuarios invitados pueden recibir invitaciones independientemente del valor que determina si **los miembros pueden invitar**.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de invitado.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/User/InviteGuest | Invita usuarios en Azure Active Directory. |
| microsoft.aad.directory/User/Read | Lee las propiedades estándar de Users en Azure Active Directory. |
| microsoft.aad.directory/User/Read/AppRoleAssignments | Lee la propiedad Users.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/User/Read/DirectReports | Lee la propiedad Users.DirectReports en Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedBy | Lee la propiedad Users.InvitedBy en Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedUsers | Lee la propiedad Users.InvitedUsers en Azure Active Directory. |
| microsoft.aad.directory/User/Read/Manager | Lee la propiedad Users.Manager en Azure Active Directory. |
| microsoft.aad.directory/User/Read/MemberOf | Lee la propiedad Users.MemberOf en Azure Active Directory. |
| microsoft.aad.directory/User/Read/OAuth2PermissionGrants | Lee la propiedad Users.OAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedDevices | Lee la propiedad Users.OwnedDevices en Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedObjects | Lee la propiedad Users.OwnedObjects en Azure Active Directory. |
| microsoft.aad.directory/User/Read/RegisteredDevices | Lee la propiedad Users.RegisteredDevices en Azure Active Directory. |

### <a name="helpdesk-administrator"></a>Administrador del departamento de soporte técnico
Puede restablecer contraseñas de usuarios que no son administradores y de administradores del departamento de soporte técnico.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lee Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.aad.directory/User/Update/PasswordHelpdeskScope | Actualiza las contraseñas de los administradores limitados y de otros administradores del departamento de soporte técnico en Azure Active Directory. Para más información, consulte la documentación en línea. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |

### <a name="information-protection-administrator"></a>Administrador de Information Protection
Puede administrar todos los aspectos del producto Azure Information Protection.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/Group/Read | Lee las propiedades estándares del elemento Groups en Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lee Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.informationprotection/AllEntities/AllActions | Administra todos los aspectos de Information Protection. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="intune-service-administrator"></a>Administrador de servicios de Intune
Puede administrar todos los aspectos del producto Intune.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/Contact/Create | Crea el elemento Contacts en Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Elimina el elemento Contacts en Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Actualiza las propiedades estándar del elemento Contacts en Azure Active Directory. |
| microsoft.aad.directory/Device/Create | Crea el elemento Devices en Azure Active Directory. |
| microsoft.aad.directory/Device/Delete | Elimina el elemento Devices en Azure Active Directory. |
| microsoft.aad.directory/Device/Update | Actualiza las propiedades estándar del elemento Devices en Azure Active Directory. |
| microsoft.aad.directory/Device/Update/RegisteredOwners | Actualiza la propiedad Devices.RegisteredOwners en Azure Active Directory. |
| microsoft.aad.directory/Device/Update/RegisteredUsers | Actualiza la propiedad Devices.RegisteredUsers en Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Crea grupos en Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Crea grupos en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos creados del creador. |
| microsoft.aad.directory/Group/Delete | Elimina el elemento Groups en Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Lee las propiedades estándares del elemento Groups en Azure Active Directory. |
| microsoft.aad.directory/Group/Read/HiddenMembers | Lectura la propiedad Groups.HiddenMembers en Azure Active Directory. |
| microsoft.aad.directory/Group/Restore | Restaura el elemento Groups en Azure Active Directory. |
| microsoft.aad.directory/Group/Update | Actualiza las propiedades estándar de Groups en Azure Active Directory. |
| microsoft.aad.directory/Group/Update/AppRoleAssignments | Actualiza la propiedad Groups.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Actualiza la propiedad Groups.Members en Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Actualiza la propiedad Groups.Owners en Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Settings | Actualiza la propiedad Groups.Settings en Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lee Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.directory/User/Update | Actualiza las propiedades estándar del elemento Users en Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Actualiza la propiedad Users.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Actualiza la propiedad Users.Manager en Azure Active Directory. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.intune/AllEntities/AllActions | Administra todos los aspectos de Intune. |

### <a name="lync-service-administrator"></a>Administrador de servicios de Lync
Puede administrar todos los aspectos del producto Skype Empresarial.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.skypeforbusiness/AllEntities/AllActions | Administra Skype Empresarial Online. |

### <a name="message-center-reader"></a>Lector del Centro de mensajes
Puede leer los mensajes y las actualizaciones para su organización solo en el Centro de mensajes de Office 365. 

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de lectores de directorios.
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/Group/Read | Lee las propiedades estándares del elemento Groups en Azure Active Directory. |
| microsoft.aad.accessmessagecenter/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en el Centro de mensajes. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |

### <a name="partner-tier1-support"></a>Soporte para asociados de nivel 1
No lo use. No está pensado para el uso general.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |
| microsoft.aad.directory/Contact/Create | Crea el elemento Contacts en Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Elimina el elemento Contacts en Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Actualiza las propiedades estándar del elemento Contacts en Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Crea grupos en Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Crea grupos en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos creados del creador. |
| microsoft.aad.directory/Group/Read | Lee las propiedades estándares del elemento Groups en Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Actualiza la propiedad Groups.Members en Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Actualiza la propiedad Groups.Owners en Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lee Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Administra licencias de los usuarios en Azure Active Directory. |
| microsoft.aad.directory/User/Delete | Elimina usuarios en Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.aad.directory/User/Restore | Restaura usuarios eliminados en Azure Active Directory. |
| microsoft.aad.directory/User/Update | Actualiza las propiedades estándar del elemento Users en Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Actualiza la propiedad Users.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Actualiza la propiedad Users.Manager en Azure Active Directory. |
| microsoft.aad.directory/User/Update/PasswordUserScope | Actualiza las contraseñas de usuarios no administradores en Azure Active Directory. Para más información, consulte la documentación en línea. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="partner-tier2-support"></a>Soporte para asociados de nivel 2
No lo use. No está pensado para el uso general.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |
| microsoft.aad.directory/Contact/Create | Crea el elemento Contacts en Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Elimina el elemento Contacts en Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Actualiza las propiedades estándar del elemento Contacts en Azure Active Directory. |
| microsoft.aad.directory/Domain/AllActions | Crea y elimina dominios, y lee y actualiza las propiedades estándar en Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Crea grupos en Azure Active Directory. |
| microsoft.aad.directory/Group/Delete | Elimina el elemento Groups en Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Lee las propiedades estándares del elemento Groups en Azure Active Directory. |
| microsoft.aad.directory/Group/Restore | Restaura el elemento Groups en Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Actualiza la propiedad Groups.Members en Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lee Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.directory/Organization/Update | Actualiza las propiedades estándar del Organizations en Azure Active Directory. |
| microsoft.aad.directory/Organization/Update/TrustedCAsForPasswordlessAuth | Actualiza Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Administra licencias de los usuarios en Azure Active Directory. |
| microsoft.aad.directory/User/Delete | Elimina usuarios en Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.aad.directory/User/Restore | Restaura usuarios eliminados en Azure Active Directory. |
| microsoft.aad.directory/User/Update | Actualiza las propiedades estándar del elemento Users en Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Actualiza la propiedad Users.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Actualiza la propiedad Users.Manager en Azure Active Directory. |
| microsoft.aad.directory/User/Update/Password | Actualiza las contraseñas de todos los usuarios en Azure Active Directory. Para más información, consulte la documentación en línea. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="power-bi-service-administrator"></a>Administrador de servicios de Power BI
Puede administrar todos los aspectos del producto Power BI.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lee Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.powerbi/AllEntities/AllActions | Administra todos los aspectos de Power BI. |

### <a name="privileged-role-administrator"></a>Administrador de roles con privilegios
Puede administrar las asignaciones de roles en Azure AD

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/DirectoryRole/Update | Actualiza las propiedades estándar de DirectoryRoles en Azure Active Directory. |
| microsoft.aad.privilegedrolemanagement/AllEntities/AllActions | Administra todos los aspectos del servicio Privileged Role Management. |

### <a name="security-administrator"></a>Administrador de seguridad
Puede leer la información y los informes de seguridad

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/Application/Update/DefaultPolicy | Actualiza la propiedad Applications.DefaultPolicy en Azure Active Directory. |
| microsoft.aad.directory/Policy/Create | Crea directivas en Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Elimina directivas en Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Actualiza las propiedades estándar de las directivas en Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Actualiza la propiedad Policies.Owners en Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Actualiza ServicePrincipals.DefaultPolicy en Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lee Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.aad.privilegedrolemanagement/AllEntities/Read | Lee todos los aspectos de Privileged Identity Management. |
| microsoft.protectioncenter/AllEntities/Read | Lee todos los aspectos del Centro de protección de Office 365. |
| microsoft.protectioncenter/AllEntities/Update | Administra el Centro de protección de Office 365. |

### <a name="reports-reader"></a>Lector de informes
Puede leer los informes de inicio de sesión y auditoría.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de lectores de directorios.
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.reports/AllEntities/Read | Lee informes de Azure AD. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.usagereports/AllEntities/Read | Lee los informes de uso de Office 365. |

### <a name="security-reader"></a>Lector de seguridad
Puede leer la información y los informes de seguridad de Azure AD y Office 365.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de lectores de directorios.
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lee Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.privilegedidentitymanagement/AllEntities/Read | Lee todos los aspectos de Privileged Identity Management. |
| microsoft.protectioncenter/AllEntities/Read | Lee todos los aspectos del Centro de protección de Office 365. |

### <a name="service-support-administrator"></a>Administrador del soporte técnico del servicio
Puede leer la información de estado del servicio y administrar las incidencias de soporte técnico.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lee Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |

### <a name="sharepoint-service-administrator"></a>Administrador de servicios de SharePoint
Puede administrar todos los aspectos del servicio SharePoint.

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.sharepoint/AllEntities/AllActions | Administra SharePoint Online. |

### <a name="user-account-administrator"></a>Administrador de cuenta de usuario
Puede administrar todos los aspectos de usuarios y grupos

  > [!NOTE]
  > Este rol hereda permisos adicionales del [rol de usuario](https://docs.microsoft.com/en-us/azure/active-directory/users-default-permissions).
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/AppRoleAssignment/Create | Crea el elemento AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Delete | Elimina AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Update | Actualiza las propiedades estándar de AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/Contact/Create | Crea el elemento Contacts en Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Elimina el elemento Contacts en Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Actualiza las propiedades estándar del elemento Contacts en Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Crea grupos en Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Crea grupos en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos creados del creador. |
| microsoft.aad.directory/Group/Delete | Elimina el elemento Groups en Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Lee las propiedades estándares del elemento Groups en Azure Active Directory. |
| microsoft.aad.directory/Group/Read/HiddenMembers | Lectura la propiedad Groups.HiddenMembers en Azure Active Directory. |
| microsoft.aad.directory/Group/Restore | Restaura el elemento Groups en Azure Active Directory. |
| microsoft.aad.directory/Group/Update | Actualiza las propiedades estándar de Groups en Azure Active Directory. |
| microsoft.aad.directory/Group/Update/AppRoleAssignments | Actualiza la propiedad Groups.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Actualiza la propiedad Groups.Members en Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Actualiza la propiedad Groups.Owners en Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Settings | Actualiza la propiedad Groups.Settings en Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Lee Organizations.TrustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Administra licencias de los usuarios en Azure Active Directory. |
| microsoft.aad.directory/User/Create | Crea usuarios en Azure Active Directory. |
| microsoft.aad.directory/User/Delete | Elimina usuarios en Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.aad.directory/User/Restore | Restaura usuarios eliminados en Azure Active Directory. |
| microsoft.aad.directory/User/Update | Actualiza las propiedades estándar del elemento Users en Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Actualiza la propiedad Users.AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Actualiza la propiedad Users.Manager en Azure Active Directory. |
| microsoft.aad.directory/User/Update/PasswordUserAcctAdminScope | Actualiza las contraseñas de los administradores limitados, de los administradores del departamento de soporte técnico y de otros administradores de cuentas de usuarios en Azure Active Directory. Para más información, consulte la documentación en línea. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en Azure Access Control. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Lee y configura el estado de mantenimiento del servicio Office 365. |

## <a name="next-steps"></a>Pasos siguientes

* Para más información acerca de cómo cambiar los administradores de una suscripción de Azure, consulte [Incorporación o cambio de roles de administrador de Azure](../billing-add-change-azure-subscription-administrator.md)
* Para más información acerca de cómo se controla el acceso a los recursos en Microsoft Azure, consulte [Descripción de acceso a los recursos de Azure](../role-based-access-control/rbac-and-directory-admin-roles.md)
* Para más información acerca de cómo se relaciona Azure Active Directory con la suscripción de Azure, consulte [Asociación de las suscripciones de Azure con Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)