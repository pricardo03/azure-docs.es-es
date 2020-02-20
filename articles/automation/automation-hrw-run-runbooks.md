---
title: Ejecución de runbooks en Azure Automation Hybrid Runbook Worker
description: Este artículo proporciona información acerca de cómo ejecutar runbooks en máquinas de un centro de datos local o en el proveedor de la nube con el rol Hybrid Runbook Worker.
services: automation
ms.subservice: process-automation
ms.date: 01/29/2019
ms.topic: conceptual
ms.openlocfilehash: 29b8a32989b5a1d60792fb5678f7ba8a9f12daba
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2020
ms.locfileid: "77443812"
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Ejecución de runbooks en Hybrid Runbook Worker

No hay ninguna diferencia en la estructura de los runbooks que se ejecutan en Azure Automation y los que se ejecutan en Hybrid Runbook Worker. Los runbooks que usa con cada uno lo más probable es que sean significativamente diferentes. Esta diferencia es porque los runbooks que tienen como destino un Hybrid Runbook Worker normalmente administran recursos en el propio equipo local o recursos en el entorno local donde se implementa. Los runbooks en Azure Automation normalmente administran recursos en la nube de Azure.

Cuando cree runbooks para que se ejecuten en Hybrid Runbook Worker, debe editarlos y probarlos en la máquina que hospeda el rol Hybrid Worker. La máquina host tiene todos los módulos de PowerShell y el acceso a la red que necesita para administrar y acceder a los recursos locales. Una vez que un runbook se prueba en la máquina de Hybrid Worker, puede cargarlo en el entorno de Azure Automation, que está disponible para ejecutarse en Hybrid Worker. Es importante saber que los trabajos se ejecutan con la cuenta del sistema local en Windows o una cuenta de usuario especial `nxautomation` en Linux. En Linux, esto significa que debe asegurarse de que la cuenta `nxautomation` tiene acceso a la ubicación donde almacena sus módulos. Al usar el cmdlet [Install-Module](/powershell/module/powershellget/install-module), especifique **AllUsers** en el parámetro `-Scope` para confirmar que la cuenta `nxautomation` tiene acceso.

