---
title: 'Acceso delegado en el escritorio Virtual de Windows (versión preliminar): Azure'
description: Cómo delegar funciones administrativas en una implementación de Escritorio Virtual de Windows, incluidos ejemplos.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 49645b697071abb8a2f8c85ebde1e6761a3536ab
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58336106"
---
# <a name="delegated-access-in-windows-virtual-desktop-preview"></a>Acceso delegado en el escritorio Virtual de Windows (versión preliminar)

Escritorio Virtual de Windows (versión preliminar) tiene un modelo de acceso delegado que permite definir la cantidad de acceso a que un usuario determinado puede tener mediante la asignación de un rol. Una asignación de roles tiene tres componentes: entidad de seguridad, la definición de rol y el ámbito. El modelo de acceso delegado de Escritorio Virtual de Windows se basa en el modelo de RBAC de Azure. Para obtener más información acerca de las asignaciones de roles específicos y sus componentes, vea [la información general de control de acceso basado en rol de Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles).

Escritorio Virtual de Windows se delegan admite los siguientes valores para cada elemento de la asignación de roles de acceso:

* Entidad de seguridad
    * Usuarios
    * Entidades de servicio
* Definición de roles
    * Roles integrados
* Ámbito
    * Grupos de inquilinos
    * Inquilinos
    * Grupos host
    * Grupos de aplicaciones

## <a name="built-in-roles"></a>Roles integrados

Acceso delegado en el escritorio Virtual de Windows tiene varias definiciones de rol integrado que se puede asignar a los usuarios y entidades de servicio.

* RDS propietario puede administrar todo, incluido el acceso a los recursos.
* Un colaborador de RDS puede administrar todo excepto el acceso a los recursos.
* Un lector de RDS puede ver todo el contenido, pero no realizar cambios.
* Un operador de RDS puede ver las actividades de diagnóstico.

## <a name="powershell-cmdlets-for-role-assignments"></a>Cmdlets de PowerShell para las asignaciones de roles

Puede ejecutar los siguientes cmdlets para crear, ver y editar las asignaciones de roles:

* **Get-RdsRoleAssignment** muestra una lista de asignaciones de roles.
* **Nuevo RdsRoleAssignment** crea una nueva asignación de roles.
* **Conjunto RdsRoleAssignment** edita las asignaciones de roles.

### <a name="accepted-parameters"></a>Parámetros aceptados

Puede modificar los tres cmdlets básicos con los siguientes parámetros:

* **AadTenantId**: especifica el identificador del inquilino de Azure Active Directory desde el que la entidad de servicio es un miembro.
* **AppGroupName**: nombre del grupo de aplicaciones de escritorio remoto.
* **Diagnósticos**: indica el ámbito de diagnóstico. (Debe estar emparejado con cualquiera el **infraestructura** o **inquilino** parámetros.)
* **HostPoolName**: nombre del grupo de host de escritorio remoto.
* **Infraestructura**: indica el ámbito de la infraestructura.
* **RoleDefinitionName**: nombre de la función de control de acceso basado en roles de servicios de escritorio remoto asignada al usuario, grupo o aplicación. (Por ejemplo, propietario de los servicios de escritorio remoto, lector de servicios de escritorio remoto y así sucesivamente.)
* **ServerPrincipleName**: nombre de la aplicación de Azure Active Directory.
* **SignInName**: dirección de correo electrónico del usuario o nombre principal de usuario.
* **TenantName**: nombre del inquilino en el escritorio remoto.

## <a name="next-steps"></a>Pasos siguientes

Para obtener una lista más completa de cmdlets de PowerShell puede usar cada rol, vea la [referencia de PowerShell](/powershell/windows-virtual-desktop/overview).

Para obtener instrucciones acerca de cómo configurar un entorno de Escritorio Virtual de Windows, vea [configurar un entorno de Escritorio Virtual de Windows](environment-setup.md).
