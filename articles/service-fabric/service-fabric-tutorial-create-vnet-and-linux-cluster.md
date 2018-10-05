---
title: Creación de un clúster de Service Fabric con Linux en Azure | Microsoft Docs
description: En este tutorial, aprenderá a implementar mediante la CLI de Azure un clúster de Service Fabric con Linux en una red virtual existente de Azure.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/27/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 27600cd4656f70b4cd01745667c0e0fd2a2f4997
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405826"
---
# <a name="tutorial-deploy-a-linux-service-fabric-cluster-into-an-azure-virtual-network"></a>Tutorial: Implementación de un clúster de Service Fabric de Linux en una red virtual de Azure

Este tutorial es la primera parte de una serie. Aprenderá a implementar un clúster de Service Fabric con Linux en una [red virtual de Azure (VNET)](../virtual-network/virtual-networks-overview.md) mediante la CLI de Azure y una plantilla. Cuando haya terminado, tendrá un clúster que se ejecuta en la nube en el que puede implementar aplicaciones. Para crear un clúster con Windows mediante PowerShell, consulte la información sobre la [creación de un clúster con Windows seguro en Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una red virtual en Azure mediante la CLI de Azure
> * Creación de un clúster de Service Fabric seguro en Azure mediante la CLI de Azure
> * Protección del clúster con un certificado X.509
> * Conexión al clúster mediante la CLI de Service Fabric
> * Eliminación de un clúster

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * Crear un clúster seguro en Azure
> * [Escalado o reducción horizontal](service-fabric-tutorial-scale-cluster.md)
> * [Actualización del entorno en tiempo de ejecución de un clúster](service-fabric-tutorial-upgrade-cluster.md)
> * [Eliminación de un clúster](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este tutorial:

* Si no tiene ninguna suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Instale la [CLI de Service Fabric](service-fabric-cli.md).
* Instale la [CLI de Azure](/cli/azure/install-azure-cli).

Los siguientes procedimientos crean un clúster de Service Fabric de cinco nodos. Para calcular el costo producido por la ejecución de un clúster de Service Fabric en Azure, use la [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/).

## <a name="key-concepts"></a>Conceptos clave

Un [clúster de Service Fabric](service-fabric-deploy-anywhere.md) es un conjunto de máquinas físicas o virtuales conectadas a la red, en las que se implementan y administran los microservicios. Los clústeres pueden escalar a miles de equipos. Cada una de las máquinas físicas o virtuales que forman parte de un clúster se denominan nodo. A cada nodo se le asigna un nombre de nodo (una cadena). Los nodos tienen características como las propiedades de colocación.

Un tipo de nodo define el tamaño, el número y las propiedades de un conjunto de máquinas virtuales en el clúster. Cada tipo de nodo definido se configura como un [conjunto de escalado de máquinas virtuales](/azure/virtual-machine-scale-sets/), un recurso de proceso de Azure que se puede usar para implementar y administrar una colección de máquinas virtuales de forma conjunta. Cada tipo de nodo se puede escalar o reducir verticalmente de forma independiente. Cada uno tiene diferentes conjuntos de puertos abiertos y puede tener distintas métricas de capacidad. Los tipos de nodo se utilizan para definir los roles para un conjunto de nodos de clúster, por ejemplo, "front-end" o "back-end".  El clúster puede tener más de un tipo de nodo, pero el tipo de nodo principal debe tener al menos cinco máquinas virtuales para los clústeres de producción (o al menos tres máquinas virtuales en clústeres de prueba).  [Los servicios del sistema de Service Fabric](service-fabric-technical-overview.md#system-services) se colocan en los dos del tipo de nodo principal.

El clúster está protegido mediante un certificado de clúster. Un certificado de clúster es un certificado X.509 que se usa para proteger la comunicación de nodo a nodo y autenticar los puntos de conexión de administración del clúster en un cliente de administración.  El certificado de clúster también proporciona SSL para la API de administración de HTTPS y para Service Fabric Explorer a través de HTTPS. Los certificados autofirmados son útiles para clústeres de prueba.  Para los clústeres de producción, use un certificado de una entidad de certificación (CA) como certificado del clúster.

El certificado de clúster debe:

* contener una clave privada.
* crearse para el intercambio de claves, que se pueda exportar a un archivo Personal Information Exchange (.pfx).
* tener un nombre de sujeto que coincida con el dominio que se usa para acceder al clúster de Service Fabric. Esta coincidencia es un requisito para proporcionar SSL a los puntos de conexión de administración HTTPS y de Service Fabric Explorer del clúster. No puede obtener un certificado SSL de una entidad de certificación (CA) para el dominio .cloudapp.azure.com. Debe adquirir un nombre de dominio personalizado para el clúster. Cuando solicite un certificado de una CA, el nombre de sujeto del certificado debe coincidir con el nombre del dominio personalizado del clúster.

Azure Key Vault se usa para administrar certificados para clústeres de Service Fabric en Azure.  Cuando un clúster se implementa en Azure, el proveedor de recursos de Azure responsable de crear clústeres de Service Fabric extrae los certificados de Key Vault y los instala en las máquinas virtuales del clúster.

Este tutorial implementa un clúster de cinco nodos en un tipo de nodo único. No obstante, en cualquier implementación del clúster de producción, el [planeamiento de la capacidad](service-fabric-cluster-capacity.md) es un paso importante. Esto es algo que hay que tener en cuenta como parte de ese proceso.

* El número de nodos y de tipos de nodo que necesita el clúster
* Propiedades de cada tipo de nodo (por ejemplo, tamaño, principal, accesible desde Internet, número de máquinas virtuales)
* Características de confiabilidad y durabilidad del clúster

## <a name="download-and-explore-the-template"></a>Descarga y exploración de la plantilla

Descargue los siguientes archivos de plantilla de Resource Manager:

* [AzureDeploy.json][template]
* [AzureDeploy.Parameters.json][parameters]

Esta plantilla implementa un clúster seguro de cinco máquinas virtuales y un tipo de nodo único en una red virtual.  Se pueden encontrar otras plantillas de ejemplo en [GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates). El archivo [AzureDeploy.json][template] permite implementar varios recursos, como los siguientes.

### <a name="service-fabric-cluster"></a>Clúster de Service Fabric

Se implementa un clúster de Linux con las siguientes características:

* un tipo de nodo único
* cinco nodos en el tipo de nodo principal (configurable en los parámetros de la plantilla)
* Sistema operativo: Ubuntu 16.04 LTS (configurable en los parámetros de la plantilla)
* protección con certificado (configurable en los parámetros de la plantilla)
* se habilita el [servicio DNS](service-fabric-dnsservice.md)
* [Nivel de durabilidad](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster): Bronze (configurable en los parámetros de plantilla)
* [Nivel de confiabilidad](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster): Silver (configurable en los parámetros de plantilla)
* punto de conexión de la conexión de cliente: 19000 (configurable en los parámetros de la plantilla)
* punto de conexión de la puerta de enlace HTTP: 19080 (configurable en los parámetros de la plantilla)

### <a name="azure-load-balancer"></a>Azure Load Balancer

Se implementa un equilibrador de carga y se configuran sondeos y reglas para los siguientes puertos:

* punto de conexión de la conexión de cliente: 19000
* punto de conexión de la puerta de enlace HTTP: 19080
* puerto de la aplicación: 80
* puerto de la aplicación: 443

### <a name="virtual-network-and-subnet"></a>Red virtual y subred

Los nombres de la red virtual y la subred se declaran en los parámetros de la plantilla.  Los espacios de direcciones de la red virtual y de la subred también se declaran en los parámetros de la plantilla:

* espacio de direcciones de red virtual: 10.0.0.0/16
* espacio de direcciones de subred de Service Fabric: 10.0.2.0/24

Si se necesitan otros puertos de aplicación, deberá ajustar el recurso Microsoft.Network/loadBalancers para permitir el tráfico de entrada.

## <a name="set-template-parameters"></a>Configuración de los parámetros de plantilla

El archivo de parámetros [AzureDeploy.Parameters][parameters] permite declarar muchos valores que se usan para implementar el clúster y los recursos asociados. Estos son algunos de los parámetros que debe modificar para su implementación:

|Parámetro|Valor de ejemplo|Notas|
|---|---||
|adminUserName|vmadmin| Nombre de usuario del administrador de las máquinas virtuales del clúster. |
|adminPassword|Password#1234| Contraseña del administrador de las máquinas virtuales del clúster.|
|clusterName|mysfcluster123| Nombre del clúster. |
|location|southcentralus| Ubicación del clúster. |
|certificateThumbprint|| <p>El valor debe estar vacío si se va a crear un certificado autofirmado o a proporcionar un archivo de certificados.</p><p>Para usar un certificado existente cargado previamente en un almacén de claves, rellene el valor de huella digital SHA1 del certificado. Por ejemplo, "6190390162C988701DB5676EB81083EA608DCCF3". </p>|
|certificateUrlValue|| <p>El valor debe estar vacío si se va a crear un certificado autofirmado o a proporcionar un archivo de certificados.</p><p>Para usar un certificado existente cargado previamente en un almacén de claves, especifique la dirección URL del certificado. Por ejemplo, "https://mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346".</p>|
|sourceVaultValue||<p>El valor debe estar vacío si se va a crear un certificado autofirmado o a proporcionar un archivo de certificados.</p><p>Para usar un certificado existente cargado previamente en un almacén de claves, especifique el valor del almacén de claves de origen. Por ejemplo, "/subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT".</p>|

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>Implementación de la red virtual y el clúster

A continuación, configure la topología de red e implemente el clúster de Service Fabric. La plantilla [AzureDeploy.json][template] de Resource Manager crea una red virtual (VNET) y una subred para Service Fabric. La plantilla permite también implementar un clúster con seguridad mediante certificados habilitada.  Para los clústeres de producción, use un certificado de una entidad de certificación (CA) como certificado del clúster. Un certificado autofirmado se puede usar para proteger los clústeres de prueba.

### <a name="create-a-cluster-using-an-existing-certificate"></a>Creación de un clúster mediante un certificado existente

El script siguiente usa el comando [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) y una plantilla para implementar un clúster nuevo protegido con un certificado existente. El comando también crea un nuevo almacén de claves de Azure y carga el certificado.

```azurecli
ResourceGroupName="sflinuxclustergroup"
Location="southcentralus"
Password="q6D7nN%6ck@6"
VaultName="linuxclusterkeyvault"
VaultGroupName="linuxclusterkeyvaultgroup"
CertPath="C:\MyCertificates\MyCertificate.pem"

# sign in to your Azure account and select your subscription
az login
az account set --subscription <guid>

# Create a new resource group for your deployment and give it a name and a location.
az group create --name $ResourceGroupName --location $Location

# Create the Service Fabric cluster.
az sf cluster create --resource-group $ResourceGroupName --location $Location \
   --certificate-password $Password --certificate-file $CertPath \
   --vault-name $VaultName --vault-resource-group $ResourceGroupName  \
   --template-file AzureDeploy.json --parameter-file AzureDeploy.Parameters.json
```

### <a name="create-a-cluster-using-a-new-self-signed-certificate"></a>Creación de un clúster mediante un nuevo certificado autofirmado

El script siguiente usa el comando [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) y una plantilla para implementar un clúster nuevo en Azure. El comando también crea un almacén de claves en Azure, agrega un nuevo certificado autofirmado al almacén de claves y carga el certificado en el archivo de certificado de forma local.

```azurecli
ResourceGroupName="sflinuxclustergroup"
ClusterName="sflinuxcluster"
Location="southcentralus"
Password="q6D7nN%6ck@6"
VaultName="linuxclusterkeyvault"
VaultGroupName="linuxclusterkeyvaultgroup"
CertPath="C:\MyCertificates"

az sf cluster create --resource-group $ResourceGroupName --location $Location --cluster-name $ClusterName --template-file C:\temp\cluster\AzureDeploy.json --parameter-file C:\temp\cluster\AzureDeploy.Parameters.json --certificate-password $Password --certificate-output-folder $CertPath --certificate-subject-name $ClusterName.$Location.cloudapp.azure.com --vault-name $VaultName --vault-resource-group $ResourceGroupName
```

## <a name="connect-to-the-secure-cluster"></a>Conexión al clúster seguro

Conéctese al clúster mediante el comando `sfctl cluster select` de la CLI de Service Fabric con su clave.  Nota: Use la opción **--no-verify** solo para obtener un certificado autofirmado.

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Compruebe que está conectado y que el estado del clúster es correcto mediante el comando `sfctl cluster health`.

```azurecli
sfctl cluster health
```

## <a name="clean-up-resources"></a>Limpieza de recursos

En el resto de artículos de esta serie de tutoriales se usa el clúster que se acaba de crear. Si no va a pasar inmediatamente al siguiente artículo, puede [eliminar el clúster](service-fabric-cluster-delete.md) para evitar incurrir en cargos.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Creación de una red virtual en Azure mediante la CLI de Azure
> * Creación de un clúster de Service Fabric seguro en Azure mediante la CLI de Azure
> * Protección del clúster con un certificado X.509
> * Conexión al clúster mediante la CLI de Service Fabric
> * Eliminación de un clúster

Luego, avance hasta el tutorial siguiente para obtener información sobre cómo escalar el clúster.
> [!div class="nextstepaction"]
> [Escalado de un clúster](service-fabric-tutorial-scale-cluster.md)

[template]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/5-VM-Ubuntu-1-NodeTypes-Secure/AzureDeploy.json
[parameters]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/5-VM-Ubuntu-1-NodeTypes-Secure/AzureDeploy.Parameters.json
