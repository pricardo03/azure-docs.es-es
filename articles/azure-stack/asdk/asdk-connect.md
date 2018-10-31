---
title: Conexión a Azure Stack | Microsoft Docs
description: Aprenda a conectarse al ASDK.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 3cebbfa6-819a-41e3-9f1b-14ca0a2aaba3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2018
ms.author: jeffgilb
ms.openlocfilehash: 55be312046f5cdea2c1481ed435b5859ab2c2540
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026829"
---
# <a name="connect-to-the-azure-stack-development-kit"></a>Conexión al Kit de desarrollo de Azure Stack

Para administrar recursos, primero debe conectarse al Kit de desarrollo de Azure Stack (ASDK). En este artículo se describen los pasos que debe seguir para conectarse al ASDK. Puede usar una de las siguientes opciones de conexión:

* [Conexión a Escritorio remoto](#connect-with-remote-desktop) Cuando se conecta mediante Conexión a Escritorio remoto, un usuario puede conectarse rápidamente al kit de desarrollo.
* [Red privada virtual (VPN)](#connect-with-vpn). Cuando se conecta mediante una red privada virtual, varios usuarios pueden conectarse a la vez desde los clientes externos a la infraestructura de Azure Stack. Una conexión VPN requiere cierta configuración.

<a name="connect-to-azure-stack-with-remote-desktop"></a>
##  <a name="connect-to-azure-stack-by-using-remote-desktop-connection"></a>Conexión a Azure Stack mediante Conexión a Escritorio remoto

Solo un usuario cada vez puede administrar los recursos del portal de operadores o el portal de usuarios mediante Conexión a Escritorio remoto.

1. Abra Conexión a Escritorio remoto (mstc.exe) y conéctese al equipo de host del kit de desarrollo como **AzureStack\AzureStackAdmin** con la contraseña especificada durante la instalación del ASDK.  

2. En el equipo host del kit de desarrollo, abra el Administrador del servidor (ServerManager.exe). Seleccione **Servidor local**, desactive la opción **Configuración de seguridad mejorada de IE** y cierre el Administrador del servidor.

3. Inicie sesión en el portal de administración como **AzureStack\CloudAdmin** o use otras credenciales de operador de Azure Stack. La dirección del portal de administración del ASDK es [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).

4. Inicie sesión en el portal de usuario como **AzureStack\CloudAdmin** o use otras credenciales de usuario de Azure Stack. La dirección del portal de usuario del ASDK es [https://portal.local.azurestack.external](https://portal.local.azurestack.external).

> [!NOTE]
> Para obtener más información sobre cuándo usar cada tipo de cuenta, consulte [Conceptos básicos de administración del Kit de desarrollo de Azure Stack](asdk-admin-basics.md#what-account-should-i-use).

<a name="connect-to-azure-stack-with-vpn"></a>
## <a name="connect-to-azure-stack-by-using-vpn"></a>Conexión a Azure Stack mediante una red privada virtual

Puede establecer un conexión VPN de túnel dividido a un ASDK para tener acceso a los portales de Azure Stack y a las herramientas instaladas localmente, como Visual Studio y PowerShell. Con las conexiones VPN, varios usuarios pueden conectarse al mismo tiempo a los recursos de Azure Stack hospedados por el ASDK.

La conectividad VPN se admite en Azure AD y en las implementaciones de Servicios de federación de Active Directory (AD FS).

> [!NOTE]
> Una conexión VPN no proporciona conectividad a las máquinas virtuales de la infraestructura de Azure Stack.

### <a name="prerequisites"></a>Requisitos previos
Antes de configurar una conexión VPN al ASDK, asegúrese de cumplir los siguientes requisitos previos.

- Instale [Azure Stack-compatible Azure PowerShell](asdk-post-deploy.md#install-azure-stack-powershell) en su equipo local.  
- Descargue las [herramientas necesarias para trabajar con Azure Stack](asdk-post-deploy.md#download-the-azure-stack-tools).

### <a name="set-up-vpn-connectivity"></a>Configuración de la conectividad VPN

Para crear una conexión VPN al ASDK, abra Windows PowerShell como administrador en el equipo local basado en Windows. A continuación, ejecute el siguiente script (actualice los valores de dirección IP y contraseña para su entorno):

```PowerShell
# Change directories to the default Azure Stack tools directory
cd C:\AzureStack-Tools-master

# Configure Windows Remote Management (WinRM), if it's not already configured.
winrm quickconfig  

Set-ExecutionPolicy RemoteSigned

# Import the Connect module.
Import-Module .\Connect\AzureStack.Connect.psm1

# Add the development kit host computer’s IP address as the ASDK certificate authority (CA) to the list of trusted hosts. Make sure you update the IP address and password values for your environment.

$hostIP = "<Azure Stack host IP address>"

$Password = ConvertTo-SecureString `
  "<operator's password provided when deploying Azure Stack>" `
  -AsPlainText `
  -Force

Set-Item wsman:\localhost\Client\TrustedHosts `
  -Value $hostIP `
  -Concatenate

# Create a VPN connection entry for the local user.
Add-AzsVpnConnection `
  -ServerAddress $hostIP `
  -Password $Password

```

Si la instalación se realiza correctamente, **azurestack** aparece en la lista de conexiones VPN.

![Conexiones de red](media/asdk-connect/image3.png)  

### <a name="connect-to-azure-stack"></a>Conexión a Azure Stack

Conéctese a la instancia de Azure Stack mediante uno los métodos siguientes:  

* Use el comando `Connect-AzsVpn `:
    
  ```PowerShell
  Connect-AzsVpn `
    -Password $Password
  ```

  Cuando se le solicite, confíe en el host de Azure Stack e instale el certificado de **AzureStackCertificateAuthority** en el almacén de certificados del equipo local. 
  
  > [!IMPORTANT]
  > El símbolo del sistema puede quedar oculto por la ventana de PowerShell.

* En el equipo local, seleccione **Configuración de red** > **VPN** > **azurestack** > **conectar**. En el símbolo del sistema de inicio de sesión, escriba el nombre de usuario (**AzureStack\AzureStackAdmin**) y la contraseña.

### <a name="test-vpn-connectivity"></a>Probar la conectividad VPN

Para probar la conexión del portal, abra un explorador web y vaya al portal de usuarios (https://portal.local.azurestack.external/) o al portal de administración (https://adminportal.local.azurestack.external/). Inicie sesión y cree los recursos.  

## <a name="next-steps"></a>Pasos siguientes

[Solución de problemas](asdk-troubleshooting.md)
