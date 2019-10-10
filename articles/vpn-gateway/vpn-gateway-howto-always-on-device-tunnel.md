---
title: Configuración de un túnel Always On en VPN Gateway
description: Pasos para configurar un túnel Always On en VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: cherylmc
ms.openlocfilehash: bc2ec2b952b4f0c6e61fc4953559fa882edfff09
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71841138"
---
# <a name="configure-an-always-on-vpn-device-tunnel"></a>Configuración de un túnel de dispositivo VPN para los Grupos de disponibilidad AlwaysOn

Una de las nuevas características del cliente de red privada virtual (VPN) de Windows 10 es la posibilidad de mantener una conexión VPN. Always On es una característica de Windows 10 que permite que el perfil activo de VPN se conecte automáticamente y permanezca conectado en función de los desencadenadores; es decir, los inicios de sesión de los usuarios, los cambios en el estado de la red o la activación de la pantalla del dispositivo.

Las puertas de enlace de red virtual de Azure pueden utilizarse con Windows 10 Always On para establecer túneles de usuario persistentes, así como túneles de dispositivo dirigidos a Azure. Este artículo le ayudará a configurar un túnel de dispositivo VPN de Always On.

Las conexiones VPN de Always On incluyen dos tipos de túneles:

* **Túnel de dispositivo**: se conecta a los servidores VPN especificados antes de que los usuarios inicien sesión en el dispositivo. Los túneles de dispositivo se utilizan en escenarios de conectividad previos al inicio de sesión y para administrar dispositivos.

* **Túnel de usuario**: solo se conecta cuando un usuario inicia sesión en el dispositivo. Los túneles de usuario permiten a los usuarios acceder a los recursos de la organización utilizando servidores VPN.

Tanto los túneles de dispositivo como de usuario funcionan de forma independiente a los perfiles VPN. Pueden conectarse al mismo tiempo y utilizar diferentes métodos de autenticación u otras opciones de configuración de VPN en función de las necesidades.

## <a name="1-configure-the-gateway"></a>1. Configuración de la puerta de enlace

Configure la puerta de enlace VPN para que utilice IKEv2 y la autenticación basada en certificados. Para ello, siga este [artículo sobre las conexiones de punto a sitio](vpn-gateway-howto-point-to-site-resource-manager-portal.md).

## <a name="2-configure-the-device-tunnel"></a>2. Configuración del túnel de dispositivo

Para poder establecer correctamente un túnel de dispositivo, deben cumplirse los siguientes requisitos:

* El dispositivo debe ser un equipo unido a un dominio que ejecute la versión 1709 u otra posterior de Windows 10 Enterprise o Education.
* El túnel solamente puede configurarse en la solución VPN integrada en Windows y debe establecerse utilizando IKEv2 con la autenticación de certificados de equipo. 
* Solo se puede configurar un túnel de dispositivo en cada dispositivo.

