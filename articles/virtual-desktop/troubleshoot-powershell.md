---
title: 'Windows PowerShell de Escritorio Virtual: Azure'
description: Cómo solucionar problemas con PowerShell al configurar un entorno de inquilinos de Escritorio Virtual de Windows.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshoot
ms.date: 04/08/2019
ms.author: v-chjenk
ms.openlocfilehash: ad32f7ff883812830dbcf2ed900c4034bd90abfc
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64927513"
---
# <a name="windows-virtual-desktop-powershell"></a>Windows PowerShell de Escritorio Virtual

Use este artículo para resolver los problemas y errores al usar PowerShell con el escritorio Virtual de Windows. Para obtener más información sobre PowerShell de servicios de escritorio remoto, consulte [Windows Powershell de Escritorio Virtual](https://docs.microsoft.com/powershell/module/windowsvirtualdesktop/).

## <a name="provide-feedback"></a>Envío de comentarios

En este momento no se aceptan casos de soporte técnico mientras Windows Virtual Desktop se encuentre en versión preliminar. Visite la [Comunidad técnica de Windows Virtual Desktop](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) para hablar sobre Windows Virtual Desktop con el equipo de producto y los miembros activos de la comunidad.

## <a name="powershell-commands-used-during-windows-virtual-desktop-setup"></a>Comandos de PowerShell que se usa durante la instalación del escritorio Virtual de Windows

Esta sección enumeran los comandos de PowerShell que normalmente se usan durante la configuración de Escritorio Virtual de Windows y proporciona maneras de resolver los problemas que pueden surgir al usarlos.

### <a name="error-add-rdsappgroupuser-command----the-specified-userprincipalname-is-already-assigned-to-a-remoteapp-app-group-in-the-specified-host-pool"></a>Error: Comando RdsAppGroupUser agregar--el UserPrincipalName especificado ya está asignado a un grupo de aplicaciones de RemoteApp en el grupo Host especificado

```Powershell
Add-RdsAppGroupUser -TenantName <TenantName> -HostPoolName <HostPoolName> -AppGroupName 'Desktop Application Group' -UserPrincipalName <UserName>
```

**Causa:** El nombre de usuario utilizado se asignó ya a un grupo de aplicaciones de un tipo diferente. Los usuarios no pueden asignarse a ambas un grupo de aplicación remota y escritorio remoto en el mismo grupo host de sesión.

**Fix:** Si el usuario necesita RemoteApp y escritorio remoto, crear grupos de host diferente o conceder acceso de usuario para el escritorio remoto, que permitirá el uso de cualquier aplicación en la máquina virtual del host de sesión.

### <a name="error-add-rdsappgroupuser-command----the-specified-userprincipalname-doesnt-exist-in-the-azure-active-directory-associated-with-the-remote-desktop-tenant"></a>Error: Comando RdsAppGroupUser agregar--el UserPrincipalName especificado no existe en Azure Active Directory asociado con el inquilino de escritorio remoto

```PowerShell
Add-RdsAppGroupUser -TenantName <TenantName> -HostPoolName <HostPoolName> -AppGroupName “Desktop Application Group” -UserPrincipalName <UserPrincipalName>
```

**Causa:** El usuario especificado por el valor de UserPrincipalName: no se encuentra en Azure Active Directory vinculada al inquilino de Escritorio Virtual de Windows.

**Fix:** Confirme que los elementos de la lista siguiente.

- El usuario se sincronizan con Azure Active Directory.
- El usuario no está vinculado a la empresa a consumidor (B2C) o de comercio de negocio a negocio (B2B).
- El inquilino de Escritorio Virtual de Windows está asociado a Active Directory de Azure correcto.

### <a name="error-get-rdsdiagnosticactivities----user-isnt-authorized-to-query-the-management-service"></a>Error: Get-RdsDiagnosticActivities: El usuario no está autorizado para consultar el servicio de administración

```PowerShell
Get-RdsDiagnosticActivities -ActivityId <ActivityId>
```

**Causa:** - TenantName parámetro

**Fix:** Emitir Get-RdsDiagnosticActivities con - TenantName <TenantName>.

### <a name="error-get-rdsdiagnosticactivities----the-user-isnt-authorized-to-query-the-management-service"></a>Error: Get-RdsDiagnosticActivities: el usuario no está autorizado para consultar el servicio de administración

```PowerShell
Get-RdsDiagnosticActivities -Deployment -username <username>
```

**Causa:** El uso de - conmutador de implementación.

**Corrección:** -conmutador de implementación se puede usar solo los administradores de implementación. Estos administradores suelen ser miembros del equipo de Escritorio Virtual de Windows/servicios de escritorio remoto. Reemplace el - conmutador de implementación con - TenantName <TenantName>.

### <a name="error-new-rdsroleassignment----the-user-isnt-authorized-to-query-the-management-service"></a>Error: Nuevo-RdsRoleAssignment: el usuario no está autorizado para consultar el servicio de administración

**Causa 1:** La cuenta utilizada no tiene permisos de propietario de los servicios de escritorio remoto en el inquilino.

**Corrección 1:** Un usuario con permisos de propietario de servicios de escritorio remoto debe ejecutar la asignación de roles.

**Causa 2:** La cuenta utilizada tiene permisos de propietario de servicios de escritorio remoto, pero no forma parte de Azure Active Directory del inquilino o no tiene permisos para consultas en Azure Active Directory donde se encuentra el usuario.

**Corrección 2:** Un usuario con permisos de Active Directory debe ejecutar la asignación de roles.

>[!Note]
>Nuevo RdsRoleAssignment no puede conceder permisos a un usuario que no existe en Azure Active Directory (AD).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información general sobre cómo solucionar problemas de Escritorio Virtual de Windows y las pistas de escalado, consulte [solución de problemas de introducción, comentarios y soporte técnico](troubleshoot-set-up-overview.md).
- Para solucionar problemas durante la creación de un grupo de inquilinos y host en un entorno de Escritorio Virtual de Windows, consulte [creación del grupo de inquilinos y host](troubleshoot-set-up-issues.md).
- Para solucionar problemas al configurar una máquina virtual (VM) en el escritorio Virtual de Windows, consulte [configuración de máquina virtual del host de sesión](troubleshoot-vm-configuration.md).
- Para solucionar problemas con conexiones de cliente de Escritorio Virtual de Windows, consulte [las conexiones de cliente de escritorio remoto](troubleshoot-client-connection.md).
- Para obtener más información sobre el servicio en versión preliminar, consulte [entorno de versión preliminar de Windows Desktop](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
- Para realizar un tutorial de solución de problemas, consulte [Tutorial: Solución de problemas de implementaciones de plantilla de Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Para más información sobre las acciones de auditoría, consulte [Operaciones de auditoría con Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Si desea conocer más detalles sobre las acciones que permiten determinar los errores durante la implementación, consulte [Visualización de operaciones de implementación con el Portal de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).