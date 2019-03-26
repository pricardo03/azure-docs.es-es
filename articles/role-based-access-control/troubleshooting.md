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
ms.date: 03/24/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: d85c49cc8533b88382de81f8f12fde7116afb69a
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407596"
---
# <a name="troubleshoot-rbac-for-azure-resources"></a>Solución de problemas del control de acceso basado en rol para recursos de Azure

En este artículo se responden preguntas comunes acerca del control de acceso basado en rol (RBAC) para los recursos de Azure, con el fin de que sepa qué esperar cuando usa los roles de Azure Portal y de que pueda solucionar los problemas de acceso.

## <a name="problems-with-rbac-role-assignments"></a>Problemas con las asignaciones de roles RBAC

- Si no estás no se puede agregar una asignación de roles en el portal de Azure en **control de acceso (IAM)** porque el **agregar** > **Agregar asignación de roles** opción está deshabilitada o Dado que reciben el error de permisos de "el cliente con el Id. de objeto no tiene autorización para realizar la acción", compruebe que ha iniciado sesión con un usuario que tiene asignado un rol que tiene el `Microsoft.Authorization/roleAssignments/write` permiso como [propietario](built-in-roles.md#owner) o [Administrador de acceso de usuario](built-in-roles.md#user-access-administrator) en el ámbito que se intenta asignar el rol.
- Si recibe el mensaje de error "no se pueden crear ningún más asignaciones de roles (código: RoleAssignmentLimitExceeded) "al intentar asignar un rol, intente reducir el número de asignaciones de roles mediante la asignación de roles a los grupos en su lugar. Azure admite hasta **2000** asignaciones de roles por suscripción.

## <a name="problems-with-custom-roles"></a>Problemas con roles personalizados

- Si desea conocer los pasos para crear un rol personalizado, consulte los tutoriales de rol personalizado mediante [Azure PowerShell](tutorial-custom-role-powershell.md) o [CLI de Azure](tutorial-custom-role-cli.md).
- Si no se puede actualizar un rol personalizado existente, compruebe que ha iniciado sesión con un usuario que tiene asignado un rol que tiene el `Microsoft.Authorization/roleDefinition/write` permiso como [propietario](built-in-roles.md#owner) o [Administrador de acceso de usuario](built-in-roles.md#user-access-administrator).
- Si no se puede eliminar un rol personalizado y obtener el mensaje de error "hay existentes que hacen referencia a la función de las asignaciones de roles (código: RoleDefinitionHasAssignments) ", posteriormente, se muestran las asignaciones de roles sigue usando el rol personalizado. Quite las asignaciones de roles y vuelva a intentar eliminar el rol personalizado.
- Si recibe el mensaje de error "se superó el límite de definición de rol. No hay más definiciones de roles se pueden crear (código: RoleDefinitionLimitExceeded) "al intentar crear un nuevo rol personalizado, eliminar los roles personalizados que no se usan. Azure admite hasta **2000** roles personalizados en un inquilino.
- Si se produce un error similar a "el cliente tiene permiso para realizar la acción 'Microsoft.Authorization/roleDefinitions/write' en el ámbito '/ subscriptions / {subscriptionid}', pero no se encontró la suscripción vinculada" al intentar actualizar un rol personalizado, consulte Si uno o más [ámbitos asignables](role-definitions.md#assignablescopes) se han eliminado en el inquilino. Si se eliminó el ámbito, cree una incidencia de soporte técnico porque no hay ninguna solución autoservicio disponible en este momento.

## <a name="recover-rbac-when-subscriptions-are-moved-across-tenants"></a>Recuperación de RBAC cuando las suscripciones se trasladan de un inquilino a otro

- Si desea conocer los pasos acerca de cómo transferir una suscripción a un Azure AD diferente de inquilinos, consulte [transferir la propiedad de una suscripción de Azure a otra cuenta](../billing/billing-subscription-transfer.md).
- Si transfiere una suscripción a un Azure AD diferente de inquilinos, todas las asignaciones de roles se eliminan permanentemente del inquilino de Azure AD de origen y no se migran al inquilino de Azure AD de destino. Debe volver a crear las asignaciones de roles en el inquilino de destino.
- Si es una instancia de Azure AD administrador Global y no tiene acceso a una suscripción después de haberse movido entre los inquilinos, use el **Access management para recursos de Azure** alternar temporalmente [elevar su acceso](elevate-access-global-admin.md) para obtener acceso a la suscripción.

## <a name="issues-with-service-admins-or-co-admins"></a>Problemas con los administradores o coadministradores de servicios

- Si tiene problemas con el Administrador de servicios o los coadministradores, vea [agregar o cambiar los administradores de suscripción de Azure](../billing/billing-add-change-azure-subscription-administrator.md) y [roles de administrador de suscripción clásica, roles RBAC de Azure y Azure AD roles de administrador](rbac-and-directory-admin-roles.md).

## <a name="access-denied-or-permission-errors"></a>Acceso denegado o errores de permisos

- Si se produce un error de permisos "el cliente con el Id. de objeto no tiene autorización para realizar la acción en el ámbito (código: AuthorizationFailed) "al intentar crear un recurso, compruebe que ha iniciado sesión con un usuario que tiene asignado un rol que tiene permiso de escritura al recurso en el ámbito seleccionado. Por ejemplo, para administrar las máquinas virtuales en un grupo de recursos, debe tener la [colaborador de máquina Virtual](built-in-roles.md#virtual-machine-contributor) rol en el grupo de recursos (o el ámbito primario). Para obtener una lista de los permisos para cada rol integrado, consulte [roles integrados para los recursos de Azure](built-in-roles.md).
- Si se produce un error de permisos "No tiene permiso para crear una solicitud de soporte técnico" al intentar crear o actualizar una incidencia de soporte técnico, compruebe que ha iniciado sesión con un usuario que tiene asignado un rol que tiene el `Microsoft.Support/supportTickets/write` permiso, como [Colaborador de la solicitud de soporte](built-in-roles.md#support-request-contributor).

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

Algunas características de [Azure Functions](../azure-functions/functions-overview.md) requieren acceso de escritura. Por ejemplo, si se asigna a un usuario el rol de lector, este no podrá ver las funciones dentro de una aplicación de función. El portal mostrará **(Sin acceso)**.

![Sin acceso a aplicaciones de función](./media/troubleshooting/functionapps-noaccess.png)

Un lector puede hacer clic en la pestaña **Características de la plataforma** y, a continuación, hacer clic en **Toda las opciones de configuración** para ver algunas opciones de configuración relacionadas con una aplicación de función (similar a una aplicación web), pero no puede modificar ninguna de estas opciones de configuración.

## <a name="next-steps"></a>Pasos siguientes
* [Administración del acceso a los recursos de Azure mediante RBAC y Azure Portal](role-assignments-portal.md)
* [Visualización de registros de actividad de cambios de RBAC en recursos de Azure](change-history-report.md)