Para obtener más información sobre PowerShell en Linux, consulte [Problemas conocidos de PowerShell en plataformas que no son de Windows](https://docs.microsoft.com/powershell/scripting/whats-new/known-issues-ps6?view=powershell-6#known-issues-for-powershell-on-non-windows-platforms).

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a>Inicio de un runbook en Hybrid Runbook Worker

[Inicio de un runbook en Azure Automation](automation-starting-a-runbook.md) describe los distintos métodos para iniciar un runbook. Trabajo híbrido de runbook agrega una opción **Ejecutar en** donde puede especificar el nombre de un grupo de Trabajos híbridos de runbook. Si se especifica un grupo, uno de los roles de trabajo del grupo recupera y ejecuta el runbook. Si no se especifica esta opción, se ejecuta en Azure Automation de la manera normal.

Cuando inicie un runbook en Azure Portal, verá una opción **Ejecutar en** donde puede seleccionar **Azure** o **Hybrid Worker**. Si selecciona **Trabajo híbrido**, puede seleccionar el grupo en una lista desplegable.

Use el parámetro **RunOn**. Puede usar el comando siguiente para iniciar un runbook llamado Test-Runbook en un grupo de Hybrid Runbook Worker denominado MyHybridGroup con Windows PowerShell.

```azurepowershell-interactive
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

> [!NOTE]
> El parámetro **RunOn** se agregó al cmdlet **Start-AzureAutomationRunbook** en la versión 0.9.1 de Microsoft Azure PowerShell. Si tiene instalada una versión anterior, deberá [descargar la versión más reciente](https://azure.microsoft.com/downloads/) . Solo necesita instalar esta versión en una estación de trabajo donde inicia el runbook desde PowerShell. No es necesario que la instale en el equipo de trabajo, a menos que tenga la intención de iniciar runbooks desde ese equipo"

## <a name="runbook-permissions"></a>Permisos de runbooks

Los runbooks que se ejecutan en una instancia de Hybrid Runbook Worker no pueden usar el mismo método que se usa normalmente para los runbooks que se autentican en los recursos de Azure, ya que acceden a recursos que no son de Azure. El runbook puede proporcionar su propia autenticación a los recursos locales o puede configurar la autenticación mediante [identidades administradas para los recursos de Azure](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager). También puede especificar una cuenta de ejecución para proporcionar un contexto de usuario para todos los runbooks.

### <a name="runbook-authentication"></a>Autenticación de runbook

De forma predeterminada, los runbooks se ejecutan en el contexto de la cuenta del sistema local en Windows y de una cuenta de usuario especial, `nxautomation`, en Linux en el equipo local, por lo que deben proporcionar su propia autenticación a los recursos a los que acceden.

Puede usar los recursos [redencial](automation-credentials.md) y [ertificado](automation-certificates.md) en el Runbook con cmdlets que le permitan especificar las credenciales, para que así pueda autenticarse en distintos recursos. El ejemplo siguiente muestra una porción de un runbook que reinicia un equipo. Recupera las credenciales desde un recurso de Credencial y el nombre del equipo desde un recurso de Variable y, a continuación, usa estos valores con el cmdlet Restart-Computer.

```powershell
$Cred = Get-AutomationPSCredential -Name "MyCredential"
$Computer = Get-AutomationVariable -Name "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

También puede usar [InlineScript](automation-powershell-workflow.md#inlinescript), que le permite ejecutar bloques de código en otro equipo con credenciales especificadas por el [parámetro común PSCredential](/powershell/module/psworkflow/about/about_workflowcommonparameters).

### <a name="runas-account"></a>Cuenta RunAs

De forma predeterminada, Hybrid Runbook Worker usa la cuenta del sistema local en Windows y una cuenta de usuario especial, `nxautomation`, en Linux para ejecutar los runbooks. En lugar de hacer que los runbooks proporcionen su propia autenticación a los recursos locales, puede especificar una cuenta **RunAs** para un grupo de Hybrid Worker. Especifique un [recurso de credencial](automation-credentials.md) que tenga acceso a los recursos locales, incluidos los almacenes de certificados. Todos los runbooks se ejecutan con estas credenciales si se ejecutan en una instancia de Hybrid Runbook Worker del grupo.

El nombre de usuario de la credencial debe tener uno de los siguientes formatos:

* dominio\nombre de usuario
* username@domain
* nombre de usuario (para cuentas locales en el equipo local)

Utilice el procedimiento siguiente para especificar una cuenta RunAs de un grupo de Hybrid Worker:

1. Cree un [recurso de credencial](automation-credentials.md) con acceso a los recursos locales.
2. Abra la cuenta de Automation en Azure Portal.
3. Seleccione el icono **Grupos de Hybrid Worker** y luego seleccione el grupo.
4. Seleccione **Toda la configuración** y luego **Configuración del grupo de Hybrid Worker**.
5. Cambie **Ejecutar como** de **Predeterminado** a **Personalizado**.
6. Seleccione la credencial y haga clic en **Guardar**.

### <a name="managed-identities-for-azure-resources"></a>Identidades administradas de recursos de Azure

Las instancias de Hybrid Runbook Worker que se ejecutan en máquinas virtuales de Azure pueden usar el servicio Managed Identities for Azure Resources para autenticarse en recursos de Azure. Hay varias ventajas de usar el servicio Managed Identities for Azure Resources sobre las cuentas de ejecución.

* No es necesario exportar el certificado de ejecución y después importarlo a Hybrid Runbook Worker
* No es necesario renovar el certificado utilizado por la cuenta de ejecución
* No es necesario controlar el objeto de conexión de ejecución en el código del runbook

Para usar una identidad administrada para los recursos de Azure en una instancia de Hybrid Runbook Worker, necesita completar los siguientes pasos:

1. Creación de una máquina virtual de Azure
2. [Configuración de Managed Identities for Azure Resources en la máquina virtual](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#enable-system-assigned-managed-identity-on-an-existing-vm)
3. [Concesión de acceso a la máquina virtual a un grupo de recursos en Resource Manager](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager) tutorial-windows-vm-access-arm.md#get-an-access-token-using-the-vms-system-assigned-managed-identity-and-use-it-to-call-azure-resource-manager)
4. [Instale Hybrid Runbook Worker de Windows](automation-windows-hrw-install.md) en la máquina virtual.

Una vez completados los pasos anteriores, puede utilizar `Connect-AzureRmAccount -Identity` en el runbook para autenticarse con los recursos de Azure. Esta configuración reduce la necesidad de usar una cuenta de ejecución y administrar el certificado para dicha cuenta.

```powershell
# Connect to Azure using the Managed identities for Azure resources identity configured on the Azure VM that is hosting the hybrid runbook worker
Connect-AzureRmAccount -Identity

# Get all VM names from the subscription
Get-AzureRmVm | Select Name
```

> [!NOTE]
> `Connect-AzureRMAccount -Identity` funciona para una instancia de Hybrid Runbook Worker que use una identidad asignada por el sistema y una única identidad asignada por el usuario. Si necesita usar varias identidades asignadas por el usuario en HRW, debe especificar el parámetro `-AccountId` para seleccionar la identidad asignada por el usuario específica.

### <a name="runas-script"></a>Cuenta de ejecución de Automation

Como parte del proceso de compilación automatizado que implementa recursos en Azure, es posible que sea necesario acceder a los sistemas locales para admitir una tarea o conjunto de pasos en la secuencia de implementación. A fin de admitir la autenticación en Azure mediante la cuenta de ejecución, debe instalar el certificado de la cuenta de ejecución.

El siguiente runbook de PowerShell, **Export-RunAsCertificateToHybridWorker**, exporta el certificado de ejecución desde la cuenta de Azure Automation y lo descarga e importa en el almacén de certificados de la máquina local de un Hybrid Worker conectado a la misma cuenta. Una vez completado dicho paso, comprueba que el trabajo puede autenticarse correctamente en Azure con la cuenta de ejecución.

```azurepowershell-interactive
<#PSScriptInfo
.VERSION 1.0
.GUID 3a796b9a-623d-499d-86c8-c249f10a6986
.AUTHOR Azure Automation Team
.COMPANYNAME Microsoft
.COPYRIGHT
.TAGS Azure Automation
.LICENSEURI
.PROJECTURI
.ICONURI
.EXTERNALMODULEDEPENDENCIES
.REQUIREDSCRIPTS
.EXTERNALSCRIPTDEPENDENCIES
.RELEASENOTES
#>

<#
.SYNOPSIS
Exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.

.DESCRIPTION
This runbook exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.
Run this runbook in the hybrid worker where you want the certificate installed.
This allows the use of the AzureRunAsConnection to authenticate to Azure and manage Azure resources from runbooks running in the hybrid worker.

.EXAMPLE
.\Export-RunAsCertificateToHybridWorker

.NOTES
LASTEDIT: 2016.10.13
#>

# Generate the password used for this certificate
Add-Type -AssemblyName System.Web -ErrorAction SilentlyContinue | Out-Null
$Password = [System.Web.Security.Membership]::GeneratePassword(25, 10)

# Stop on errors
$ErrorActionPreference = 'stop'

# Get the management certificate that will be used to make calls into Azure Service Management resources
$RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"

# location to store temporary certificate in the Automation service host
$CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"

# Save the certificate
$Cert = $RunAsCert.Export("pfx",$Password)
Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose

Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
$SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

# Test that authentication to Azure Resource Manager is working
$RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection"

Connect-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $RunAsConnection.TenantId `
    -ApplicationId $RunAsConnection.ApplicationId `
    -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

Set-AzureRmContext -SubscriptionId $RunAsConnection.SubscriptionID | Write-Verbose

# List automation accounts to confirm Azure Resource Manager calls are working
Get-AzureRmAutomationAccount | Select-Object AutomationAccountName
```

> [!IMPORTANT]
> **Add-AzureRmAccount** es ahora un alias de **Connect-AzureRMAccount**. Al buscar elementos de biblioteca, si no ve **Connect-AzureRMAccount**, puede usar **Add-AzureRmAccount** o actualizar los módulos en su cuenta de Automation.

Guarde el runbook *Export-RunAsCertificateToHybridWorker* en el equipo con una extensión `.ps1`. Impórtelo en la cuenta de Automation y edite el runbook, cambiando el valor de la variable `$Password` con su propia contraseña. Publique y, a continuación, ejecute el runbook. Establezca como destino el grupo de Hybrid Worker que va a ejecutar y autenticar los runbooks con la cuenta de ejecución. El flujo de trabajo informa sobre el intento de importar el certificado en el almacén del equipo local y sigue con varias líneas. Este comportamiento depende de cuántas cuentas de Automation se definan en la suscripción y de si la autenticación es correcta.

## <a name="job-behavior"></a>Comportamiento del trabajo

Los trabajos se controlan de forma ligeramente diferente en instancias de Hybrid Runbook Worker que cuando se ejecutan en espacios aislados de Azure. Una diferencia importante es que no hay ningún límite en la duración del trabajo en Hybrid Runbook Worker. Los runbooks que se ejecutan en espacios aislados de Azure están limitados a 3 horas debido al [reparto equitativo](automation-runbook-execution.md#fair-share). Para un runbook de larga ejecución, querrá asegurarse de que sea resistente a posibles reinicios. Por ejemplo, si se reinicia el equipo que hospeda la instancia de Hybrid Worker. Si la máquina host de Hybrid Worker se reinicia, cualquier trabajo de runbook en ejecución se reinicia desde el principio o desde el último punto de comprobación para runbooks de flujo de trabajo de PowerShell. Si un trabajo de runbook se reinicia más de 3 veces, se suspende.

## <a name="run-only-signed-runbooks"></a>Ejecución solo de Runbooks firmados

Es posible configurar instancias de Hybrid Runbook Worker para que ejecuten solo runbooks firmados con cierta configuración. En la sección siguiente se describe cómo configurar las instancias de Hybrid Runbook Worker para ejecutar [Hybrid Runbook Worker de Windows](#windows-hybrid-runbook-worker) y [Hybrid Runbook Worker de Linux](#linux-hybrid-runbook-worker).

> [!NOTE]
> Una vez que haya configurado una instancia de Hybrid Runbook Worker para que solo ejecute runbooks firmados, los runbooks que **no** estén firmados no se ejecutarán en el trabajo.

### <a name="windows-hybrid-runbook-worker"></a>Hybrid Runbook Worker de Windows

#### <a name="create-signing-certificate"></a>Creación de certificado de firma

En el ejemplo siguiente se crea un certificado autofirmado que se puede usar para firmar runbooks. El ejemplo crea el certificado y lo exporta. El certificado se importa a las instancias de Hybrid Runbook Worker más adelante. La huella digital también se devuelve y este valor se usa más adelante para hacer referencia al certificado.

```powershell
# Create a self-signed certificate that can be used for code signing
$SigningCert = New-SelfSignedCertificate -CertStoreLocation cert:\LocalMachine\my `
                                        -Subject "CN=contoso.com" `
                                        -KeyAlgorithm RSA `
                                        -KeyLength 2048 `
                                        -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
                                        -KeyExportPolicy Exportable `
                                        -KeyUsage DigitalSignature `
                                        -Type CodeSigningCert


# Export the certificate so that it can be imported to the hybrid workers
Export-Certificate -Cert $SigningCert -FilePath .\hybridworkersigningcertificate.cer

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Retrieve the thumbprint for later use
$SigningCert.Thumbprint
```

#### <a name="configure-the-hybrid-runbook-workers"></a>Configuración de instancias de Hybrid Runbook Worker

Copie el certificado que se creó en cada instancia de Hybrid Runbook Worker en un grupo. Ejecute el script siguiente para importar el certificado y configurar la instancia de Hybrid Worker para usar la validación de firma en los runbooks.

```powershell
# Install the certificate into a location that will be used for validation.
New-Item -Path Cert:\LocalMachine\AutomationHybridStore
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\AutomationHybridStore

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Configure the hybrid worker to use signature validation on runbooks.
Set-HybridRunbookWorkerSignatureValidation -Enable $true -TrustedCertStoreLocation "Cert:\LocalMachine\AutomationHybridStore"
```

#### <a name="sign-your-runbooks-using-the-certificate"></a>Firma de los runbooks con el certificado

Con las instancias de Hybrid Runbook Worker configuradas para usar solo runbooks firmados, tiene que firmar los runbooks que se van a utilizar en Hybrid Runbook Worker. Use la instancia de PowerShell de ejemplo siguiente para firmar los runbooks.

```powershell
$SigningCert = ( Get-ChildItem -Path cert:\LocalMachine\My\<CertificateThumbprint>)
Set-AuthenticodeSignature .\TestRunbook.ps1 -Certificate $SigningCert
```

Una vez firmado el runbook, se tiene que importar a la cuenta de Automation y publicar con el bloque de firma. Para saber cómo importar runbooks, consulte [Importar un runbook de un archivo en Azure Automation](manage-runbooks.md#import-a-runbook).

### <a name="linux-hybrid-runbook-worker"></a>Hybrid Runbook Worker de Linux

Para iniciar runbooks en Hybrid Runbook Worker de Linux, la instancia de Hybrid Runbook Worker debe tener el ejecutable [GPG](https://gnupg.org/index.html) presente en el equipo.

#### <a name="create-a-gpg-keyring-and-keypair"></a>Creación un conjunto de claves y un par de claves de GPG

Para crear el conjunto de claves y el par de claves, deberá usar la cuenta de Hybrid Runbook Worker `nxautomation`.

Use `sudo` para iniciar sesión con la cuenta `nxautomation`.

```bash
sudo su – nxautomation
```

Una vez que use la cuenta de `nxautomation`, genere el par de claves de gpg.

```bash
sudo gpg --generate-key
```

GPG le guiará por los pasos para crear el par de claves. Deberá proporcionar un nombre, una dirección de correo electrónico, una fecha de expiración, la frase de contraseña y esperar la suficiente entropía en el equipo para que la clave se genere.

Como el directorio GPG se generó con sudo, deberá cambiar el propietario a `nxautomation`.

Ejecute el siguiente comando para cambiar el propietario.

```bash
sudo chown -R nxautomation ~/.gnupg
```

#### <a name="make-the-keyring-available-the-hybrid-runbook-worker"></a>Poner el conjunto de claves a disposición en Hybrid Runbook Worker

Una vez creado el conjunto de claves, debe ponerlo a disposición de Hybrid Runbook Worker. Modifique el archivo de configuración `/var/opt/microsoft/omsagent/state/automationworker/diy/worker.conf` para que incluya el siguiente ejemplo en la sección `[worker-optional]`

```bash
gpg_public_keyring_path = /var/opt/microsoft/omsagent/run/.gnupg/pubring.kbx
```

#### <a name="verify-signature-validation-is-on"></a>Verificación de que la validación de firmas está activada

Si se ha deshabilitado la validación de firmas en el equipo, deberá activarla. Para habilitar la validación de firmas, ejecute el siguiente comando. Reemplace `<LogAnalyticsworkspaceId>` por el identificador de su área de trabajo.

```bash
sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --true <LogAnalyticsworkspaceId>
```

#### <a name="sign-a-runbook"></a>Ejecución de un runbook

Una vez configurada la validación de firmas, puede usar el siguiente comando para iniciar un runbook:

```bash
gpg –-clear-sign <runbook name>
```

El runbook firmado tendrá el nombre `<runbook name>.asc`.

El runbook firmado ahora se puede cargar en Azure Automation y se puede ejecutar como un runbook normal.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los distintos métodos que se pueden utilizar para iniciar un runbook, consulte [Inicio de un runbook en Azure Automation](automation-starting-a-runbook.md).
* Para entender las diferentes formas para trabajar con runbooks de PowerShell en Azure Automation mediante el editor de texto, consulte [Edición de un runbook en Azure Automation](automation-edit-textual-runbook.md)
* Si los runbooks no finalizan correctamente, revise la guía de solución de problemas sobre [errores de ejecución de un runbook](troubleshoot/hybrid-runbook-worker.md#runbook-execution-fails).
* Para obtener más información sobre PowerShell, incluidos los módulos de referencia de lenguaje y aprendizaje, consulte la [documentación de PowerShell](https://docs.microsoft.com/powershell/scripting/overview).
