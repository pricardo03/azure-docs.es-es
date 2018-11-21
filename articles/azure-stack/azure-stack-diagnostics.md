---
title: Diagnósticos de Azure Stack
description: Cómo recopilar archivos de registro de diagnósticos en Azure Stack
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 11/13/2018
ms.author: jeffgilb
ms.reviewer: adshar
ms.openlocfilehash: f9a7ae76f2d52b3439bfb33f306e164bb81549eb
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51623985"
---
# <a name="azure-stack-diagnostics-tools"></a>Herramientas de diagnóstico de Azure Stack

Azure Stack es una gran colección de componentes que funcionan juntos e interactúan entre sí. Todos estos componentes generan sus propios registros únicos. Esto puede dificultar el diagnóstico de problemas, especialmente en el caso de errores procedentes de varios componentes de Azure Stack que interactúan. 

Nuestras herramientas de diagnóstico ayudan a asegurar que el mecanismo de recopilación de registros es fácil y eficaz. En el siguiente diagrama se muestra cómo funcionan las herramientas de recopilación de registros en Azure Stack:

![Herramientas de diagnóstico de Azure Stack](media/azure-stack-diagnostics/get-azslogs.png)
 
 
## <a name="trace-collector"></a>Recopilador de seguimiento
 
El recopilador de seguimiento está habilitado de forma predeterminada y se ejecuta continuamente en segundo plano para recopilar el todos los registros de Seguimiento de eventos para Windows (ETW) de los servicios de componentes de Azure Stack. Los registros de ETW se almacenan en un recurso compartido local común con un límite de antigüedad de cinco días. Una vez que se alcanza este límite, se eliminan los archivos más antiguos cuando se crean nuevos. El tamaño máximo predeterminado permitido para cada archivo es de 200 MB. Se realiza una comprobación de tamaño cada dos minutos y si el archivo actual es > = 200 MB, se guarda y se genera otro. También hay un límite de 8 GB en el tamaño total del archivo generado por cada sesión de eventos. 

## <a name="log-collection-tool"></a>Herramienta de recopilación de registros
 
El cmdlet de PowerShell **Get-AzureStackLog** se puede utilizar para recopilar registros de todos los componentes en un entorno de Azure Stack. Los guarda en archivos ZIP en una ubicación definida por el usuario. Si el equipo de soporte técnico de Azure Stack necesita los registros para ayudarle a solucionar un problema, puede que le pidan que ejecute esta herramienta.

> [!CAUTION]
> Estos archivos de registro pueden contener información de identificación personal (PII). Tenga esto en cuenta antes de publicar los archivos de registro.
 
Estos son algunos ejemplos de tipos de registro que se recopilan:
*   **Registros de implementación de Azure Stack**
*   **Registros de eventos de Windows**
*   **Registros de Panther**
*   **Registros de clúster**
*   **Registros de diagnóstico de almacenamiento**
*   **Registros de ETW**

El Recopilador de seguimiento recopila estos archivos y los guarda en un recurso compartido. El cmdlet de PowerShell **Get-AzureStackLog** se puede utilizar después para recopilarlos cuando sea necesario.

### <a name="to-run-get-azurestacklog-on-azure-stack-integrated-systems"></a>Para ejecutar Get-AzureStackLog en un sistema Azure Stack integrado 
Para ejecutar la herramienta de recopilación de registros en un sistema integrado, debe tener acceso al punto de conexión con privilegios (PEP). Este es un script de ejemplo que se puede ejecutar mediante el PEP para recopilar registros en un sistema integrado:

```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
 
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
 
$shareCred = Get-Credential
 
$s = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2)  #provide the time that includes the period for your issue
 
Invoke-Command -Session $s {    Get-AzureStackLog -OutputSharePath "<EXTERNAL SHARE ADDRESS>" -OutputShareCredential $using:shareCred  -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate}

if($s)
{
    Remove-PSSession $s
}
```
- Los parámetros **OutputSharePath** y **OutputShareCredential** se utilizan para almacenar registros en una ubicación especificada por el usuario.
- Los parámetros **FromDate** y **ToDate** pueden utilizarse para recopilar registros durante un período de tiempo determinado. Si no se especifican los parámetros, se recopilan los registros de las últimas cuatro horas de forma predeterminada.


### <a name="to-run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system"></a>Para ejecutar Get-AzureStackLog en un sistema con el Kit de desarrollo de Azure Stack (ASDK)
1. Inicie sesión como **AzureStack\CloudAdmin** en el host.
2. Abra una ventana de PowerShell como administrador.
3. Ejecute el cmdlet de PowerShell en **AzureStackLog Get**.

**Ejemplos:**

  Recopilar todos los registros de todos los roles:

  ```powershell
  Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred
  ```

  Recopilar registros de los roles VirtualMachines y BareMetal:

  ```powershell
  Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred -FilterByRole VirtualMachines,BareMetal
  ```

  Recopilar registros de los roles VirtualMachines y BareMetal, filtrando por fecha los archivos de registro de las últimas 8 horas:
    
  ```powershell
  Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)
  ```

  Recopilar registros de los roles VirtualMachines y BareMetal, filtrando por fecha los archivos de registro del período comprendido entre las últimas entre 8 y 2 horas:

  ```powershell
  Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)
  ```

### <a name="parameter-considerations-for-both-asdk-and-integrated-systems"></a>Consideraciones sobre los parámetros para ASDK y sistemas integrados

