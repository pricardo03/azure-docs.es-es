---
title: Creación de plantillas vinculadas de Azure Resource Manager | Microsoft Docs
description: Creación de plantillas vinculadas de Azure Resource Manager para crear máquinas virtuales
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 09/07/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: d25ca14b78465a6c4fec7e90bc20436e3ca553fc
ms.sourcegitcommit: 3150596c9d4a53d3650cc9254c107871ae0aab88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47419634"
---
# <a name="tutorial-create-linked-azure-resource-manager-templates"></a>Tutorial: Creación de plantillas vinculadas de Azure Resource Manager

Obtenga información sobre cómo crear plantillas vinculadas de Azure Resource Manager. Con las plantillas vinculadas, puede hacer que una plantilla llame a otra. Resulta muy útil para el diseño modular de las plantillas. En este tutorial, puede usar la misma plantilla que se usa en el [tutorial para la creación de varias instancias de recursos con plantillas de Resource Manager](./resource-manager-tutorial-create-multiple-instances.md) y que crea una máquina virtual, una red virtual y otros recursos dependientes, incluida una cuenta de almacenamiento. Puede separar el recurso de la cuenta de almacenamiento a una plantilla vinculada.

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Apertura de una plantilla de inicio rápido
> * Creación de la plantilla vinculada
> * Carga de la plantilla vinculada
> * Vínculo a la plantilla vinculada
> * Configuración de la dependencia
> * Obtención de valores a partir de la plantilla vinculada
> * Implementación de la plantilla

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

