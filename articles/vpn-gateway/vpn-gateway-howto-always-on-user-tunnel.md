---
title: Configuración de un túnel de usuario de VPN Always On
titleSuffix: Azure VPN Gateway
description: En este artículo se describe cómo configurar un túnel de usuario de VPN para Grupos de disponibilidad AlwaysOn en VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: cherylmc
ms.openlocfilehash: a22ba5d8b33dd41fcc76c65fcddaf60c1c0ed5e3
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76514755"
---
# <a name="configure-an-always-on-vpn-user-tunnel"></a>Configuración de un túnel de usuario de VPN para Grupos de disponibilidad AlwaysOn

Always On es una nueva característica del cliente VPN de Windows 10 que ofrece la posibilidad de mantener una conexión VPN. Con Always On, el perfil activo de VPN se puede conectar automáticamente y permanecer conectado según los desencadenadores como, por ejemplo, en los inicios de sesión de los usuarios, los cambios en el estado de la red o la activación de la pantalla del dispositivo.

Las puertas de enlace de red virtual de Azure pueden utilizarse con Windows 10 Always On para establecer túneles de usuario persistentes, así como túneles de dispositivo dirigidos a Azure. Este artículo le ayudará a configurar un túnel de usuario de VPN de Always On.

Las conexiones VPN de Always On incluyen uno de estos dos tipos de túneles:

* **Túnel de dispositivo**: se conecta a los servidores VPN especificados antes de que los usuarios inicien sesión en el dispositivo. Los túneles de dispositivo se utilizan en escenarios de conectividad previos al inicio de sesión y para administrar dispositivos.

* **Túnel de usuario**: solo se conecta después de que los usuarios inicien sesión en el dispositivo. Mediante los túneles de usuario puede acceder a los recursos de la organización utilizando servidores VPN.

Los túneles de dispositivo y los de usuario funcionan de forma independiente a los perfiles VPN. Pueden conectarse al mismo tiempo y utilizar diferentes métodos de autenticación u otras opciones de configuración de VPN en función de las necesidades.

En las secciones siguientes, puede configurar una puerta de enlace de VPN y un túnel de usuario.

## <a name="step-1-configure-a-vpn-gateway"></a>Paso 1: Configuración de una instancia de VPN Gateway

Configure la puerta de enlace VPN para que utilice IKEv2 y la autenticación basada en certificados mediante las instrucciones que se encuentran en este artículo sobre las [conexiones de punto a sitio](vpn-gateway-howto-point-to-site-resource-manager-portal.md).

## <a name="step-2-configure-a-user-tunnel"></a>Paso 2: Configuración de un túnel de usuario

1. Instale certificados de cliente en el cliente de Windows 10, tal y como se muestra en este [artículo sobre los clientes VPN de punto a sitio](point-to-site-how-to-vpn-client-install-azure-cert.md). El certificado debe estar en el almacén del usuario actual.

1. Configure el cliente VPN de Always On mediante PowerShell, Configuration Manager o Intune siguiendo las instrucciones de [Configuración de conexiones VPN de Always On en el cliente de Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections).

### <a name="example-configuration-for-the-user-tunnel"></a>Ejemplo de configuración para el túnel de usuario

Una vez que haya configurado la puerta de enlace de red virtual e instalado el certificado de cliente en el almacén de la máquina local del cliente de Windows 10, use los ejemplos siguientes para configurar un túnel de dispositivo de cliente:

1. Copie el texto siguiente y guárdelo como *usercert.ps1*:

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
1. Copie el texto siguiente y guárdelo como *VPNProfile.xml* en la misma carpeta que *usercert.ps1*. Modifique el texto siguiente para adaptarlo a su entorno:

   * `<Servers>azuregateway-1234-56-78dc.cloudapp.net</Servers>  <= Can be found in the VpnSettings.xml in the downloaded profile zip file`
   * `<Address>192.168.3.5</Address>  <= IP of resource in the vnet or the vnet address space`
   * `<Address>192.168.3.4</Address>  <= IP of resource in the vnet or the vnet address space`
   * `<PrefixSize>32</PrefixSize>     <= Subnet mask`

   ```
    <VPNProfile>  
      <NativeProfile>  
    <Servers>azuregateway-b115055e-0882-49bc-a9b9-7de45cba12c0-8e6946892333.vpn.azure.com</Servers>  
    <NativeProtocolType>IKEv2</NativeProtocolType>  
    <Authentication>  
    <UserMethod>Eap</UserMethod>
    <Eap>
    <Configuration>
    <EapHostConfig xmlns="http://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="http://www.microsoft.com/provisioning/EapCommon">13</Type><VendorId xmlns="http://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="http://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="http://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="http://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="http://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>false</DisableUserPromptForServerValidation><ServerNames></ServerNames></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">false</PerformServerValidation><AcceptServerName xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">false</AcceptServerName></EapType></Eap></Config></EapHostConfig>
    </Configuration>
    </Eap>
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
    <!-- traffic filters for the routes specified above so that only this traffic can go over the device tunnel --> 
      <TrafficFilter>  
    <RemoteAddressRanges>192.168.3.4, 192.168.3.5</RemoteAddressRanges>  
      </TrafficFilter>
    <!-- need to specify always on = true --> 
    <AlwaysOn>true</AlwaysOn>
    <RememberCredentials>true</RememberCredentials>
    <!--new node to register client IP address in DNS to enable manage out -->
    <RegisterDNS>true</RegisterDNS>
    </VPNProfile>
   ```
1. Ejecute PowerShell como administrador.

1. En PowerShell, vaya la carpeta donde se encuentra *usercert.ps1* y *VPNProfile.xml* y ejecute el siguiente comando:

   ```powershell
   C:\> .\usercert.ps1 .\VPNProfile.xml UserTest
   ```
   
   ![MachineCertTest](./media/vpn-gateway-howto-always-on-user-tunnel/p2s2.jpg)
1. En **Configuración de VPN**, busque la entrada **UserTest** y, a continuación, seleccione **Conectar**.

1. Si la conexión se realiza correctamente, habrá configurado correctamente un túnel de usuario de Always On.

## <a name="clean-up-your-resources"></a>Limpiar los recursos

Para quitar el perfil, haga lo siguiente:

1. Ejecute el siguiente comando:

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

1. Desconecte la conexión y desactive la opción **Conectar automáticamente**.

![Limpieza](./media/vpn-gateway-howto-always-on-user-tunnel/p2s4..jpg)

## <a name="next-steps"></a>Pasos siguientes

Para solucionar los problemas de conexión que puedan producirse, consulte [Problemas de conexión de punto a sitio de Azure](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