1. Instale certificados de cliente en el cliente de Windows 10, tal y como se muestra en este [artículo sobre los clientes VPN de punto a sitio](point-to-site-how-to-vpn-client-install-azure-cert.md). El certificado debe estar en el almacén del equipo local.
1. Use [estas instrucciones](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-device-tunnel-config#vpn-device-tunnel-configuration) para crear un perfil de VPN y configurar el túnel de dispositivo en el contexto de la cuenta de sistema local.

### <a name="configuration-example-for-device-tunnel"></a>Ejemplo de configuración de túnel de dispositivo

Una vez que haya configurado la puerta de enlace de red virtual e instalado el certificado de cliente en el almacén del equipo local del cliente de Windows 10, use los ejemplos siguientes para configurar un túnel de dispositivo de cliente.

1. Copie el texto siguiente y guárdelo como ***devicecert.ps1***.

   ```
   Param(
   [string]$xmlFilePath,
   [string]$ProfileName
   )

   $a = Test-Path $xmlFilePath
   echo $a

   $ProfileXML = Get-Content $xmlFilePath

   echo $XML

   $ProfileNameEscaped = $ProfileName -replace ' ', '%20'

   $Version = 201606090004

   $ProfileXML = $ProfileXML -replace '<', '&lt;'
   $ProfileXML = $ProfileXML -replace '>', '&gt;'
   $ProfileXML = $ProfileXML -replace '"', '&quot;'

   $nodeCSPURI = './Vendor/MSFT/VPNv2'
   $namespaceName = "root\cimv2\mdm\dmmap"
   $className = "MDM_VPNv2_01"

   $session = New-CimSession

   try
   {
   $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", 'String', 'Key')
   $newInstance.CimInstanceProperties.Add($property)
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", 'String', 'Key')
   $newInstance.CimInstanceProperties.Add($property)
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", 'String', 'Property')
   $newInstance.CimInstanceProperties.Add($property)

   $session.CreateInstance($namespaceName, $newInstance)
   $Message = "Created $ProfileName profile."
   Write-Host "$Message"
   }
   catch [Exception]
   {
   $Message = "Unable to create $ProfileName profile: $_"
   Write-Host "$Message"
   exit
   }
   $Message = "Complete."
   Write-Host "$Message"
   ```
1. Copie el texto siguiente y guárdelo como ***VPNProfile.xml*** en la misma carpeta que **devicecert.ps1**. Modifique el texto siguiente para adaptarlo a su entorno.

   * `<Servers>azuregateway-1234-56-78dc.cloudapp.net</Servers>`
   * `<Address>192.168.3.5</Address>`
   * `<Address>192.168.3.4</Address>`

   ```
   <VPNProfile>  
     <NativeProfile>  
   <Servers>azuregateway-1234-56-78dc.cloudapp.net</Servers>  
   <NativeProtocolType>IKEv2</NativeProtocolType>  
   <Authentication>  
     <MachineMethod>Certificate</MachineMethod>  
   </Authentication>  
   <RoutingPolicyType>SplitTunnel</RoutingPolicyType>  
    <!-- disable the addition of a class based route for the assigned IP address on the VPN interface -->
   <DisableClassBasedDefaultRoute>true</DisableClassBasedDefaultRoute>  
     </NativeProfile> 
     <!-- use host routes(/32) to prevent routing conflicts -->  
     <Route>  
   <Address>192.168.3.5</Address>  
   <PrefixSize>32</PrefixSize>  
     </Route>  
     <Route>  
   <Address>192.168.3.4</Address>  
   <PrefixSize>32</PrefixSize>  
     </Route>  
   <!-- need to specify always on = true --> 
     <AlwaysOn>true</AlwaysOn> 
   <!-- new node to specify that this is a device tunnel -->  
    <DeviceTunnel>true</DeviceTunnel>
   <!--new node to register client IP address in DNS to enable manage out -->
   <RegisterDNS>true</RegisterDNS>
   </VPNProfile>
   ```
1. Descargue **PsExec** de [Sysinternals](https://docs.microsoft.com/sysinternals/downloads/psexec) y extraiga los archivos en **C:\PSTools**.
1. En un símbolo del sistema de CMD con privilegios de administrador, inicie PowerShell ejecutando:

   ```
   C:\PsTools\PsExec.exe Powershell for 32-bit Windows
   C:\PsTools\PsExec64.exe Powershell for 64-bit Windows
   ```

   ![powershell](./media/vpn-gateway-howto-always-on-device-tunnel/powershell.png)
1. En PowerShell, vaya la carpeta donde se encuentra **devicecert.ps1** y **VPNProfile.xml** y ejecute el siguiente comando:

   ```powershell
   C:\> .\devicecert.ps1 .\VPNProfile.xml MachineCertTest
   ```
   
   ![MachineCertTest](./media/vpn-gateway-howto-always-on-device-tunnel/machinecerttest.png)
1. Ejecute **rasphone**.

   ![rasphone](./media/vpn-gateway-howto-always-on-device-tunnel/rasphone.png)
1. Busque la entrada **MachineCertTest** y haga clic en **Conectar**.

   ![Conectar](./media/vpn-gateway-howto-always-on-device-tunnel/connect.png)
1. Si la conexión se realiza correctamente, reinicie el equipo. El túnel se conectará automáticamente.

## <a name="cleanup"></a>Limpieza

Para quitar el perfil, ejecute el siguiente comando:

![Limpieza](./media/vpn-gateway-howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>Pasos siguientes

Para solucionar problemas, consulte [Problemas de conexión de punto a sitio de Azure](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
