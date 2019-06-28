---
title: Migración de recursos de identidad de Azure desde Azure Alemania a Azure global
description: En este artículo se proporciona información sobre cómo migrar los recursos de identidad de Azure desde Azure Alemania a Azure global.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 8/15/2018
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: 07ecaa564f2fda21967ab6f0c30c06fa876e4171
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033527"
---
# <a name="migrate-identity-resources-to-global-azure"></a>Migración de recursos de identidad a Azure global

En este artículo se proporciona información que puede ayudarle a migrar los recursos de identidad de Azure desde Azure Alemania a Azure global.

La guía acerca de la identidad o los inquilinos está pensada solo para clientes de Azure. Si se utilizan inquilinos comunes de Azure Active Directory (Azure AD) para Azure y O365 (u otros productos de Microsoft), existen complejidades en la migración de identidades y hay que ponerse en contacto primero con el administrador de cuentas antes de utilizar esta guía de migración.

## <a name="azure-active-directory"></a>Azure Active Directory

Azure AD en Azure Alemania es independiente de Azure AD en Azure global. Actualmente, no es posible mover usuarios de Azure AD de Azure Alemania a Azure global.

Los nombres de inquilino predeterminados en Azure Alemania y Azure global son siempre diferentes porque Azure anexa automáticamente un sufijo en función del entorno. Por ejemplo, un nombre de usuario de un miembro del inquilino **contoso** en Azure global es **user1\@contoso.microsoftazure.com**. En Azure Alemania, es **user1\@contoso.microsoftazure.de**.

Si se usan nombres de dominio personalizados (como **contoso.com**) en Azure AD, es preciso registrar el nombre de dominio en Azure. Los nombres de dominio personalizados se pueden definir en *un solo* entorno de nube a la vez. Se produce un error en la validación de dominio cuando el dominio ya está registrado en *cualquier* instancia de Azure Active Directory. Por ejemplo, el usuario **user1\@contoso.com** que existe en Azure Alemania no pueden existir también en Azure global en el mismo nombre al mismo tiempo. El registro de **contoso.com** produciría un error.

Una migración "parcial", en la que algunos usuarios ya están en el entorno nuevo y otros se encuentran aún en el entorno anterior, requiere nombres de inicio de sesión diferentes para los distintos entornos de nube.

En este artículo no se tratan todos los escenarios de migración posibles. Una recomendación depende, por ejemplo, de cómo se aprovisionen los usuarios, las opciones que hay para usar diferentes nombres de usuario o UserPrincipalNames, y otras dependencias. Sin embargo, hemos compilado algunas sugerencias que le ayudarán a inventariar usuarios y grupos en su entorno actual.

Para obtener una lista de todos los cmdlets relacionados con Azure AD, ejecute:

```powershell
Get-Help Get-AzureAD*
```

### <a name="inventory-users"></a>Inventario de usuarios

Para obtener una visión general de todos los usuarios y grupos que existen en una instancia de Azure AD:

```powershell
Get-AzureADUser -All $true
```

Para enumerar solo las cuentas habilitadas, agregue el siguiente filtro:

```powershell
Get-AzureADUser -All $true | Where-Object {$_.AccountEnabled -eq $true}
```

Para realizar un volcado completo de todos los atributos, por si olvida algo:

```powershell
Get-AzureADUser -All $true | Where-Object {$_.AccountEnabled -eq $true} | Format-List *
```

Para seleccionar los atributos que necesite para volver a crear los usuarios:

```powershell
Get-AzureADUser -All $true | Where-Object {$_.AccountEnabled -eq $true} | select UserPrincipalName,DisplayName,GivenName,Surname
```

Para exportar la lista a Excel, use el cmdlet **Export-Csv** al final de esta lista. Una exportación completa podría parecerse a la de este ejemplo:

```powershell
Get-AzureADUser -All $true | Where-Object {$_.AccountEnabled -eq $true} | select UserPrincipalName,DisplayName,GivenName,Surname | Export-Csv -Path c:\temp\alluserUTF8.csv -Delimiter ";" -Encoding UTF8
```

> [!NOTE]
> Las contraseñas no se pueden migrar. En su lugar, en función del escenario, es preciso asignar contraseñas nuevas o usar un mecanismo de autoservicio.
>
>Además, dependiendo del entorno, es posible que deba recopilar otro tipo de información, como por ejemplo, los valores de **Extensions**, **DirectReport**, o **LicenseDetail**.

Dé formato al archivo CSV según sea necesario. Luego, siga los pasos descritos en [Importación de datos desde CSV](/powershell/azure/active-directory/importing-data) para volver a crear los usuarios en el nuevo entorno.

### <a name="inventory-groups"></a>Grupos de inventario

Para documentar la pertenencia al grupo:

```powershell
Get-AzureADGroup
```

