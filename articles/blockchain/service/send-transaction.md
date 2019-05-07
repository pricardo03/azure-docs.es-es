---
title: Envío de transacciones mediante Azure Blockchain Service
description: Tutorial sobre el uso de Azure Blockchain Service para implementar contratos inteligentes y enviar transacciones privadas.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: tutorial
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: d3ad4cdfe33948c04c278ed3dfef7aa6fda637ab
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027501"
---
# <a name="tutorial-send-transactions-using-azure-blockchain-service"></a>Tutorial: Envío de transacciones mediante Azure Blockchain Service

En este tutorial, creará nodos de transacción para probar la privacidad de los contratos y transacciones.  Usará Truffle para crear un entorno de desarrollo local e implementar un contrato inteligente y enviar una transacción privada.

Aprenderá a:

> [!div class="checklist"]
> * Agregar nodos de transacción
> * Usar Truffle para implementar un contrato inteligente
> * Enviar una transacción
> * Validar la privacidad de la transacción

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

* Realizar el tutorial [Create a blockchain member using the Azure portal](create-member.md) (Creación de un miembro de cadena de bloques mediante Azure Portal)
* Realizar el tutorial [Quickstart: Use Truffle to connect to a consortium network](connect-truffle.md) (Inicio rápido: Uso de Truffle para conectarse a una red del consorcio)
* Para usar Truffle es necesario instalar varias herramientas, como [Node.js](https://nodejs.org), [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) y [Truffle](https://github.com/trufflesuite/truffle).

    Para una instalación rápida en Windows 10, instale [Ubuntu en Windows](https://www.microsoft.com/p/ubuntu/9nblggh4msv6) para un terminal de shell de Bash de Unix y, luego, instale [Truffle](https://github.com/trufflesuite/truffle). La distribución de Ubuntu en Windows incluye Node.js y Git.

* Instalar [Visual Studio Code](https://code.visualstudio.com/Download)
* Instalar la [extensión Solidity de Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity)

## <a name="create-transaction-nodes"></a>Creación de nodos de transacción

De forma predeterminada, tiene un nodo de transacción. Vamos a agregar dos más. Uno de los nodos participa en la transacción privada. El otro no se incluye en ella.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Vaya al miembro de Azure Blockchain y seleccione **Nodos de transacción > Agregar**.
1. Realice la configuración del nuevo nodo de transacción llamado `alpha`.

    ![Creación del nodo de transacción](./media/send-transaction/create-node.png)

    | Configuración | Valor | DESCRIPCIÓN |
    |---------|-------|-------------|
    | NOMBRE | `alpha` | Nombre del nodo de transacción. El nombre se usa para crear la dirección DNS del punto de conexión del nodo de transacción. Por ejemplo, `alpha-mymanagedledger.blockchain.azure.com`. |
    | Contraseña | Contraseña segura | La contraseña se usa para acceder al punto de conexión del nodo de transacción con autenticación básica.

1. Seleccione **Crear**.

    El aprovisionamiento de un nuevo nodo de transacción tarda unos 10 minutos.

1. Repita los pasos del 2 al 4 para agregar un nodo de transacción llamado `beta`.

Mientras se aprovisionan los nodos, puede continuar con el tutorial. Cuando haya finalizado el aprovisionamiento, tendrá tres nodos de transacción.

## <a name="open-truffle-project"></a>Apertura del proyecto de Truffle

1. Abra un terminal de shell de Bash.
1. Cambie la ruta de acceso por el directorio del proyecto de Truffle que se indica en [Quickstart: Use Truffle to connect to a consortium network](connect-truffle.md) (Inicio rápido: Uso de Truffle para conectarse a una red del consorcio). Por ejemplo,

    ```bash
    cd truffledemo
    ```

1. Inicie la consola de desarrollo interactiva de Truffle.

    ``` bash
    truffle develop
    ```

    Truffle crea una cadena de bloques de desarrollo local y proporciona una consola interactiva.

## <a name="connect-to-transaction-node"></a>Conexión al nodo de transacción

Use Web3 para conectarse al nodo de transacción predeterminado y crear una cuenta. Puede obtener la cadena de conexión de Web3 en Azure Portal.

1. En Azure Portal, vaya al nodo de transacción predeterminado y seleccione **Nodos transacción > Código de ejemplo > Web3**.
1. Copie el código de JavaScript de **HTTPS (Access key 1)** (HTTPS [clave de acceso 1]).![código de ejemplo de Web3](./media/send-transaction/web3-code.png)

1. Pegue el código de JavaScript de Web3 correspondiente al nodo de transacción predeterminado en la consola de desarrollo interactiva de Truffle. El código crea un objeto web3 que está conectado a su nodo de transacción de Azure Blockchain Service.

    ```bash
    truffle(develop)> var Web3 = require("Web3");
    truffle(develop)> var provider = new Web3.providers.HttpProvider("https://myblockchainmember.blockchain.azure.com:3200/hy5FMu5TaPR0Zg8GxiPwned");
    truffle(develop)> var web3 = new Web3(provider);
    ```

    Puede llamar a los métodos del objeto web3 para interactuar con el nodo de transacción.

1. Cree una cuenta en el nodo de transacción predeterminado. Reemplace el parámetro de contraseña por su propia contraseña segura.

    ```bash
    web3.eth.personal.newAccount("1@myStrongPassword");
    ```

    Para la siguiente sección, anote la dirección de la cuenta devuelta y la contraseña que ha usado.

1. Salga del entorno de desarrollo de Truffle.

    ```bash
    .exit
    ```

## <a name="configure-truffle-project"></a>Configuración del proyecto de Truffle

Para configurar el proyecto de Truffle, debe ir a Azure Portal para obtener información del nodo de transacción.

### <a name="transaction-node-public-key"></a>Clave pública del nodo de transacción

Cada nodo de transacción tiene una clave pública. La clave pública permite enviar una transacción privada al nodo. Para enviar una transacción desde el nodo de transacción predeterminado al nodo de transacción *alpha*, necesita la clave pública de este último nodo.

Puede obtener la clave pública de la lista de nodos de transacción. Copie la clave pública del nodo alpha y guarde el valor. Lo necesitará más adelante en el tutorial.

![Lista de nodos de transacción](./media/send-transaction/node-list.png)

### <a name="transaction-node-endpoint-addresses"></a>Direcciones de punto de conexión de nodo de transacción

1. En Azure Portal, vaya a cada nodo de la transacción y seleccione **Nodos de transacción > Cadenas de conexión**.
1. Copie y guarde la dirección URL del punto de conexión que aparece en **HTTPS (Access key 1) (HTTPS [clave de acceso]) para cada nodo de transacción. Más adelante en el tutorial necesitará las direcciones de punto de conexión del archivo de configuración del contrato inteligente.

    ![Dirección de punto de conexión de transacción](./media/send-transaction/endpoint.png)

### <a name="edit-configuration-file"></a>Edición del archivo de configuración

1. Inicie Visual Studio Code y abra la carpeta de directorio del proyecto de Truffle mediante el menú **File > Open Folder** (Archivo > Abrir carpeta).
1. Abra el archivo de configuración de Truffle `truffle-config.js`.
1. Reemplace el contenido del archivo por la siguiente información de configuración. Agregue variables que contengan las direcciones de punto de conexión y la información de la cuenta. Reemplace las secciones de corchetes angulares por los valores recopilados en las secciones anteriores.

``` javascript
var defaultnode = "<default transaction node connection string>";
var alpha = "<alpha transaction node connection string>";
var beta = "<beta transaction node connection string>";

var myAccount = "<account address>";
var myPassword = "<account password>";

var Web3 = require("web3");
```

Agregue el código de configuración a la sección **module.exports** de la configuración.

```javascript
module.exports = {
  networks: {
    defaultnode: {
      provider:(() =>  {
      const AzureBlockchainProvider = new Web3.providers.HttpProvider(defaultnode);

      const web3 = new Web3(AzureBlockchainProvider);
      web3.eth.personal.unlockAccount(myAccount, myPassword);

      return AzureBlockchainProvider;
      })(),

      network_id: "*",
      gas: 0,
      gasPrice: 0,
      from: myAccount
    },
    alpha: {
      provider: new Web3.providers.HttpProvider(alpha),
      network_id: "*",
      gas: 0,
      gasPrice: 0
    },
    beta: {
      provider: new Web3.providers.HttpProvider(beta),
      network_id: "*",
      gas: 0,
      gasPrice: 0
    }
  }
}
```

## <a name="create-smart-contract"></a>Creación de un contrato inteligente

En la carpeta **contracts**, cree un archivo llamado `SimpleStorage.sol`. Agregue el siguiente código.

```solidity
pragma solidity >=0.4.21 <0.6.0;

contract SimpleStorage {
    string public storedData;

    constructor(string memory initVal) public {
        storedData = initVal;
    }

    function set(string memory x) public {
        storedData = x;
    }

    function get() view public returns (string memory retVal) {
        return storedData;
    }
}
```

En la carpeta **migrations**, cree un archivo llamado `2_deploy_simplestorage.js`. Agregue el siguiente código.

```solidity
var SimpleStorage = artifacts.require("SimpleStorage.sol");

module.exports = function(deployer) {

  // Pass 42 to the contract as the first constructor parameter
  deployer.deploy(SimpleStorage, "42", {privateFor: ["<alpha node public key>"], from:"<Account address>"})  
};
```

Reemplace los valores de los corchetes angulares.

| Valor | DESCRIPCIÓN
|-------|-------------
| \<clave pública del nodo alpha\> | Clave pública del nodo alpha
| \<Dirección de la cuenta\> | Dirección de la cuenta creada en el nodo de transacción predeterminado.

En este ejemplo, el valor inicial del valor **storeData** se establece en 42.

**privateFor** define los nodos para los que está disponible el contrato. En este ejemplo, la cuenta del nodo de transacción predeterminado puede transmitir las transacciones privadas al nodo **alpha**. Deberá agregar claves públicas para todos los participantes de la transacción privada. Si no incluye **privateFor:** y **from:**, las transacciones de contrato inteligente son públicas y todos los miembros del consorcio pueden verlas.

Para guardar todos los archivos, seleccione **File > Save All** (Archivo > Guardar todo).

## <a name="deploy-smart-contract"></a>Implementación de un contrato inteligente

Use Truffle para implementar `SimpleStorage.sol` en la red del nodo de transacción predeterminado.

```bash
truffle migrate --network defaultnode
```

Truffle primero compila y luego implementa el contrato inteligente **SimpleStorage**.

Salida de ejemplo:

```
pat@DESKTOP:/mnt/c/truffledemo$ truffle migrate --network defaultnode

2_deploy_simplestorage.js
=========================

   Deploying 'SimpleStorage'
   -------------------------
   > transaction hash:    0x3f695ff225e7d11a0239ffcaaab0d5f72adb545912693a77fbfc11c0dbe7ba72
   > Blocks: 2            Seconds: 12
   > contract address:    0x0b15c15C739c1F3C1e041ef70E0011e641C9D763
   > account:             0x1a0B9683B449A8FcAd294A01E881c90c734735C3
   > balance:             0
   > gas used:            0
   > gas price:           0 gwei
   > value sent:          0 ETH
   > total cost:          0 ETH


   > Saving migration to chain.
   > Saving artifacts
   -------------------------------------
   > Total cost:                   0 ETH


Summary
=======
> Total deployments:   2
> Final cost:          0 ETH
```

## <a name="validate-contract-privacy"></a>Validación de la privacidad del contrato

Debido a la privacidad del contrato, los valores de este solo se pueden consultar desde nodos declarados en **privateFor**. En este ejemplo, podemos consultar el nodo de transacción predeterminado porque la cuenta existe en ese nodo. Mediante la consola de Truffle, conéctese al nodo de transacción predeterminado.

```bash
truffle console --network defaultnode
```

Ejecute un comando que devuelva el valor de la instancia del contrato.

```bash
SimpleStorage.deployed().then(function(instance){return instance.get();})
```

Si la consulta del nodo de transacción predeterminado es correcta, se devuelve el valor 42.

Salida de ejemplo:

```
pat@DESKTOP-J41EP5S:/mnt/c/truffledemo$ truffle console --network defaultnode
truffle(defaultnode)> SimpleStorage.deployed().then(function(instance){return instance.get();})
'42'
```

Salga de la consola.

```bash
.exit
```

Como se ha declarado la clave pública de **alpha** en **privateFor**, podemos consultar el nodo **alpha**. Con la consola de Truffle, conéctese al nodo **alpha**.

```bash
truffle console --network alpha
```

Ejecute un comando que devuelva el valor de la instancia del contrato.

```bash
SimpleStorage.deployed().then(function(instance){return instance.get();})
```

Si la consulta del nodo **alpha** se realiza correctamente, se devuelve el valor 42.

Salida de ejemplo:

```
pat@DESKTOP-J41EP5S:/mnt/c/truffledemo$ truffle console --network alpha
truffle(alpha)> SimpleStorage.deployed().then(function(instance){return instance.get();})
'42'
```

Salga de la consola.

```bash
.exit
```

Puesto que no se declaró la clave pública del nodo **beta** en **privateFor**, no se podrá consultar este nodo debido a la privacidad del contrato. Mediante la consola de Truffle, conéctese al nodo **beta**.

```bash
truffle console --network beta
```

Ejecute un comando que devuelva el valor de la instancia del contrato.

```bash
SimpleStorage.deployed().then(function(instance){return instance.get();})
```

La consulta del nodo **beta** no se puede realizar porque el contrato es privado.

Salida de ejemplo:

```
pat@DESKTOP-J41EP5S:/mnt/c/truffledemo$ truffle console --network beta
truffle(beta)> SimpleStorage.deployed().then(function(instance){return instance.get();})
Thrown:
Error: Returned values aren't valid, did it run Out of Gas?
    at XMLHttpRequest._onHttpResponseEnd (/mnt/c/truffledemo/node_modules/xhr2-cookies/xml-http-request.ts:345:8)
    at XMLHttpRequest._setReadyState (/mnt/c/truffledemo/node_modules/xhr2-cookies/xml-http-request.ts:219:8)
    at XMLHttpRequestEventTarget.dispatchEvent (/mnt/c/truffledemo/node_modules/xhr2-cookies/xml-http-request-event-target.ts:44:13)
    at XMLHttpRequest.request.onreadystatechange (/mnt/c/truffledemo/node_modules/web3-providers-http/src/index.js:96:13)
```

Salga de la consola.

```bash
.exit
```

## <a name="send-a-transaction"></a>Enviar una transacción

Cree un archivo llamado `sampletx.js`. Guárdelo en la raíz del proyecto.

Este script establece el valor de la variable **storedData** del contrato en 65. Agregue el código al nuevo archivo.

```javascript
var SimpleStorage = artifacts.require("SimpleStorage");

module.exports = function(done) {
  console.log("Getting deployed version of SimpleStorage...")
  SimpleStorage.deployed().then(function(instance) {
    console.log("Setting value to 65...");
    return instance.set("65", {privateFor: ["<alpha node public key>"], from:"<Account address>"});
  }).then(function(result) {
    console.log("Transaction:", result.tx);
    console.log("Finished!");
    done();
  }).catch(function(e) {
    console.log(e);
    done();
  });
};
```

Reemplace los valores de los corchetes angulares y, luego, guarde el archivo.

| Valor | DESCRIPCIÓN
|-------|-------------
| \<clave pública del nodo alpha\> | Clave pública del nodo alpha
| \<Dirección de la cuenta\> | Dirección de la cuenta creada en el nodo de transacción predeterminado.

**privateFor** define los nodos para los que está disponible la transacción. En este ejemplo, la cuenta del nodo de transacción predeterminado puede transmitir las transacciones privadas al nodo **alpha**. Deberá agregar claves públicas para todos los participantes de la transacción privada.

Use Truffle para ejecutar el script del nodo de transacción predeterminado.

```bash
truffle exec sampletx.js --network defaultnode
```

Ejecute un comando que devuelva el valor de la instancia del contrato.

```bash
SimpleStorage.deployed().then(function(instance){return instance.get();})
```

Si la transacción se realizó correctamente, se devuelve el valor 65.

Salida de ejemplo:

```
Getting deployed version of SimpleStorage...
Setting value to 65...
Transaction: 0x864e67744c2502ce75ef6e5e09d1bfeb5cdfb7b880428fceca84bc8fd44e6ce0
Finished!
```

Salga de la consola.

```bash
.exit
```

## <a name="validate-transaction-privacy"></a>Validación de la privacidad de la transacción

Debido a la privacidad de la transacción, solo pueden realizarse transacciones en los nodos que se han declarado en **privateFor**. En este ejemplo, podemos realizar transacciones dado que se declaró la clave pública de **alpha** en **privateFor**. Use Truffle para ejecutar la transacción en el nodo **alpha**.

```bash
truffle exec sampletx.js --network alpha
```

Ejecute un comando que devuelva el valor de la instancia del contrato.

```bash
SimpleStorage.deployed().then(function(instance){return instance.get();})
```

Si la transacción se realizó correctamente, se devuelve el valor 65.

Salida de ejemplo:

```
Getting deployed version of SimpleStorage...
Setting value to 65...
Transaction: 0x864e67744c2502ce75ef6e5e09d1bfeb5cdfb7b880428fceca84bc8fd44e6ce0
Finished!
```

Salga de la consola.

```bash
.exit
```

En este tutorial, agregó dos nodos de transacción para demostrar la privacidad de los contratos y transacciones. Usó el nodo predeterminado para implementar un contrato inteligente privado. Probó la privacidad mediante la consulta de los valores de contrato y la realización de transacciones en la cadena de bloques.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite los recursos, puede eliminarlos; para ello, elimine el grupo de recursos `myResourceGroup` que creó mediante Azure Blockchain Service.

Para eliminar el grupo de recursos:

1. En Azure Portal, vaya a **Grupo de recursos** en el panel de navegación izquierdo y seleccione el grupo de recursos que desea eliminar.
1. Seleccione **Eliminar grupo de recursos**. Compruebe la eliminación escribiendo el nombre del grupo de recursos y seleccionando **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Desarrollo de aplicaciones de cadena de bloques con Azure Blockchain Service](develop.md)
