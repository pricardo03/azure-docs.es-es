---
title: Problemas con la pertenencia fija y dinámica de los grupos en Azure Active Directory | Microsoft Docs
description: Solución de problemas para la pertenencia dinámica para grupos en Azure AD.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0594d99874ea9bb83673013a9a03272edcd8ce0b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57897680"
---
# <a name="troubleshoot-and-resolve-groups-issues"></a>Localizar y solucionar problemas de grupos

## <a name="troubleshooting-group-creation-issues"></a>Solución de problemas de creación de grupo
**Deshabilitado la creación del grupo de seguridad en Azure portal, pero todavía se pueden crear grupos a través de Powershell** el **usuario puede crear grupos de seguridad en los portales de Azure** en los controles del portal de Azure o no que no es administrador los usuarios pueden crear grupos de seguridad en el panel de acceso o el portal de Azure. No controla la creación del grupo de seguridad a través de Powershell.

Para deshabilitar la creación de grupos para los usuarios sin derechos administrativos en Powershell:
1. Compruebe que los usuarios no administradores pueden crear grupos:
   
   ```
   PS C:\> Get-MsolCompanyInformation | fl UsersPermissionToCreateGroupsEnabled
   ```
  
2. Si devuelve `UsersPermissionToCreateGroupsEnabled : True`, los usuarios no administrativos pueden crear grupos. Para deshabilitar esta característica:
  
   ``` 
   Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False
   ```

<br/>**He recibido un grupos máximo permitido de error al intentar crear un grupo dinámico en Powershell**<br/>
Si recibe un mensaje que indica el Powershell _max permitidas alcanzado el recuento de grupos de directivas de grupo dinámico_, esto significa que se ha alcanzado el límite máximo de grupos dinámicos en el inquilino. El número máximo de grupos dinámicos por inquilino es 5000.

Para crear los nuevos grupos dinámicos, primero deberá eliminar algunos grupos dinámicos existentes. No hay ninguna manera de aumentar el límite.

## <a name="troubleshooting-dynamic-memberships-for-groups"></a>Solución de problemas relacionados con las pertenencias dinámicas para grupos

**He configurado una regla en un grupo pero las pertenencias no se actualizan en el grupo**.<br/>
1. Compruebe los valores de atributos de dispositivo en la regla o de usuario. Asegúrese de que hay usuarios que cumplen la regla. Para los dispositivos, compruebe las propiedades del dispositivo para asegurarse de que todos los atributos sincronizados contienen los valores esperados.<br/>
2. Compruebe la pertenencia del estado de procesamiento para confirmar si ha finalizado. Puede comprobar el [pertenencia al estado de procesamiento](groups-create-rule.md#check-processing-status-for-a-rule) y la última fecha actualizada en el **información general sobre** página para el grupo.

Si todo es correcto, el grupo tardará un poco en rellenarse. Dependiendo del tamaño del inquilino, el grupo puede tardar hasta 24 horas en rellenarse por primera vez o después de un cambio de la regla.

**He configurado una regla, pero ahora se han quitado los miembros existentes de la regla**.<br/> Este es el comportamiento esperado. Los miembros existentes del grupo se quitan cuando una regla se habilita o se cambia. Los usuarios devueltos tras la evaluación de la regla se agregan como miembros al grupo.

**No veo los cambios en la pertenencia al instante cuando agrego o cambio una regla, ¿por qué pasa esto?**<br/>La evaluación de pertenencia dedicada se realiza periódicamente en un proceso asincrónico en segundo plano. La duración del proceso viene determinada por el número de usuarios del directorio y el tamaño del grupo creado como resultado de la regla. Normalmente, los directorios con un número pequeño de usuarios verán los cambios en la pertenencia al grupo en unos pocos minutos. Los directorios con un gran número de usuarios pueden tardar 30 minutos o más en completarse.

**¿Cómo se puede forzar el grupo de procesarse ahora?**<br/>
Actualmente, no hay ninguna manera de desencadenar automáticamente el grupo al que se procesan a petición. Sin embargo, puede desencadenar manualmente el reprocesamiento de mediante la actualización de la regla de pertenencia para agregar un espacio en blanco al final.  

**Se detectó una error de procesamiento de la regla**<br/>En la tabla siguiente se enumeran los errores de reglas de pertenencia dinámica más habituales y se indica cómo corregirlos.

| Error del analizador de reglas | Uso con error | Uso corregido |
| --- | --- | --- |
| Error: no se admite el atributo. |(user.invalidProperty -eq "Value") |(user.department -eq "value")<br/><br/>Asegúrese de que el atributo se encuentra en la [lista de propiedades admitidas](groups-dynamic-membership.md#supported-properties). |
| Error: no se admite el operador en el atributo. |(user.accountEnabled -contains true) |(user.accountEnabled -eq true)<br/><br/>El operador usado no se admite para el tipo de propiedad (en este ejemplo, -contains no se puede usar con el tipo boolean). Utilice los operadores correctos para el tipo de propiedad. |
| Error: error de compilación de consulta. | 1. (user.department -eq "Sales") (user.department -eq "Marketing")<br>2.  (user.userPrincipalName -match "*@domain.ext") | 1. Falta el operador. Use -and u -or para predicados de combinación<br>(user.department -eq "Sales") -or (user.department -eq "Marketing")<br>2. Error en la expresión regular usada con -match<br>(user.userPrincipalName -match ".*@domain.ext")<br>o bien (user.userPrincipalName -match "@domain.ext$") |

## <a name="next-steps"></a>Pasos siguientes

Estos artículos proporcionan información adicional sobre Azure Active Directory.

* [Administración del acceso a los recursos con grupos de Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Administración de aplicaciones en Azure Active Directory](../manage-apps/what-is-application-management.md)
* [¿Qué es Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Integración de las identidades locales con Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
