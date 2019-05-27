---
title: 'Permisos de usuario predeterminados: Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre los distintos permisos de usuario disponibles en Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 02/16/2019
ms.author: lizross
ms.reviewer: vincesm
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0cb0fe056ff7ff4794667d6b28782daad100609f
ms.sourcegitcommit: d73c46af1465c7fd879b5a97ddc45c38ec3f5c0d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65921037"
---
# <a name="what-are-the-default-user-permissions-in-azure-active-directory"></a>¿Cuales son los permisos de usuario predeterminados en Azure Active Directory?
En Azure Active Directory (Azure AD), a todos los usuarios se les otorga un conjunto de permisos predeterminados. El acceso de un usuario consta del tipo de usuario, sus [asignaciones de roles](active-directory-users-assign-role-azure-portal.md) y su propiedad de objetos individuales. En este artículo se describen dichos permisos predeterminados y contiene una comparación de los valores predeterminados de los usuarios miembros e invitados. Los permisos de usuario predeterminados solo se pueden cambiar en la configuración de usuario de Azure AD.

## <a name="member-and-guest-users"></a>Usuarios miembros e invitados
El conjunto de permisos predeterminados recibido depende de si el usuario es miembro nativo del inquilino (usuario miembro) o si el usuario se incorpora desde otro directorio como un invitado de la colaboración B2B (usuario invitado). Vea [¿Qué es el acceso de usuarios invitados de B2B?](../b2b/what-is-b2b.md) para más información sobre la adición de usuarios invitados.
* Los usuarios miembro pueden registrar aplicaciones, administrar el número de teléfono móvil y la fotografía de su propio perfil, cambiar su contraseña e invitar a los invitados de B2B. Además, los usuarios pueden leer toda la información del directorio (con algunas excepciones). 
* Los usuarios invitados tienen permisos de directorio restringidos. Por ejemplo, los usuarios invitados no pueden buscar la información del inquilino más allá de su propia información de perfil. Sin embargo, un usuario invitado puede recuperar información acerca de otro usuario si proporciona el nombre principal de usuario u objectId. Un usuario invitado puede leer las propiedades de los grupos a los que pertenece, incluida la pertenencia a grupos, con independencia del valor **Guest users permissions are limited** (Los permisos de los usuarios invitados están limitados). Un invitado no puede ver información sobre cualquier otro objeto de inquilino.

De forma predeterminada, los permisos predeterminados de los invitados son restrictivos. Los invitados se pueden agregar a los roles de administrador, que les conceden permisos completos de lectura y escritura incluidos en el rol. Hay una restricción adicional disponible, la capacidad que tienen los invitados para invitar a otros. Si **Los invitados pueden invitar** se establece en **No** se evita que los invitados puedan invitar a otros. Consulte [Delegación de invitaciones de la colaboración B2B de Azure Active Directory](../b2b/delegate-invitations.md) para aprender a hacerlo. Para conceder a los usuarios invitados los mismos permisos que a los usuarios miembros de forma predeterminada, establezca **Los permisos de los usuarios invitados están limitados** en **No**. Esta configuración concede a todos los miembros permisos de usuario para usuarios invitados de forma predeterminada y permite que se agreguen invitados a los roles administrativos.

## <a name="compare-member-and-guest-default-permissions"></a>Comparación de los permisos predeterminados de miembros e invitados

**Ámbito** | **Permisos de usuarios miembros** | **Permisos de usuarios invitados**
------------ | --------- | ----------
Usuarios y contactos | Leer todas las propiedades públicas de usuarios y contactos<br>Invitar a los invitados<br>Cambiar la contraseña propia<br>Administrar el número de teléfono móvil propio<br>Administrar la fotografía propia<br>Invalidar tokens de actualización propios | Leer las propiedades propias<br>Leer el nombre para mostrar, el correo electrónico, el nombre de inicio de sesión, la fotografía, el nombre principal de usuario y las propiedades de tipo de usuario de otros usuarios y contactos<br>Cambiar la contraseña propia
Grupos | Crear grupos de seguridad<br>Crear grupos de Office 365<br>Leer todas las propiedades de los grupos<br>Leer las pertenencias a grupos no ocultos<br>Leer las pertenencias a grupos de Office 365 ocultos para los grupos a los que se ha unido<br>Administrar las propiedades, la propiedad y la pertenencia de los grupos propiedad del usuario<br>Agregar invitados a los grupos que se poseen<br>Administrar la configuración de pertenencia dinámica<br>Eliminar los grupos que se poseen<br>Restaurar los grupos de Office 365 que se poseen | Leer todas las propiedades de los grupos<br>Leer las pertenencias a grupos no ocultos<br>Leer las pertenencias a grupos de Office 365 ocultos para los grupos a los que se ha unido<br>Administrar los grupos que se poseen<br>Agregar invitados a los grupos que se poseen (si se permite)<br>Eliminar los grupos que se poseen<br>Restaurar los grupos de Office 365 que se poseen<br>Leer las propiedades de los grupos a los que pertenecen, incluida la pertenencia.
APLICACIONES | Registrar aplicaciones nuevas<br>Leer las propiedades de las aplicaciones registradas y empresariales<br>Administrar las propiedades, asignaciones y credenciales de las aplicaciones que se poseen<br>Crear o eliminar contraseña de la aplicación para el usuario<br>Eliminar las aplicaciones que se poseen<br>Restaurar las aplicaciones que se poseen | Leer las propiedades de las aplicaciones registradas y empresariales<br>Administrar las propiedades, asignaciones y credenciales de las aplicaciones que se poseen<br>Eliminar las aplicaciones que se poseen<br>Restaurar las aplicaciones que se poseen
Dispositivos | Leer todas las propiedades de los dispositivos<br>Administrar todas las propiedades de los dispositivos que se poseen<br> | Sin permisos<br>Eliminar los dispositivos que se poseen<br>
Directorio | Leer toda la información de la compañía<br>Leer todos los dominios<br>Leer todos los contratos de los asociados | Leer el nombre para mostrar y los dominios comprobados
Roles y ámbitos | Leer todos los roles y las pertenencias administrativas<br>Leer todas las propiedades y la pertenencia de las unidades administrativas | Sin permisos 
Suscripciones | Leer todas las suscripciones<br>Habilitar a miembro del plan de servicio | Sin permisos
Directivas | Leer todas las propiedades de las directivas<br>Administrar todas las propiedades de las directivas que se poseen | Sin permisos

