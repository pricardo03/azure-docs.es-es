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
ms.date: 08/10/2018
ms.author: sethm
ms.reviewer: thoroet
ms.openlocfilehash: 2619f959dbefba84ea1a4d5aa974055998b78b5a
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "41946437"
---
# <a name="install-powershell-for-azure-stack"></a>Instalación de PowerShell para Azure Stack

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Deberá instalar los módulos de PowerShell compatibles de Azure Stack para trabajar con la nube. La compatibilidad se habilita mediante una característica denominada *perfiles de API*.

Los perfiles de API proporcionan una manera de administrar las diferencias de versión entre Azure y Azure Stack. Un perfil de versión de la API es un conjunto de módulos de PowerShell de Azure Resource Manager con versiones específicas de la API. Cada plataforma de la nube tiene un conjunto de perfiles de versión de API compatibles. Por ejemplo, Azure Stack es compatible con una versión de perfil con fecha específica **2017-03-09-perfil**, y Azure es compatible con la versión **más reciente** del perfil de la API. Cuando se instala un perfil, se instalan los módulos de PowerShell Azure Resource Manager que se corresponden con el perfil especificado.

Puede instalar los módulos de PowerShell compatibles con Azure Stack en un escenario desconectado, parcialmente conectado o conectado a Internet. En este artículo, le guiaremos en las instrucciones detalladas para instalar PowerShell para Azure Stack para estos escenarios.

## <a name="1-verify-your-prerequisites"></a>1. Comprobación de los requisitos previos

Antes de comenzar con Azure Stack y PowerShell, necesita cumplir unos requisitos.

- **PowerShell versión 5.0**  
Para comprobar la versión, ejecute $PSVersionTable.PSVersion y compare la versión **principal**. Si no tiene PowerShell 5.0, siga el [vínculo](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell) para actualizar a PowerShell 5.0.

  > [!Note]  
  > PowerShell 5.0 requiere una máquina Windows.

- **Ejecute Powershell con un símbolo del sistema con privilegios elevados**  
  Deberá ser capaz de ejecutar PowerShell con privilegios administrativos.

