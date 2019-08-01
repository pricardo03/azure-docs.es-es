---
title: Solución de problemas del control de acceso basado en rol para recursos de Azure | Microsoft Docs
description: Solucione problemas con el control de acceso basado en rol (RBAC) para recursos de Azure.
services: azure-portal
documentationcenter: na
author: rolyon
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: role-based-access-control
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: 59ece9c37a563efba6329a30c06c1b596b1a5d57
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67058157"
---
# <a name="troubleshoot-rbac-for-azure-resources"></a>Solución de problemas del control de acceso basado en rol para recursos de Azure

En este artículo se responden preguntas comunes acerca del control de acceso basado en rol (RBAC) para los recursos de Azure, con el fin de que sepa qué esperar cuando usa los roles de Azure Portal y de que pueda solucionar los problemas de acceso.

## <a name="problems-with-rbac-role-assignments"></a>Problemas con las asignaciones de roles RBAC

- Si no puede agregar una asignación de roles en Azure Portal, en **Control de acceso (IAM)** porque la opción **Agregar** > **Agregar asignación de roles** está deshabilitada o porque recibe el error de permisos "El cliente con el identificador de objeto no está autorizado para realizar la acción", compruebe que ha iniciado sesión con un usuario que tenga asignado un rol con el permiso `Microsoft.Authorization/roleAssignments/write`, como [Propietario](built-in-roles.md#owner) o [Administrador de acceso de usuario](built-in-roles.md#user-access-administrator), en el ámbito en el que intenta asignar el rol.
- Si aparece el mensaje de error "No se pueden crear más asignaciones de roles (código: RoleAssignmentLimitExceeded)" al intentar asignar un rol, pruebe a asignar los roles a grupos en su lugar para reducir el número de asignaciones de roles. Azure admite hasta **2000** asignaciones de roles por suscripción.

## <a name="problems-with-custom-roles"></a>Problemas con roles personalizados

- Si necesita conocer los pasos para crear un rol personalizado, consulte los tutoriales sobre roles personalizados con [Azure PowerShell](tutorial-custom-role-powershell.md) o la [CLI de Azure](tutorial-custom-role-cli.md).
- Si no puede actualizar un rol personalizado existente, compruebe que haya iniciado sesión con un usuario que tenga asignado un rol con el permiso `Microsoft.Authorization/roleDefinition/write`, como [Propietario](built-in-roles.md#owner) o [Administrador de acceso de usuario](built-in-roles.md#user-access-administrator).
- Si no puede eliminar un rol personalizado y obtiene el mensaje de error "Hay asignaciones de roles existentes que hacen referencia al rol (código: RoleDefinitionHasAssignments)", significa que hay asignaciones de roles que siguen usando el rol personalizado. Quite las asignaciones de roles y vuelva a intentar eliminarlo.
- Si recibe el mensaje de error "Se ha superado el límite de definiciones de roles. No se pueden crear más definiciones de roles (código: RoleDefinitionLimitExceeded)" al intentar crear un nuevo rol personalizado, elimine los roles personalizados que no se usan. Azure admite hasta **5000** roles personalizados en un inquilino. (Para nubes especializadas, como Azure Government, Azure Alemania y Azure China 21Vianet, el límite es de 2000 roles personalizados).
- Si obtiene un error similar a "El cliente tiene permiso para realizar la acción 'Microsoft.Authorization/roleDefinitions/write' en el ámbito '/subscriptions/{subscriptionid}', aunque la suscripción vinculada no se encontró" al intentar actualizar un rol personalizado, consulte si se han eliminado uno o varios [ámbitos asignables](role-definitions.md#assignablescopes) del inquilino. Si el ámbito se ha eliminado, cree una incidencia de soporte técnico porque no hay ninguna solución de autoservicio disponible en este momento.

## <a name="recover-rbac-when-subscriptions-are-moved-across-tenants"></a>Recuperación de RBAC cuando las suscripciones se trasladan de un inquilino a otro

- Si necesita consultar los pasos para transferir una suscripción a otro inquilino de Azure AD, consulte [Transferencia de la propiedad de una suscripción de Azure a otra cuenta](../billing/billing-subscription-transfer.md).
- Si transfiere una suscripción a un nuevo inquilino de Azure AD, todas las asignaciones de roles se eliminan permanentemente del inquilino de Azure AD de origen y no se migran al inquilino de Azure AD de destino. Debe volver a crear las asignaciones de roles en el inquilino de destino. También debe volver a crear manualmente las identidades administradas para los recursos de Azure. Para más información, consulte [Preguntas frecuentes y problemas conocidos con identidades administradas](../active-directory/managed-identities-azure-resources/known-issues.md).
- Si es un administrador global de Azure AD y no tiene acceso a una suscripción después de trasladarla de un inquilino a otro, use la **administración de acceso a los recursos de Azure** para [elevar sus permisos de acceso](elevate-access-global-admin.md) temporalmente y obtener acceso a la suscripción.

## <a name="issues-with-service-admins-or-co-admins"></a>Problemas con los administradores o coadministradores de servicios

- Si tiene problemas con el administrador del servicio o los coadministradores, consulte [Agregar o cambiar los administradores de la suscripción de Azure](../billing/billing-add-change-azure-subscription-administrator.md) y [Roles de administrador de suscripciones clásico de RBAC de Azure y de administrador de Azure AD](rbac-and-directory-admin-roles.md).

## <a name="access-denied-or-permission-errors"></a>Errores de permiso o acceso denegado

- Si recibe el error de permiso "El cliente con el identificador de objeto no está autorizado para realizar la acción sobre el ámbito (código: AuthorizationFailed)" al intentar crear un recurso, compruebe que ha iniciado sesión con un usuario que tiene asignado un rol con permiso de escritura en el recurso y en el ámbito seleccionado. Por ejemplo, para administrar las máquinas virtuales de un grupo de recursos, debe tener el rol [Colaborador de máquina virtual](built-in-roles.md#virtual-machine-contributor) en el grupo de recursos (o el ámbito primario). Para ver una lista de todos los permisos para cada uno de los roles integrados, consulte [Roles integrados en los recursos de Azure](built-in-roles.md).
- Si aparece el error de permiso "No tiene permiso para crear una solicitud de soporte técnico" al intentar crear o actualizar una incidencia de soporte técnico, compruebe que ha iniciado sesión con un usuario que tenga asignado un rol con el permiso `Microsoft.Support/supportTickets/write`, como [Colaborador de solicitud de soporte técnico](built-in-roles.md#support-request-contributor).

## <a name="role-assignments-without-a-security-principal"></a>Asignaciones de roles sin una entidad de seguridad

Al enumerar las asignaciones de roles con Azure PowerShell, puede que vea asignaciones con un valor de `DisplayName` vacío y un valor de `ObjectType` desconocido. Por ejemplo, [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment) devuelve una asignación de roles que es similar a la siguiente:

```azurepowershell
RoleAssignmentId   : /subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
Scope              : /subscriptions/11111111-1111-1111-1111-111111111111
DisplayName        :
SignInName         :
RoleDefinitionName : Storage Blob Data Contributor
RoleDefinitionId   : ba92f5b4-2d11-453d-a403-e96b0029c9fe
ObjectId           : 33333333-3333-3333-3333-333333333333
ObjectType         : Unknown
CanDelegate        : False
```

Igualmente, al enumerar las asignaciones de roles mediante la CLI de Azure, puede que vea asignaciones con un valor de `principalName` vacío. Por ejemplo, [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list) devuelve una asignación de roles que es similar a la siguiente:

```azurecli
{
    "canDelegate": null,
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222",
    "name": "22222222-2222-2222-2222-222222222222",
    "principalId": "33333333-3333-3333-3333-333333333333",
    "principalName": "",
    "roleDefinitionId": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/ba92f5b4-2d11-453d-a403-e96b0029c9fe",
    "roleDefinitionName": "Storage Blob Data Contributor",
    "scope": "/subscriptions/11111111-1111-1111-1111-111111111111",
    "type": "Microsoft.Authorization/roleAssignments"
}
```

Estas asignaciones de roles se producen cuando se asigna un rol a una entidad de seguridad (usuario, grupo, entidad de servicio o identidad administrada) y posteriormente se elimina esa entidad de seguridad. Estas asignaciones de roles no se muestran en Azure Portal y no es un problema dejarlas. Sin embargo, si lo desea, puede quitarlas.

Para quitar estas asignaciones de roles, use los comandos [Remove-AzRoleAssignment](/powershell/module/az.resources/remove-azroleassignment) o [az role assignment delete](/cli/azure/role/assignment#az-role-assignment-delete).

En PowerShell, si intenta quitar las asignaciones de roles mediante el identificador de objeto y el nombre de la definición de roles, y más de una asignación de roles coincide con los parámetros, obtendrá el mensaje de error: "The provided information does not map to a role assignment" (La información proporcionada no se asigna a una asignación de roles). A continuación se muestra un ejemplo del mensaje de error:

```Example
PS C:\> Remove-AzRoleAssignment -ObjectId 33333333-3333-3333-3333-333333333333 -RoleDefinitionName "Storage Blob Data Contributor"

Remove-AzRoleAssignment : The provided information does not map to a role assignment.
At line:1 char:1
+ Remove-AzRoleAssignment -ObjectId 33333333-3333-3333-3333-333333333333 ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : CloseError: (:) [Remove-AzRoleAssignment], KeyNotFoundException
+ FullyQualifiedErrorId : Microsoft.Azure.Commands.Resources.RemoveAzureRoleAssignmentCommand
```

Si recibe este mensaje de error, asegúrese de especificar también los parámetros `-Scope` o `-ResourceGroupName`.

```Example
PS C:\> Remove-AzRoleAssignment -ObjectId 33333333-3333-3333-3333-333333333333 -RoleDefinitionName "Storage Blob Data Contributor" - Scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="rbac-changes-are-not-being-detected"></a>Los cambios RBAC no se detectan

Azure Resource Manager a veces almacena en caché las configuraciones y los datos para mejorar el rendimiento. Al crear o eliminar asignaciones de roles, los cambios pueden tardar hasta 30 minutos en aplicarse. Si usa Azure Portal, Azure PowerShell o la CLI de Azure, puede exigir una actualización de los cambios de asignación de roles cerrando e iniciando sesión. Si va a realizar cambios de asignación de roles con llamadas API de REST, puede exigir una actualización renovando el token de acceso.

## <a name="web-app-features-that-require-write-access"></a>Características de aplicaciones web que requieren acceso de escritura

Si concede a un usuario acceso de solo lectura a una única aplicación web, se deshabilitan algunas características que no cabría esperar. Las siguientes funcionalidades de administración requieren acceso de **escritura** a una aplicación web (como colaborador o como propietario) y no están disponibles en un escenario de solo lectura.

* Comandos (p. ej., iniciar, parar, etc.)
* Cambiar opciones, como la configuración general, opciones de escala, opciones de copia de seguridad y opciones de supervisión
* Acceder a las credenciales de publicación y otros secretos, como opciones de aplicaciones y cadenas de conexión
* Registros de streaming
* Configuración de registros de diagnóstico
* Consola (símbolo del sistema)
* Implementaciones activas y recientes (para implementaciones git continuas locales)
* Gasto estimado
* Pruebas web
* Red virtual (solo visible para un lector si un usuario con acceso de escritura ha configurado previamente una red virtual)

Si no puede acceder a ninguno de estos iconos, debe pedirle al administrador el acceso de colaborador a la aplicación web.

## <a name="web-app-resources-that-require-write-access"></a>Recursos de aplicaciones web que requieren acceso de escritura

Las aplicaciones web pueden resultar complicadas si entran en juego distintos recursos. Este es un grupo de recursos típico con un par de sitios web:

![Grupo de recursos de aplicación web](./media/troubleshooting/website-resource-model.png)

Como consecuencia, si le concede a alguien acceso solo a la aplicación web, muchas de las funcionalidades de la hoja del sitio web de Azure Portal están deshabilitadas.

Estos elementos requieren acceso de **escritura** al **plan de App Service** que corresponde a su sitio web:  

* Visualización del plan de tarifa de la aplicación web (gratis o estándar).  
* Configuración de escala (número de instancias, tamaño de la máquina virtual y configuración de escala automática).  
* Cuotas (almacenamiento, ancho de banda y CPU).  

Estos elementos requieren acceso de **escritura** a todo el **grupo de recursos** que contiene su sitio web:  

* Enlaces y certificados SSL (los certificados SSL se pueden compartir entre sitios en el mismo grupo de recursos y la misma ubicación geográfica)  
* Reglas de alertas  
* Opciones de escala automática  
* Componentes de Application Insights  
* Pruebas web  

## <a name="virtual-machine-features-that-require-write-access"></a>Características de máquina virtual que requieren acceso de escritura

De manera similar a las aplicaciones web, algunas funciones de la hoja de máquina virtual requieren acceso de escritura a la máquina virtual o a otros recursos del grupo de recursos.

Las máquinas virtuales están relacionadas con los nombres de dominio, las redes virtuales, las cuentas de almacenamiento y las reglas de alerta.

Estos elementos requieren acceso de **escritura** a la **máquina virtual**:

* Puntos de conexión  
* Direcciones IP  
* Discos  
* Extensiones  

Estos requieren acceso de **escritura** a la **máquina virtual** y al **grupo de recursos** (junto con el nombre de dominio) donde se encuentran:  

* Conjunto de disponibilidad  
* El conjunto de carga equilibrada  
* Reglas de alertas  

Si no puede acceder a ninguno de estos iconos, debe pedirle al administrador el acceso de colaborador al grupo de recursos.

## <a name="azure-functions-and-write-access"></a>Azure Functions y acceso de escritura

Algunas características de [Azure Functions](../azure-functions/functions-overview.md) requieren acceso de escritura. Por ejemplo, si se asigna a un usuario el rol de lector, este no podrá ver las funciones dentro de una aplicación de función. El portal mostrará **(Sin acceso)** .

![Sin acceso a aplicaciones de función](./media/troubleshooting/functionapps-noaccess.png)

Un lector puede hacer clic en la pestaña **Características de la plataforma** y, a continuación, hacer clic en **Toda las opciones de configuración** para ver algunas opciones de configuración relacionadas con una aplicación de función (similar a una aplicación web), pero no puede modificar ninguna de estas opciones de configuración.

## <a name="next-steps"></a>Pasos siguientes
* [Administración del acceso a los recursos de Azure mediante RBAC y Azure Portal](role-assignments-portal.md)
* [Visualización de registros de actividad de cambios de RBAC en recursos de Azure](change-history-report.md)