## <a name="to-restrict-the-default-permissions-for-member-users"></a>Restricción de los permisos predeterminados de los usuarios miembros

Los permisos predeterminados de los usuarios miembros se pueden restringir de las siguientes maneras.

Permiso | Explicación del valor
---------- | ------------
Los usuarios pueden registrar la aplicación | Si esta opción No impide que los usuarios crear registros de aplicación. La capacidad, a continuación, se puede conceder a usuarios específicos agregándolos a la función de programador de aplicaciones.
Permitir a los usuarios conectar su cuenta profesional o educativa con LinkedIn | Si esta opción No impide que los usuarios conectarse a su cuenta profesional o educativa con su cuenta de LinkedIn.  Consulte [consentimiento y uso compartido de datos de las conexiones de cuentas de LinkedIn](https://docs.microsoft.com/azure/active-directory/users-groups-roles/linkedin-user-consent) para obtener más información.
Capacidad para crear grupos de seguridad | Si se selecciona No en esta opción, se impide que los usuarios creen grupos de seguridad. Los administradores globales y usuario todavía puede crear grupos de seguridad. Para aprender a hacerlo, consulte [Cmdlets de Azure Active Directory para configurar las opciones de grupo](../users-groups-roles/groups-settings-cmdlets.md).
Capacidad para crear grupos de Office 365 | Si se selecciona No en esta opción, se impide que los usuarios creen grupos de Office 365. Si se selecciona algunos en esta opción, se permite que un conjunto de usuarios creen grupos de Office 365. Los administradores globales y usuario aún podrá crear grupos de Office 365. Para aprender a hacerlo, consulte [Cmdlets de Azure Active Directory para configurar las opciones de grupo](../users-groups-roles/groups-settings-cmdlets.md).
Restringir el acceso al portal de administración de Azure AD | Al establecer esta opción en Sí, impide que los usuarios acceso a Azure Active Directory a través de Azure portal solo.
Capacidad para leer otros usuarios | Esta configuración solo está disponible en PowerShell. Si establece esta configuración en $false, se impide que quienes no son administradores lean la información de los usuarios desde el directorio. Esto no impide que puedan leer la información de los usuarios en otros servicios de Microsoft, como Exchange Online. Esta configuración está pensada para circunstancias especiales y no se recomienda establecerla en $false.

## <a name="object-ownership"></a>Propiedad del objeto

### <a name="application-registration-owner-permissions"></a>Permisos de propietario del registro de una aplicación
Cuando un usuario registra una aplicación, se agrega automáticamente como propietario de la misma. Como propietario, puede administrar los metadatos de la aplicación, como el nombre y los permisos que solicita la aplicación. También pueden administrar la configuración específica del inquilino de la aplicación, como la configuración de SSO y las asignaciones de usuarios. Un propietario también puede agregar o quitar otros propietarios. A diferencia de los administradores globales, los propietarios solo pueden administrar las aplicaciones que poseen.

<!-- ### Enterprise application owner permissions

When a user adds a new enterprise application, they are automatically added as an owner for the tenant-specific configuration of the application. As an owner, they can manage the tenant-specific configuration of the application, such as the SSO configuration, provisioning, and user assignments. An owner can also add or remove other owners. Unlike Global Administrators, owners can manage only the applications they own. <!--To assign an enterprise application owner, see *Assigning Owners for an Application*.-->

### <a name="group-owner-permissions"></a>Permisos de propietario de grupo

Cuando un usuario crea un grupo, se agrega automáticamente como propietario de dicho grupo. Como propietario, puede administrar las propiedades del grupo, como el nombre, así como administrar la pertenencia al grupo. Un propietario también puede agregar o quitar otros propietarios. A diferencia de los administradores globales y usuario, los propietarios solo pueden administrar los grupos que poseen. Para asignar un propietario de grupo, consulte [Administración de propietarios de un grupo](active-directory-accessmanagement-managing-group-owners.md).

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre cómo asignar roles de administrador de Azure AD, vea [Asignación de roles de administrador a un usuario en Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
* Para más información acerca de cómo se controla el acceso a los recursos en Microsoft Azure, consulte [Descripción de acceso a los recursos de Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Para más información sobre cómo se relaciona Azure Active Directory con la suscripción de Azure, consulte [Asociación de las suscripciones de Azure con Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Administrar usuarios](add-users-azure-active-directory.md)
