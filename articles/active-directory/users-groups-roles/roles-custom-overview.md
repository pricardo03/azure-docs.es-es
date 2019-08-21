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
ms.openlocfilehash: eabf29b10814d19e89c21f27ec66fce5355c9bfb
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68880713"
---
# <a name="custom-administrator-roles-in-azure-active-directory-preview"></a>Roles de administrador personalizados en Azure Active Directory (versión preliminar)

En este artículo se describe cómo funciona el nuevo control de acceso basado en roles (RBAC) personalizado y los ámbitos de recurso en Azure Active Directory (Azure AD). Los roles personalizados de RBAC emergen de los permisos subyacentes de los [roles integrados](directory-assign-admin-roles.md), por lo que es posible crear y organizar sus propios roles. Este enfoque le permite conceder acceso de forma más pormenorizada que los roles integrados, cuando sea necesario. Esta primera versión de roles de RBAC personalizados incluye la posibilidad de crear un rol para asignar permisos para administrar los registros de aplicaciones. Con el tiempo, se agregarán permisos adicionales para los recursos de la organización, como aplicaciones empresariales, usuarios y dispositivos.  

Además, los roles de RBAC personalizados admiten asignaciones en cada recurso, además de las asignaciones más tradicionales de toda la organización. Este enfoque le proporciona la capacidad de conceder acceso para administrar algunos recursos (por ejemplo, un registro de aplicación) sin conceder acceso a todos los recursos (todos los registros de aplicaciones).

El control de acceso basado en rol de Azure AD es una característica en vista previa (GB) pública de Azure AD y están disponibles con cualquier plan de licencias de Azure AD de pago. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="understand-azure-ad-role-based-access-control"></a>Información sobre el control de acceso basado en rol de Azure AD

La concesión de permisos mediante roles de RBAC personalizados es un proceso de dos pasos que implica la creación de una definición de roles personalizada y su asignación mediante una asignación de roles. Una definición de roles personalizada es una colección de permisos que se agregan desde una lista preestablecida. Estos permisos son los mismos que se usan en los roles integrados.  

Una vez que haya creado la definición de rol, puede crear una asignación de roles para asignársela a alguien. Una asignación de roles concede a un usuario los permisos en una definición de roles y en un ámbito específico. Este proceso de dos etapas le permite crear una definición de roles y asignarla muchas veces en distintos ámbitos. Un ámbito define el conjunto de recursos a los que tiene acceso el miembro de rol. El ámbito más común es el de toda la organización. Un rol personalizado se puede asignar en el ámbito de toda la organización, lo que significa que el miembro del rol tiene los permisos de rol en todos los recursos de la organización. También se puede asignar un rol personalizado en un ámbito de objeto. Un ejemplo del ámbito de objeto sería una aplicación única. De este modo, el mismo rol se puede asignar a Sally en todas las aplicaciones de la organización y, a continuación, a Naveen solo en la aplicación de informes de gastos de Contoso.  

RBAC de Azure AD opera con conceptos similares al [control de acceso basado en roles de Azure](../../role-based-access-control/overview.md). La diferencia es que RBAC de Azure controla el acceso a los recursos de Azure, como las máquinas virtuales y los sitios web, y RBAC de Azure AD controla el acceso a Azure AD. Ambos sistemas aprovechan el concepto de definiciones de roles y asignaciones de roles.

### <a name="role-assignments"></a>Asignaciones de roles

Una asignación de roles es el proceso de asociar una definición de roles a un usuario en un ámbito determinado con el fin de conceder acceso. El acceso se concede mediante la creación de una asignación de roles y se revoca al quitar una asignación de roles. Una asignación de roles consta de tres elementos:
- Usuario
- Definición de roles
- Ámbito de recursos

Puede [crear asignaciones de roles](roles-create-custom.md) mediante Azure Portal, Azure AD PowerShell y Graph API. También puede [ver las asignaciones de un rol personalizado](roles-view-assignments.md#view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview).

El diagrama siguiente muestra un ejemplo de una asignación de roles. En este ejemplo, se ha asignado a Chris Green el rol personalizado de administrador del registro de aplicaciones en el ámbito del registro de la aplicación del generador de widgets de Contoso. Esta asignación concede a Chris los permisos del rol de administrador del registro de aplicaciones solo en este registro de aplicación específico.

![La asignación de roles es la manera en que se aplican los permisos y consta de tres partes.](./media/roles-custom-overview/rbac-overview.png)

### <a name="security-principal"></a>Entidad de seguridad

Una entidad de seguridad representa al usuario a quien se le va a asignar el acceso a los recursos de Azure AD. Un *usuario* es un individuo que tiene un perfil de usuario en Azure Active Directory.

### <a name="role"></a>Role

Una definición de roles, o rol, es una colección de permisos. En una definición de roles se muestran las operaciones que se pueden realizar en los recursos de Azure AD, como crear, leer, actualizar y eliminar. Hay dos tipos de roles en Azure AD:

- Roles integrados que Microsoft crea y que no se pueden modificar. El rol integrado Administrador global tiene todos los permisos sobre todos los recursos de Azure AD.
- Roles personalizados que su organización crea y administra.

### <a name="scope"></a>Ámbito

Un ámbito es la restricción de las acciones permitidas a un recurso de Azure AD determinado como parte de una asignación de roles. Cuando asigna un rol, puede especificar un ámbito que limita el acceso del administrador a un recurso específico. Por ejemplo, si quiere conceder un rol personalizado a un desarrollador, pero solo para administrar un registro de aplicación específico, puede incluir el registro de aplicación específico como ámbito en la asignación de roles.

  > [!Note]
  > Los roles personalizados se pueden asignar en el ámbito de directorio y en el ámbito de recurso. Todavía no se pueden asignar en el ámbito de unidad administrativa.
  > Los roles integrados se pueden asignar en el ámbito de directorio y, en algunos casos, en el ámbito de unidad administrativa. Todavía no se pueden asignar a un ámbito de objeto.

## <a name="required-license-plan"></a>Plan de licencia necesario

[!INCLUDE [License requirement for using custom roles in Azure AD](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Pasos siguientes

- Creación de asignaciones de roles personalizados mediante [Azure Portal, Azure AD PowerShell y Graph API](roles-create-custom.md).
- [Visualización de las asignaciones de un rol personalizado](roles-view-assignments.md#view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview).
