---
title: Solución de problemas de RBAC en Azure | Microsoft Docs
description: Solución de problemas con el control de acceso basado en rol (RBAC) de Azure.
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
ms.date: 01/18/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: e204beea5bdf72c2ec5ebcf661d3c983a2e0e6b4
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2019
ms.locfileid: "54411244"
---
# <a name="troubleshoot-rbac-in-azure"></a>Solución de problemas de RBAC en Azure

En este artículo se responden preguntas comunes acerca del control de acceso basado en rol (RBAC), con el fin de que sepa qué esperar cuando usa los roles de Azure Portal y de que pueda solucionar los problemas de acceso.

## <a name="problems-with-rbac-role-assignments"></a>Problemas con las asignaciones de roles RBAC

- Si no se puede agregar una asignación de roles porque la opción **Agregar asignación de roles** está deshabilitada o porque recibe un error de permisos, compruebe que está utilizando un rol que tenga el permiso `Microsoft.Authorization/roleAssignments/*` en el ámbito en el que está intentando asignar el rol. Si no tiene este permiso, póngase en contacto con el administrador de la suscripción.
- Si recibe un error de permisos al intentar crear un recurso, compruebe que está utilizando un rol que tenga permiso para crear recursos en el ámbito seleccionado. Por ejemplo, puede que tenga que ser Colaborador. Si no tiene este permiso, póngase en contacto con el administrador de la suscripción.
- Si recibe un error de permisos al intentar crear o actualizar una incidencia de soporte técnico, compruebe que está utilizando un rol que tenga el permiso `Microsoft.Support/*`, por ejemplo, [Colaborador de solicitudes de soporte técnico](built-in-roles.md#support-request-contributor).
- Si, al intentar asignar un rol, se produce un error que indica que se ha superado el número de asignaciones de roles, intente asignar los roles a grupos en su lugar para reducir el número de asignaciones de roles. Azure admite hasta **2000** asignaciones de roles por suscripción.

## <a name="problems-with-custom-roles"></a>Problemas con roles personalizados

- Si no se puede actualizar un rol personalizado existente, compruebe si tiene el `Microsoft.Authorization/roleDefinition/write` permiso.
- Si no puede actualizar un rol personalizado existente, compruebe si se han eliminado uno o varios ámbitos asignables en el inquilino. La propiedad `AssignableScopes` de un rol personalizado controla [quién puede crear, eliminar, actualizar o ver dicho rol](custom-roles.md#who-can-create-delete-update-or-view-a-custom-role).
- Si, al intentar crear un nuevo rol, se produce un error que indica que ha superado el límite de definiciones de rol, elimine los roles personalizados que no se utilizan. También puede intentar consolidar o volver a usar los roles personalizados existentes. Azure admite hasta **2000** roles personalizados en un inquilino.
- Si no puede eliminar un rol personalizado, compruebe si alguna asignación de roles sigue usando el rol personalizado.

## <a name="recover-rbac-when-subscriptions-are-moved-across-tenants"></a>Recuperación de RBAC cuando las suscripciones se trasladan de un inquilino a otro

- Si necesita pasos para transferir una suscripción a otro inquilino, consulte [Transferencia de la propiedad de una suscripción de Azure a otra cuenta](../billing/billing-subscription-transfer.md).
- Si transfiere una suscripción a un nuevo inquilino, todas las asignaciones de roles se eliminan permanentemente del inquilino de origen y no se migran al inquilino de destino. Debe volver a crear las asignaciones de roles en el inquilino de destino.
- Si es un administrador global y ya no tiene acceso a una suscripción, use la **administración de acceso a los recursos de Azure** para [elevar sus permisos de acceso](elevate-access-global-admin.md) temporalmente y volver a obtener acceso a la suscripción.

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
* [Administración del acceso mediante RBAC y Azure Portal](role-assignments-portal.md)
* [Visualización de los registros de actividad de cambios de RBAC](change-history-report.md)

