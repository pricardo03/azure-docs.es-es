---
title: Configuración de una VPN de punto a sitio (P2S) en Linux para su uso con Azure Files | Microsoft Docs
description: Cómo configurar una VPN de punto a sitio (P2S) en Linux para su uso con Azure Files
author: roygara
ms.service: storage
ms.topic: overview
ms.date: 10/19/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: b988435fc6928d52321cb427e2412e7ca81680d2
ms.sourcegitcommit: b45ee7acf4f26ef2c09300ff2dba2eaa90e09bc7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73126471"
---
# <a name="configure-a-point-to-site-p2s-vpn-on-linux-for-use-with-azure-files"></a>Configuración de una VPN de punto a sitio (P2S) en Linux para su uso con Azure Files
Puede usar una conexión VPN de punto a sitio (P2S) para montar los recursos compartidos de archivos de Azure a través de SMB desde fuera de Azure, sin necesidad de abrir el puerto 445. Una conexión VPN de punto a sitio es una conexión VPN entre Azure y un cliente individual. Para usar una conexión VPN de punto a sitio con Azure Files, se debe configurar una conexión VPN de punto a sitio para cada cliente que quiera conectarse. Si tiene muchos clientes que necesitan conectarse a sus recursos compartidos de archivos de Azure desde la red local, puede usar una conexión VPN de sitio a sitio (S2S) en lugar de una conexión de punto a sitio para cada cliente. Para obtener más información, consulte [Configuración de una VPN de sitio a sitio para su uso con Azure Files](storage-files-configure-s2s-vpn.md).

Se recomienda que lea [Introducción a las redes de Azure Files](storage-files-networking-overview.md) antes de continuar con este artículo para obtener una descripción completa de las opciones de red disponibles para Azure Files.

En este artículo se detallan los pasos para configurar una VPN de punto a sitio en Linux para montar recursos compartidos de archivos de Azure directamente en el entorno local. Si quiere enrutar el tráfico de Azure File Sync a través de una VPN, consulte [Configuración del proxy y el firewall de Azure File Sync](storage-sync-files-firewall-and-proxy.md).

