---
title: 'Versión preliminar de los roles de administrador de Azure con permisos personalizables: Azure Active Directory | Microsoft Docs'
description: Versión preliminar de los roles personalizados de Azure AD para delegar la administración de identidades. Administre roles de Azure en Azure Portal, PowerShell o Graph API.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 82638e3e102f7b8e39cd797960a11f3193132bc1
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2019
ms.locfileid: "68779391"
---
# <a name="custom-administrator-roles-in-azure-active-directory-preview"></a>Roles de administrador personalizados en Azure Active Directory (versión preliminar)

En este artículo se describe cómo entender el nuevo RBAC personalizado (control de acceso basado en roles) y los ámbitos de recurso en Azure Active Directory (Azure AD). Los roles personalizados de RBAC emergen de los permisos subyacentes de los [roles integrados](directory-assign-admin-roles.md), por lo que es posible crear y organizar sus propios roles. Los ámbitos de recurso proporcionan una manera de asignar el rol personalizado para administrar algunos recursos (por ejemplo, una aplicación) sin dar acceso a todos los recursos (todas las aplicaciones).

La concesión de permisos mediante roles de RBAC personalizados es un proceso de dos pasos. En primer lugar, cree una definición de roles personalizada y agréguele permisos a partir de una lista preestablecida. Estos son los mismos permisos que se usan en los roles integrados. Una vez que haya creado el rol, cree una asignación de roles para asignárselo a alguien. Este proceso de dos pasos le permite crear un rol y asignarlo muchas veces en distintos ámbitos. Un rol personalizado se puede asignar en el ámbito de directorio o bien, en el ámbito de objeto. Un ejemplo del ámbito de objeto sería una aplicación única. De este modo, el mismo rol se puede asignar a Sally en todas las aplicaciones del directorio y, a continuación, a Naveen solo en la aplicación de informes de gastos de Contoso.

Esta primera versión de roles de RBAC personalizados incluye la posibilidad de crear un rol para asignar permisos para administrar los registros de aplicaciones. Con el tiempo, se agregarán permisos adicionales para los recursos de la organización, como aplicaciones empresariales, usuarios y dispositivos.

Características en vista previa (GB):

- Actualizaciones de la interfaz de usuario del portal para crear y administrar los roles personalizados y asignarlos a los usuarios en el ámbito de organización.
- Un módulo de PowerShell de versión preliminar con cmdlets nuevos para:
  - Crear y administrar los roles personalizados.
  - Asignar los roles personalizados con un ámbito de registro de toda la organización o por aplicación.
  - Asignar los roles en un ámbito de toda la organización (paridad con cmdlets de GA).
  - Compatibilidad con Graph API de Azure AD

El control de acceso basado en rol de Azure AD es una característica en vista previa (GB) pública de Azure AD y están disponibles con cualquier plan de licencias de Azure AD de pago. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="understand-azure-ad-role-based-access-control"></a>Información sobre el control de acceso basado en rol de Azure AD

Con el control de acceso basado en rol de Azure AD puede asignar roles que están personalizados para permitir acciones en un solo tipo de recurso de Azure AD. El acceso basado en rol de Azure AD funciona en conceptos similares al control de acceso basado en rol de Azure ([Azure RBAC](../../role-based-access-control/overview.md) para el acceso a los recursos de Azure, pero el control de acceso basado en rol de Azure AD se basa en Microsoft Graph y Azure RBAC, en Azure Resource Manager. Sin embargo, ambos sistemas basan sus funciones en las asignaciones de roles.

### <a name="role-assignments"></a>Asignaciones de roles

La manera de controlar el acceso mediante el control de acceso basado en rol de Azure AD es crear **asignaciones de roles**, que se usan para aplicar los permisos. Una asignación de roles consta de tres elementos:

- Entidad de seguridad
- Definición de roles
- Ámbito de recursos

El acceso se concede mediante la creación de una asignación de roles y se revoca al quitar una asignación de roles. Puede [crear asignaciones de roles](roles-create-custom.md) mediante Azure Portal, Azure AD PowerShell y Graph API. Puede [ver las asignaciones de un rol personalizado](roles-view-assignments.md#view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview) por separado.

El diagrama siguiente muestra un ejemplo de una asignación de roles. En este ejemplo, a Chris Green se le asignó el rol [Administrador de aplicaciones](directory-assign-admin-roles.md#application-administrator) en el ámbito de la aplicación SalesForce. Chris no tiene acceso para administrar ninguna otra aplicación, a menos que formen parte de una asignación de roles distinta.

![La asignación de roles es la manera en que se aplican los permisos y consta de tres partes.](./media/roles-custom-overview/rbac-overview.png)

### <a name="security-principal"></a>Entidad de seguridad

Una entidad de seguridad representa al usuario o a la entidad de servicio a quien se le va a asignar el acceso a los recursos de Azure AD. Un *usuario* es un individuo que tiene un perfil de usuario en Azure Active Directory. Una *entidad de servicio* es una identidad de seguridad que las aplicaciones o los servicios usan para acceder a recursos específicos de Azure AD.

Una entidad de seguridad es similar a una identidad de usuario en que representa a un nombre de usuario y una contraseña o certificado, pero para una aplicación o servicio en lugar de un usuario.

### <a name="role"></a>Role

Una definición de roles, o rol, es una colección de permisos. En una definición de roles se muestran las operaciones que se pueden realizar en los recursos de Azure AD, como crear, leer, actualizar y eliminar. Hay dos tipos de roles en Azure AD:

- Roles integrados que Microsoft crea y que no se pueden modificar. El rol integrado Administrador global tiene todos los permisos sobre todos los recursos de Azure AD.
- Roles personalizados que su organización crea y administra.

### <a name="scope"></a>Ámbito

Un ámbito es la restricción de las acciones permitidas a un recurso de Azure AD determinado. Cuando asigna un rol, puede especificar un ámbito que limita las acciones permitidas del administración a un recurso específico. Por ejemplo, si quiere conceder un rol personalizado a un desarrollador, pero solo para administrar un registro de aplicación específico, puede incluir el registro de aplicación específico como ámbito en la asignación de roles.

  > [!Note]
  > Los roles personalizados se pueden asignar en el ámbito de directorio y en el ámbito de recurso. Todavía no se pueden asignar en el ámbito de unidad administrativa.
  > Los roles integrados se pueden asignar en el ámbito de directorio y, en algunos casos, en el ámbito de unidad administrativa. Todavía no se pueden asignar a un ámbito de objeto.

## <a name="required-license-plan"></a>Plan de licencia necesario

[!INCLUDE [License requirement for using custom roles in Azure AD](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Pasos siguientes

- Creación de asignaciones de roles personalizados mediante [Azure Portal, Azure AD PowerShell y Graph API](roles-create-custom.md).
- [Visualización de las asignaciones de un rol personalizado](roles-view-assignments.md#view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview).
