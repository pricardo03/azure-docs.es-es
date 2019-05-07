---
title: Instrucciones detalladas de Azure v2 de Monitor de estado | Microsoft Docs
description: Instrucciones detalladas para Introducción al Monitor de estado v2. Supervisar el rendimiento del sitio Web sin volver a implementar el sitio Web. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: 3aca64c7b0f1ad04967782cb3349da302db557a0
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65145086"
---
# <a name="status-monitor-v2-detailed-instructions"></a>Instrucciones detalladas de v2 del Monitor de estado

Este documento detalles cómo incorporar a la Galería de PowerShell y la descarga del módulo ApplicationMonitor. Hemos documentado los parámetros más comunes necesarios para empezar a trabajar.
También hemos incluido instrucciones manuales en caso de que no está disponible el acceso a internet.

> [!IMPORTANT]
> V2 de Monitor de estado está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para obtener más información, consulte [términos de uso complementarios para vistas previas de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="instrumentation-key"></a>Clave de instrumentación

Para empezar, debe tener una clave de instrumentación. Para obtener más información, lea [crear un recurso de Application Insights](create-new-resource.md#copy-the-instrumentation-key).

## <a name="run-powershell-as-administrator-with-an-elevated-execution-policy"></a>Ejecute PowerShell como administrador con una directiva de ejecución con privilegios elevados

**Ejecutar como administrador**: 
- Description: PowerShell necesitará permisos de nivel de administrador para realizar cambios en el equipo.

**La directiva de ejecución**:
- Description: De forma predeterminada, ejecutar scripts de PowerShell se deshabilitará. Se recomienda permitir que las secuencias de comandos RemoteSigned para solo en el ámbito actual.
- Referencia: [Acerca de las directivas de ejecución](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6) y [Set-ExecutionPolicy.](
https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6
)
- Cmd: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process`
- Parámetros opcionales:
    - `-Force` Esto lo omitirá la solicitud de confirmación.

**Ejemplos de errores:**

```
Install-Module : The 'Install-Module' command was found in the module 'PowerShellGet', but the module could not be
loaded. For more information, run 'Import-Module PowerShellGet'.
    
Import-Module : File C:\Program Files\WindowsPowerShell\Modules\PackageManagement\1.3.1\PackageManagement.psm1 cannot
be loaded because running scripts is disabled on this system. For more information, see about_Execution_Policies at
https:/go.microsoft.com/fwlink/?LinkID=135170.
```


## <a name="prerequisites-for-powershell"></a>Requisitos previos para PowerShell

Auditar su versión actual de PowerShell, ejecute el comando: `$PSVersionTable`.
El comando genera el siguiente resultado:


```
Name                           Value
----                           -----
PSVersion                      5.1.17763.316
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.17763.316
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

Estas instrucciones se han escrito y probado en un equipo Windows 10 con las versiones anteriores.

## <a name="prerequisites-for-powershell-gallery"></a>Requisitos previos para la Galería de PowerShell

Estos pasos se preparará el servidor para descargar los módulos desde la Galería de PowerShell.

> [!NOTE] 
> Compatibilidad con la Galería de PowerShell se incluye en Windows 10, Windows Server 2016 y PowerShell 6. Las versiones anteriores, revise este documento: [Instalación de PowerShellGet](https://docs.microsoft.com/powershell/gallery/installing-psget)


1. Con una directiva de ejecución con privilegios elevados, ejecute PowerShell como administrador.
2. Proveedor de paquete de NuGet 
    - Description: Este proveedor se necesita para interactuar con repositorios basados en NuGet, como la Galería de PowerShell
    - Referencia: [Install-PackageProvider](https://docs.microsoft.com/powershell/module/packagemanagement/install-packageprovider?view=powershell-6)
    - Cmd: `Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201`
    - Parámetros opcionales:
        - `-Proxy` Especifica un servidor proxy para la solicitud.
        - `-Force` Esto lo omitirá la solicitud de confirmación. 
    
    Recibirá este mensaje si NuGet no se ha configurado:
        
        NuGet provider is required to continue
        PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
         provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
        'C:\Users\t\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
        'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import
         the NuGet provider now?
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    
3. Repositorios de confianza
    - Description: De forma predeterminada, la Galería de PowerShell es un repositorio no confiable.
    - Referencia: [Set-PSRepository](https://docs.microsoft.com/powershell/module/powershellget/set-psrepository?view=powershell-6)
    - Cmd: `Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted`
    - Parámetros opcionales:
        - `-Proxy` Especifica un servidor proxy para la solicitud.

    Recibirá este mensaje si la Galería de PowerShell no es de confianza:

        Untrusted repository
        You are installing the modules from an untrusted repository. If you trust this repository, change its
        InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to install the modules from
        'PSGallery'?
        [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):

    - Puede confirmar este cambio y auditar todos los PSRepositories ejecutando el comando: `Get-PSRepository`

4. Versión de PowerShellGet 
    - Description: Este módulo contiene las herramientas que se usa para obtener otros módulos de galería de PowerShell. V1.0.0.1 se incluye con Windows 10 y Windows Server. Versión mínima requerida es v1.6.0, siempre. Para auditar la versión instalada, ejecute el comando: `Get-Command -Module PowerShellGet`
    - Referencia: [Instalación de PowerShellGet](https://docs.microsoft.com/powershell/gallery/installing-psget)
    - Cmd: `Install-Module -Name PowerShellGet`
    - Parámetros opcionales:
        - `-Proxy` Especifica un servidor proxy para la solicitud.
        - `-Force` Esto se ignore la advertencia "ya está instalado" e instalará la versión más reciente.

    Si no usa la versión más reciente de PowerShellGet, recibirá este error:
    
        Install-Module : A parameter cannot be found that matches parameter name 'AllowPrerelease'.
        At line:1 char:20
        Install-Module abc -AllowPrerelease
                           ~~~~~~~~~~~~~~~~
            CategoryInfo          : InvalidArgument: (:) [Install-Module], ParameterBindingException
            FullyQualifiedErrorId : NamedParameterNotFound,Install-Module
    
5. Reiniciar PowerShell. No se puede cargar la nueva versión de la sesión actual. Las sesiones de Powershell nuevo tendrá el módulo PowerShellGet más reciente que se cargan.

## <a name="download--install-via-powershell-gallery"></a>Descargar e instalar a través de la Galería de PowerShell

Estos pasos descargarán el módulo Az.ApplicationMonitor desde la Galería de PowerShell.

1. Requisitos previos para la Galería de PowerShell son necesarios.
2. Con una directiva de ejecución con privilegios elevados, ejecute PowerShell como administrador.
3. Instalar el módulo Az.ApplicationMonitor
    - Referencia: [Install-Module](https://docs.microsoft.com/powershell/module/powershellget/install-module?view=powershell-6)
    - Cmd: `Install-Module -Name Az.ApplicationMonitor`
    - Parámetros opcionales:
        - `-Proxy` Especifica un servidor proxy para la solicitud.
        - `-AllowPrerelease` Esto le permitirá instalar versiones alfa y beta.
        - `-AcceptLicense` Esto omitirá el símbolo del sistema de "Acepte la licencia"
        - `-Force` Esto pasará por alto la advertencia "Repositorio de confianza"

## <a name="download--install-manually-offline-option"></a>Descargar e instalar manualmente (opción sin conexión)

Si por algún motivo no puede conectarse al módulo de PowerShell, puede descargar manualmente e instalar el módulo Az.ApplicationMonitor.

### <a name="manually-download-the-latest-nupkg"></a>Descargar manualmente la última nupkg

1. Desplácese hasta: https://www.powershellgallery.com/packages/Az.ApplicationMonitor
2. Seleccione la versión más reciente del historial de versiones.
3. Busque "Opciones de instalación" y seleccione "Descargar Manual".

### <a name="option-1-install-into-powershell-modules-directory"></a>Opción 1: instalar en el directorio de módulos de PowerShell
Instale el módulo de PowerShell descargado manualmente en un directorio de PowerShell para que pueda ser reconocible en las sesiones de PowerShell.
Para más información, consulte: [Instalación de un módulo de PowerShell](https://docs.microsoft.com/powershell/developer/module/installing-a-powershell-module)


#### <a name="unzip-nupkg-as-zip-using-expand-archive-v1010"></a>Descomprima nupkg como zip mediante Expand-Archive (v1.0.1.0)

- Description: La versión base de Microsoft.PowerShell.Archive (v1.0.1.0) no puede descomprimir los archivos nupkg. Cambie el nombre del archivo con la extensión "zip".
- Referencia: [Expand-Archive](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6)
- Cmd: 

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.2.1-alpha.nupkg"
    $pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
    $pathToNupkg | rename-item -newname $pathToZip
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
    ```

#### <a name="unzip-nupkg-using-expand-archive-v1100"></a>Descomprima nupkg mediante Expand-Archive (v1.1.0.0).

- Description: Utilice una versión actual de Expand-Archive para descomprimir la disponibilidad de nupkgs sin cambiar el nombre de la extensión. 
- Referencia: [Expanda-Archive](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6) y [Microsoft.PowerShell.Archive](https://www.powershellgallery.com/packages/Microsoft.PowerShell.Archive/1.1.0.0)
- Cmd:

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.2.1-alpha.nupkg"
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToNupkg -DestinationPath $pathInstalledModule
    ```

### <a name="option-2-unzip-and-import-manually"></a>Opción 2: descomprima e importe manualmente
Instale el módulo de PowerShell descargado manualmente en un directorio de PowerShell para que pueda ser reconocible en las sesiones de PowerShell.
Para más información, consulte: [Instalación de un módulo de PowerShell](https://docs.microsoft.com/powershell/developer/module/installing-a-powershell-module)

Si la instalación en cualquier otro directorio, deberá importar manualmente el módulo mediante [Import-Module](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/import-module?view=powershell-6)

> [!IMPORTANT] 
> La instalación instalará los archivos DLL a través de rutas de acceso relativas. Store el contenido de este paquete en el directorio en tiempo de ejecución previsto y confirme que los permisos de acceso permiten lectura pero no la escritura.

1. Cambie la extensión a "zip" y extraiga el contenido del paquete en el directorio de instalación deseada.
2. Busque la ruta de acceso de archivo a "Az.ApplicationMonitor.psd1".
3. Con una directiva de ejecución con privilegios elevados, ejecute PowerShell como administrador. 
4. Cargar el módulo mediante el comando: `Import-Module Az.ApplicationMonitor.psd1`.
    

## <a name="proxy"></a>Proxy

Al supervisar un equipo en su intranet privada, será necesario enrutar el tráfico http a través de un servidor proxy.

Los comandos de PowerShell para descargar e instalar el Az.ApplicationMonitor desde la Galería de PowerShell admite una `-Proxy` parámetro.
Revise las instrucciones anteriores al escribir los scripts de instalación.

Necesitará el SDK de Application Insights enviar la telemetría de la aplicación a Microsoft. Se recomienda la configuración de proxy para la aplicación en el archivo web.config. Consulte [preguntas más frecuentes de Application Insights: Paso a través del proxy](https://docs.microsoft.com/azure/azure-monitor/app/troubleshoot-faq#proxy-passthrough) para obtener más información.


## <a name="enable-monitoring"></a>Habilitar supervisión 

Cmd: `Enable-ApplicationInsightsMonitoring`

Revise nuestra [referencia de API](status-monitor-v2-api-enable-monitoring.md) para obtener una descripción detallada de cómo usar este cmdlet. 



## <a name="next-steps"></a>Pasos siguientes

 Vea la telemetría:

- [Explore las métricas](../../azure-monitor/app/metrics-explorer.md) para supervisar el rendimiento y uso.
- [Busque eventos y registros](../../azure-monitor/app/diagnostic-search.md) para diagnosticar problemas
- [Análisis](../../azure-monitor/app/analytics.md) para más consultas avanzadas
- [Creación de paneles](../../azure-monitor/app/app-insights-dashboards.md)

 Agregue más telemetría:

- [Cree pruebas web](monitor-web-app-availability.md) para asegurarse de que el sitio permanece activo.
- [Agregue telemetría de cliente web](../../azure-monitor/app/javascript.md) para ver las excepciones de código de la página web y para que le permitan insertar llamadas de seguimiento.
- [Agregue el SDK de Application Insights al código](../../azure-monitor/app/asp-net.md) para que pueda insertar seguimiento y registrar las llamadas

Hacer más con el Monitor de estado v2:

- Use nuestra guía para [solucionar](status-monitor-v2-troubleshoot.md) v2 del Monitor de estado.