## <a name="prerequisites"></a>Requisitos previos
- La versión más reciente de la CLI de Azure. Para obtener más información sobre cómo instalar la CLI de Azure, consulte [Instalación de la CLI de Azure PowerShell](https://docs.microsoft.com/cli/azure/install-azure-cli) y seleccione el sistema operativo. Si prefiere usar el módulo de Azure PowerShell en Linux, puede hacerlo. Sin embargo, las instrucciones siguientes son para la CLI de Azure.

- Un recurso compartido de archivos de Azure que le gustaría montar en el entorno local. Puede usar un [recurso compartido de archivos de Azure estándar](storage-how-to-create-file-share.md) o [Premium](storage-how-to-create-premium-fileshare.md) con la VPN de punto a sitio.

## <a name="install-required-software"></a>Instalación del software necesario
La puerta de enlace de red virtual de Azure puede ofrecer conexiones VPN mediante varios protocolos VPN, entre los que se incluyen IPsec y OpenVPN. En esta guía se muestra cómo usar IPsec y se usa el paquete strongSwan para proporcionar compatibilidad con Linux. 

> Comprobada con Ubuntu 18.10.

```bash
sudo apt install strongswan strongswan-pki libstrongswan-extra-plugins curl libxml2-utils cifs-utils

installDir="/etc/"
```

### <a name="deploy-a-virtual-network"></a>Implementar una red virtual 
Para acceder al recurso compartido de archivos de Azure y a otros recursos de Azure desde el entorno local a través de una VPN de punto a sitio, debe crear una red virtual o VNet. La conexión VPN de punto a sitio que creará automáticamente es un puente entre la máquina Linux local y esta red virtual de Azure.

El siguiente script creará una red virtual de Azure con tres subredes: una para el punto de conexión de servicio de la cuenta de almacenamiento, otra para el punto de conexión privado de la cuenta de almacenamiento (que es necesaria para acceder a la cuenta de almacenamiento local sin crear un enrutamiento personalizado para la dirección IP pública de la cuenta de almacenamiento que puede cambiar) y otra para la puerta de enlace de red virtual que proporciona el servicio VPN. 

No olvide reemplazar `<region>`, `<resource-group>` y `<desired-vnet-name>` por los valores correctos para su entorno.

```bash
region="<region>"
resourceGroupName="<resource-group>"
virtualNetworkName="<desired-vnet-name>"

virtualNetwork=$(az network vnet create \
    --resource-group $resourceGroupName \
    --name $virtualNetworkName \
    --location $region \
    --address-prefixes "192.168.0.0/16" \
    --query "newVNet.id" | tr -d '"')

serviceEndpointSubnet=$(az network vnet subnet create \
    --resource-group $resourceGroupName \
    --vnet-name $virtualNetworkName \
    --name "ServiceEndpointSubnet" \
    --address-prefixes "192.168.0.0/24" \
    --service-endpoints "Microsoft.Storage" \
    --query "id" | tr -d '"')

privateEndpointSubnet=$(az network vnet subnet create \
    --resource-group $resourceGroupName \
    --vnet-name $virtualNetworkName \
    --name "PrivateEndpointSubnet" \
    --address-prefixes "192.168.1.0/24" \
    --query "id" | tr -d '"')

gatewaySubnet=$(az network vnet subnet create \
    --resource-group $resourceGroupName \
    --vnet-name $virtualNetworkName \
    --name "GatewaySubnet" \
    --address-prefixes "192.168.2.0/24" \
    --query "id" | tr -d '"')
```

## <a name="restrict-the-storage-account-to-the-virtual-network"></a>Restricción de la cuenta de almacenamiento a la red virtual
De forma predeterminada, al crear una cuenta de almacenamiento, puede acceder a ella desde cualquier lugar del mundo siempre que tenga los medios para autenticar la solicitud (por ejemplo, con la identidad de Active Directory o con la clave de la cuenta de almacenamiento). Para restringir el acceso de esta cuenta de almacenamiento a la red virtual que acaba de crear, debe crear un conjunto de reglas de red que permita el acceso dentro de la red virtual y rechace todos los demás accesos.

Para restringir la cuenta de almacenamiento a la red virtual, es necesario usar un punto de conexión de servicio. El punto de conexión de servicio es una construcción de red a través de la que solo se puede tener acceso a la dirección IP pública o de DNS desde dentro de la red virtual. Puesto que no se garantiza que la dirección IP pública siga siendo la misma, es preferible usar un punto de conexión privado en lugar de un punto de conexión de servicio para la cuenta de almacenamiento. Sin embargo, no es posible restringir la cuenta de almacenamiento a menos que también se exponga un punto de conexión de servicio.

No olvide reemplazar `<storage-account-name>` por la cuenta de almacenamiento a la que quiere acceder.

```bash
storageAccountName="<storage-account-name>"

az storage account network-rule add \
    --resource-group $resourceGroupName \
    --account-name $storageAccountName \
    --subnet $serviceEndpointSubnet > /dev/null

az storage account update \
    --resource-group $resourceGroupName \
    --name $storageAccountName \
    --bypass "AzureServices" \
    --default-action "Deny" > /dev/null
```

## <a name="create-a-private-endpoint-preview"></a>Creación de un punto de conexión privado (versión preliminar)
Al crear un punto de conexión privado para la cuenta de almacenamiento, la cuenta de almacenamiento proporciona una dirección IP dentro del espacio de direcciones IP de la red virtual. Al montar el recurso compartido de archivos de Azure desde el entorno local mediante esta dirección IP privada, las reglas de enrutamiento autodefinidas por la instalación de VPN enrutarán la solicitud de montaje a la cuenta de almacenamiento a través de la VPN. 

```bash
zoneName="privatelink.file.core.windows.net"

storageAccount=$(az storage account show \
    --resource-group $resourceGroupName \
    --name $storageAccountName \
    --query "id" | tr -d '"')

az resource update \
    --ids $privateEndpointSubnet \
    --set properties.privateEndpointNetworkPolicies=Disabled > /dev/null

az network private-endpoint create \
    --resource-group $resourceGroupName \
    --name "$storageAccountName-PrivateEndpoint" \
    --location $region \
    --subnet $privateEndpointSubnet \
    --private-connection-resource-id $storageAccount \
    --group-ids "file" \
    --connection-name "privateEndpointConnection" > /dev/null

az network private-dns zone create \
    --resource-group $resourceGroupName \
    --name $zoneName > /dev/null

az network private-dns link vnet create \
    --resource-group $resourceGroupName \
    --zone-name $zoneName \
    --name "$virtualNetworkName-link" \
    --virtual-network $virtualNetworkName \
    --registration-enabled false > /dev/null

networkInterfaceId=$(az network private-endpoint show \
    --name "$storageAccountName-PrivateEndpoint" \
    --resource-group $resourceGroupName \
    --query 'networkInterfaces[0].id' | tr -d '"')
 
storageAccountPrivateIP=$(az resource show \
    --ids $networkInterfaceId \
    --api-version 2019-04-01 \
    --query "properties.ipConfigurations[0].properties.privateIPAddress" | tr -d '"')

fqdnQuery="properties.ipConfigurations[0].properties.privateLinkConnectionProperties.fqdns[0]"
fqdn=$(az resource show \
    --ids $networkInterfaceId \
    --api-version 2019-04-01 \
    --query $fqdnQuery | tr -d '"')

az network private-dns record-set a create \
    --name $storageAccountName \
    --zone-name $zoneName \
    --resource-group $resourceGroupName > /dev/null
```

## <a name="create-certificates-for-vpn-authentication"></a>Creación de certificados para la autenticación de VPN
Para que las conexiones VPN de las máquinas Linux locales se autentiquen para acceder a la red virtual, debe crear dos certificados: un certificado raíz, que se proporcionará a la puerta de enlace de máquina virtual, y un certificado de cliente, que se firmará con el certificado raíz. El siguiente script crea los certificados necesarios.

```bash
rootCertName="P2SRootCert"
username="client"
password="1234"

mkdir temp
cd temp

sudo ipsec pki --gen --outform pem > rootKey.pem
sudo ipsec pki --self --in rootKey.pem --dn "CN=$rootCertName" --ca --outform pem > rootCert.pem

rootCertificate=$(openssl x509 -in rootCert.pem -outform der | base64 -w0 ; echo)

sudo ipsec pki --gen --size 4096 --outform pem > "clientKey.pem"
sudo ipsec pki --pub --in "clientKey.pem" | \
    sudo ipsec pki \
        --issue \
        --cacert rootCert.pem \
        --cakey rootKey.pem \
        --dn "CN=$username" \
        --san $username \
        --flag clientAuth \
        --outform pem > "clientCert.pem"

openssl pkcs12 -in "clientCert.pem" -inkey "clientKey.pem" -certfile rootCert.pem -export -out "client.p12" -password "pass:$password"
```

## <a name="deploy-virtual-network-gateway"></a>Implementación de puerta de enlace de red virtual
La puerta de enlace de red virtual de Azure es el servicio al que se conectarán las máquinas Linux locales. La implementación de este servicio requiere dos componentes básicos: una dirección IP pública que identificará la puerta de enlace para los clientes dondequiera que se encuentren en el mundo y un certificado raíz creado anteriormente que se usará para autenticar a los clientes.

No olvide reemplazar `<desired-vpn-name-here>` por el nombre que quiera para estos recursos.

> [!Note]  
> La implementación de una puerta de enlace de red virtual de Azure puede tardar hasta 45 minutos. Aunque este recurso va a implementarse, el script de Bash se bloqueará para que se complete la implementación. Se espera que esto sea así.

```bash
vpnName="<desired-vpn-name-here>"
publicIpAddressName="$vpnName-PublicIP"

publicIpAddress=$(az network public-ip create \
    --resource-group $resourceGroupName \
    --name $publicIpAddressName \
    --location $region \
    --sku "Basic" \
    --allocation-method "Dynamic" \
    --query "publicIp.id" | tr -d '"')

az network vnet-gateway create \
    --resource-group $resourceGroupName \
    --name $vpnName \
    --vnet $virtualNetworkName \
    --public-ip-addresses $publicIpAddress \
    --location $region \
    --sku "VpnGw1" \
    --gateway-typ "Vpn" \
    --vpn-type "RouteBased" \
    --address-prefixes "172.16.201.0/24" \
    --client-protocol "IkeV2" > /dev/null

az network vnet-gateway root-cert create \
    --resource-group $resourceGroupName \
    --gateway-name $vpnName \
    --name $rootCertName \
    --public-cert-data $rootCertificate \
    --output none
```

## <a name="configure-the-vpn-client"></a>Configuración del cliente VPN
La puerta de enlace de red virtual de Azure creará un paquete descargable con los archivos de configuración necesarios para inicializar la conexión VPN en la máquina Linux local. El siguiente script colocará los certificados creados en el lugar correcto y configurará el archivo `ipsec.conf` con los valores correctos del archivo de configuración en el paquete descargable.

```bash
vpnClient=$(az network vnet-gateway vpn-client generate \
    --resource-group $resourceGroupName \
    --name $vpnName \
    --authentication-method EAPTLS | tr -d '"')

curl $vpnClient --output vpnClient.zip
unzip vpnClient.zip

vpnServer=$(xmllint --xpath "string(/VpnProfile/VpnServer)" Generic/VpnSettings.xml)
vpnType=$(xmllint --xpath "string(/VpnProfile/VpnType)" Generic/VpnSettings.xml | tr '[:upper:]' '[:lower:]')
routes=$(xmllint --xpath "string(/VpnProfile/Routes)" Generic/VpnSettings.xml)

sudo cp "${installDir}ipsec.conf" "${installDir}ipsec.conf.backup"
sudo cp "Generic/VpnServerRoot.cer" "${installDir}ipsec.d/cacerts"
sudo cp "${username}.p12" "${installDir}ipsec.d/private" 

echo -e "\nconn $virtualNetworkName" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\tkeyexchange=$vpnType" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\ttype=tunnel" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\tleftfirewall=yes" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\tleft=%any" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\tleftauth=eap-tls" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\tleftid=%client" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\tright=$vpnServer" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\trightid=%$vpnServer" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\trightsubnet=$routes" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\tleftsourceip=%config" | sudo tee -a "${installDir}ipsec.conf" > /dev/null 
echo -e "\tauto=add" | sudo tee -a "${installDir}ipsec.conf" > /dev/null

echo ": P12 client.p12 '$password'" | sudo tee -a "${installDir}ipsec.secrets" > /dev/null

sudo ipsec restart
sudo ipsec up $virtualNetworkName 
```

## <a name="mount-azure-file-share"></a>Montaje de un recurso compartido de archivos de Azure
Ahora que ha configurado la VPN de punto a sitio, puede montar el recurso compartido de archivos de Azure. En el ejemplo siguiente se montará el recurso compartido de forma no persistente. Para montar de forma persistente, consulte [Uso de un recurso compartido de archivos de Azure con Linux](storage-how-to-use-files-linux.md). 

```bash
fileShareName="myshare"

mntPath="/mnt/$storageAccountName/$fileShareName"
sudo mkdir -p $mntPath

storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroupName \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"')

smbPath="//$storageAccountPrivateIP/$fileShareName"
sudo mount -t cifs $smbPath $mntPath -o vers=3.0,username=$storageAccountName,password=$storageAccountKey,serverino
```

## <a name="see-also"></a>Otras referencias
- [Introducción a las redes de Azure Files](storage-files-networking-overview.md)
- [Configuración de una VPN de punto a sitio (P2S) en Windows para su uso con Azure Files](storage-files-configure-p2s-vpn-windows.md)
- [Configuración de una VPN de sitio a sitio (S2S) para su uso con Azure Files](storage-files-configure-s2s-vpn.md)