---
title: Uso de Visual Studio Code para crear, compilar e implementar contratos inteligentes con Azure Blockchain Service
description: Tutorial sobre cómo usar el kit de desarrollo de Azure Blockchain para la extensión Ethereum en Visual Studio Code a fin de crear, compilar e implementar un contrato inteligente en Azure Blockchain Service.
services: azure-blockchain
author: PatAltimore
ms.author: patricka
ms.date: 09/10/2019
ms.topic: tutorial
ms.service: azure-blockchain
ms.reviewer: chrisseg
ms.openlocfilehash: 96fe4d77efdd1fda309d7da021bcc208edd2dfe9
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70934965"
---
# <a name="tutorial-usevisual-studio-code-to-create-buildanddeploysmartcontracts"></a>Tutorial: Uso de Visual Studio Code para crear, compilar e implementar contratos inteligentes

En este tutorial, usará el kit de desarrollo de Azure Blockchain para la extensión Ethereum en Visual Studio Code a fin de crear, compilar e implementar un contrato inteligente en Azure Blockchain Service. También usará Truffle para ejecutar una función de contrato inteligente mediante una transacción.

Use el kit de desarrollo de Azure Blockchain para Ethereum para:

> [!div class="checklist"]
> * Crear un contrato inteligente
> * Implementar un contrato inteligente
> * Ejecutar una función de contrato inteligente mediante una transacción
> * Consultar el estado del contrato

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

* Realizar el tutorial [Quickstart: Uso de Visual Studio Code para conectarse a una red del consorcio de Azure Blockchain Service](connect-vscode.md)

## <a name="create-a-smart-contract"></a>Crear un contrato inteligente

El kit de desarrollo de Azure Blockchain para Ethereum usa plantillas de proyecto y herramientas de Truffle para ayudar a preparar, compilar e implementar contratos. Antes de empezar, debe cumplir los siguientes requisitos previos [Inicio rápido: Uso de Visual Studio Code para conectarse a una red del consorcio de Azure Blockchain Service](connect-vscode.md). El inicio rápido le guía en la instalación y configuración del Kit de desarrollo de Azure Blockchain para Ethereum.

1. En la paleta de comandos de VS Code, elija **Azure Blockchain: New Solidity Project** (Azure Blockchain: nuevo proyecto de Solidity).
1. Elija **Create basic project** (Crear proyecto básico).
1. Cree una nueva carpeta denominada `HelloBlockchain` y seleccione **New project path** (Nueva ruta de acceso de proyecto).

El kit de desarrollo de Azure Blockchain crea e inicializa un nuevo proyecto de Solidity automáticamente. El proyecto básico incluye un contrato inteligente de ejemplo **HelloBlockchain** y todos los archivos necesarios para compilar e implementar en el miembro del consorcio en Azure Blockchain Service. La creación del proyecto puede tardar unos minutos. Puede supervisar el progreso en el panel del terminal de VS Code; para ello, seleccione la salida de Azure Blockchain.

La estructura del proyecto debe ser similar al siguiente ejemplo:

   ![Proyecto de Solidity](./media/send-transaction/solidity-project.png)

## <a name="build-a-smart-contract"></a>Creación de un contrato inteligente

Los contratos inteligentes se encuentran en el directorio de **contratos** del proyecto. Primero se crean los contratos inteligentes y después se implementan en una cadena de bloques. Use el comando **Build Contracts** para crear todos los contratos inteligentes del proyecto.

1. En la barra lateral del explorador de VS Code, expanda la carpeta de **contratos** del proyecto.
1. Haga clic con el botón derecho en **HelloBlockchain.sol** y elija **Build Contracts** (Crear contratos) en el menú.

    ![Creación de contratos](./media/send-transaction/build-contracts.png)

El kit de desarrollo de Azure Blockchain usa Truffle para compilar los contratos inteligentes.

![Salida de la compilación](./media/send-transaction/compile-output.png)

