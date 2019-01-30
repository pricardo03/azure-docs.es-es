---
title: Instalación de PowerShell para Azure Stack | Microsoft Docs
description: Aprenda a instalar PowerShell para Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 01/17/2018
ms.author: sethm
ms.reviewer: thoroet
ms.openlocfilehash: 9c99de88ee1e3054a04512c72b9f9f41886663da
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54391076"
---
# <a name="install-powershell-for-azure-stack"></a>Instalación de PowerShell para Azure Stack

*Se aplica a: Sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Para trabajar con la nube, debe instalar los módulos de PowerShell compatibles con Azure Stack. La compatibilidad se habilita mediante una característica denominada *perfiles de API*.

Los perfiles de API proporcionan una manera de administrar las diferencias de versión entre Azure y Azure Stack. Un perfil de versión de la API es un conjunto de módulos de PowerShell de Azure Resource Manager con versiones específicas de la API. Cada plataforma de la nube tiene un conjunto de perfiles de versión de API compatibles. Por ejemplo, Azure Stack admite una versión de perfil con fecha específica, como **2018-03-01-hybrid**. Cuando se instala un perfil, se instalan los módulos de PowerShell Azure Resource Manager que se corresponden con el perfil especificado.

Puede instalar los módulos de PowerShell compatibles con Azure Stack en un escenario desconectado, parcialmente conectado o conectado a Internet. Este artículo le indica los pasos detallados para instalar PowerShell para Azure Stack para estos escenarios.

## <a name="1-verify-your-prerequisites"></a>1. Comprobación de los requisitos previos

Antes de empezar con Azure Stack y PowerShell, debe cumplir estos requisitos previos:

- **PowerShell versión 5.0** Para comprobar la versión, ejecute **$PSVersionTable.PSVersion** y compare la versión **principal**. Si no tiene PowerShell 5.0, siga las instrucciones de [Actualización de Windows PowerShell existente](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).

  > [!Note]
  > PowerShell 5.0 requiere una máquina Windows.

- **Ejecute Powershell en un símbolo del sistema con privilegios elevados**.
  Debe ejecutar PowerShell con privilegios administrativos.