Para obtener la lista de miembros de cada grupo:

```powershell
Get-AzureADGroup | ForEach-Object {$_.DisplayName; Get-AzureADGroupMember -ObjectId $_.ObjectId}
```

### <a name="inventory-service-principals-and-applications"></a>Inventariado de aplicaciones y entidades de servicio

Aunque debe volver a crear todas las aplicaciones y entidades de servicio, es aconsejable documentar el estado de ambos elementos. Para obtener una lista completa de todas las entidades de servicio puede usar los siguientes cmdlets:

```powershell
Get-AzureADServicePrincipal |Format-List *
```

```powershell
Get-AzureADApplication |Format-List *
```

Puede obtener más información con otros cmdlets que empiezan por `Get-AzureADServicePrincipal*` o `Get-AzureADApplication*`. 

### <a name="inventory-directory-roles"></a>Inventario de roles de directorio

Para documentar la asignación de roles actual:

```powershell
Get-AzureADDirectoryRole
```

Recorra todos los roles para buscar usuarios o aplicaciones que estén asociadas con cada uno de ellos:

```powershell
Get-AzureADDirectoryRole | ForEach-Object {$_.DisplayName; Get-AzureADDirectoryRoleMember -ObjectId
$_.ObjectId | Format-Table}
```
Para obtener más información:

- Más información acerca de las [soluciones de identidad híbrida](../active-directory/choose-hybrid-identity-solution.md).
- Lea la entrada de blog [Use Azure AD Connect with multiple clouds](https://blogs.technet.microsoft.com/ralfwi/2017/01/24/using-adconnect-with-multiple-clouds/) (Uso de Azure AD Connect con varias nubes) para aprender distintas formas de realizar la sincronización con diferentes entornos de nube.
- Más información acerca de [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).
- Más información acerca de los [nombres de dominio personalizados](../active-directory/fundamentals/add-custom-domain.md).
- Aprenda a [importar datos de CSV en Azure DB](/powershell/azure/active-directory/importing-data).

## <a name="azure-ad-connect"></a>Azure AD Connect

Azure AD Connect es una herramienta que sincroniza datos de identidad entre una instancia de Active Directory local y Azure Active Directory (Azure AD). La versión actual de Azure AD Connect funciona tanto para Azure Germany como para Azure global. Azure AD Connect no puede sincronizarse con más de una instancia de Azure AD a la vez. Si desea sincronizar con Azure Alemania y Azure global al mismo tiempo, considere las siguientes opciones:

- Usar un servidor adicional para la segunda instancia de Azure AD Connect. No puede tener varias instancias de Azure AD Connect en el mismo servidor.
- Defina un nuevo nombre de inicio de sesión para los usuarios. La parte del dominio (después de **\@** ) del nombre de inicio de sesión debe ser diferente en cada entorno.
- Defina un "origen de verdad" claro cuando realice la sincronización en el sentido inverso (desde Azure AD a un entorno local de Active Directory).

Si ya usa Azure AD Connect para sincronizar a Azure Alemania, como desde esta, asegúrese de migrar todos los usuarios creados manualmente. El siguiente cmdlet de PowerShell enumera todos los usuarios que no se han sincronizado sincronizados mediante Azure AD Connect:

```powershell
Get-AzureADUser -All $true |Where-Object {$_.DirSyncEnabled -ne "True"}
```

Para obtener más información:

- Más información acerca de [Azure AD Connect](../active-directory/hybrid/reference-connect-dirsync-deprecated.md).

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Debe volver a crear los usuarios y volver a definir la instancia de Azure Multi-factor Authentication en el nuevo entorno. 

Para obtener una lista de cuentas de usuario para las que la autenticación multifactor está habilitada o es obligatoria:

1. Inicie sesión en el Portal de Azure.
1. Seleccione **Usuarios** > **Todos los usuarios** > **Autenticación multifactor**.
1. Cuando se le redirija a la página del servicio de autenticación multifactor, establezca los filtros apropiados para obtener una lista de usuarios.

Para obtener más información:

- Más información acerca de [Azure Multi-Factor Authentication](../active-directory/authentication/howto-mfa-getstarted.md).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las distintas herramientas, técnicas y recomendaciones para migrar los recursos en las siguientes categorías de servicio:

- [Proceso](./germany-migration-compute.md)
- [Redes](./germany-migration-networking.md)
- [Storage](./germany-migration-storage.md)
- [Web](./germany-migration-web.md)
- [Bases de datos](./germany-migration-databases.md)
- [Analytics](./germany-migration-analytics.md)
- [IoT](./germany-migration-iot.md)
- [Integración](./germany-migration-integration.md)
- [Seguridad](./germany-migration-security.md)
- [Herramientas de administración](./germany-migration-management-tools.md)
- [Elementos multimedia](./germany-migration-media.md)
