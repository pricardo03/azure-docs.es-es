---
title: 'Guía de inicio rápido: Dirección del tráfico web con Azure Application Gateway: CLI de Azure | Microsoft Docs'
description: Obtenga información acerca de cómo utilizar la CLI de Azure para crear una instancia de Azure Application Gateway que dirija el tráfico web a las máquinas virtuales de un grupo de back-end.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 1/8/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: a4f6cc2af7b9e044e5a72767898f876932fbf973
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66133939"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-cli"></a>Inicio rápido: Dirección del tráfico web con Azure Application Gateway: CLI de Azure

En este artículo de inicio rápido se muestra cómo usar la CLI de Azure para crear una puerta de enlace de aplicaciones.  Después de crear la puerta de enlace de aplicaciones, pruébela para asegurarse de que funciona correctamente. Con Azure Application Gateway, puede dirigir el tráfico web de la aplicación a recursos específicos mediante la asignación de agentes de escucha a los puertos, la creación de reglas y la adición de recursos a un grupo de back-end. Para simplificar, en este artículo se usa una configuración sencilla con una dirección IP de front-end pública, un agente de escucha básico para hospedar un único sitio en esta puerta de enlace de aplicaciones, dos máquinas virtuales que se usan con el grupo de back-end y una regla de enrutamiento de solicitudes básica.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Requisitos previos

### <a name="azure-cli"></a>Azure CLI

Si decide instalar y usar la CLI localmente, ejecute la CLI de Azure versión 2.0.4 o posterior. Para averiguar la versión, ejecute el comando **az --version**. Para más información sobre la instalación o actualización, consulte [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).

### <a name="resource-group"></a>Grupos de recursos

