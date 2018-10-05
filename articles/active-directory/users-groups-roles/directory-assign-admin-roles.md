---
title: Permisos de roles de administrador en Azure Active Directory | Microsoft Docs
description: Un rol de administrador puede agregar usuarios, asignar roles administrativos, restablecer contraseñas de usuario, administrar licencias de usuario o administrar dominios.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 09/25/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 293d8376d83d729588aab0aeaa1040d9b3e5e0b5
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182287"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Permisos de roles de administrador en Azure Active Directory

Con Azure Active Directory (Azure AD), puede designar administradores independientes que desempeñen distintas funciones. Los administradores se pueden designar en el portal de Azure AD para realizar tareas como agregar usuarios o cambiarlos, asignar roles administrativos, restablecer contraseñas de usuario, administrar licencias de usuario y administración de nombres de dominio.

El administrador global tiene acceso a todas las características administrativas. De forma predeterminada, a la persona que se suscribe a una suscripción de Azure se le asigna el rol de administrador global para el directorio. Los administradores globales son los únicos que pueden delegar roles de administrador.

## <a name="assign-or-remove-administrator-roles"></a>Asignación o eliminación de roles de administrador

Para obtener información sobre cómo asignar roles administrativos a un usuario en Azure Active Directory, consulte [Ver y asignar roles de administrador en Azure Active Directory](directory-manage-roles-portal.md).

## <a name="available-roles"></a>Roles disponibles

Los roles de administrador disponibles son los siguientes:

