---
title: Uso de herramientas remotas para solucionar problemas con la VM de Azure | Microsoft Docs
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 01/11/2018
ms.author: delhan
ms.openlocfilehash: 6f55491ba7d422b19b3ee9db8b9ee804b920e422
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2019
ms.locfileid: "55983855"
---
# <a name="use-remote-tools-to-troubleshoot-azure-vm-issues"></a>Uso de herramientas remotas para solucionar problemas con la VM de Azure

Para solucionar problemas en una máquina virtual (VM) de Azure, puede conectarse a la VM mediante el uso de las herramientas remotas que se describen en este artículo en lugar de utilizar el Protocolo de escritorio remoto (RDP).

## <a name="serial-console"></a>Consola serie

Use la [consola serie de la máquina virtual](serial-console-windows.md) para ejecutar comandos en la máquina virtual remota de Azure.

## <a name="remote-cmd"></a>CMD remoto

Descargue [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec). Conéctese a la máquina virtual ejecutando el comando siguiente:

```cmd
psexec \\<computer>-u user -s cmd
```

>[!Note]
>* El comando debe ejecutarse en un equipo que esté en la misma red virtual.
>* Los valores de DIP o HostName pueden usarse para reemplazar <computer>.
>* El parámetro -s asegura que se invoca el comando mediante el uso de la cuenta del sistema (permisos de administrador).
>* PsExec usa los puertos TCP 135 y 445. Por lo tanto, los dos puertos deben estar abiertos en el firewall.

## <a name="run-commands"></a>Ejecutar comandos

Consulte [Ejecución de scripts de PowerShell en la máquina virtual Windows con el comando Ejecutar](../windows/run-command.md) para obtener más información acerca de cómo usar la característica Ejecutar comandos para ejecutar scripts en la máquina virtual.

## <a name="customer-script-extension"></a>Extensión de script personalizado

Puede usar la característica Extensión de script personalizado para ejecutar un script personalizado en la máquina virtual de destino. Para poder usar esta característica, deben cumplirse las condiciones siguientes:

* La máquina virtual debe tener conectividad.

* Agente de Azure está instalado y funciona según lo previsto en la máquina virtual.

* La extensión no se instaló previamente en la máquina virtual.
 
  La extensión insertará el script solo la primera vez que se use. Si usa esta característica más adelante, la extensión reconocerá que ya se ha utilizado y no se cargará el nuevo script.

Tendrá que cargar el script en una cuenta de almacenamiento y generar su propio contenedor. A continuación, ejecute el siguiente script de Azure PowerShell en un equipo que tenga conectividad a la máquina virtual.

### <a name="for-v1-vms"></a>Para máquinas virtuales V1

```powershell
#Setup the basic variables
$subscriptionID = "<<SUBSCRIPTION ID>>" 
$storageAccount = "<<STORAGE ACCOUNT>>" 
$localScript = "<<FULL PATH OF THE PS1 FILE TO EXECUTE ON THE VM>>" 
$blobName = "file.ps1" #Name you want for the blob in the storage
$vmName = "<<VM NAME>>" 
$vmCloudService = "<<CLOUD SERVICE>>" #Resource group/Cloud Service where the VM is hosted. I.E.: For "demo305.cloudapp.net" the cloud service is going to be demo305

#Setup the Azure Powershell module and ensure the access to the subscription
Import-Module Azure
Add-AzureAccount  #Ensure Login with account associated with subscription ID
Get-AzureSubscription -SubscriptionId $subscriptionID | Select-AzureSubscription

#Setup the access to the storage account and upload the script
$storageKey = (Get-AzureStorageKey -StorageAccountName $storageAccount).Primary
$context = New-AzureStorageContext -StorageAccountName $storageAccount -StorageAccountKey $storageKey
$container = "cse" + (Get-Date -Format yyyyMMddhhmmss)<
New-AzureStorageContainer -Name $container -Permission Off -Context $context
Set-AzureStorageBlobContent -File $localScript -Container $container -Blob $blobName  -Context $context

#Push the script into the VM
$vm = Get-AzureVM -ServiceName $vmCloudService -Name $vmName
Set-AzureVMCustomScriptExtension "CustomScriptExtension" -VM $vm -StorageAccountName $storageAccount -StorageAccountKey $storagekey -ContainerName $container -FileName $blobName -Run $blobName | Update-AzureVM
```