- **Acceso a la Galería de PowerShell**  
  Necesitará acceso a la [Galería de PowerShell](https://www.powershellgallery.com). La Galería es el repositorio central para el contenido de PowerShell. El módulo **PowerShellGet** contiene cmdlets para detectar, instalar, actualizar y publicar los artefactos de PowerShell como los módulos, recursos DSC, funcionalidades de rol y scripts desde la Galería de PowerShell y otros repositorios privados. Si usa PowerShell en un escenario desconectado, deberá recuperar los recursos desde una maquina con una conexión a Internet y almacenarlos en una ubicación accesible para la máquina desconectada.


<!-- Nuget? -->

## <a name="2-validate-if-the-powershell-gallery-is-accessible"></a>2. Confirmación de que la Galería de PowerShell es accesible

Confirme si PSGallery está registrado como repositorio.

> [!Note]  
> Este paso requiere acceso a Internet. 

Abra un símbolo de sistema de PowerShell con privilegios elevados y ejecute los siguientes cmdlet:

````PowerShell  
Import-Module -Name PowerShellGet -ErrorAction Stop
Import-Module -Name PackageManagement -ErrorAction Stop
Get-PSRepository -Name "PSGallery"
````

Si el repositorio no está registrado, abra una sesión de PowerShell con privilegios elevados y ejecute el siguiente comando:

```PowerShell
Register-PsRepository -Default
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

## <a name="3-uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>3. Desinstalar las versiones existentes de los módulos de PowerShell de Azure Stack

Antes de instalar la versión requerida, asegúrese de desinstalar los módulos de PowerShell de Azure Stack AzureRM instalados previamente. Puede desinstalarlos mediante uno de los dos métodos siguientes:

1. Para desinstalar los módulos de PowerShell de AzureRM existentes, cierre todas las sesiones de PowerShell activas y ejecute los siguientes cmdlets:

  ````PowerShell  
    Uninstall-Module AzureRM.AzureStackAdmin -Force
    Uninstall-Module AzureRM.AzureStackStorage -Force
    Uninstall-Module -Name AzureStack -Force
  ````

2. Elimine todas las carpetas que empiecen con `Azure` de las carpetas `C:\Program Files\WindowsPowerShell\Modules` y `C:\Users\AzureStackAdmin\Documents\WindowsPowerShell\Modules`. Al eliminar estas carpetas se quitan todos los módulos de PowerShell existentes.

## <a name="4-connected-install-powershell-for-azure-stack-with-internet-connectivity"></a>4. Implementación conectada: Instalación de PowerShell para Azure Stack con conectividad a Internet

Azure Stack requiere el perfil de la versión de API **2017-03-09-profile**, que está disponible al instalar el módulo **AzureRM.Bootstrapper**. Además de los módulos AzureRM, también debe instalar los módulos de PowerShell específicos de Azure Stack. 

Ejecute el siguiente script de PowerShell para instalar estos módulos en la estación de trabajo de desarrollo:

  - **Versión 1.4.0** (Azure Stack 1804 o posterior)

    ```PowerShell  
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet 
    Install-Module -Name AzureRm.BootStrapper 

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session. 
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force 

    # Install Module Version 1.4.0 if Azure Stack is running 1804 at a minimum 
    Install-Module -Name AzureStack -RequiredVersion 1.4.0
    ```

- **Versión 1.2.11** (anterior a 1804)

    ```PowerShell  
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet 
    Install-Module -Name AzureRm.BootStrapper 

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session. 
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force 

    # Install Module Version 1.2.11 if Azure Stack is running a lower version than 1804 
    Install-Module -Name AzureStack -RequiredVersion 1.2.11 
    ```

Ejecute el comando siguiente para comprobar la instalación:

```PowerShell  
Get-Module -ListAvailable | where-Object {$_.Name -like "Azs*"}
```

Si la instalación es correcta, los módulos AzureRM y AzureStack se muestran en la salida.

## <a name="5-disconnected-install-powershell-without-an-internet-connection"></a>5. Implementación desconectada: Instalación de PowerShell sin una conexión a Internet

En un escenario desconectado, primero debe descargar los módulos de PowerShell en un equipo que tenga conectividad a Internet y luego transferirlos al Kit de desarrollo de Azure Stack para la instalación.

Inicie sesión en un equipo que tenga conexión a Internet y use los scripts siguientes para descargar los paquetes de Azure Resource Manager y AzureStack en el equipo local, según su versión de Azure Stack.


  - **Versión 1.3.0** (Azure Stack 1804 o posterior)
  
    > [!Note]  
    Para actualizar desde la versión 1.2.11, consulte la [guía de migración](https://aka.ms/azspowershellmigration).

    ````PowerShell  
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

      $Path = "<Path that is used to save the packages>"
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.4.0
    ````

  - **Versión 1.2.11** (anterior a 1804)

    ````PowerShell  
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

      $Path = "<Path that is used to save the packages>"

      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 1.2.11
    ````

2. Copie los paquetes descargados a través en un dispositivo USB.

3. Inicie sesión en la estación de trabajo y copie los paquetes desde el dispositivo USB en una ubicación en dicha estación de trabajo.

4. Ahora debe registrar esta ubicación como el repositorio predeterminado e instalar los módulos AzureRM y AzureStack desde este repositorio:

   ```PowerShell
   #requires -Version 5
   #requires -RunAsAdministrator
   #requires -Module PowerShellGet
   #requires -Module PackageManagement

   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation  -InstallationPolicy Trusted

   Install-Module AzureRM -Repository $RepoName

   Install-Module AzureStack -Repository $RepoName 
   ```

## <a name="6-configure-powershell-to-use-a-proxy-server"></a>6. Configuración de PowerShell para usar un servidor proxy

En escenarios que requieren un servidor proxy para acceder a Internet, primero debe configurar PowerShell para usar un servidor proxy existente.

1. Abra un símbolo del sistema de PowerShell con privilegios elevados.
2. Ejecute los comandos siguientes:

````PowerShell  
  #To use Windows credentials for proxy authentication
  [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

  #Alternatively, to prompt for separate credentials that can be used for #proxy authentication

  [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
````

## <a name="next-steps"></a>Pasos siguientes

 - [Descarga de herramientas de Azure Stack desde GitHub](azure-stack-powershell-download.md)
 - [Configuración del entorno de PowerShell del usuario de Azure Stack](user/azure-stack-powershell-configure-user.md)  
 - [Configuración del entorno de PowerShell del operador de Azure Stack](azure-stack-powershell-configure-admin.md) 
 - [Administración de perfiles de la versión de API en Azure Stack](user/azure-stack-version-profiles.md)  
