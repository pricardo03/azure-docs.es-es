---
title: Creación de una instancia de Azure Blockchain Service mediante la CLI de Azure
description: Use Azure Blockchain Service para crear a un miembro de cadena de bloques mediante la CLI de Azure.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: seal
manager: femila
ms.openlocfilehash: e1b7558ea83c8948a8984215e15040e4d929cb1b
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65141378"
---
# <a name="quickstart-create-an-azure-blockchain-service-blockchain-member-using-azure-cli"></a>Inicio rápido: Creación de un miembro de cadena de bloques de Azure Blockchain Service mediante la CLI de Azure

Azure Blockchain Service es una plataforma de cadena de bloques que puede usar para ejecutar su lógica de negocios dentro de un contrato inteligente. Para comenzar, en este inicio rápido se muestra cómo crear un miembro de la cadena de bloques mediante la CLI de Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="launch-azure-cloud-shell"></a>Inicio de Azure Cloud Shell

Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta.

Para abrir Cloud Shell, seleccione **Pruébelo** en la esquina superior derecha de un bloque de código. También puede ir a [https://shell.azure.com/bash](https://shell.azure.com/bash) para iniciar Cloud Shell en una pestaña independiente del explorador. Seleccione **Copiar** para copiar los bloques de código, péguelos en Cloud Shell y, luego, presione Entrar para ejecutarlos.

Si prefiere instalar y usar la CLI de forma local, en este inicio rápido se requiere la versión 2.0.51 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con el comando [az group create](https://docs.microsoft.com/cli/azure/group). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure. En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-blockchain-member"></a>Creación de un miembro de cadena de bloques

Cree un miembro de cadena de bloques en Azure Blockchain Service que ejecute el protocolo de libro de contabilidad de quorum en un nuevo consorcio.

Hay varios parámetros y propiedades que se deben pasar. Reemplace los parámetros siguientes por los suyos.

| Parámetro | DESCRIPCIÓN |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos donde se crean los recursos de Azure Blockchain Service. Use el grupo de recursos que creó en la sección anterior.
| **name** | Un nombre único que identifica al miembro de cadena de bloques de Azure Blockchain Service. El nombre se usa como dirección de punto de conexión público. Por ejemplo, `myblockchainmember.blockchain.azure.com`.
| **ubicación** | Región de Azure donde se crea el miembro de cadena de bloques. Por ejemplo, `eastus`. Elija la ubicación más cercana a los usuarios o a sus otras aplicaciones de Azure.
| **password** | La contraseña de la cuenta del miembro. La contraseña de la cuenta del miembro se usa para autenticarse en el punto de conexión público del miembro de cadena de bloques mediante autenticación básica.
| **consortium** | Nombre del consorcio al que unirse o que se va a crear.
| **consortiumManagementAccountPassword** | La contraseña de administración del consorcio. Se usa para unirse a un consorcio.
| **skuName** | Tipo de nivel. Use S0 para Estándar y B0 para Básico.

```azurecli-interactive
az resource create --resource-group myResourceGroup --name myblockchainmember --resource-type Microsoft.Blockchain/blockchainMembers --is-full-object --properties "{ \"location\": \"eastus\", \"properties\": {\"password\": \"strongMemberAccountPassword@1\", \"protocol\": \"Quorum\", \"consortium\": \"myConsortiumName\", \"consortiumManagementAccountPassword\": \"strongConsortiumManagementPassword@1\" }, \"sku\": { \"name\": \"S0\" } }"
```

La creación del miembro de cadena de bloques y de los recursos auxiliares tarda unos 10 minutos.

A continuación se muestra la salida de ejemplo de una operación de creación correcta.

```json
{
  "id": "/subscriptions/<subscriptionId>/resourceGroups/myResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/mymembername",
  "kind": null,
  "location": "eastus",
  "name": "mymembername",
  "properties": {
    "ConsortiumMemberDisplayName": "mymembername",
    "consortium": "myConsortiumName",
    "consortiumManagementAccountAddress": "0xfe5fbb9d1036298abf415282f52397ade5d5beef",
    "consortiumManagementAccountPassword": null,
    "consortiumRole": "ADMIN",
    "dns": "mymembername.blockchain.azure.com",
    "protocol": "Quorum",
    "provisioningState": "Succeeded",
    "userName": "mymembername",
    "validatorNodesSku": {
      "capacity": 2
    }
  },
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "S0",
    "tier": "Standard"
  },
  "type": "Microsoft.Blockchain/blockchainMembers"
}
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Puede usar el miembro de cadena de bloques que ha creado en el siguiente inicio rápido o tutorial. Cuando ya no necesite los recursos, puede eliminarlos; para ello, elimine el grupo de recursos `myResourceGroup` que creó mediante Azure Blockchain Service.

Ejecute el siguiente comando para quitar el grupo de recursos y todos los recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha creado un miembro de cadena de bloques, pruebe uno de los inicios rápidos de conexión al nodo de transacción para [Geth](connect-geth.md), [MetaMask](connect-metamask.md) o [Truffle](connect-truffle.md).

> [!div class="nextstepaction"]
> [Uso de Truffle para conectarse a una red de Azure Blockchain Service](connect-truffle.md)