### <a name="for-v2-vms"></a>Para máquinas virtuales V2

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

```powershell
#Setup the basic variables
$subscriptionID = "<<SUBSCRIPTION ID>>"
$storageAccount = "<<STORAGE ACCOUNT>>"
$storageRG = "<<RESOURCE GROUP OF THE STORAGE ACCOUNT>>" 
$localScript = "<<FULL PATH OF THE PS1 FILE TO EXECUTE ON THE VM>>" 
$blobName = "file.ps1" #Name you want for blob in storage
$vmName = "<<VM NAME>>" 
$vmResourceGroup = "<<RESOURCE GROUP>>"
$vmLocation = "<<DATACENTER>>" 
 
#Setup the Azure Powershell module and ensure the access to the subscription
Login-AzAccount #Ensure Login with account associated with subscription ID
Get-AzSubscription -SubscriptionId $subscriptionID | Select-AzSubscription

#Setup the access to the storage account and upload the script 
$storageKey = (Get-AzStorageAccountKey -ResourceGroupName $storageRG -Name $storageAccount).Value[0]
$context = New-AzureStorageContext -StorageAccountName $storageAccount -StorageAccountKey $storageKey
$container = "cse" + (Get-Date -Format yyyyMMddhhmmss)
New-AzureStorageContainer -Name $container -Permission Off -Context $context
Set-AzureStorageBlobContent -File $localScript -Container $container -Blob $blobName  -Context $context

#Push the script into the VM
Set-AzVMCustomScriptExtension -Name "CustomScriptExtension" -ResourceGroupName $vmResourceGroup -VMName $vmName -Location $vmLocation -StorageAccountName $storageAccount -StorageAccountKey $storagekey -ContainerName $container -FileName $blobName -Run $blobName
```

## <a name="remote-powershell"></a>PowerShell remoto

>[!Note]
>Puerto TCP 5986 (HTTPS) debe estar abierto para que pueda usar esta opción.
>
>En el caso de las máquinas virtuales de ARM, debe abrir el puerto 5986 en el grupo de seguridad de red (NSG). Para más información, consulte Grupos de seguridad. 
>
>En el caso de las máquinas virtuales de RDFE, debe tener un punto de conexión con un puerto privado (5986) y un puerto público. A continuación, también tendrá que abrir ese puerto público en NSG.

### <a name="set-up-the-client-computer"></a>Configuración del equipo cliente

Para usar PowerShell para conectarse de forma remota a la máquina virtual, primero debe configurar el equipo cliente para permitir la conexión. Para ello, agregue la máquina virtual a la lista de hosts de confianza de PowerShell ejecutando el comando siguiente, según corresponda.

Para agregar una máquina virtual a la lista de hosts de confianza:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value <ComputerName>
```

Para agregar varias máquinas virtuales a la lista de hosts de confianza:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value <ComputerName1>,<ComputerName2>
```