## <a name="deploy-a-smart-contract"></a>Implementar un contrato inteligente

Truffle usa scripts de migración para implementar los contratos en una red de Ethereum. Las migraciones son archivos JavaScript ubicados en el directorio de **migraciones** del proyecto.

1. Para implementar el contrato inteligente, haga clic con el botón derecho en **HelloBlockchain.sol** y elija **Deploy Contracts** (Implementar contratos) en el menú.
1. Elija su red de consorcio de Azure Blockchain en la paleta de comandos. La red de consorcio de la red de la cadena de bloques se agregó al archivo de configuración Truffle del proyecto al crearlo.
1. Elija **Generate mnemonic** (Generar mnemotécnico). Elija un nombre de archivo y guarde el archivo mnemotécnico en la carpeta del proyecto. Por ejemplo, `myblockchainmember.env`. El archivo mnemotécnico se usa para generar una clave privada de Ethereum para el miembro de la cadena de bloques.

El kit de desarrollo de Azure Blockchain usa Truffle para ejecutar el script de migración con el fin de implementar los contratos en la cadena de bloques.

![Contrato implementado correctamente](./media/send-transaction/deploy-contract.png)

## <a name="call-a-contract-function"></a>Llamada a una función de contrato

La función **SendRequest** del contrato **HelloBlockchain** cambia la variable de estado **RequestMessage**. El cambio del estado de una red de la cadena de bloques se realiza con una transacción. Puede crear un script para ejecutar la función **SendRequest** con una transacción.

1. Cree un nuevo archivo en la raíz del proyecto de Truffle y asígnele el nombre `sendrequest.js`. Agregue el siguiente código Web3 JavaScript al archivo.

    ```javascript
    var HelloBlockchain = artifacts.require("HelloBlockchain");
        
    module.exports = function(done) {
      console.log("Getting the deployed version of the HelloBlockchain smart contract")
      HelloBlockchain.deployed().then(function(instance) {
        console.log("Calling SendRequest function for contract ", instance.address);
        return instance.SendRequest("Hello, blockchain!");
      }).then(function(result) {
        console.log("Transaction hash: ", result.tx);
        console.log("Request complete");
        done();
      }).catch(function(e) {
        console.log(e);
        done();
      });
    };
    ```

1. Cuando el kit de desarrollo de Azure Blockchain crea un proyecto, se genera el archivo de configuración de Truffle con los detalles del punto de conexión de la red de la cadena de bloques del consorcio. Abra **truffle-config.js** en el proyecto. El archivo de configuración muestra dos redes: una denominada "development" y otra con el mismo nombre que el consorcio.
1. En el panel del terminal de VS Code, use Truffle para ejecutar el script en la red de la cadena de bloques del consorcio. En la barra de menús del panel del terminal, seleccione la pestaña **Terminal** y **PowerShell** en la lista desplegable.

    ```PowerShell
    truffle exec sendrequest.js --network <blockchain network>
    ```

    Reemplace \<blockchain network\> por el nombre de la red de la cadena de bloques definida en **truffle-config.js**.

Truffle ejecuta el script en la red de la cadena de bloques.

![Salida del script](./media/send-transaction/execute-transaction.png)

Al ejecutar una función de contrato con una transacción, esta no se procesa hasta que se crea un bloque. Las funciones diseñadas para ejecutarse con una transacción y devolver un identificador de transacción en lugar de un valor devuelto.

## <a name="query-contract-state"></a>Consultar el estado del contrato

Las funciones de contrato inteligente pueden devolver el valor actual de las variables de estado. Vamos a agregar una función para devolver el valor de una variable de estado.

1. En **HelloBlockchain.sol**, agregue una función **getMessage** al contrato inteligente **HelloBlockchain**.

    ``` solidity
    function getMessage() public view returns (string memory)
    {
        if (State == StateType.Request)
            return RequestMessage;
        else
            return ResponseMessage;
    }
    ```

    La función devuelve el mensaje almacenado en una variable de estado en dependiendo del estado actual del contrato.

