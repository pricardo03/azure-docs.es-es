---
title: 'Creación, cambio o eliminación de una instancia de Virtual Network TAP: CLI de Azure | Microsoft Docs'
description: Aprenda a crear, cambiar o eliminar una instancia de Virtual Network TAP mediante la CLI de Azure.
services: virtual-network
documentationcenter: na
author: karthikananth
manager: ganesr
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/17/2018
ms.author: kaanan
ms.openlocfilehash: 36de5ec6f7384663106bfb88ee9f236cced6930a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46997954"
---
# <a name="work-with-a-virtual-network-tap-using-the-azure-cli"></a>Uso de una instancia de Virtual Network TAP con la CLI de Azure

Azure Virtual Network TAP (punto de acceso del terminal) permite transmitir continuamente el tráfico de red de la máquina virtual a un recopilador de paquetes de red o a una herramienta de análisis. Un asociado de la [aplicación virtual de red](https://azure.microsoft.com/solutions/network-appliances/) proporciona el recopilador o la herramienta de análisis de la herramienta. Para ver una lista de soluciones de asociados que estén validadas para funcionar con Virtual Network TAP, consulte las [soluciones de asociados](virtual-network-tap-overview.md#virtual-network-tap-partner-solutions). 

## <a name="create-a-virtual-network-tap-resource"></a>Creación de un recurso de Virtual Network TAP

Antes de crear un recurso de Virtual Network TAP, lea los [requisitos previos](virtual-network-tap-overview.md#prerequisites). Puede ejecutar los comandos siguientes en [Azure Cloud Shell](https://shell.azure.com/bash), o mediante la ejecución de la interfaz de la línea de comandos (CLI) de Azure en el equipo. Azure Cloud Shell es un shell interactivo gratuito, que no requiere la instalación de la CLI de Azure en el equipo. Debe iniciar sesión Azure con una cuenta que tenga los [permisos](virtual-network-tap-overview.md#permissions) adecuados. En este artículo se necesita la CLI de Azure versión 2.0.46 o posterior. Ejecute `az --version` para buscar la versión instalada. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0](/cli/azure/install-azure-cli). Si ejecuta de forma local la CLI de Azure, también debe ejecutar `az login` para crear una conexión con Azure.

1. Recupere el identificador de su suscripción en una variable que se usará en un paso posterior:

   ```azurecli-interactive
   subscriptionId=$(az account show \
   --query id \
   --out tsv)
   ```

2. Establezca el identificador de suscripción que usará para crear un recurso de Virtual Network TAP.

   ```azurecli-interactive
   az account set --subscription $subscriptionId
   ```

3. Vuelva a registrar el identificador de suscripción que usará para crear un recurso de Virtual Network TAP. Si recibe un error de registro al crear un recurso TAP, ejecute el siguiente comando:

   ```azurecli-interactive
   az provider register --namespace Microsoft.Network --subscription $subscriptionId
   ```

4. Si el destino de Virtual Network TAP es la interfaz de red en la aplicación virtual de red del recopilador o de la herramienta de análisis:

   - Recupere la configuración IP de la interfaz de red de la aplicación virtual de red en una variable que se usará en un paso posterior. El identificador es el punto de conexión que agregará el tráfico TAP. En el ejemplo siguiente se recupera el identificador de la configuración IP *ipconfig1* de una interfaz de red llamada *myNetworkInterface*, en un grupo de recursos llamado *myResourceGroup*:

      ```azurecli-interactive
       IpConfigId=$(az network nic ip-config show \
       --name ipconfig1 \
       --nic-name myNetworkInterface \
       --resource-group myResourceGroup \
       --query id \
       --out tsv)
      ```

   - Cree la instancia de Virtual Network TAP en la región westcentralus azure y use el identificador de la configuración IP como destino y una propiedad de puerto opcional. El puerto especifica el puerto de destino en la configuración IP de la interfaz de red donde se recibirá el tráfico TAP:  

      ```azurecli-interactive
       az network vnet tap create \
       --resource-group myResourceGroup \
       --name myTap \
       --destination $IpConfigId \
       --port 4789 \
       --location westcentralus
      ```

5. Si el destino de Virtual Network TAP es un equilibrador de carga interno de Azure:
  
   - Recupere la configuración IP de front-end del equilibrador de carga interno de Azure en una variable que se usará en un paso posterior. El identificador es el punto de conexión que agregará el tráfico TAP. En el ejemplo siguiente se recupera el identificador de la configuración IP del front-end *frontendipconfig1* para un equilibrador de carga llamado *myInternalLoadBalancer*, en un grupo de recursos llamado *myResourceGroup*:

      ```azurecli-interactive
      FrondendIpConfigId=$(az network lb fronend-ip show \
      --name frontendipconfig1 \
      --lb-name myInternalLoadBalancer \
      --resource-group myResourceGroup \
      --query id \
      --out tsv)
      ```
   - Cree la instancia de Virtual Network TAP y use el identificador de la configuración IP de front-end como destino y una propiedad de puerto opcional. El puerto especifica el puerto de destino en la configuración IP del front-end donde se recibirá el tráfico TAP:  

      ```azurecli-interactive
      az network vnet tap create \
      --resource-group myResourceGroup \
      --name myTap \
      --destination $FrontendIpConfigId \
      --port 4789 \
     --location westcentralus
     ```

6. Confirme la creación de la instancia de Virtual Network TAP:

   ```azurecli-interactive
   az network vnet tap show \
   --resource-group myResourceGroup
   --name myTap
   ```

## <a name="add-a-tap-configuration-to-a-network-interface"></a>Agregar una configuración de TAP a una interfaz de red

1. Recupere el identificador de un recurso de Virtual Network TAP existente. En el ejemplo siguiente se recupera una instancia de Virtual Network TAP denominada *myTap* de un grupo de recursos llamado *myResourceGroup*:

   ```azurecli-interactive
   tapId=$(az network tap show show \
   --name myTap \
   --resource-group myResourceGroup \
   --query id \
   --out tsv)
   ```

2. Cree una configuración de TAP en la interfaz de red de la máquina virtual supervisada. En el ejemplo siguiente se crea una configuración de TAP para una interfaz de red llamada *myNetworkInterface*:

   ```azurecli-interactive
   az network nic vtap-config create \
   --resource-group myResourceGroup \
   --nic myNetworkInterface \
   --vnet-tap $tapId \
   --name mytapconfig \
   --subscription subscriptionId
   ```

3. Confirme la creación de la configuración de TAP:

   ```azurecli-interactive
   az network nic vtap-config show \
   --resource-group myResourceGroup \
   --nic-name myNetworkInterface \
   --name mytapconfig \
   --subscription subscriptionId
   ```

## <a name="delete-the-tap-configuration-on-a-network-interface"></a>Eliminar la configuración de TAP en una interfaz de red

   ```azure-cli-interactive
   az network nic vtap-config delete \
   --resource-group myResourceGroup \
   --nic myNetworkInterface \
   --tap-configuration-name myTapConfig \
   --subscription subscriptionId
   ```

## <a name="list-virtual-network-taps-in-a-subscription"></a>Enumerar las instancias de Virtual Network TAP de una suscripción

   ```azurecli-interactive
   az network vnet tap list
   ```

## <a name="delete-a-virtual-network-tap-in-a-resource-group"></a>Eliminar una instancia de Virtual Network TAP de un grupo de recursos

   ```azurecli-interactive
   az network vnet tap delete \
   --resource-group myResourceGroup \
   --name myTap
   ```