* **[Administrador de aplicaciones](#application-administrator)**: los usuarios con este rol pueden crear y administrar todos los aspectos de las aplicaciones empresariales, los registros de aplicaciones y la configuración del proxy de aplicación. Este rol proporciona igualmente la capacidad de dar el consentimiento para permisos delegados y permisos de aplicaciones, excepto Microsoft Graph y Azure AD Graph. Los miembros de este rol no se agregan como propietarios al crear nuevos registros de aplicaciones o aplicaciones empresariales.

* **[Desarrollador de aplicaciones](#application-developer)**: los usuarios con este rol pueden crear registros de aplicaciones cuando la opción "Los usuarios pueden registrar aplicaciones" está establecida en No. Esta función también permite a los miembros dar su consentimiento en su propio nombre cuando la opción "Los usuarios pueden permitir que las aplicaciones accedan a los datos de la compañía en su nombre" está establecida en No. Los miembros de este rol se agregan como propietarios al crear nuevos registros de aplicaciones o aplicaciones empresariales.

* **[Administrador de facturación](#billing-administrator)**: hace compras, administra suscripciones, administra incidencias de soporte técnico y supervisa el estado del servicio.

* **[Administrador de aplicaciones en la nube](#cloud-application-administrator)**: los usuarios con este rol tienen los mismos permisos que el rol de administrador de la aplicación, excluida la capacidad de administrar el proxy de aplicación. Este rol concede la capacidad de crear y administrar todos los aspectos de las aplicaciones empresariales y los registros de aplicaciones. Este rol proporciona igualmente la capacidad de dar el consentimiento para permisos delegados y permisos de aplicaciones, excepto Microsoft Graph y Azure AD Graph. Los miembros de este rol no se agregan como propietarios al crear nuevos registros de aplicaciones o aplicaciones empresariales.

* **[Administrador de dispositivos en la nube](#cloud-device-administrator)**: los usuarios de este rol pueden habilitar, deshabilitar y eliminar dispositivos en Azure AD y leer las claves de BitLocker de Windows 10 (si las hay) en Azure Portal. El rol no concede permisos para administrar otras propiedades en el dispositivo.

* **[Administrador de cumplimiento](#compliance-administrator)**: los usuarios con este rol tienen permisos de administración en el Centro de seguridad y cumplimiento de Office 365 y el Centro de administración de Exchange. Más información en [Acerca de los roles de administrador de Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Administrador de acceso condicional](#conditional-access-administrator)**: los usuarios con este rol tienen la capacidad de administrar la configuración de acceso condicional de Azure Active Directory.
  > [!NOTE]
  > Para implementar la directiva de acceso condicional de Exchange ActiveSync en Azure, el usuario también debe ser administrador global.
  
* **[Administradores de dispositivo](#device-administrators)**: este rol está disponible solo para la asignación como un administrador local adicional en la [Configuración del dispositivo](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Los usuarios con este rol pasarán a ser administradores del equipo local en todos los dispositivos Windows 10 que estén unidos a Azure Active Directory. No tienen la capacidad de administrar objetos de dispositivos en Azure Active Directory. 

* **[Lectores de directorio](#directory-readers)**: se trata de un rol heredado que se va a asignar a las aplicaciones que no admitan el [marco de consentimiento](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). No se debe asignar a ningún usuario.

* **[Cuentas de sincronización de directorios](#directory-synchronization-accounts)**: no las use. Este rol se asigna automáticamente al servicio de Azure AD Connect y no está previsto ni se admite para ningún otro uso.

* **[Lectores de directorio](#directory-writers)**: es un rol heredado que se asigna a las aplicaciones que no admiten el [marco de consentimiento](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). No se debe asignar a ningún usuario.

* **[Administrador de servicios de Dynamics 365 y Administrador de servicios de CRM](#dynamics-365-service-administrator)**: los usuarios con este rol tienen permisos globales en Microsoft Dynamics 365 Online cuando existe el servicio, así como también la posibilidad de administrar incidencias de soporte técnico y supervisar el estado del servicio. Para obtener más información, consulte [Uso del rol de administrador de servicios para administrar inquilinos](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).

* **[Administrador de servicios de Exchange](#exchange-service-administrator)**: los usuarios con este rol tienen permisos globales en Microsoft Exchange Online, cuando existe el servicio. Más información en [Acerca de los roles de administrador de Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Administrador global/administrador de la compañía](#company-administrator)**: los usuarios con este rol tienen acceso a todas las características administrativas en Azure Active Directory, así como a los servicios que usan identidades de Azure Active Directory, como Exchange Online, SharePoint Online y Skype Empresarial Online. La persona que se suscribe al inquilino de Azure Active Directory se convierte en un administrador global. Los administradores globales son los únicos que pueden asignar otros roles de administrador. Puede haber más de un administrador global en su empresa. Los administradores globales pueden restablecer la contraseña de todos los usuarios y de todos los demás administradores.

  > [!NOTE]
  > En Microsoft Graph API, Graph API de Azure AD, y Azure AD PowerShell, este rol se identifica como "administrador de la compañía". Es "administrador Global" en [Azure Portal](https://portal.azure.com).
  >
  >

* **[Invitador de usuarios](#guest-inviter)**: los usuarios con este rol pueden administrar las invitaciones de usuarios invitados de Azure Active Directory B2B cuando la configuración de usuario **Members can invite** (Los miembros pueden invitar) está establecida en No. Más información sobre la colaboración B2B en [¿Qué es la colaboración B2B de Azure AD?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) No incluye otros permisos.

* **[Administrador de Information Protection](#information-protection-administrator)**: los usuarios con este rol tienen todos los permisos en el servicio Azure Information Protection. Este rol permite configurar las etiquetas de la directiva de Azure Information Protection, administrar plantillas de protección y activar la protección. Este rol no concede ningún permiso de Identity Protection Center, Privileged Identity Management, Supervisión del estado de mantenimiento del servicio Office 365 y Centro de seguridad y cumplimiento de Office 365.

* **[Administrador de servicios de Intune](#intune-service-administrator)**: los usuarios con este rol tienen permisos globales en Microsoft Intune Online, cuando el servicio está presente. Además, este rol contiene la capacidad de administrar usuarios y dispositivos para asociar una directiva, así como también para crear y administrar grupos. Para más información, consulte [Control de administración basado en rol (RBAC) con Microsoft Intune](https://docs.microsoft.com/intune/role-based-access-control)

* **[Administrador de licencias](#license-administrator)**: los usuarios con este rol pueden agregar, quitar y actualizar las asignaciones de licencias de usuarios y grupos (mediante licencias basadas en grupo) y administrar la ubicación de uso de los usuarios. El rol no otorga la posibilidad de adquirir o administrar suscripciones, crear o administrar grupos o crear o administrar usuarios más allá de la ubicación de uso.

* **[Lector del centro de mensajes](#message-center-reader)**: los usuarios con este rol pueden supervisar las notificaciones y las actualizaciones de mantenimiento de aviso en el [Centro de mensajes de Office 365](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) para su organización en los servicios configurados, como Exchange, Intune y de Microsoft Teams. Los lectores del centro de mensajes reciben semanal resúmenes de correo electrónico de publicaciones y actualizaciones y pueden compartir entradas del centro de mensajes en Office 365. En Azure AD, los usuarios asignados a este rol solo tendrán acceso de solo lectura en los servicios de Azure AD, como usuarios y grupos. 

* **[Soporte para asociados de nivel 1](#partner-tier1-support)**: no se usa. Este rol está en desuso y se quitará de Azure AD en el futuro. Este rol está diseñado para que lo usen un pequeño número de asociados de reventa de Microsoft, no para un uso general.

* **[Soporte para asociados de nivel 2](#partner-tier2-support)**: no se usa. Este rol está en desuso y se quitará de Azure AD en el futuro. Este rol está diseñado para que lo usen un pequeño número de asociados de reventa de Microsoft, no para un uso general.

* **[Administrador de contraseñas/Administrador del departamento de soporte técnico](#helpdesk-administrator)**: los usuarios con este rol pueden cambiar contraseñas, invalidar tokens de actualización, administrar solicitudes de servicio y supervisar el estado del servicio. Los administradores del departamento de soporte técnico pueden cambiar contraseñas e invalidar tokens de actualización solo para los usuarios y otros administradores del mismo departamento. Si invalida un token de actualización, obligará al usuario a iniciar sesión de nuevo.

  > [!NOTE]
  > En Microsoft Graph API, Azure AD Graph API y Azure AD PowerShell, este rol se identifica como "Administrador del departamento de soporte técnico". En "Administrador de contraseñas" en [Azure Portal](https://portal.azure.com/).
  >
  >
  
* **[Administrador de servicios de Power BI](#power-bi-service-administrator)**: los usuarios con este rol tienen permisos globales en Microsoft Power BI, cuando el servicio está presente, así como también la capacidad de administrar incidencias de soporte técnico y supervisar el estado del servicio. Puede obtener más información en el artículo [Descripción del rol de administrador de Power BI](https://docs.microsoft.com/power-bi/service-admin-role).

* **[Administrador de roles con privilegios](#privileged-role-administrator)**: los usuarios con este rol pueden administrar asignaciones de roles en Azure Active Directory, así como en Azure AD Privileged Identity Management. Además, este rol permite administrar todos los aspectos de Privileged Identity Management.

* **[Lector de informes](#reports-reader)**: los usuarios con este rol pueden ver datos de informes de uso y el panel de informes en el centro de administración de Office 365 y el paquete del contexto de adopción en Power BI. Además, el rol proporciona acceso a informes de inicio de sesión y a actividad de Azure AD y a los datos devueltos por la API de informes de Microsoft Graph. Un usuario asignado al rol de lector de informes puede acceder solo a métricas de uso y adopción pertinentes. No tienen permisos de administrador para configurar valores ni acceder a los centros de administración específicos de productos como Exchange. 

* **[Administrador de seguridad](#security-administrator)**: los usuarios con este rol tienen todos los permisos de solo lectura del rol Lector de seguridad más la capacidad de administrar la configuración de servicios relacionados con la seguridad: Azure Active Directory Identity Protection, Azure Information Protection y Centro de seguridad y cumplimiento de Office 365. Encuentre más información disponible sobre los permisos de Office 365 en [Permisos del Centro de seguridad y cumplimiento de Office 365](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
  | En | Puede hacer |
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

* **[Skype Empresarial/Administrador de servicios de Lync](#skype-for-business-administrator)**: los usuarios con este rol tienen permisos globales en Microsoft Skype Empresarial, cuando el servicio está presente, así como administrar los atributos de usuario específicos de Skype en Azure Active Directory. Además, este rol concede la posibilidad de administrar incidencias de soporte técnico, supervisar el estado del servicio y acceder al centro de administración de Teams y de Skype Empresarial. La cuenta también debe tener licencia para Teams o no podrá ejecutar los cmdlets de PowerShell de Teams. Para más información, visite [Acerca del rol de administrador de Skype Empresarial](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) y para ver la información de licencias de Teams, consulte [Licencias complementarias de Skype Empresarial y Microsoft Teams](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > En Microsoft Graph API, Azure AD Graph API y Azure AD PowerShell, este rol se identifica como "Administrador de servicios de Lync". Es "Administrador del servicio Skype Empresarial" en [Azure Portal](https://portal.azure.com/).
  >
  >

* **[Administrador de comunicaciones de Teams](#teams-communications-administrator)**: los usuarios con este rol pueden administrar aspectos de la carga de trabajo de Microsoft Teams relacionados con la voz y la telefonía. Esto incluye las herramientas de administración para la asignación de números de teléfono, directivas de voz y reunión, y acceso total al conjunto de herramientas de análisis de llamada.

* **[Ingeniero de soporte técnico de comunicaciones de Teams](#teams-communications-support-engineer)**: los usuarios con este rol pueden solucionar problemas de comunicación de Microsoft Teams y Skype Empresarial mediante las herramientas de solución de problemas de llamadas del usuario del centro de administración de Microsoft Teams y Skype Empresarial. Los usuarios con este rol pueden ver la información completa de registro de llamadas de todos los participantes implicados.

* **[Especialista de soporte técnico de comunicaciones de Teams](#teams-communications-support-specialist)**: los usuarios con este rol pueden solucionar problemas de comunicación de Microsoft Teams y Skype Empresarial mediante las herramientas de solución de problemas de llamadas del usuario del centro de administración de Microsoft Teams y Skype Empresarial. Los usuarios con este rol solo pueden ver los detalles del usuario en la llamada al usuario específico que han buscado.

* **[Administrador de servicios de Teams](#teams-service-administrator)**: los usuarios con este rol pueden administrar todos los aspectos de la carga de trabajo de Microsoft Teams a través del centro de administración de Microsoft Teams y Skype Empresarial y los módulos de PowerShell correspondientes. Esto incluye, entre otras áreas, todas las herramientas de administración relacionadas con telefonía, mensajería, reuniones y los propios equipos. Este rol también concede la posibilidad de administrar grupos de Office 365.

* **[Administrador de cuenta de usuario](#user-account-administrator)**: los usuarios con este rol pueden crear y administrar todos los aspectos de los usuarios y grupos. Además, este rol incluye la capacidad de administrar incidencias de soporte técnico y supervisar el estado del servicio. Se aplican algunas restricciones. Por ejemplo, este rol no permite eliminar un administrador global. Los administradores de cuentas de usuario solo pueden cambiar las contraseñas e invalidar los tokens de actualización de los usuarios, de los administradores del departamento de soporte técnico y de otros administradores de cuentas de usuario. Si invalida un token de actualización, obligará al usuario a iniciar sesión de nuevo.

| Puede hacer | No puede hacer |
| --- | --- |
| <p>Ver información de usuario y de la compañía</p><p>Administrar incidencias de soporte técnico de Office</p><p>Cambiar solo las contraseñas de los usuarios, de los administradores del departamento de soporte técnico y de otros administradores de cuentas de usuario</p><p>Crear y administrar vistas de usuario</p><p>Crear, editar y eliminar usuarios y grupos, y administrar licencias de usuarios, con limitaciones. No puede eliminar un administrador global ni crear otros administradores.</p> |<p>Realizar operaciones de facturación y compra productos de Office</p><p>Administrar dominios</p><p>Administrar información de la compañía</p><p>Delegar roles administrativos a otros usuarios</p><p>Usar la sincronización de directorios</p><p>Habilitar o deshabilitar Multi-Factor Authentication</p><p>Visualización de registros de auditoría</p> |

En las tablas siguientes se describen los permisos específicos concedidos a cada rol en Azure Active Directory. Algunos roles pueden tener permisos adicionales en los servicios de Microsoft fuera de Azure Active Directory.

## <a name="adhoc-license-administrator"></a>Administrador de licencias ad hoc
Puede crear y administrar todos los aspectos de los registros de aplicaciones y de las aplicaciones empresariales.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de lectores de directorios.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/domains/default/read | Lee las propiedades básicas de los dominios de Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Lee la propiedad groups.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/groups/default/read | Lee las propiedades básicas de los grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/memberOf/read | Lee la propiedad groups.memberOf en Azure Active Directory. |
| microsoft.aad.directory/groups/members/read | Lee la propiedad groups.members en Azure Active Directory. |
| microsoft.aad.directory/groups/owners/read | Lee la propiedad groups.owners en Azure Active Directory. |
| microsoft.aad.directory/groups/settings/read | Lee la propiedad groups.settings en Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/default/read | Lee las propiedades básicas de oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/update | Actualiza oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/organization/default/read | Lee las propiedades básicas de la organización en Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Lee organizations.trustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Administrar licencias de usuarios de Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/read | Lee la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/users/default/read | Lee las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Lee la propiedad users.directReports en Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Lee la propiedad users.invitedBy en Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Lee la propiedad users.invitedUsers en Azure Active Directory. |
| microsoft.aad.directory/users/manager/read | Lee la propiedad users.manager en Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Lee la propiedad users.memberOf en Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Lee la propiedad users.oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Lee la propiedad users.ownedDevices en Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Lee la propiedad users.ownedObjects en Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Lee la propiedad users.registeredDevices en Azure Active Directory. |

## <a name="application-administrator"></a>Administrador de aplicaciones
Puede crear y administrar todos los aspectos de los registros de aplicaciones y de las aplicaciones empresariales.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de lectores de directorios.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | Actualiza la propiedad applications.audience en Azure Active Directory. |
| microsoft.aad.directory/applications/authentication/update | Actualiza la propiedad applications.authentication en Azure Active Directory. |
| microsoft.aad.directory/applications/default/update | Actualiza las propiedades básicas de las aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/applications/create | Crea aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/applications/credentials/update | Actualiza la propiedad applications.credentials en Azure Active Directory. |
| microsoft.aad.directory/applications/delete | Elimina aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/applications/owners/update | Actualiza la propiedad applications.owners en Azure Active Directory. |
| microsoft.aad.directory/applications/permissions/update | Actualiza la propiedad applications.permissions en Azure Active Directory. |
| microsoft.aad.directory/applications/policies/update | Actualiza la propiedad applications.policies en Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/create | Crea el elemento AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/read | Lee el elemento AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Actualiza el elemento appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Elimina elementos appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/default/read | Lee la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/default/update | Actualiza la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Crea directivas en Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Elimina directivas en Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Lee la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Actualiza la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Lee la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/update | Actualiza las propiedades básicas de servicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Crea elementos sevicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Elimina servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Actualiza la propiedad servicePrincipals.appRoleAssignedTo en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Actualiza la propiedad servicePrincipals.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Actualiza la propiedad servicePrincipals.owners en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Actualiza la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Administrar licencias de usuarios de Azure Active Directory. |
| microsoft.aad.reports/allEntities/read | Lee informes de Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="application-developer"></a>Desarrollador de aplicaciones
Puede crear registros de aplicación independientemente de la opción de configuración �Los usuarios pueden registrar aplicaciones�.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de lectores de directorios.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Crea aplicaciones en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | Crea el elemento AppRoleAssignments en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | Crea el elemento oAuth2PermissionGrants en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Crea elementos sevicePrincipals en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |

## <a name="billing-administrator"></a>Administrador de facturación
Puede realizar tareas comunes relacionadas con la facturación como actualizar la información de pago.

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
| microsoft.aad.directory/organization/default/update | Actualiza las propiedades básicas de la organización en Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Actualiza organizations.trustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.commerce.billing/allEntities/allTasks | Administra todos los aspectos de la facturación de Office 365. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="cloud-application-administrator"></a>Administrador de aplicaciones en la nube
Puede crear y administrar todos los aspectos de los registros de aplicaciones y de las aplicaciones empresariales, excepto el proxy de aplicación.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de lectores de directorios.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | Actualiza la propiedad applications.audience en Azure Active Directory. |
| microsoft.aad.directory/applications/authentication/update | Actualiza la propiedad applications.authentication en Azure Active Directory. |
| microsoft.aad.directory/applications/default/update | Actualiza las propiedades básicas de las aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/applications/create | Crea aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/applications/credentials/update | Actualiza la propiedad applications.credentials en Azure Active Directory. |
| microsoft.aad.directory/applications/delete | Elimina aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/applications/owners/update | Actualiza la propiedad applications.owners en Azure Active Directory. |
| microsoft.aad.directory/applications/permissions/update | Actualiza la propiedad applications.permissions en Azure Active Directory. |
| microsoft.aad.directory/applications/policies/update | Actualiza la propiedad applications.policies en Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/create | Crea el elemento AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Actualiza el elemento appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Elimina elementos appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Crea directivas en Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/default/read | Lee la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/default/update | Actualiza la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Elimina directivas en Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Lee la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Actualiza la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Lee la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Actualiza la propiedad servicePrincipals.appRoleAssignedTo en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Actualiza la propiedad servicePrincipals.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/update | Actualiza las propiedades básicas de servicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Crea elementos sevicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Elimina servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Actualiza la propiedad servicePrincipals.owners en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Actualiza la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Administrar licencias de usuarios de Azure Active Directory. |
| microsoft.aad.reports/allEntities/read | Lee informes de Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="cloud-device-administrator"></a>Administrador de dispositivos en la nube
Acceso total para administrar los dispositivos de Azure AD.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de lectores de directorios.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/devices/delete | Elimina dispositivos en Azure Active Directory. |
| microsoft.aad.directory/devices/disable | Deshabilita dispositivos en Azure Active Directory. |
| microsoft.aad.directory/devices/enable | Habilita dispositivos en Azure Active Directory. |
| microsoft.aad.reports/allEntities/read | Lee informes de Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |

## <a name="company-administrator"></a>Administrador de la compañía
Puede administrar todos los aspectos de los servicios de Azure AD y Microsoft que usan identidades de Azure AD.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol.
  >
  >

  > [!NOTE]
  > Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información vea la descripción del rol arriba.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | Crea y elimina elementos administrativeUnits, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/applications/allProperties/allTasks | Crea y elimina aplicaciones, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | Crea y elimina elementos appRoleAssignments, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/contacts/allProperties/allTasks | Crea y elimina contactos, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/contracts/allProperties/allTasks | Crea y elimina contratos, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/devices/allProperties/allTasks | Crea y elimina dispositivos, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/allProperties/allTasks | Crea y elimina elementos directoryRoles, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/directoryRoleTemplates/allProperties/allTasks | Crea y elimina elementos directoryRoleTemplates, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/domains/allProperties/allTasks | Crea y elimina dominios, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/groups/allProperties/allTasks | Crea y elimina grupos, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/groupSettings/allProperties/allTasks | Crea y elimina groupSettings, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/allProperties/allTasks | Crea y elimina groupSettingTemplates, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/loginTenantBranding/allProperties/allTasks | Crea y elimina loginTenantBranding, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/allProperties/allTasks | Crea y elimina oAuth2PermissionGrants, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/organization/allProperties/allTasks | Crea y elimina el elemento organization, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/policies/allProperties/allTasks | Crea y elimina directivas, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | Crea y elimina elementos roleAssignments, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | Crea y elimina elementos RoleDefinitions, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/scopedRoleMemberships/allProperties/allTasks | Crea y elimina elementos scopedRoleMemberships, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/serviceAction/activateService | Puede realizar la acción de servicio Activateservice en Azure Active Directory. |
| microsoft.aad.directory/serviceAction/disableDirectoryFeature | Puede realizar la acción de servicio Disabledirectoryfeature en Azure Active Directory. |
| microsoft.aad.directory/serviceAction/enableDirectoryFeature | Puede realizar la acción de servicio Enabledirectoryfeature en Azure Active Directory. |
| microsoft.aad.directory/serviceAction/getAvailableExtentionProperties | Puede realizar la acción de servicio Getavailableextentionproperties en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/allProperties/allTasks | Crea y elimina servicePrincipals, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | Crea y elimina elementos subscribedSku, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directory/users/allProperties/allTasks | Crea y elimina usuarios, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.aad.directorySync/allEntities/allTasks | Realiza todas las acciones en Azure AD Connect. |
| microsoft.aad.identityProtection/allEntities/allTasks | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lee todos los recursos de microsoft.aad.privilegedIdentityManagement. |
| microsoft.aad.reports/allEntities/allTasks | Lee y configura de informes de Azure AD. |
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.informationProtection/allEntities/allTasks | Administra todos los aspectos de Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.commerce.billing/allEntities/allTasks | Administra todos los aspectos de la facturación de Office 365. |
| microsoft.intune/allEntities/allTasks | Administra todos los aspectos de Intune. |
| microsoft.office365.complianceManager/allEntities/allTasks | Administra todos los aspectos del Administrador de cumplimiento de Office 365 |
| microsoft.office365.exchange/allEntities/allTasks | Administra todos los aspectos de Exchange Online. |
| microsoft.office365.lockbox/allEntities/allTasks | Administra todos los aspectos de la Caja de seguridad del cliente de Office 365. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Administra todos los aspectos de Power BI. |
| microsoft.office365.protectionCenter/allEntities/allTasks | Administra todos los aspectos del Centro de protección de Office 365. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.sharepoint/allEntities/allTasks | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en microsoft.office365.sharepoint. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Administra todos los aspectos de Skype Empresarial Online. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Administra todos los aspectos de Dynamics 365. |

## <a name="compliance-administrator"></a>Administrador de cumplimiento
Puede leer y administrar los informes y la configuración de cumplimiento en Azure AD y Office 365.

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
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.complianceManager/allEntities/allTasks | Administra todos los aspectos del Administrador de cumplimiento de Office 365 |
| microsoft.office365.exchange/allEntities/allTasks | Administra todos los aspectos de Exchange Online. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.sharepoint/allEntities/allTasks | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en microsoft.office365.sharepoint. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Administra todos los aspectos de Skype Empresarial Online. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="conditional-access-administrator"></a>Administrador de acceso condicional
Puede administrar las funcionalidades de acceso condicional.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de lectores de directorios.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/default/read | Lee la propiedad policies.conditionalAccess en Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/default/update | Actualiza la propiedad policies.conditionalAccess en Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/create | Crea directivas en Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/delete | Elimina directivas en Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | Lee la propiedad policies.conditionalAccess en Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | Actualiza la propiedad policies.conditionalAccess en Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | Lee la propiedad policies.conditionalAccess en Azure Active Directory. |

## <a name="customer-lockbox-access-approver"></a>Aprobador del acceso a la Caja de seguridad del cliente
Puede aprobar solicitudes de soporte técnico de Microsoft para acceder a datos de la organización del cliente.

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
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.office365.lockbox/allEntities/allTasks | Administra todos los aspectos de la Caja de seguridad del cliente de Office 365. |

## <a name="device-administrators"></a>Administradores de dispositivos
Los miembros de este rol se agregan al grupo de administradores locales en dispositivos unidos a Azure AD.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de lectores de directorios.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/groupSettings/default/read | Lee las propiedades básicas de groupSettings en Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/default/read | Lee las propiedades básicas de groupSettingTemplates en Azure Active Directory. |

## <a name="device-managers"></a>Administradores de dispositivos
Puede aprobar solicitudes de soporte técnico de Microsoft para acceder a datos de la organización del cliente.

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
| microsoft.aad.directory/devices/default/read | Lee las propiedades básicas en los dispositivos de Azure Active Directory. |
| microsoft.aad.directory/devices/default/update | Actualiza las propiedades básicas en los dispositivos de Azure Active Directory. |
| microsoft.aad.directory/devices/memberOf/read | Lee la propiedad devices.memberOf en Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/read | Lee la propiedad devices.registeredOwners en Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/update | Actualiza la propiedad devices.registeredOwners en Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/read | Lee la propiedad devices.registeredUsers en Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/update | Actualiza la propiedad devices.registeredUsers en Azure Active Directory. |

## <a name="directory-readers"></a>Lectores de directorios
Puede leer la información básica del directorio. Para conceder acceso a aplicaciones

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/default/read | Lee las propiedades básicas de administrativeUnits en Azure Active Directory. |
| microsoft.aad.directory/administrativeUnits/members/read | Lee la propiedad administrativeUnits.members en Azure Active Directory. |
Azure Active Directory. |
| microsoft.aad.directory/applications/default/read | Lee las propiedades básicas de las aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/applications/owners/read | Lee la propiedad applications.owners en Azure Active Directory. |
| microsoft.aad.directory/contacts/default/read | Lee las propiedades básicas de los contactos en Azure Active Directory. |
| microsoft.aad.directory/contacts/memberOf/read | Lee la propiedad contacts.memberOf en Azure Active Directory. |
| microsoft.aad.directory/contracts/default/read | Lee las propiedades básicas en los contratos de Azure Active Directory. |
| microsoft.aad.directory/devices/default/read | Lee las propiedades básicas en los dispositivos en Azure Active Directory. |
| microsoft.aad.directory/devices/memberOf/read | Lee la propiedad devices.memberOf en Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/read | Lee la propiedad devices.registeredOwners en Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/read | Lee la propiedad devices.registeredUsers en Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/default/read | Lee las propiedades básicas de directoryRoles en Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/eligibleMembers/read | Lee la propiedad directoryRoles.eligibleMembers en Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/members/read | Lee la propiedad directoryRoles.members en Azure Active Directory. |
| microsoft.aad.directory/domains/default/read | Lee las propiedades básicas de los dominios de Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Lee la propiedad groups.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/groups/default/read | Lee las propiedades básicas de los grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/memberOf/read | Lee la propiedad groups.memberOf en Azure Active Directory. |
| microsoft.aad.directory/groups/members/read | Lee la propiedad groups.members en Azure Active Directory. |
| microsoft.aad.directory/groups/owners/read | Lee la propiedad groups.owners en Azure Active Directory. |
| microsoft.aad.directory/groups/settings/read | Lee la propiedad groups.settings en Azure Active Directory. |
| microsoft.aad.directory/groupSettings/default/read | Lee las propiedades básicas de groupSettings en Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/default/read | Lee las propiedades básicas de groupSettingTemplates en Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/default/read | Lee las propiedades básicas de oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/organization/default/read | Lee las propiedades básicas de la organización en Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Lee organizations.trustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lee la propiedad servicePrincipals.appRoleAssignedTo en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lee la propiedad servicePrincipals.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/read | Lee las propiedades básicas de servicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lee la propiedad servicePrincipals.memberOf en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/default/read | Lee la propiedad servicePrincipals.oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lee la propiedad servicePrincipals.ownedObjects en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Lee la propiedad servicePrincipals.owners en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Lee la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.aad.directory/subscribedSkus/default/read | Lee las propiedades básicas de subscribedSkus en Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/read | Lee la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/users/default/read | Lee las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Lee la propiedad users.directReports en Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Lee la propiedad users.invitedBy en Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Lee la propiedad users.invitedUsers en Azure Active Directory. |
| microsoft.aad.directory/users/manager/read | Lee la propiedad users.manager en Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Lee la propiedad users.memberOf en Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Lee la propiedad users.oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Lee la propiedad users.ownedDevices en Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Lee la propiedad users.ownedObjects en Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Lee la propiedad users.registeredDevices en Azure Active Directory. |

## <a name="directory-synchronization-accounts"></a>Cuentas de sincronización de directorios
Solo las usa el servicio de Azure AD Connect.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/organization/dirSync/update | Actualiza la propiedad organization.dirSync en Azure Active Directory. |
| microsoft.aad.directory/policies/create | Crea directivas en Azure Active Directory. |
| microsoft.aad.directory/policies/delete | Elimina directivas en Azure Active Directory. |
| microsoft.aad.directory/policies/default/read | Lee las propiedades básicas en las directivas de Azure Active Directory. |
| microsoft.aad.directory/policies/default/update | Actualiza las propiedades básicas en las directivas de Azure Active Directory. |
| microsoft.aad.directory/policies/owners/read | Lee la propiedad policies.owners en Azure Active Directory. |
| microsoft.aad.directory/policies/owners/update | Actualiza la propiedad policies.owners en Azure Active Directory. |
| microsoft.aad.directory/policies/policiesAppliedTo/read | Lee la propiedad policies.policiesAppliedTo en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lee la propiedad servicePrincipals.appRoleAssignedTo en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Actualiza la propiedad servicePrincipals.appRoleAssignedTo en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lee la propiedad servicePrincipals.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Actualiza la propiedad servicePrincipals.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/read | Lee las propiedades básicas de servicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/update | Actualiza las propiedades básicas de servicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Crea elementos sevicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lee la propiedad servicePrincipals.memberOf en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/default/read | Lee la propiedad servicePrincipals.oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Lee la propiedad servicePrincipals.owners en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Actualiza la propiedad servicePrincipals.owners en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lee la propiedad servicePrincipals.ownedObjects en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Lee la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Actualiza la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.aad.directorySync/allEntities/allTasks | Realiza todas las acciones en Azure AD Connect. |

## <a name="directory-writers"></a>Escritores de directorios
Puede leer y escribir información básica del directorio. Para conceder acceso a aplicaciones

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de lectores de directorios.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/groups/create | Crea grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Crea grupos en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Actualiza la propiedad groups.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/groups/default/update | Actualiza las propiedades básicas de los grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/members/update | Actualiza la propiedad groups.members en Azure Active Directory. |
| microsoft.aad.directory/groups/owners/update | Actualiza la propiedad groups.owners en Azure Active Directory. |
| microsoft.aad.directory/groups/settings/update | Actualiza la propiedad groups.settings en Azure Active Directory. |
| microsoft.aad.directory/groupSettings/default/update | Actualiza las propiedades básicas de groupSettings en Azure Active Directory. |
| microsoft.aad.directory/groupSettings/create | Crea elementos groupSettings en Azure Active Directory. |
| microsoft.aad.directory/groupSettings/delete | Elimina elementos groupSettings en Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Actualiza la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Administrar licencias de usuarios de Azure Active Directory. |
| microsoft.aad.directory/users/default/update | Actualiza las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Actualiza la propiedad users.manager en Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Actualiza la propiedad users.userPrincipalName en Azure Active Directory. |

## <a name="dynamics-365-administrator"></a>Administrador de Dynamics 365
Puede administrar todos los aspectos del producto Dynamics 365. Anteriormente, el administrador de servicios de CRM.

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
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Administra todos los aspectos de Dynamics 365. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="exchange-service-administrator"></a>Administrador de servicios de Exchange
Puede administrar todos los aspectos del producto Exchange.

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
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.exchange/allEntities/allTasks | Administra todos los aspectos de Exchange Online. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="guest"></a>Invitado
Rol predeterminado para los usuarios invitados. Puede leer un conjunto limitado de información del directorio.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de usuario.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/applications/default/read | Lee las propiedades básicas de las aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/applications/owners/read | Lee la propiedad applications.owners en Azure Active Directory. |
| microsoft.aad.directory/domains/default/read | Lee las propiedades básicas de los dominios de Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Lee la propiedad groups.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/groups/default/read | Lee las propiedades básicas de los grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/memberOf/read | Lee la propiedad groups.memberOf en Azure Active Directory. |
| microsoft.aad.directory/groups/members/read | Lee la propiedad groups.members en Azure Active Directory. |
| microsoft.aad.directory/groups/owners/read | Lee la propiedad groups.owners en Azure Active Directory. |
| microsoft.aad.directory/groups/settings/read | Lee la propiedad groups.settings en Azure Active Directory. |
| microsoft.aad.directory/organization/basicProfile/read | Lee la información básica del perfil de la organización en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lee la propiedad servicePrincipals.appRoleAssignedTo en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lee la propiedad servicePrincipals.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/read | Lee las propiedades básicas de servicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lee la propiedad servicePrincipals.memberOf en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/members/read | Lee la propiedad servicePrincipals.members en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/default/read | Lee la propiedad servicePrincipals.oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Lee la propiedad servicePrincipals.owners en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lee la propiedad servicePrincipals.ownedObjects en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Lee la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.aad.directory/users/basicProfile/read | Lee la propiedad users.basicProfile en Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/read | Lee la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/users/default/read | Lee las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Lee la propiedad users.directReports en Azure Active Directory. |
| microsoft.aad.directory/users/eligibleMemberOf/read | Lee la propiedad users.eligibleMemberOf en Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Lee la propiedad users.invitedBy en Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Lee la propiedad users.invitedUsers en Azure Active Directory. |
| microsoft.aad.directory/users/manager/read | Lee la propiedad users.manager en Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Lee la propiedad users.memberOf en Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Lee la propiedad users.oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Lee la propiedad users.ownedDevices en Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Lee la propiedad users.ownedObjects en Azure Active Directory. |
| microsoft.aad.directory/users/password/update | Actualiza las contraseñas de todos los usuarios en Azure Active Directory. Para obtener más información, consulte la documentación en línea. |
| microsoft.aad.directory/users/pendingMemberOf/read | Lee la propiedad users.pendingMemberOf en Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Lee la propiedad users.registeredDevices en Azure Active Directory. |
| microsoft.aad.directory/users/scopedAdministratorOf/read | Lee la propiedad users.scopedAdministratorOf en Azure Active Directory. |

## <a name="guest-inviter"></a>Invitador de usuarios
Puede invitar a usuarios independientemente de la configuración de �Members can invite guests� (Los miembros pueden invitar a usuarios).

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de usuario.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | Lee la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/users/default/read | Lee las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Lee la propiedad users.directReports en Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Lee la propiedad users.invitedBy en Azure Active Directory. |
| microsoft.aad.directory/users/inviteGuest | Invita usuarios en Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Lee la propiedad users.invitedUsers en Azure Active Directory. |
| microsoft.aad.directory/users/manager/read | Lee la propiedad users.manager en Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Lee la propiedad users.memberOf en Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Lee la propiedad users.oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Lee la propiedad users.ownedDevices en Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Lee la propiedad users.ownedObjects en Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Lee la propiedad users.registeredDevices en Azure Active Directory. |

## <a name="helpdesk-administrator"></a>Administrador del departamento de soporte técnico
Puede restablecer contraseñas de usuarios que no son administradores y de administradores del departamento de soporte técnico.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de lectores de directorios.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.aad.directory/users/password/update | Actualiza las contraseñas de todos los usuarios en Azure Active Directory. Para obtener más información, consulte la documentación en línea. |
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="information-protection-administrator"></a>Administrador de Information Protection
Puede administrar todos los aspectos del producto Azure Information Protection.

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
| microsoft.azure.informationProtection/allEntities/allTasks | Administra todos los aspectos de Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="intune-service-administrator"></a>Administrador de servicios de Intune
Puede administrar todos los aspectos del producto Intune.

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
| microsoft.aad.directory/contacts/default/update | Actualiza las propiedades básicas de los contactos en Azure Active Directory. |
| microsoft.aad.directory/contacts/create | Crea contactos en Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Elimina los contactos en Azure Active Directory. |
| microsoft.aad.directory/devices/default/update | Actualiza las propiedades básicas en los dispositivos de Azure Active Directory. |
| microsoft.aad.directory/devices/create | Crea dispositivos en Azure Active Directory. |
| microsoft.aad.directory/devices/delete | Elimina dispositivos en Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/update | Actualiza la propiedad devices.registeredOwners en Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/update | Actualiza la propiedad devices.registeredUsers en Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Actualiza la propiedad groups.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/groups/default/update | Actualiza las propiedades básicas de los grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/create | Crea grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Crea grupos en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.aad.directory/groups/delete | Elimina grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/hiddenMembers/read | Lee la propiedad groups.hiddenMembers en Azure Active Directory. |
| microsoft.aad.directory/groups/members/update | Actualiza la propiedad groups.members en Azure Active Directory. |
| microsoft.aad.directory/groups/owners/update | Actualiza la propiedad groups.owners en Azure Active Directory. |
| microsoft.aad.directory/groups/restore | Restaura grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/settings/update | Actualiza la propiedad groups.settings en Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Actualiza la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/users/default/update | Actualiza las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Actualiza la propiedad users.manager en Azure Active Directory. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.intune/allEntities/allTasks | Administra todos los aspectos de Intune. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="license-administrator"></a>Administrador de licencias
Puede administrar licencias de producto de usuarios y grupos.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de lectores de directorios.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/users/assignLicense | Administrar licencias de usuarios de Azure Active Directory. |
| microsoft.aad.directory/users/usageLocation/update | Actualizar la propiedad users.UsageLocation en Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |

## <a name="message-center-reader"></a>Lector del Centro de mensajes
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
| microsoft.aad.accessmessagecenter/allEntities/allTasks | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en el Centro de mensajes. |
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |

## <a name="partner-tier1-support"></a>Soporte para asociados de nivel 1
No lo use. No está pensado para el uso general.

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
| microsoft.aad.directory/contacts/default/update | Actualiza las propiedades básicas de los contactos en Azure Active Directory. |
| microsoft.aad.directory/contacts/create | Crea contactos en Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Elimina los contactos en Azure Active Directory. |
| microsoft.aad.directory/groups/create | Crea grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Crea grupos en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.aad.directory/groups/members/update | Actualiza la propiedad groups.members en Azure Active Directory. |
| microsoft.aad.directory/groups/owners/update | Actualiza la propiedad groups.owners en Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Actualiza la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Administrar licencias de usuarios de Azure Active Directory. |
| microsoft.aad.directory/users/default/update | Actualiza las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.aad.directory/users/delete | Elimina usuarios en Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Actualiza la propiedad users.manager en Azure Active Directory. |
| microsoft.aad.directory/users/password/update | Actualiza las contraseñas de todos los usuarios en Azure Active Directory. Para obtener más información, consulte la documentación en línea. |
| microsoft.aad.directory/users/restore | Restaura usuarios eliminados en Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Actualiza la propiedad users.userPrincipalName en Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="partner-tier2-support"></a>Soporte para asociados de nivel 2
No lo use. No está pensado para el uso general.

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
| microsoft.aad.directory/contacts/default/update | Actualiza las propiedades básicas de los contactos en Azure Active Directory. |
| microsoft.aad.directory/contacts/create | Crea contactos en Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Elimina los contactos en Azure Active Directory. |
| microsoft.aad.directory/domains/allTasks | Crea y elimina dominios, y lee y actualiza las propiedades estándar en Azure Active Directory. |
| microsoft.aad.directory/groups/create | Crea grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/delete | Elimina grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/members/update | Actualiza la propiedad groups.members en Azure Active Directory. |
| microsoft.aad.directory/groups/restore | Restaura grupos en Azure Active Directory. |
| microsoft.aad.directory/organization/default/update | Actualiza las propiedades básicas de la organización en Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Actualiza organizations.trustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Actualiza la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Administrar licencias de usuarios de Azure Active Directory. |
| microsoft.aad.directory/users/default/update | Actualiza las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.aad.directory/users/delete | Elimina usuarios en Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Actualiza la propiedad users.manager en Azure Active Directory. |
| microsoft.aad.directory/users/password/update | Actualiza las contraseñas de todos los usuarios en Azure Active Directory. Para obtener más información, consulte la documentación en línea. |
| microsoft.aad.directory/users/restore | Restaura usuarios eliminados en Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Actualiza la propiedad users.userPrincipalName en Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="power-bi-service-administrator"></a>Administrador de servicios de Power BI
Puede administrar todos los aspectos del producto Power BI.

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
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Administra todos los aspectos de Power BI. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="privileged-role-administrator"></a>Administrador de roles con privilegios
Puede administrar las asignaciones de roles en Azure AD

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
| microsoft.aad.directory/directoryRoles/update | Actualiza DirectoryRoles en Azure Active Directory. |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en microsoft.aad.privilegedIdentityManagement. |

## <a name="reports-reader"></a>Lector de informes
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
| microsoft.aad.reports/allEntities/read | Lee informes de Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.usageReports/allEntities/read | Lee los informes de uso de Office 365. |

## <a name="security-administrator"></a>Administrador de seguridad
Puede leer la información y los informes de seguridad

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
| microsoft.aad.directory/applications/policies/update | Actualiza la propiedad applications.policies en Azure Active Directory. |
| microsoft.aad.directory/policies/default/update | Actualiza las propiedades básicas en las directivas de Azure Active Directory. |
| microsoft.aad.directory/policies/create | Crea directivas en Azure Active Directory. |
| microsoft.aad.directory/policies/delete | Elimina directivas en Azure Active Directory. |
| microsoft.aad.directory/policies/owners/update | Actualiza la propiedad policies.owners en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Actualiza la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.aad.identityProtection/allEntities/read | Lee todos los recursos en microsoft.aad.identityProtection. |
| microsoft.aad.identityProtection/allEntities/update | Actualiza todos los recursos en microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lee todos los recursos de microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.office365.protectionCenter/allEntities/read | Lee todos los aspectos del Centro de protección de Office 365. |
| microsoft.office365.protectionCenter/allEntities/update | Actualiza todos los recursos en microsoft.office365.protectionCenter. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |

## <a name="security-reader"></a>Lector de seguridad
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
| microsoft.aad.identityProtection/allEntities/read | Lee todos los recursos en microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lee todos los recursos de microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.office365.protectionCenter/allEntities/read | Lee todos los aspectos del Centro de protección de Office 365. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |

## <a name="service-support-administrator"></a>Administrador del soporte técnico del servicio
Puede leer la información de estado del servicio y administrar las incidencias de soporte técnico.

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
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="sharepoint-service-administrator"></a>Administrador de servicios de SharePoint
Puede administrar todos los aspectos del servicio SharePoint.

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
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.sharepoint/allEntities/allTasks | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en microsoft.office365.sharepoint. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="skype-for-business-administrator"></a>Administrador de Skype Empresarial
Puede administrar todos los aspectos del producto Skype Empresarial. Anteriormente, a.dministrador de servicios de Lync.

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
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Administra todos los aspectos de Skype Empresarial Online. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="teams-communications-administrator"></a>Administrador de comunicaciones de Teams
Puede administrar las características de llamadas y reuniones del servicio Microsoft Teams.

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
| microsoft.aad.directory/policies/basic/read | Lee las propiedades básicas en las directivas de Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.office365.usageReports/allEntities/read | Lee los informes de uso de Office 365. |

## <a name="teams-communications-support-engineer"></a>Ingeniero de soporte técnico de comunicaciones de Teams
Puede solucionar los problemas de comunicaciones dentro de Teams mediante herramientas avanzadas.

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
| microsoft.aad.directory/policies/basic/read | Lee las propiedades básicas en las directivas de Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |

## <a name="teams-communications-support-specialist"></a>Especialista de soporte técnico de comunicaciones de Teams
Puede solucionar los problemas de comunicaciones dentro de Teams mediante herramientas básicas.

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
| microsoft.aad.directory/policies/basic/read | Lee las propiedades básicas en las directivas de Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |

## <a name="teams-service-administrator"></a>Administrador de servicios de Teams
Puede administrar el servicio Microsoft Teams.

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
| microsoft.aad.directory/groups/hiddenMembers/read | Lee la propiedad groups.hiddenMembers en Azure Active Directory. |
| microsoft.aad.directory/policies/basic/read | Lee las propiedades básicas en las directivas de Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.office365.usageReports/allEntities/read | Lee los informes de uso de Office 365. |

## <a name="user-account-administrator"></a>Administrador de cuenta de usuario
Puede administrar todos los aspectos de usuarios y grupos

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de lectores de directorios.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | Crea el elemento AppRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Elimina elementos appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Actualiza el elemento appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/contacts/default/update | Actualiza las propiedades básicas de los contactos en Azure Active Directory. |
| microsoft.aad.directory/contacts/create | Crea contactos en Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Elimina los contactos en Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Actualiza la propiedad groups.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/groups/default/update | Actualiza las propiedades básicas de los grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/create | Crea grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Crea grupos en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.aad.directory/groups/delete | Elimina grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/hiddenMembers/read | Lee la propiedad groups.hiddenMembers en Azure Active Directory. |
| microsoft.aad.directory/groups/members/update | Actualiza la propiedad groups.members en Azure Active Directory. |
| microsoft.aad.directory/groups/owners/update | Actualiza la propiedad groups.owners en Azure Active Directory. |
| microsoft.aad.directory/groups/restore | Restaura grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/settings/update | Actualiza la propiedad groups.settings en Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Actualiza la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Administrar licencias de usuarios de Azure Active Directory. |
| microsoft.aad.directory/users/default/update | Actualiza las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.aad.directory/users/create | Crea usuarios en Azure Active Directory. |
| microsoft.aad.directory/users/delete | Elimina usuarios en Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Actualiza la propiedad users.manager en Azure Active Directory. |
| microsoft.aad.directory/users/password/update | Actualiza las contraseñas de todos los usuarios en Azure Active Directory. Para obtener más información, consulte la documentación en línea. |
| microsoft.aad.directory/users/restore | Restaura usuarios eliminados en Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Actualiza la propiedad users.userPrincipalName en Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Administrar todos los aspectos del servicio de acceso de Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="user"></a>Usuario
Rol predeterminado de los usuarios miembros. Puede leer toda la información del directorio y escribir una parte limitada de esta.

  > [!NOTE]
  > Este rol hereda permisos adicionales del rol de lectores de directorios.
  >
  >

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Crea aplicaciones en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.aad.directory/groups/default/read | Lee las propiedades básicas de los grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Crea grupos en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.aad.directory/oAuth2PermissionGrants/create | Crea el elemento oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/delete | Elimina OAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/update | Actualiza oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Crea elementos sevicePrincipals en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.aad.directory/users/activateServicePlan | Activa usuarios de planes de servicios en Azure Active Directory. |
| microsoft.aad.directory/users/inviteGuest | Invita usuarios en Azure Active Directory. |
| microsoft.aad.directory/applications/default/update | Actualiza las propiedades básicas de las aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/applications/delete | Elimina aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/applications/owners/update | Actualiza la propiedad applications.owners en Azure Active Directory. |
| microsoft.aad.directory/applications/permissions/update | Actualiza la propiedad applications.permissions en Azure Active Directory. |
| microsoft.aad.directory/applications/policies/update | Actualiza la propiedad applications.policies en Azure Active Directory. |
| microsoft.aad.directory/applications/restore | Restaura aplicaciones en Azure Active Directory. |
| microsoft.aad.directory/devices/disable | Deshabilita dispositivos en Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Actualiza la propiedad groups.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/groups/default/update | Actualiza las propiedades básicas de los grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/delete | Elimina grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/dynamicMembershipRule/update | Actualiza la propiedad groups.dynamicMembershipRule en Azure Active Directory. |
| microsoft.aad.directory/groups/members/update | Actualiza la propiedad groups.members en Azure Active Directory. |
| microsoft.aad.directory/groups/owners/update | Actualiza la propiedad groups.owners en Azure Active Directory. |
| microsoft.aad.directory/groups/restore | Restaura grupos en Azure Active Directory. |
| microsoft.aad.directory/groups/settings/update | Actualiza la propiedad groups.settings en Azure Active Directory. |
| microsoft.aad.directory/policies/default/update | Actualiza las propiedades básicas en las directivas de Azure Active Directory. |
| microsoft.aad.directory/policies/delete | Elimina directivas en Azure Active Directory. |
| microsoft.aad.directory/policies/owners/update | Actualiza la propiedad policies.owners en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Actualiza la propiedad servicePrincipals.appRoleAssignedTo en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Actualiza la propiedad servicePrincipals.appRoleAssignments en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/update | Actualiza las propiedades básicas de servicePrincipals en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Elimina servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Actualiza la propiedad servicePrincipals.owners en Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Actualiza la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.aad.directory/users/changePassword | Cambia las contraseñas de todos los usuarios en Azure Active Directory. Para obtener más información, consulte la documentación en línea. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.aad.directory/users/basicProfile/update | Actualiza la propiedad users.basicProfile en Azure Active Directory. |
| microsoft.aad.directory/users/mobile/update | Actualiza la propiedad users.mobile en Azure Active Directory. |
| microsoft.aad.directory/users/searchableDeviceKey/update | Actualiza la propiedad users.searchableDeviceKey en Azure Active Directory. |

## <a name="deprecated-roles"></a>Roles en desuso

Los siguientes roles no deben usarse. Están en desuso y se eliminarán de Azure AD más adelante.

* Administrador de licencias ad hoc
* Combinación de dispositivos
* Administradores de dispositivos
* Usuarios de dispositivos
* Creador de usuarios comprobados de correo electrónico
* Administrador de buzones de correo
* Combinación de dispositivos de área de trabajo

## <a name="next-steps"></a>Pasos siguientes

* Para más información acerca de cómo asignar un usuario como administrador de una suscripción de Azure, consulte [Administración del acceso mediante RBAC y Azure Portal](../../role-based-access-control/role-assignments-portal.md)
* Para más información acerca de cómo se controla el acceso a los recursos en Microsoft Azure, consulte [Descripción de acceso a los recursos de Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Para más información acerca de cómo se relaciona Azure Active Directory con la suscripción de Azure, consulte [Asociación de las suscripciones de Azure con Azure Active Directory](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
