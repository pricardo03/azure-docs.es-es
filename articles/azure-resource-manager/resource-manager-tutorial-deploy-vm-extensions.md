---
title: Implementación de extensiones de máquina virtual con plantillas de Azure Resource Manager | Microsoft Docs
description: Aprenda a implementar extensiones de máquina virtual con plantillas de Azure Resource Manager.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/30/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: ca07f044a423a4ea1b5dee484c56f457c7d785de
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50239738"
---
# <a name="tutorial-deploy-virtual-machine-extensions-with-azure-resource-manager-templates"></a>Tutorial: Implementación de extensiones de máquina virtual con plantillas de Azure Resource Manager.

Aprenda a usar [extensiones de máquina virtual de Azure](../virtual-machines/extensions/features-windows.md) para realizar tareas de automatización y configuración posteriores a la implementación en máquinas virtuales de Azure. Hay muchas extensiones de máquina virtual diferentes disponibles para su uso con máquinas virtuales de Azure. En este tutorial se implementa una extensión de Custom Script desde una plantilla de Resource Manager para ejecutar un script de PowerShell en una máquina virtual de Windows.  El script instala un servidor web en la máquina virtual.

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Preparación de un script de PowerShell.
> * Apertura de una plantilla de inicio rápido
> * Edición de la plantilla
> * Implementación de la plantilla
> * Comprobar la implementación

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

* [Visual Studio Code](https://code.visualstudio.com/) con la extensión Resource Manager Tools.  Consulte [Instalación de la extensión ](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Para aumentar la seguridad, utilice una contraseña generada para la cuenta de administrador de máquina virtual. Este es un ejemplo para generar una contraseña:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault está diseñado para proteger las claves criptográficas y otros secretos. Para más información, consulte [Tutorial: Integración de Azure Key Vault en la implementación de la plantilla de Resource Manager](./resource-manager-tutorial-use-key-vault.md). También se recomienda actualizar la contraseña cada tres meses.

## <a name="prepare-a-powershell-script"></a>Preparación de un script de PowerShell.

Un script de PowerShell con el siguiente contenido se comparte desde un [cuenta de Azure Storage con acceso público](https://armtutorials.blob.core.windows.net/usescriptextensions/installWebServer.ps1):

```azurepowershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Si elige publicar el archivo en su propia ubicación, tendrá que actualizar el elemento [fileUri] de la plantilla más adelante en el tutorial.

## <a name="open-a-quickstart-template"></a>Abra una plantilla de inicio rápido.

Las plantillas de inicio rápido de Azure consisten en un repositorio de plantillas de Resource Manager. En lugar de crear una plantilla desde cero, puede buscar una plantilla de ejemplo y personalizarla. La plantilla que se usa en este tutorial se denomina [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
2. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Seleccione **Abrir** para abrir el archivo.
4. La plantilla define cinco recursos:

    * `Microsoft.Storage/storageAccounts`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
    * `Microsoft.Network/publicIPAddresses`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
    * `Microsoft.Network/virtualNetworks`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
    * `Microsoft.Network/networkInterfaces`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
    * `Microsoft.Compute/virtualMachines`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

    Resulta útil obtener cierta información básica de la plantilla antes de personalizarla.
5. Seleccione **Archivo**>**Guardar como** para guardar una copia del archivo en la máquina local con el nombre **azuredeploy.json**.

## <a name="edit-the-template"></a>Edición de la plantilla

Agregue un recurso de extensión de máquina virtual a la plantilla existente con el siguiente contenido:

```json
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "[concat(variables('vmName'),'/', 'InstallWebServer')]",
    "location": "[parameters('location')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/',variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.7",
        "autoUpgradeMinorVersion":true,
        "settings": {
            "fileUris": [
                "https://armtutorials.blob.core.windows.net/usescriptextensions/installWebServer.ps1"
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File installWebServer.ps1"
        }
    }
}
```

Consulte la [referencia de la extensión](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines/extensions) si necesita más información acerca de la definición de este recurso. Estos son algunos elementos importantes:

* **name**: dado que el recurso de extensión es un recurso secundario del objeto de máquina virtual, el nombre debe tener el prefijo del nombre de máquina virtual. Consulte [Recursos secundarios](./resource-manager-templates-resources.md#child-resources).
* **dependsOn**: el recurso de la extensión debe crearse después de que se haya creado la máquina virtual.
* **fileUris**: son las ubicaciones donde se almacenan los archivos de script. Si elige no utilizar el que se proporciona, deberá actualizar los valores.
* **commandToExecute**: el comando para invocar el script.  

## <a name="deploy-the-template"></a>Implementación de la plantilla

Consulte la sección [Implementación de la plantilla](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template) para conocer el procedimiento de implementación. Se recomienda usar una contraseña generada para la cuenta de administrador de la máquina virtual. Consulte [Requisitos previos](#prerequisites).

## <a name="verify-the-deployment"></a>Comprobar la implementación

En el portal, seleccione la maquina virtual y, en su información general, utilice **Haga clic para copiar** a la derecha de la dirección IP para copiarla y pegarla en una pestaña del explorador. Se abre la página de bienvenida de IIS predeterminada, que debería ser como esta:

![Azure Resource Manager implementa el servidor web de IIS del script del cliente de extensiones de la máquina virtual](./media/resource-manager-tutorial-deploy-vm-extensions/resource-manager-template-deploy-extensions-customer-script-web-server.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
2. Escriba el nombre del grupo de recursos en el campo **Filtrar por nombre**.
3. Seleccione el nombre del grupo de recursos.  Verá un total de seis recursos en el grupo de recursos.
4. Seleccione **Eliminar grupo de recursos** del menú superior.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha implementado una máquina virtual y una extensión de virtual machine. La extensión ha instalado al servidor web de IIS en la máquina virtual. Para aprender a usar la extensión de SQL Database para importar un archivo BACPAC, consulte:

> [!div class="nextstepaction"]
> [](./resource-manager-tutorial-deploy-vm-extensions.md)