1. Haga clic con el botón derecho en **HelloBlockchain.sol** y elija **Build Contracts** (Compilar contratos) en el menú para compilar los cambios en el contrato inteligente.
1. Para la implementación, haga clic con el botón derecho en **HelloBlockchain.sol** y elija **Deploy Contracts** (Implementar contratos) en el menú. Cuando se le pida, elija su red de consorcio de Azure Blockchain en la paleta de comandos.
1. A continuación, cree un script para llamar a la función **getMessage**. Cree un nuevo archivo en la raíz del proyecto de Truffle y asígnele el nombre `getmessage.js`. Agregue el siguiente código Web3 JavaScript al archivo.

    ```javascript
    var HelloBlockchain = artifacts.require("HelloBlockchain");
    
    module.exports = function(done) {
      console.log("Getting the deployed version of the HelloBlockchain smart contract")
      HelloBlockchain.deployed().then(function(instance) {
        console.log("Calling getMessage function for contract ", instance.address);
        return instance.getMessage();
      }).then(function(result) {
        console.log("Request message value: ", result);
        console.log("Request complete");
        done();
      }).catch(function(e) {
        console.log(e);
        done();
      });
    };
    ```

1. En el panel del terminal de VS Code, use Truffle para ejecutar el script en la red de la cadena de bloques. En la barra de menús del panel del terminal, seleccione la pestaña **Terminal** y **PowerShell** en la lista desplegable.

    ```bash
    truffle exec getmessage.js --network <blockchain network>
    ```

    Reemplace \<blockchain network\> por el nombre de la red de la cadena de bloques definida en **truffle-config.js**.

El script consulta el contrato inteligente mediante una llamada a la función getMessage. Se devuelve el valor actual de la variable de estado **RequestMessage**.

![Salida del script](./media/send-transaction/execute-get.png)

Observe que el valor no es **Hello, blockchain!** En su lugar, el valor devuelto es un marcador de posición. Al cambiar e implementar el contrato, este obtiene una nueva dirección y se les asignan valores a las variables de estado en el constructor del contrato inteligente. El script de migración de ejemplo de **2_deploy_contracts.js** de Truffle implementa el contrato inteligente y pasa un valor de marcador de posición como argumento. El constructor establece la variable de estado **RequestMessage** en el valor de marcador de posición y eso es lo que se devuelve.

1. Para establecer la variable de estado **RequestMessage** y consultar el valor, ejecute de nuevo los scripts **sendrequest.js** y **getmessage.js**.

    ![Salida del script](./media/send-transaction/execute-set-get.png)

    **sendrequest.js** establece la variable de estado **RequestMessage** en **Hello, blockchain!** y **getmessage.js** consulta el valor de la variable de estado **RequestMessage** en el contrato y devuelve **Hello, blockchain!**

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite los recursos, puede eliminarlos; para ello, elimine el grupo de recursos `myResourceGroup` que creó en el inicio rápido de requisitos previos *Creación de un miembro de cadena de bloques*.

Para eliminar el grupo de recursos:

1. En Azure Portal, vaya a **Grupo de recursos** en el panel de navegación izquierdo y seleccione el grupo de recursos que desea eliminar.
1. Seleccione **Eliminar grupo de recursos**. Compruebe la eliminación escribiendo el nombre del grupo de recursos y seleccionando **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha creado un proyecto de Solidity de ejemplo mediante el kit de desarrollo de Azure Blockchain. Ha creado e implementado un contrato inteligente y ha llamado a una función con una transacción en una red de cadena de bloques de consorcio hospedada Azure Blockchain Service.

> [!div class="nextstepaction"]
> [Desarrollo de aplicaciones de cadena de bloques con Azure Blockchain Service](develop.md)
