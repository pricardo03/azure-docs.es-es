---
title: Uso de perfiles de la versión de la API con PowerShell en Azure Stack | Microsoft Docs
description: Obtenga información sobre cómo usar los perfiles de la versión de API con PowerShell en Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: EBAEA4D2-098B-4B5A-A197-2CEA631A1882
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 87052b39524ae7e3a789cada4ef9720f080726a6
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985488"
---
# <a name="use-api-version-profiles-for-powershell-in-azure-stack"></a>Uso de los perfiles de la versión de la API para PowerShell en Azure Stack

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Los perfiles de versión de la API proporcionan una manera de administrar las diferencias de versión entre Azure y Azure Stack. Un perfil de la versión de la API es un conjunto de módulos de PowerShell AzureRM con versiones específicas de la API. Cada plataforma de la nube tiene un conjunto de perfiles de versión de API compatibles. Por ejemplo, Azure Stack admite una versión de perfil con fecha específica, como **2018-03-01-hybrid**, y Azure admite el perfil de la versión de API **más reciente**. Cuando se instala un perfil, se instalan los módulos de PowerShell AzureRM que se corresponden con el perfil especificado.

## <a name="install-the-powershell-module-required-to-use-api-version-profiles"></a>Instale el módulo de PowerShell necesario para usar perfiles de la versión de la API

El módulo **AzureRM.Bootstrapper** que está disponible a través de la Galería de PowerShell proporciona cmdlet de PowerShell que son necesarios para trabajar con perfiles de la versión de API. Use el siguiente cmdlet para instalar el módulo AzureRM.Bootstrapper:

```PowerShell
Install-Module -Name AzureRm.BootStrapper
```

## <a name="azure-stack-version-and-profile-versions"></a>Versión de Azure Stack y versiones de perfiles

En la siguiente tabla se muestran la versión de perfil de API requerida y el moniker del módulo de administrador de PowerShell que se usa en las versiones recientes de Azure Stack. Si usa este artículo con una versión anterior a 1808, actualice su perfil de la versión y el moniker con el valor correcto.

| Número de versión | Perfil de la versión de API | Moniker del módulo de administrador PS |
| --- | --- | --- |
| 1808 o posterior | 2018-03-01-hybrid | 1.5.0 |
| 1804 o posterior | 2017-03-09-profile | 1.4.0 |
| Versiones anteriores a la 1804 | 2017-03-09-profile | 1.2.11 |

> [!Note]  
> Para actualizar desde la versión 1.2.11, consulte la [guía de migración](https://aka.ms/azpsh130migration).

## <a name="install-a-profile"></a>Instalar un perfil

Use el cmdlet **Install-AzureRmProfile** con el perfil de versión **2018-03-01-hybrid** de la API para instalar los módulos de AzureRM que necesita Azure Stack. Los módulos de operador de Azure Stack no se instalan con este perfil de la versión de la API. Estos módulos deben instalarse por separado, tal y como se especificó en el paso 3 del artículo [Instalación de PowerShell para Azure Stack](../azure-stack-powershell-install.md).

```PowerShell 
Install-AzureRMProfile -Profile 2018-03-01-hybrid
```

## <a name="install-and-import-modules-in-a-profile"></a>Instalar e importar módulos en un perfil

Use el cmdlet de **AzureRmProfile Use** para instalar e importar módulos que están asociados a un perfil de la versión de la API. Puede importar un solo perfil de la versión de la API en una sesión de PowerShell. Para importar un perfil de versión de la API diferente, debe abrir una nueva sesión de PowerShell. El cmdlet Use-AzureRMProfile ejecuta las siguientes tareas:  
1. Comprueba si están instalados los módulos de PowerShell asociados al perfil de versión de la API especificado en el ámbito actual.  
2. Descarga e instala los módulos si no están ya instalados.   
3. Importa los módulos en la sesión actual de PowerShell. 

```PowerShell
# Installs and imports the specified API version profile into the current PowerShell session.
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Scope CurrentUser

# Installs and imports the specified API version profile into the current PowerShell session without any prompts
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Scope CurrentUser -Force
```

Para instalar e importar los módulos seleccionados de AzureRM de un perfil de la versión de la API, ejecute el cmdlet Use-AzureRMProfile con el parámetro **Módulo**:

```PowerShell
# Installs and imports the compute, Storage and Network modules from the specified API version profile into your current PowerShell session.
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Module AzureRM.Compute, AzureRM.Storage, AzureRM.Network
```

## <a name="get-the-installed-profiles"></a>Obtener los perfiles instalados

Use el cmdlet **Get AzureRmProfile** para obtener una lista disponible de perfiles de versión de la API: 

```PowerShell
# lists all API version profiles provided by the AzureRM.BootStrapper module.
Get-AzureRmProfile -ListAvailable 

# lists the API version profiles which are installed on your machine
Get-AzureRmProfile
```

## <a name="update-profiles"></a>Administrar perfiles

Use el cmdlet **Update-AzureRmProfile** para actualizar los módulos de un perfil de versión de la API a la versión más reciente de los módulos que están disponibles en la PSGallery. Se recomienda ejecutar siempre el cmdlet **Update-AzureRmProfile** cmdlet en una nueva sesión de PowerShell para evitar conflictos al importar módulos. El cmdlet Update-AzureRmProfile ejecuta las siguientes tareas:

1. Comprueba si se instalan las versiones más recientes de módulos en el perfil determinado de versión de API para el ámbito actual.  
2. Le pide que las instale si no están ya instaladas.  
3. Instala e importa los módulos actualizados en la sesión actual de PowerShell.  

```PowerShell
Update-AzureRmProfile -Profile 2018-03-01-hybrid
```

<!-- To remove the previously installed versions of the modules before updating to the latest available version, use the Update-AzureRmProfile cmdlet along with the **-RemovePreviousVersions** parameter:

```PowerShell 
Update-AzureRmProfile -Profile 2018-03-01-hybrid -RemovePreviousVersions
``` --> 

Este cmdlet ejecuta las siguientes tareas:  

1. Comprueba si se instalan las versiones más recientes de módulos en el perfil determinado de versión de API para el ámbito actual.  
2. Quita las versiones anteriores de módulos con el perfil de versión de API actual y en la sesión actual de PowerShell.  
4. le pide que instale la versión más reciente.  
5. Instala e importa los módulos actualizados en la sesión actual de PowerShell.  
 
## <a name="uninstall-profiles"></a>Desinstalar perfiles

Use el cmdlet **Uninstall-AzureRmProfile** para desinstalar el perfil de versión de API especificado.

```PowerShell 
Uninstall-AzureRmProfile -Profile  2018-03-01-hybrid
```

## <a name="next-steps"></a>Pasos siguientes
* [Install PowerShell for Azure Stack](azure-stack-powershell-install.md) (Instalación de PowerShell para Azure Stack)
* [Configuración del entorno de PowerShell del usuario de Azure Stack](azure-stack-powershell-configure-user.md)  