- **Acceso a la Galería de PowerShell** Necesita tener acceso a la [Galería de PowerShell](https://www.powershellgallery.com). La Galería es el repositorio central para el contenido de PowerShell. El módulo **PowerShellGet** contiene cmdlets para detectar, instalar, actualizar y publicar los artefactos de PowerShell como los módulos, recursos DSC, funcionalidades de rol y scripts desde la Galería de PowerShell y otros repositorios privados. Si usa PowerShell en un escenario desconectado, debe recuperar los recursos desde una máquina con conexión a Internet y almacenarlos en una ubicación accesible para la máquina desconectada.


## <a name="2-validate-the-powershell-gallery-accessibility"></a>2. Confirmación de la accesibilidad de la Galería de PowerShell

Confirme si PSGallery está registrado como repositorio.

> [!Note]
> Este paso requiere acceso a Internet.

Abra un símbolo de sistema de PowerShell con privilegios elevados y ejecute los siguientes cmdlet:

```PowerShell
Import-Module -Name PowerShellGet -ErrorAction Stop
Import-Module -Name PackageManagement -ErrorAction Stop
Get-PSRepository -Name "PSGallery"
```

Si el repositorio no está registrado, abra una sesión de PowerShell con privilegios elevados y ejecute el siguiente comando:

```PowerShell
Register-PsRepository -Default
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

## <a name="3-uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>3. Desinstalar las versiones existentes de los módulos de PowerShell de Azure Stack

Antes de instalar la versión requerida, asegúrese de desinstalar los módulos de PowerShell de Azure Stack AzureRM instalados previamente. Puede desinstalarlos mediante uno de los dos métodos siguientes:

1. Para desinstalar los módulos de PowerShell de AzureRM existentes, cierre todas las sesiones de PowerShell activas y ejecute los siguientes cmdlets:

    ```PowerShell
    Get-Module -Name Azs.* -ListAvailable | Uninstall-Module -Force -Verbose
    Get-Module -Name Azure* -ListAvailable | Uninstall-Module -Force -Verbose
    ```
    Si se produce algún error, como "el módulo ya se está utilizando", cierre las sesiones de PowerShell que usan los módulos y vuelva a ejecutar el script anterior.

2. Elimine todas las carpetas que empiecen con `Azure` o `Azs.` de las carpetas `C:\Program Files\WindowsPowerShell\Modules` y `C:\Users\{yourusername}\Documents\WindowsPowerShell\Modules`. Al eliminar estas carpetas se quitan todos los módulos de PowerShell existentes.

## <a name="4-connected-install-powershell-for-azure-stack-with-internet-connectivity"></a>4. Escenario conectado: Instalación de PowerShell para Azure Stack con conectividad a Internet

Azure Stack necesita el perfil de versión de API **2018-03-01-hybrid** para Azure Stack versión 1808, o versiones posteriores. El perfil está disponible al instalar el módulo **AzureRM.Bootstrapper**. Además de los módulos AzureRM, también debe instalar los módulos de PowerShell específicos de Azure Stack. El perfil de la versión de API y los módulos de PowerShell de Azure Stack que necesite dependerán de la versión de Azure Stack que esté ejecutando.

Ejecute el siguiente script de PowerShell para instalar estos módulos en la estación de trabajo de desarrollo:

- Azure Stack 1811 o una versión posterior.

    ```PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    Install-Module -Name AzureStack -RequiredVersion 1.6.0
    ```

    Para asegurarse de usar las características adicionales de almacenamiento (mencionadas en la sección del escenario conectado), descargue e instale también los siguientes paquetes.

    ```PowerShell
    # Install the Azure.Storage module version 4.5.0
    Install-Module -Name Azure.Storage -RequiredVersion 4.5.0 -Force -AllowClobber

    # Install the AzureRm.Storage module version 5.0.4
    Install-Module -Name AzureRM.Storage -RequiredVersion 5.0.4 -Force -AllowClobber

    # Remove incompatible storage module installed by AzureRM.Storage
    Uninstall-Module Azure.Storage -RequiredVersion 4.6.1 -Force

    # Load the modules explicitly specifying the versions
    Import-Module -Name Azure.Storage -RequiredVersion 4.5.0
    Import-Module -Name AzureRM.Storage -RequiredVersion 5.0.4
    ```

> [!Note]
> Para actualizar Azure PowerShell de **2017-03-09-profile** a **2018-03-01-hybrid**, vea [Guía de migración](https://github.com/azure/azure-powershell/blob/AzureRM/documentation/migration-guides/Stack/migration-guide.2.3.0.md).

- Azure Stack 1811 o una versión posterior.

    ```PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    Install-Module -Name AzureStack -RequiredVersion 1.6.0
    ```

- Azure Stack 1809 o una versión anterior.

    ```PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    Install-Module -Name AzureStack -RequiredVersion 1.5.0
    ```

Ejecute el comando siguiente para comprobar la instalación:

```PowerShell
Get-Module -Name "Azure*" -ListAvailable
Get-Module -Name "Azs*" -ListAvailable
```

Si la instalación es correcta, los módulos AzureRM y AzureStack se muestran en la salida.

## <a name="5-disconnected-install-powershell-without-an-internet-connection"></a>5. Escenario desconectado: Instalación de PowerShell sin una conexión a Internet

En un escenario desconectado, primero debe descargar los módulos de PowerShell en un equipo que tenga conectividad a Internet y luego transferirlos al Kit de desarrollo de Azure Stack para la instalación.

Inicie sesión en un equipo con conexión a Internet y use estos scripts para descargar los paquetes de Azure Resource Manager y Azure Stack, según su versión de Azure Stack:

  - Azure Stack 1811 o una versión posterior.

    ```PowerShell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.6.0
    ```

    Para asegurarse de usar las características adicionales de almacenamiento (mencionadas en la sección del escenario conectado), descargue e instale también los siguientes paquetes.

    ```PowerShell
    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name Azure.Storage -Path $Path -Force -RequiredVersion 4.5.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRm.Storage -Path $Path -Force -RequiredVersion 5.0.4
    ```

  - Azure Stack 1811 o una versión posterior.

    ```PowerShell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.6.0
    ```

  - Azure Stack 1809 o una versión anterior.

    ```PowerShell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
    ```

2. Copie los paquetes descargados en un dispositivo USB.

3. Inicie sesión en la estación de trabajo y copie los paquetes desde el dispositivo USB en una ubicación en dicha estación de trabajo.

4. Ahora registre esta ubicación como el repositorio predeterminado e instale los módulos AzureRM y AzureStack desde este repositorio:

   ```PowerShell
   #requires -Version 5
   #requires -RunAsAdministrator
   #requires -Module PowerShellGet
   #requires -Module PackageManagement

   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation  -InstallationPolicy Trusted

   Install-Module -Name AzureRM -Repository $RepoName

   Install-Module -Name AzureStack -Repository $RepoName
   ```

## <a name="6-configure-powershell-to-use-a-proxy-server"></a>6. Configuración de PowerShell para usar un servidor proxy

En escenarios donde se necesita un servidor proxy para acceder a Internet, primero debe configurar PowerShell para que use un servidor proxy existente:

1. Abra un símbolo del sistema de PowerShell con privilegios elevados.
2. Ejecute los comandos siguientes:

   ```PowerShell
   #To use Windows credentials for proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

   #Alternatively, to prompt for separate credentials that can be used for #proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
   ```

## <a name="next-steps"></a>Pasos siguientes

 - [Descarga de herramientas de Azure Stack desde GitHub](azure-stack-powershell-download.md)
 - [Configuración del entorno de PowerShell del usuario de Azure Stack](user/azure-stack-powershell-configure-user.md)
 - [Configuración del entorno de PowerShell del operador de Azure Stack](azure-stack-powershell-configure-admin.md)
 - [Administración de perfiles de la versión de API en Azure Stack](user/azure-stack-version-profiles.md)