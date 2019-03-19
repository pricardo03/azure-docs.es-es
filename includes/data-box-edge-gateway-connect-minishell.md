---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/06/2019
ms.author: alkohli
ms.openlocfilehash: 348f7bdd333da4f4a6cb41a438b7aee08d6a6bbb
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57556297"
---
Dependiendo del sistema operativo de cliente, los procedimientos para conectarse de forma remota al dispositivo son diferentes.

### <a name="remotely-connect-from-a-windows-client"></a>Conectarse de forma remota desde un cliente de Windows

Antes de comenzar, asegúrese de que el cliente de Windows se está ejecutando Windows PowerShell 5.0 o posterior.

Siga estos pasos para conectarse de forma remota desde un cliente de Windows.

1. Ejecutar una sesión de Windows PowerShell como administrador.
2. Asegúrese de que el servicio Administración remota de Windows se está ejecutando en el cliente. En el símbolo del sistema, escriba:

    `winrm quickconfig`

3. Asignar una variable a la dirección IP del dispositivo.

    $ip = "<device_ip>"

    Reemplace `<device_ip>` con la dirección IP del dispositivo.

4. Para agregar la dirección IP del dispositivo a la lista de hosts de confianza del cliente, escriba el siguiente comando:

    `Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force`

5. Inicie una sesión de Windows PowerShell en el dispositivo:

    `Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell`

6. Proporcione la contraseña cuando se le solicite. Utilice la misma contraseña que se usa para iniciar sesión en la interfaz de usuario web local. La contraseña de la interfaz de usuario web local predeterminada es *Password1*. Cuando se conecta correctamente al dispositivo mediante PowerShell remoto, vea la siguiente salida de ejemplo:  

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    PS C:\WINDOWS\system32> winrm quickconfig
    WinRM service is already running on this machine.
    PS C:\WINDOWS\system32> $ip = "10.100.10.10"
    PS C:\WINDOWS\system32> Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force
    PS C:\WINDOWS\system32> Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell

    WARNING: The Windows PowerShell interface of your device is intended to be used only for the initial network configuration. Please engage Microsoft Support if you need to access this interface to troubleshoot any potential issues you may be experiencing. Changes made through this interface without involving Microsoft Support could result in an unsupported configuration.
    [10.100.10.10]: PS>
    ```

### <a name="remotely-connect-from-a-linux-client"></a>Conectarse de forma remota desde un cliente Linux

En el cliente de Linux que usará para conectarse:

- [Instalar la más reciente de PowerShell Core para Linux](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-linux?view=powershell-6) de GitHub para obtener la característica de comunicación remota SSH. 
- [Instalar solo el `gss-ntlmssp` paquete desde el módulo NTLM](https://github.com/Microsoft/omi/blob/master/Unix/doc/setup-ntlm-omi.md). Para los clientes de Ubuntu, use el siguiente comando:
    - `sudo apt-get install gss-ntlmssp`

Para obtener más información, vaya a [comunicación remota de PowerShell a través de SSH](https://docs.microsoft.com/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core?view=powershell-6).

Siga estos pasos para conectarse de forma remota desde un cliente NFS.

1. Para abrir la sesión de PowerShell, escriba:

    `sudo pwsh`
 
2. Para conectar con el cliente remoto, escriba:

    `Enter-PSSession -ComputerName $ip -Authentication Negotiate -ConfigurationName Minishell -Credential ~\EdgeUser`

    Cuando se le solicite, proporcione la contraseña utilizada para iniciar sesión en el dispositivo.
 
> [!NOTE]
> Este procedimiento no funciona en Mac OS.