- Si no se especifican los parámetros **FromDate** y **ToDate**, se recopilan los registros de las últimas cuatro horas de forma predeterminada.
- Use el parámetro **FilterByNode** para filtrar los registros por nombre de equipo. Por ejemplo: 

    ```powershell
    Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred -FilterByNode azs-xrp01
    ```
- Use el parámetro **FilterByLogType** para filtrar los registros por tipo. Puede elegir filtrar por archivo, por recurso compartido o por WindowsEvent. Por ejemplo: 

    ```powershell
    Get-AzureStackLog -OutputSharePath “<path>” -OutputShareCredential $cred -FilterByLogType File
    ```
- Puede usar el parámetro **TimeOutInMinutes** para establecer el tiempo de espera para la colección de registros. Se establece en 150 (2,5 horas) de forma predeterminada.
- La recopilación de registros de archivos de copia de seguridad está deshabilitada de forma predeterminada. Para habilitarla, utilice el parámetro de modificador **IncludeDumpFile**. 
- Actualmente, no se puede usar el parámetro **FilterByRole** para filtrar la colección de registros de los roles siguientes:

 |   |   |   |    |
 | - | - | - | -  |   
 |ACS                   |CacheService                   |IBC                            |OEM|
 |ACSDownloadService    |Compute                        |InfraServiceController         |OnboardRP|
 |ACSFabric             |CPI                            |KeyVaultAdminResourceProvider  |PXE|
 |ACSFrontEnd           |CRP                            |KeyVaultControlPlane           |QueryServiceCoordinator|
 |ACSMetrics            |DeploymentMachine              |KeyVaultDataPlane              |QueryServiceWorker|
 |ACSMigrationService   |DiskRP                         |KeyVaultInternalControlPlane   |SeedRing|
 |ACSMonitoringService  |Dominio                         |KeyVaultInternalDataPlane      |SeedRingServices|
 |ACSSettingsService    |ECE                            |KeyVaultNamingService          |SLB|
 |ACSTableMaster        |EventAdminRP                   |MDM                            |SQL|
 |ACSTableServer        |EventRP                        |MetricsAdminRP                 |SRP   |
 |ACSWac                |ExternalDNS                    |MetricsRP                      |Storage|
 |ADFS                  |FabricRing                     |MetricsServer                  |StorageController   |
 |ApplicationController |FabricRingServices             |MetricsStoreService            |URP   |
 |ASAppGateway          |FirstTierAggregationService    |MonAdminRP                     |UsageBridge|
 |AzureBridge           |FRP                            |MonRP                          |VirtualMachines   |
 |AzureMonitor          |Puerta de enlace                        |NC                             |WAS|
 |BareMetal             |HealthMonitoring               |NonPrivilegedAppGateway        |WASPUBLIC|
 |BRP                   |HintingServiceV2               |NRP                            |   |
 |CA                    |HRP                            |OboService                     |   |
 |   |   |   |    |

### <a name="additional-considerations"></a>Consideraciones adicionales

* Este comando tardará un rato en ejecutarse, en función de los roles que recopilen los registros. Entre los factores que contribuyen, se incluye la duración especificada para la colección de registros y el número de nodos en el entorno de Azure Stack.
* Cuando se ejecuta la recopilación de registros, compruebe la nueva carpeta creada en el parámetro **OutputSharePath** especificado en el comando.
* Cada rol tiene sus registros dentro de archivos ZIP individuales. Según el tamaño de los registros recopilados, un rol puede dividir sus registros en varios archivos ZIP. Para tal rol, si desea que todos los archivos de registro se descompriman en una sola carpeta, use una herramienta que pueda descomprimir de forma masiva (por ejemplo, 7zip). Seleccione todos los archivos comprimidos para el rol y seleccione **extraer aquí**. Así se descomprimen todos los archivos de registro para ese rol en una sola carpeta combinada.
* También se crea un archivo denominado **Get-AzureStackLog_Output.log** en la carpeta que contiene los archivos de registro comprimidos. Este archivo es un registro de la salida del comando, que se puede usar para solucionar problemas durante la recopilación de registros. En ocasiones, el archivo de registro incluye entradas `PS>TerminatingError` que se pueden omitir sin problema, a menos que falten los archivos de registro esperados después de ejecutar la recopilación de registros.
* Para investigar un error específico, es posible que sean necesarios registros de más de un componente.
    -   Los registros de eventos y del sistema de todas las máquinas virtuales de la infraestructura se recopilan en el rol *VirtualMachines*.
    -   Los registros de eventos y del sistema de todos los hosts se recopilan en el rol *Storage*.
    -   Los registros de eventos de Hyper-V y del clúster de conmutación por error se recopilan en el rol *Storage*.
    -   Los registros de ACS se recopilan en los roles *Storage* y *ACS*.

> [!NOTE]
> Se aplican límites en el tamaño y la antigüedad para los registros recopilados, ya que resulta esencial para garantizar un uso eficaz del espacio de almacenamiento y para asegurarse de que este no recibe demasiados registros. Sin embargo, cuando se diagnostica un problema, a veces se necesitan registros que podrían no existir ya debido a estos límites. Por lo tanto, se **recomienda encarecidamente** descargar los registros en un espacio de almacenamiento externo (una cuenta de almacenamiento de Azure, un dispositivo de almacenamiento local adicional, etc.) cada entre 8 a 12 horas y conservarlos allí de 1 a 3 meses, en función de los requisitos. Asegúrese también de que la ubicación de almacenamiento está cifrada.

## <a name="next-steps"></a>Pasos siguientes
[Solución de problemas de Microsoft Azure Stack](azure-stack-troubleshooting.md)