Para agregar todos los equipos a la lista de hosts de confianza:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value *
```

### <a name="enable-remoteps-on-the-vm"></a>Habilitar RemotePS en la máquina virtual

En el caso de las máquinas virtuales clásicas, use la extensión de script personalizado para ejecutar el script siguiente:

```powershell
Enable-PSRemoting -Force
New-NetFirewallRule -Name "Allow WinRM HTTPS" -DisplayName "WinRM HTTPS" -Enabled True -Profile Any -Action Allow -Direction Inbound -LocalPort 5986 -Protocol TCP
$thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME -CertStoreLocation Cert:\LocalMachine\My).Thumbprint
$command = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:computername""; CertificateThumbprint=""$thumbprint""}"
cmd.exe /C $command
```

En el caso de las máquinas virtuales de ARM, utilice Ejecutar comandos desde el portal para ejecutar el script EnableRemotePS:

![Ejecución de comando](./media/remote-tools-troubleshoot-azure-vm-issues/run-command.png)

### <a name="connect-to-the-vm"></a>Conexión a la máquina virtual

Ejecute el siguiente comando, en función de la ubicación del equipo cliente:

* Fuera de la red virtual o implementación

    * Para máquinas virtuales clásicas, ejecute el siguiente comando:

    ```powershell
    $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
    Enter-PSSession -ComputerName  "<<CLOUDSERVICENAME.cloudapp.net>>" -port "<<PUBLIC PORT NUMBER>>" -Credential (Get-Credential) -useSSL -SessionOption $Skip
    ```

    * Para máquinas virtuales de ARM, primero agregue un nombre DNS a la dirección IP pública. Para obtener pasos detallados, vea [Creación de un nombre de dominio completo en Azure Portal para una máquina virtual Windows](../windows/portal-create-fqdn.md). Luego, ejecute el siguiente comando:

    ```powershell
    $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
    Enter-PSSession -ComputerName "<<DNSname.DataCenter.cloudapp.azure.com>>" -port "5986" -Credential (Get-Credential) -useSSL -SessionOption $Skip
    ```

* En la red virtual o implementación, ejecute el comando siguiente:
  
  ```powershell
  $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
  Enter-PSSession -ComputerName  "<<HOSTNAME>>" -port 5986 -Credential (Get-Credential) -useSSL -SessionOption $Skip
  ```

>[!Note] 
>Al activar la marca SkipCaCheck se omite el requisito de importar un certificado a la máquina virtual cuando se inicia la sesión.

También puede usar el cmdlet Invoke-Command para ejecutar un script en la máquina virtual de forma remota:

```powershell
Invoke-Command -ComputerName "<<COMPUTERNAME>" -ScriptBlock {"<<SCRIPT BLOCK>>"}
```

## <a name="remote-registry"></a>Registro remoto

>[!Note]
>El puerto TCP 135 o 445 debe estar abierto para poder usar esta opción.
>
>En el caso de las máquinas virtuales de ARM, tendrá que abrir el puerto 5986 en el NSG. Para más información, consulte Grupos de seguridad. 
>
>En el caso de las máquinas virtuales de RDFE, debe tener un punto de conexión con un puerto privado 5986 y un puerto público. También tendrá que abrir ese puerto público en NSG.

1. Desde otra máquina virtual en la misma red virtual, abra el editor del registro (regedit.exe).

2. Seleccione **Archivo** >**Connect Network Registry** (Conectar registro de red).

   ![Opción remota](./media/remote-tools-troubleshoot-azure-vm-issues/remote-registry.png) 

3. Busque la máquina virtual de destino por **hostname** o **IP dinámica** (preferible) introduciéndola en el cuadro "Escriba el nombre del objeto que desea seleccionar".

   ![Opción remota](./media/remote-tools-troubleshoot-azure-vm-issues/input-computer-name.png) 
 
4. Introduzca las credenciales para la VM de destino.

5. Realice los cambios necesarios en el registro.

## <a name="remote-services-console"></a>Consola de servicios remotos

>[!Note]
>Los puertos TCP 135 o 445 deben estar abiertos para poder usar esta opción.
>
>En el caso de las máquinas virtuales de ARM, tendrá que abrir el puerto 5986 en el NSG. Para más información, consulte Grupos de seguridad. 
>
>En el caso de las máquinas virtuales de RDFE, debe tener un punto de conexión con un puerto privado 5986 y un puerto público. A continuación, también tendrá que abrir ese puerto público en NSG.

1. Desde otra máquina virtual en la misma red virtual, abra una instancia de **Services.msc**.

2. Haga clic con el botón derecho en **Servicios (Local)**.

3. Seleccione **Conectarse a otro equipo**.

   ![Servicio remoto](./media/remote-tools-troubleshoot-azure-vm-issues/remote-services.png)

4. Escriba la dirección IP dinámica de la máquina virtual de destino.

   ![DIP de entrada](./media/remote-tools-troubleshoot-azure-vm-issues/input-ip-address.png)

5. Realice los cambios necesarios en los servicios.

## <a name="next-steps"></a>Pasos siguientes

[Enter-PSSession](https://technet.microsoft.com/library/hh849707.aspx)

[Extensión de script personalizado para Windows con el modelo de implementación clásico](../extensions/custom-script-classic.md)

PsExec forma parte de [PSTools Suite](https://download.sysinternals.com/files/PSTools.zip).

Para obtener más información acerca de PSTools Suite, consulte [PSTools Suite](https://docs.microsoft.com/sysinternals/downloads/pstools).