* [Visual Studio Code](https://code.visualstudio.com/)
* Extensión de herramientas de Resource Manager  Consulte [Instalación de la extensión ](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Complete el [tutorial para la creación de varias instancias de recursos con plantillas de Resource Manager](./resource-manager-tutorial-create-multiple-instances.md).

## <a name="open-a-quickstart-template"></a>Abra una plantilla de inicio rápido.

Las plantillas de inicio rápido de Azure consisten en un repositorio de plantillas de Resource Manager. En lugar de crear una plantilla desde cero, puede buscar una plantilla de ejemplo y personalizarla. La plantilla que se usa en este tutorial se denomina [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/). Se trata de la misma plantilla que se usa en el [tutorial para la creación de varias instancias de recursos con plantillas de Resource Manager](./resource-manager-tutorial-create-multiple-instances.md). Se guardan dos copias de la misma plantilla para usarlas de esta manera:

- **La plantilla principal**: cree todos los recursos, excepto la cuenta de almacenamiento.
- **La plantilla vinculada**: cree la cuenta de almacenamiento.

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
2. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Seleccione **Abrir** para abrir el archivo.
4. Seleccione **Archivo**>**Guardar como** para guardar una copia del archivo en la máquina local con el nombre **azuredeploy.json**.
5. Seleccione **Archivo**>**Guardar como** para crear otra copia del archivo con el nombre **linkedTemplate.json**.

## <a name="create-the-linked-template"></a>Creación de la plantilla vinculada

La plantilla vinculada crea una cuenta de almacenamiento. La plantilla vinculada es casi idéntica a la plantilla independiente que crea una cuenta de almacenamiento. En este tutorial, la plantilla vinculada debe pasar un valor de vuelta a la plantilla principal. Este valor se define en el elemento `outputs`.

1. Abra linkedTemplate.json en Visual Studio Code si no está abierto.
2. Se han realizado los siguientes cambios:

    - Quite todos los recursos, excepto la cuenta de almacenamiento. Puede quitar un total de cuatro recursos.
    - Actualice el elemento **outputs** para que tenga este aspecto:

        ```json
        "outputs": {
            "storageUri": {
                "type": "string",
                "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
              }
        }
        ```
        La definición del recurso de máquina virtual necesita **storageUri** en la plantilla principal.  El valor se pasa de vuelta a la plantilla principal como un valor de salida.
    - Quite los parámetros que no se usan nunca. Estos parámetros tienen una línea ondulada verde debajo. Solo quedará un parámetro llamado **location**.
    - Quite el elemento **variables**. No es necesario en este tutorial.
    - Agregue un parámetro llamado **storageAccountName**. El nombre de la cuenta de almacenamiento se pasa como parámetro de la plantilla principal a la plantilla vinculada.

    Cuando termine, la plantilla deberá verse así:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountName":{
            "type": "string",
            "metadata": {
              "description": "Azure Storage account name."
            }
          },
          "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
              "description": "Location for all resources."
            }
          }
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccountName')]",
            "apiVersion": "2016-01-01",
            "location": "[parameters('location')]",
            "sku": {
              "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
          }
        ],
        "outputs": {
            "storageUri": {
                "type": "string",
                "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
              }
        }
    }
    ```
3. Guarde los cambios.

## <a name="upload-the-linked-template"></a>Carga de la plantilla vinculada

Las plantillas deben ser accesibles desde donde ejecuta la implementación. Esta ubicación podría ser una cuenta de almacenamiento de Azure, GitHub o Dropbox. Si las plantillas contienen información confidencial, asegúrese de proteger el acceso a ellas. En este tutorial, se usa el método de implementación de Cloud Shell que se usó en el [tutorial para la creación de varias instancias de recursos con plantillas de Resource Manager](./resource-manager-tutorial-create-multiple-instances.md). La plantilla principal (azuredeploy.json) se carga en el shell. La plantilla vinculada (linkedTemplate.json) se debe compartir en algún lugar.  Para reducir las tareas de este tutorial, la plantilla vinculada definida en la sección anterior se cargó en [una cuenta de almacenamiento de Azure](https://armtutorials.blob.core.windows.net/linkedtemplates/linkedStorageAccount.json).

## <a name="call-the-linked-template"></a>Llamada a la plantilla vinculada

La plantilla principal se llama azuredeploy.json.

1. Abra azuredeploy.json en Visual Studio Code en caso de que no esté abierto.
2. Elimine la definición de recurso de la cuenta de almacenamiento desde la plantilla.
3. Agregue el siguiente fragmento de código JSON en el lugar donde estaba la definición de la cuenta de almacenamiento:

    ```json
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri":"https://armtutorials.blob.core.windows.net/linkedtemplates/linkedStorageAccount.json"
          },
          "parameters": {
              "storageAccountName":{"value": "[variables('storageAccountName')]"},
              "location":{"value": "[parameters('location')]"}
          }
      }
    },
    ```

    Ponga atención a estos detalles:

    - Un recurso `Microsoft.Resources/deployments` en la plantilla principal se usa para vincular a otra plantilla.
    - El recurso `deployments` tiene un nombre llamado `linkedTemplate`. Este nombre de usa para [configurar la dependencia](#configure-dependency).  
    - Solo puede usar el modo de implementación [incremental](./deployment-modes.md) al llamar a las plantillas vinculadas.
    - `templateLink/uri` contiene el URI de la plantilla vinculada. La plantilla vinculada se cargó en una cuenta de almacenamiento compartido. Puede actualizar el URI si carga la plantilla en otra ubicación en Internet.
    - Use `parameters` para pasar valores desde la plantilla principal a la plantilla vinculada.
4. Guarde los cambios.

## <a name="configure-dependency"></a>Configuración de la dependencia

Recuerde que, según lo visto en el [tutorial para la creación de varias instancias de recursos con plantillas de Resource Manager](./resource-manager-tutorial-create-multiple-instances.md), el recurso de máquina virtual depende de la cuenta de almacenamiento:

![Diagrama de dependencia de las plantillas de Azure Resource Manager](./media/resource-manager-tutorial-create-linked-templates/resource-manager-template-visual-studio-code-dependency-diagram.png)

Como la cuenta de almacenamiento ahora se define en la plantilla vinculada, debe actualizar estos dos elementos del recurso `Microsoft.Compute/virtualMachines`.

- Vuelva a configurar el elemento `dependOn`. La definición de la cuenta de almacenamiento se traslada a la plantilla vinculada.
- Vuelva a configurar el elemento `properties/diagnosticsProfile/bootDiagnostics/storageUri`. En [Creación de la plantilla vinculada](#create-the-linked-template), agregó un valor de salida:

    ```json
    "outputs": {
        "storageUri": {
            "type": "string",
            "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
    }
    ```
    La plantilla principal requiere este valor.

1. Abra azuredeploy.json en Visual Studio Code en caso de que no esté abierto.
2. Expanda la definición del recurso de máquina virtual, actualice **dependsOn** como se muestra en esta captura de pantalla:

    ![Las plantillas vinculadas de Azure Resource Manager configuran la dependencia ](./media/resource-manager-tutorial-create-linked-templates/resource-manager-template-linked-templates-configure-dependency.png)
    
    "linkedTemplate" es el nombre del recurso de implementación.  
3. Actualice **properties/diagnosticsProfile/bootDiagnostics/storageUri** como se muestra en la captura de pantalla anterior.

Para más información, consulte [Uso de plantillas vinculadas y anidadas al implementar recursos de Azure](./resource-group-linked-templates.md).

## <a name="deploy-the-template"></a>Implementación de la plantilla

Consulte la sección [Implementación de la plantilla](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template) para conocer el procedimiento de implementación.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
2. Escriba el nombre del grupo de recursos en el campo **Filtrar por nombre**.
3. Seleccione el nombre del grupo de recursos.  Verá un total de seis recursos en el grupo de recursos.
4. Seleccione **Eliminar grupo de recursos** del menú superior.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, desarrolla e implementa plantillas vinculadas. Para información sobre cómo implementar los recursos de Azure en las distintas regiones y cómo usar prácticas de implementación seguras, consulte


> [!div class="nextstepaction"]
> [Uso de Azure Deployment Manager](./deployment-manager-tutorial.md)

