---
title: Instrucciones detalladas de Azure v2 de Monitor de estado | Microsoft Docs
description: Instrucciones detalladas para Introducción al Monitor de estado v2. Supervisar el rendimiento del sitio Web sin volver a implementar el sitio Web. Funciona con las aplicaciones web ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.openlocfilehash: d0960c749d74903acc778c0f21d5c49f380195ae
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734198"
---
# <a name="status-monitor-v2-detailed-instructions"></a>V2 de Monitor de estado: Instrucciones detalladas

Este artículo se describe cómo incorporar a la Galería de PowerShell y la descarga del módulo ApplicationMonitor.
Describe los parámetros más comunes que necesitará para empezar a trabajar.
También incluye instrucciones manuales en caso de que no tiene acceso a internet.

> [!IMPORTANT]
> V2 de Monitor de estado está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no es aconsejable para cargas de trabajo de producción. Podrían no admitir algunas características, y algunas pueden tener funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="get-an-instrumentation-key"></a>Obtener una clave de instrumentación

Para empezar, necesita una clave de instrumentación. Para obtener más información, consulte [crear un recurso de Application Insights](create-new-resource.md#copy-the-instrumentation-key).

## <a name="run-powershell-as-admin-with-an-elevated-execution-policy"></a>Ejecute PowerShell como administrador con una directiva de ejecución con privilegios elevados

**Ejecutar como administrador**

PowerShell necesita permisos de nivel de administrador para realizar cambios en el equipo.

**Directiva de ejecución**
- Description: De forma predeterminada, los scripts de PowerShell en ejecución están deshabilitados. Se recomienda permitir que las secuencias de comandos RemoteSigned para únicamente al ámbito actual.
- Referencia: [Acerca de las directivas de ejecución](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6) y [Set-ExecutionPolicy](
https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6
).
- (Comando): `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process`.
- Parámetro opcional:
    - `-Force`. Se omite el mensaje de confirmación.

**Ejemplos de errores**

```
Install-Module : The 'Install-Module' command was found in the module 'PowerShellGet', but the module could not be
loaded. For more information, run 'Import-Module PowerShellGet'.
    
Import-Module : File C:\Program Files\WindowsPowerShell\Modules\PackageManagement\1.3.1\PackageManagement.psm1 cannot
be loaded because running scripts is disabled on this system. For more information, see about_Execution_Policies at
https:/go.microsoft.com/fwlink/?LinkID=135170.
```


## <a name="prerequisites-for-powershell"></a>Requisitos previos para PowerShell

La instancia de PowerShell de auditoría mediante la ejecución de la `$PSVersionTable` comando.
Este comando genera el siguiente resultado:


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

Estas instrucciones se han escrito y probado en un equipo que ejecuta Windows 10 y las versiones anteriores.

## <a name="prerequisites-for-powershell-gallery"></a>Requisitos previos para la Galería de PowerShell

Estos pasos se preparará el servidor para descargar los módulos de galería de PowerShell.

> [!NOTE] 
> Galería de PowerShell se admite en Windows 10, Windows Server 2016 y PowerShell 6.
> Para obtener información acerca de las versiones anteriores, consulte [instalación PowerShellGet](https://docs.microsoft.com/powershell/gallery/installing-psget).


1. Con una directiva de ejecución con privilegios elevados, ejecute PowerShell como administrador.
2. Instale al proveedor del paquete de NuGet.
    - Description: Necesita este proveedor para interactuar con repositorios basados en NuGet, como la Galería de PowerShell.
    - Referencia: [Install-PackageProvider](https://docs.microsoft.com/powershell/module/packagemanagement/install-packageprovider?view=powershell-6).
    - (Comando): `Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201`.
    - Parámetros opcionales:
        - `-Proxy`. Especifica un servidor proxy para la solicitud.
        - `-Force`. Se omite el mensaje de confirmación.
    
    Recibirá este mensaje si NuGet no se ha configurado:
        
        NuGet provider is required to continue
        PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
         provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
        'C:\Users\t\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
        'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import
         the NuGet provider now?
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    
3. Configure la Galería de PowerShell como un repositorio de confianza.
    - Description: De forma predeterminada, la Galería de PowerShell es un repositorio no confiable.
    - Referencia: [Set-PSRepository](https://docs.microsoft.com/powershell/module/powershellget/set-psrepository?view=powershell-6).
    - (Comando): `Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted`.
    - Parámetro opcional:
        - `-Proxy`. Especifica un servidor proxy para la solicitud.

    Recibirá este mensaje si la Galería de PowerShell no es de confianza:

        Untrusted repository
        You are installing the modules from an untrusted repository. If you trust this repository, change its
        InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to install the modules from
        'PSGallery'?
        [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):

    Puede confirmar este cambio y auditoría PSRepositories todos ejecutando la `Get-PSRepository` comando.

4. Instale la versión más reciente de PowerShellGet.
    - Description: Este módulo contiene las herramientas que se usa para obtener otros módulos de galería de PowerShell. La versión 1.0.0.1 se distribuye con Windows 10 y Windows Server. Versión 1.6.0 se requiere o superior. Para determinar qué versión está instalada, ejecute el `Get-Command -Module PowerShellGet` comando.
    - Referencia: [Instalación de PowerShellGet](https://docs.microsoft.com/powershell/gallery/installing-psget).
    - (Comando): `Install-Module -Name PowerShellGet`.
    - Parámetros opcionales:
        - `-Proxy`. Especifica un servidor proxy para la solicitud.
        - `-Force`. Omite la advertencia "ya está instalado" e instala la versión más reciente.

    Si no usa la versión más reciente de PowerShellGet, recibirá este error:
    
        Install-Module : A parameter cannot be found that matches parameter name 'AllowPrerelease'.
        At line:1 char:20
        Install-Module abc -AllowPrerelease
                           ~~~~~~~~~~~~~~~~
            CategoryInfo          : InvalidArgument: (:) [Install-Module], ParameterBindingException
            FullyQualifiedErrorId : NamedParameterNotFound,Install-Module
    
5. Reiniciar PowerShell. No se puede cargar la nueva versión de la sesión actual. Las nuevas sesiones de PowerShell cargará la versión más reciente de PowerShellGet.

## <a name="download-and-install-the-module-via-powershell-gallery"></a>Descargue e instale el módulo a través de la Galería de PowerShell

Estos pasos descargarán el módulo Az.ApplicationMonitor desde la Galería de PowerShell.

1. Asegúrese de que se cumplen todos los requisitos previos para la Galería de PowerShell.
2. Con una directiva de ejecución con privilegios elevados, ejecute PowerShell como administrador.
3. Instale el módulo Az.ApplicationMonitor.
    - Referencia: [Install-Module](https://docs.microsoft.com/powershell/module/powershellget/install-module?view=powershell-6).
    - (Comando): `Install-Module -Name Az.ApplicationMonitor`.
    - Parámetros opcionales:
        - `-Proxy`. Especifica un servidor proxy para la solicitud.
        - `-AllowPrerelease`. Permite la instalación de las versiones alfa y beta.
        - `-AcceptLicense`. Omite la confirmación "Acepte la licencia"
        - `-Force`. Omite la advertencia "Repositorio de confianza".

## <a name="download-and-install-the-module-manually-offline-option"></a>Descargar e instalar manualmente el módulo (opción sin conexión)

Si por algún motivo que no se puede conectar al módulo de PowerShell, puede descargar manualmente e instalar el módulo Az.ApplicationMonitor.

### <a name="manually-download-the-latest-nupkg-file"></a>Descargar manualmente el archivo nupkg más reciente

1. Vaya a https://www.powershellgallery.com/packages/Az.ApplicationMonitor.
2. Seleccione la versión más reciente del archivo en el **historial de versiones** tabla.
3. En **opciones de instalación**, seleccione **descarga Manual**.

### <a name="option-1-install-into-a-powershell-modules-directory"></a>Opción 1: Instalar en un directorio de módulos de PowerShell
Instale el módulo de PowerShell descargado manualmente en un directorio de PowerShell para que sea reconocible por las sesiones de PowerShell.
Para obtener más información, consulte [instalar un módulo de PowerShell](https://docs.microsoft.com/powershell/developer/module/installing-a-powershell-module).


#### <a name="unzip-nupkg-as-a-zip-file-by-using-expand-archive-v1010"></a>Descomprima nupkg como un archivo zip mediante el uso de Expand-Archive (v1.0.1.0)

- Description: La versión base de Microsoft.PowerShell.Archive (v1.0.1.0) no puede descomprimir los archivos nupkg. Cambie el nombre del archivo con la extensión .zip.
- Referencia: [Expand-Archive](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6).
- Comando:

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.3.0-alpha.nupkg"
    $pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
    $pathToNupkg | rename-item -newname $pathToZip
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
    ```

#### <a name="unzip-nupkg-by-using-expand-archive-v1100"></a>Descomprima nupkg mediante el uso de Expand-Archive (v1.1.0.0)

- Description: Utilice una versión actual de Expand-Archive para descomprimir los archivos nupkg sin cambiar la extensión.
- Referencia: [Expanda-Archive](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6) y [Microsoft.PowerShell.Archive](https://www.powershellgallery.com/packages/Microsoft.PowerShell.Archive/1.1.0.0).
- Comando:

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.2.1-alpha.nupkg"
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToNupkg -DestinationPath $pathInstalledModule
    ```

### <a name="option-2-unzip-and-import-nupkg-manually"></a>Opción 2: Descomprima e importar nupkg manualmente
Instale el módulo de PowerShell descargado manualmente en un directorio de PowerShell para que sea reconocible por las sesiones de PowerShell.
Para obtener más información, consulte [instalar un módulo de PowerShell](https://docs.mircrosoft.com/powershell/developer/module/installing-a-powershell-module).

Si va a instalar el módulo en cualquier otro directorio, importar manualmente el módulo mediante el uso de [Import-Module](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/import-module?view=powershell-6).

> [!IMPORTANT] 
> Se instalará los archivos DLL a través de rutas de acceso relativas.
> Store el contenido del paquete en el directorio en tiempo de ejecución previsto y confirme que los permisos de acceso permiten lectura pero no la escritura.

1. Cambie la extensión a "zip" y extraiga el contenido del paquete en el directorio de instalación deseada.
2. Busque la ruta de acceso de Az.ApplicationMonitor.psd1.
3. Con una directiva de ejecución con privilegios elevados, ejecute PowerShell como administrador.
4. Cargar el módulo mediante el `Import-Module Az.ApplicationMonitor.psd1` comando.
    

## <a name="route-traffic-through-a-proxy"></a>Enrutar el tráfico a través de un servidor proxy

Al supervisar un equipo en su intranet privada, deberá redirigir el tráfico HTTP a través de un servidor proxy.

Los comandos de PowerShell para descargar e instalar Az.ApplicationMonitor desde la Galería de PowerShell admiten una `-Proxy` parámetro.
Al escribir las secuencias de comandos de instalación, revise las instrucciones anteriores.

El SDK de Application Insights tendrá que enviar la telemetría de la aplicación a Microsoft. Se recomienda configurar la configuración de proxy para la aplicación en el archivo web.config. Para obtener más información, consulte [preguntas más frecuentes de Application Insights: Paso a través del proxy](https://docs.microsoft.com/azure/azure-monitor/app/troubleshoot-faq#proxy-passthrough).


## <a name="enable-monitoring"></a>Habilitar supervisión

Use el `Enable-ApplicationInsightsMonitoring` comando para habilitar la supervisión.

Consulte la [referencia de API](status-monitor-v2-api-enable-monitoring.md) para obtener una descripción detallada de cómo usar este cmdlet.



## <a name="next-steps"></a>Pasos siguientes

 Vea la telemetría:

- [Explorar métricas](../../azure-monitor/app/metrics-explorer.md) para supervisar el rendimiento y uso.
- [Busque eventos y registros](../../azure-monitor/app/diagnostic-search.md) para diagnosticar problemas.
- [Uso de Analytics](../../azure-monitor/app/analytics.md) para obtener más consultas avanzadas.
- [Crear paneles](../../azure-monitor/app/overview-dashboard.md).

 Agregue más telemetría:

- [Cree pruebas web](monitor-web-app-availability.md) para asegurarse de que el sitio permanece activo.
- [Agregue telemetría de cliente web](../../azure-monitor/app/javascript.md) para ver las excepciones de código de la página web y para habilitar las llamadas de seguimiento.
- [Agregue el SDK de Application Insights al código](../../azure-monitor/app/asp-net.md) para que pueda insertar seguimiento y registrar las llamadas.

Hacer más con el Monitor de estado v2:

- Use nuestra guía para [solucionar](status-monitor-v2-troubleshoot.md) v2 del Monitor de estado.