En Azure, puede asignar recursos relacionados a un grupo de recursos. Para crear un grupo de recursos, use [az group create](/cli/azure/group#az-group-create). 

En el ejemplo siguiente, se crea un grupo de recursos llamado *myResourceGroupAG* en la ubicación *eastus*.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

### <a name="required-network-resources"></a>Recursos de red necesarios 

Para que Azure se comunique entre los recursos que se crean, se necesita una red virtual.  La subred de la puerta de enlace de aplicaciones solo puede contener puertas de enlace de aplicaciones. No se permite ningún otro recurso.  Puede crear una subred para Application Gateway o usar una existente. En este ejemplo se crean dos subredes: una para la puerta de enlace de aplicaciones y la otra para los servidores back-end. Puede configurar la dirección IP de front-end de Application Gateway para que sea pública o privada, según el caso de uso. En este ejemplo, elegimos una IP de front-end pública.

Cree la red virtual y una subred mediante [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create). Cree la dirección IP pública mediante [az network public-ip create](/cli/azure/network/public-ip).

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create \
  --name myBackendSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet   \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

### <a name="backend-servers"></a>Servidores back-end

Los servidores back-end pueden constar de NIC, conjuntos de escalado de máquinas virtuales, direcciones IP públicas e internas, nombres de dominio completos (FQDN) y servidores back-end multiinquilino como Azure App Service. En este ejemplo, se crean dos máquinas virtuales que usará Azure como servidores back-end para la puerta de enlace de aplicaciones. También se instala IIS en las máquinas virtuales para comprobar que Azure ha instalado correctamente la puerta de enlace de aplicaciones.

#### <a name="create-two-virtual-machines"></a>Creación de dos máquinas virtuales

Instale el [servidor web NGINX](https://docs.nginx.com/nginx/) en las máquinas virtuales para verificar que la puerta de enlace de aplicaciones se ha creado correctamente. Puede usar un archivo de configuración cloud-init para instalar NGINX y ejecutar una aplicación Node.js "Hola mundo" en una máquina virtual Linux. Para más información sobre cloud-init, consulte [Compatibilidad con cloud-init para máquinas virtuales en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/using-cloud-init).

En Azure Cloud Shell, copie y pegue la siguiente configuración en un archivo llamado *cloud-init.txt*. Escriba *editor cloud-init.txt* para crear el archivo.

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

Cree las interfaces de red con [az network nic create](/cli/azure/network/nic#az-network-nic-create). Para crear las máquinas virtuales, use [az vm create](/cli/azure/vm#az-vm-create).

```azurecli-interactive
for i in `seq 1 2`; do
  az network nic create \
    --resource-group myResourceGroupAG \
    --name myNic$i \
    --vnet-name myVNet \
    --subnet myBackendSubnet
  az vm create \
    --resource-group myResourceGroupAG \
    --name myVM$i \
    --nics myNic$i \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
done
```

## <a name="create-the-application-gateway"></a>Creación de la puerta de enlace de aplicaciones

Cree una puerta de enlace de aplicaciones mediante [az network application-gateway create](/cli/azure/network/application-gateway). Cuando se crea una puerta de enlace de aplicaciones mediante la CLI de Azure, se especifica información de configuración, como capacidad, SKU y HTTP. Posteriormente, Azure agrega las direcciones IP privadas de las interfaces de red como servidores en el grupo de servidores back-end de la puerta de enlace de aplicaciones.

```azurecli-interactive
address1=$(az network nic show --name myNic1 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
address2=$(az network nic show --name myNic2 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Enabled \
  --public-ip-address myAGPublicIPAddress \
  --vnet-name myVNet \
  --subnet myAGSubnet \
  --servers "$address1" "$address2"
```

La puerta de enlace de aplicaciones puede tardar hasta 30 minutos en crearse. Una vez creada, puede ver los siguientes valores en la sección **Configuración** de la página **Puerta de enlace de aplicaciones**:

- **appGatewayBackendPool**: Se encuentra en la página **Grupos de back-end**. Especifica el grupo de back-end necesario.
- **appGatewayBackendHttpSettings**: Se encuentra en la página **Configuración HTTP**. Este especifica que la puerta de enlace de aplicaciones usa el puerto 80 y el protocolo HTTP para la comunicación.
- **appGatewayHttpListener**: Se encuentra en la **página de Agentes de escucha**. Este valor, especifica el agente de escucha predeterminado asociado con **appGatewayBackendPool**.
- **appGatewayFrontendIP**: Se encuentra en la página **Configuraciones de IP de front-end**. Asigna *myAGPublicIPAddress* a **appGatewayHttpListener**.
- **rule1**: Se encuentra en la página **Reglas**. Especifica la regla de enrutamiento predeterminada asociada a **appGatewayHttpListener**.

## <a name="test-the-application-gateway"></a>Prueba de la puerta de enlace de aplicaciones

Aunque Azure no necesita un servidor web NGINX para crear la puerta de enlace de aplicaciones, lo instaló en este inicio rápido para comprobar si Azure la creaba correctamente. Para obtener la dirección IP pública de la nueva puerta de enlace de aplicaciones, use [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show). 

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

Copie la dirección IP pública y péguela en la barra de direcciones del explorador.
    
![Prueba de la puerta de enlace de aplicaciones](./media/quick-create-cli/application-gateway-nginxtest.png)

Al actualizar el explorador, verá aparecer el nombre de la segunda máquina virtual. Una respuesta válida corrobora que la puerta de enlace de aplicaciones se ha creado correctamente y puede conectarse correctamente con el back-end.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite más los recursos que creó con la puerta de enlace de aplicaciones, use el comando [az group delete](/cli/azure/group#az-group-delete) para eliminar el grupo de recursos. Mediante la eliminación del grupo de recursos también elimina la puerta de enlace de aplicaciones y todos sus recursos relacionados.

```azurecli-interactive 
az group delete --name myResourceGroupAG
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Administrar el tráfico web con Application Gateway mediante la CLI de Azure](./tutorial-manage-web-traffic-cli.md)

