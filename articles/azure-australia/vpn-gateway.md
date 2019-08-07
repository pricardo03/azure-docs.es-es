---
title: Azure VPN Gateway en Azure Australia
description: Implementación de VPN Gateway en Azure Australia para cumplir lo dispuesto en el ISM y proteger eficazmente las agencias gubernamentales australianas
author: galey801
ms.service: azure-australia
ms.topic: article
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 442ad6334a1775033018005d4a85875dbcb08ada
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571240"
---
# <a name="azure-vpn-gateway-in-azure-australia"></a>Azure VPN Gateway en Azure Australia

Un servicio crítico con cualquier nube pública es la conexión segura de los recursos y servicios en la nube a los sistemas locales existentes.  El servicio que proporciona esta funcionalidad en Azure es Azure VPN Gateway (VPN Gateway). En este artículo se describen las consideraciones clave relativas a la configuración de VPN Gateway para cumplir los [controles del manual de seguridad de la información](https://acsc.gov.au/infosec/ism/) (ISM) de Australian Signals Directorate (ASD).

VPN Gateway se usa para enviar tráfico cifrado entre una red virtual de Azure y otra red.  Hay tres escenarios abordados por VPN Gateway:

- **Sitio a sitio** (S2S)
- **Punto a sitio** (P2S)
- **De red virtual a red virtual**

Este artículo se centrará en las puertas de enlace VPN S2S. En el diagrama 1 se muestra un ejemplo de configuración de una puerta de enlace VPN de sitio a sitio.

![VPN Gateway con conexiones de varios sitios](media/vpngateway-multisite-connection-diagram.png)

*Diagrama 1: Azure VPN Gateway de sitio a sitio*

## <a name="key-design-considerations"></a>Consideraciones clave sobre el diseño

Hay tres opciones de red para conectar Azure a los clientes gubernamentales australianos:

- **ICON**
- **ExpressRoute**
- **Red pública de Internet**

La [guía del consumidor](https://servicetrust.microsoft.com/viewpage/Australia) del Centro Australiano de Seguridad Cibernética para Azure recomienda que VPN Gateway o un servicio de terceros certificado PROTEGIDO equivalente se use junto con las tres opciones de red para asegurarse de que las conexiones cumplen con los controles del ISM para el cifrado y la integridad.

### <a name="encryption-and-integrity"></a>Cifrado e integridad

De forma predeterminada, la VPN negocia los parámetros y algoritmos de cifrado e integridad durante el establecimiento de la conexión como parte de los protocolos de enlace de IKE.  Durante el protocolo de enlace de IKE, la configuración y el orden de preferencia dependerán de si VPN Gateway es el iniciador o el respondedor (Nota: Esto se controla a través del dispositivo VPN).  La configuración final de la conexión se controla mediante la configuración del dispositivo VPN.  Para más información sobre los dispositivos VPN validados y su configuración, haga clic aquí: [Acerca de los dispositivos VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices)

VPN Gateway puede controlar el cifrado y la integridad mediante la configuración de una directiva de IPsec o IKE personalizada en la conexión.

### <a name="resource-operations"></a>Operaciones de los recursos

VPN Gateway crea una conexión entre los entornos de Azure y los que no son de Azure a través de la red pública de Internet.  El ISM tiene controles relacionados con la autorización explícita de las conexiones.  De forma predeterminada, es posible usar VPN Gateway para crear túneles no autorizados en entornos seguros.  Por tanto, es fundamental que las organizaciones usen el control de acceso basado en rol (RBAC) de Azure para controlar quién puede crear y modificar las instancias de VPN Gateway y sus conexiones.  Azure no tiene ningún rol "integrado" para administrar las instancias de VPN Gateway, por lo que necesitará un rol personalizado.

El acceso a los roles "propietario", "colaborador" y "colaborador de la red" está muy controlado.  También se recomienda usar Azure AD Privileged Identity Management para un control más granular del acceso.

### <a name="high-availability"></a>Alta disponibilidad

Las instancias de Azure VPN Gateway pueden tener varias conexiones (consulte el diagrama 1) y admitir varios dispositivos VPN locales en el mismo entorno local.  

Las redes virtuales de Azure pueden tener varias instancias de VPN Gateway que se pueden implementar en configuraciones independientes, activas-pasivas o activas-activas.

Se recomienda que todas las instancias de VPN Gateway se implementen en una [configuración de alta disponibilidad](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-highlyavailable): por ejemplo, dos dispositivos VPN locales conectados a dos instancias de VPN Gateway en modo activo-pasivo o activo-activo (consulte el diagrama 2).

![Conexiones redundantes de VPN Gateway](media/dual-redundancy.png)

*Diagrama 2: Instancias de VPN Gateway activas-activas y dos dispositivos VPN*

### <a name="forced-tunneling"></a>Tunelización forzada

La tunelización forzada redirige o "fuerza" todo el tráfico vinculado a Internet de nuevo al entorno local a través de VPN Gateway para la inspección y la auditoría. Sin la tunelización forzada, el tráfico vinculado a Internet desde las máquinas virtuales en Azure atraviesa la infraestructura de red de Azure hasta la red pública de Internet, sin la opción para inspeccionar o auditar el tráfico.  Esto es fundamental cuando se requiere que la organización use una puerta de enlace de Internet segura (SIG) para un entorno.

## <a name="detailed-configuration"></a>Configuración detallada

### <a name="service-attributes"></a>Atributos del servicio

Las instancias de VPN Gateway para conexiones S2S configuradas para el gobierno australiano deben tener los siguientes atributos:

|Atributo | DEBE|
|--- | --- |
|gatewayType | “VPN”|
|

La configuración del atributo necesaria para cumplir los controles del ISM para la protección es:

|Atributo | DEBE|
|--- |---|
|vpnType |“RouteBased”|
|vpnClientConfiguration/vpnClientProtocols | “IkeV2”|
|

Azure VPN Gateway admite un conjunto de algoritmos criptográficos de los estándares de los protocolos IPsec e IKE.  Los conjuntos de directivas predeterminados maximizan la interoperabilidad con una amplia gama de dispositivos VPN de terceros.  Como resultado, es posible que durante el protocolo de enlace de IKE se negocie una configuración no compatible.  Por lo tanto, se recomienda encarecidamente que los parámetros de la [directivas de IPsec/IKE personalizadas](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-ipsecikepolicy-rm-powershell) se apliquen a vpnClientConfiguration en las instancias de VPN Gateway para asegurarse de que las conexiones cumplen los controles de ISM para las conexiones del entorno local a Azure.  Los atributos clave son:

|Atributo|PUEDE|DEBE|
|---|---|---|
|saLifeTimeSeconds|<14 400 s|>300 s|
|saDataSizeKilobytes| |>1024 KB|
|ipsecEncryption| |AES256-GCMAES256|
|ipsecIntegrity| |SHA256-GCMAES256|
|ikeEncryption| |AES256-GCMAES256|
|ikeIntegrity| |SHA256-GCMAES256|
|dhGroup|DHGroup14, DHGroup24, ECP256, ECP384|DHGroup2|
|pfsGroup|PFS2048, PFS24, ECP256, ECP384||
|

*Para dhGroup y pfsGroup en la tabla anterior, se prefieren ECP256 y ECP384, aunque se pueden usar otras opciones de configuración*.

### <a name="related-services"></a>Servicios relacionados

Al diseñar y configurar una instancia de Azure VPN Gateway, hay una serie de servicios relacionados que también deben existir y configurarse:

|Servicio | Acción requerida|
|--- | ---|
|Virtual Network | Las instancias de VPN Gateway están conectadas a una red virtual.  Es necesario crear una red virtual antes de crear una instancia de VPN Gateway.|
|Dirección IP pública | Las puertas de enlace VPN S2S necesitan una dirección IP pública para establecer la conectividad entre el dispositivo VPN local y la instancia de VPN Gateway.  Debe crearse una dirección IP pública antes de crear una puerta de enlace VPN S2S.|
|Subnet | Es necesario crear una subred de la red virtual para la instancia de VPN Gateway.|
|

## <a name="implementation-steps-using-powershell"></a>Pasos de implementación con PowerShell

### <a name="role-based-access-control-rbac"></a>Control de acceso basado en rol (RBAC)

1. Cree un rol personalizado (por ejemplo, el colaborador virtualNetworkGateway).  Cree un rol que se asignará a los usuarios a los que se les permitirá crear y modificar instancias de VPN Gateway. El rol personalizado debe permitir las siguientes operaciones:

   Microsoft.Network/virtualNetworkGateways/*  
   Microsoft.Network/connections/*  
   Microsoft.Network/localnetworkgateways/*  
   Microsoft.Network/virtualNetworks/subnets/*  
   Microsoft.Network/publicIPAddresses/*  
   Microsoft.Network/publicIPPrefixes/*  
   Microsoft.Network/routeTables/*  

2. Agregue un rol personalizado a los usuarios que tengan permiso para crear y administrar las instancias de VPN Gateway y las conexiones a los entornos locales.

### <a name="create-vpn-gateway"></a>Creación de una instancia de VPN Gateway

*En estos pasos se supone que ya se ha creado una red virtual*.

1. Crear una dirección IP pública
2. Crear una subred de VPN Gateway
3. Crear la configuración de una dirección IP de VPN Gateway
4. Crear una instancia de VPN Gateway
5. Crear una puerta de enlace de red local para el dispositivo VPN local
6. Crear una directiva de IPsec (suponiendo que se usen directivas de IPsec/IKE personalizadas)
7. Crear una conexión entre VPN Gateway y la puerta de enlace de red local con la directiva de IPsec

### <a name="enforce-tunneling"></a>Aplicación de la tunelización

Si se requiere la tunelización forzada, antes de crear VPN Gateway:

1. Cree la tabla de enrutamiento y la regla de enrutamiento.
2. Asocie la tabla de enrutamiento a las subredes adecuadas.

Después de crear la instancia de VPN Gateway:

1. Establezca GatewayDefaultSite en el entorno local en VPN Gateway.

### <a name="example-powershell-script"></a>Script de PowerShell de ejemplo

Un script de PowerShell de ejemplo para crear una directiva de IPsec o IKE personalizada que cumpla los controles del ISM para la clasificación de seguridad PROTEGIDA de Australia.

Se supone que ya existen la red virtual, la red de VPN Gateway y las puertas de enlace locales.

#### <a name="create-an-ipsecike-policy"></a>Cree una directiva IPsec/IKE.

El script de ejemplo siguiente crea una directiva de IPsec o IKE con los algoritmos y parámetros siguientes:

- IKEv2: AES256, SHA256, DHGroup ECP256
- IPsec: AES256, SHA256, PFS ECP256, vigencia de SA de 14 400 segundos y 102 400 000 KB

```powershell
$custompolicy = New-AzIpsecPolicy `
                    -IkeEncryption AES256 `
                    -IkeIntegrity SHA256 `
                    -DhGroup ECP256 `
                    -IpsecEncryption AES256 `
                    -IpsecIntegrity SHA256 `
                    -PfsGroup ECP256 `
                    -SALifeTimeSeconds 14400 `
                    -SADataSizeKilobytes 102400000
```

#### <a name="create-a-s2s-vpn-connection-with-the-custom-ipsecike-policy"></a>Creación de una conexión VPN de sitio a sitio con una directiva IPsec/IKE personalizada

```powershell
$vpngw = Get-AzVirtualNetworkGateway `
                    -Name "<yourVPNGatewayName>" `
                    -ResourceGroupName "<yourResourceGroupName>"
$localgw = Get-AzLocalNetworkGateway  `
                    -Name "<yourLocalGatewayName>" `
                    -ResourceGroupName "<yourResourceGroupName>"

New-AzVirtualNetworkGatewayConnection `
                    -Name "ConnectionName" `
                    -ResourceGroupName "<yourResourceGroupName>" `
                    -VirtualNetworkGateway1 $vpngw `
                    -LocalNetworkGateway2 $localgw `
                    -Location "Australia Central" `
                    -ConnectionType IPsec `
                    -IpsecPolicies $custompolicy `
                    -SharedKey "AzureA1b2C3"
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo se ha tratado la configuración específica de VPN Gateway para cumplir los requisitos especificados en el manual de seguridad de la información (ISM) para proteger los datos PROTEGIDOS del gobierno australiano mientras están en tránsito. Para obtener pasos detallados para configurar VPN Gateway:

- [Información general sobre Azure Virtual Network Gateway](https://docs.microsoft.com/azure/vpn-gateway/)  
- [¿Qué es VPN Gateway?](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)  
- [Creación de una red virtual con una conexión VPN de sitio a sitio mediante Azure Resource Manager y PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell)  
- [Creación y administración de una instancia de VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-tutorial-create-gateway-powershell)