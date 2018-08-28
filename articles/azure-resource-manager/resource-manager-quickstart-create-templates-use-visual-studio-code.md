---
title: Uso de Visual Studio Code para crear plantillas de Azure Resource Manager | Microsoft Docs
description: Use la extensión de herramientas de Azure Resource Manager para trabajar con plantillas de Resource Manager.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/17/2018
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: d50b84ac61210fc89665341ae0c2de3fc4ce0c11
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "42022725"
---
# <a name="quickstart-create-azure-resource-manager-templates-by-using-visual-studio-code"></a>Guía de inicio rápido: Creación de plantillas de Azure Resource Manager mediante Visual Studio Code

Obtenga información sobre cómo crear plantillas de Azure Resource Manager mediante el uso de Visual Studio Code y la extensión Herramientas de Azure Resource Manager. Puede crear plantillas de Resource Manager en Visual Studio Code sin la extensión, pero la extensión proporciona opciones de autocompletar que simplifican el desarrollo de la plantilla. Para entender los conceptos asociados a la implementación y administración de sus soluciones de Azure, consulte [Introducción a Azure Resource Manager](resource-group-overview.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

- [Visual Studio Code](https://code.visualstudio.com/)
- Extensión de herramientas de Resource Manager Para instalarla, siga estos pasos:

    1. Abra Visual Studio Code.
    2. Presione **CTRL + MAYÚS + X** para abrir el panel de extensiones.
    3. Busque **Herramientas de Azure Resource Manager** y seleccione **Instalar**.
    4. Seleccione **Recargar** para finalizar la instalación de la extensión.

## <a name="open-a-quickstart-template"></a>Abra una plantilla de inicio rápido.

En lugar de crear una plantilla desde cero, abra una plantilla en las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/). Las plantillas de inicio rápido de Azure consisten en un repositorio de plantillas de Resource Manager.

La plantilla usada en esta guía de inicio rápido se denomina [Crear una cuenta de almacenamiento estándar](https://azure.microsoft.com/resources/templates/101-storage-account-create/). La plantilla define un recurso de la cuenta de almacenamiento de Azure.

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
2. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Seleccione **Abrir** para abrir el archivo.
4. Seleccione **Archivo**>**Guardar como** para guardar el archivo como **azuredeploy.json** en el equipo local.

## <a name="edit-the-template"></a>Edición de la plantilla

Para saber cómo editar una plantilla mediante Visual Studio Code, puede agregar más de un elemento en la sección outputs.

1. En Visual Studio Code, agregue una salida más a la plantilla exportada:

    ```json
    "storageUri": {
      "type": "string",
      "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
    }
    ```

    Cuando haya terminado, la sección outputs tendrá este aspecto:

    ```json
    "outputs": {
      "storageAccountName": {
        "type": "string",
        "value": "[variables('storageAccountName')]"
      },
      "storageUri": {
        "type": "string",
        "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
      }
    }
    ```

    Si ha copiado y pegado el código dentro de Visual Studio Code, intente volver a escribir el elemento **value** para probar la funcionalidad de Intellisense de la extensión de herramientas de Resource Manager.

    ![Intellisense de Visual Studio Code de la plantilla de Resource Manager](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/resource-manager-templates-visual-studio-code-intellisense.png)

2. Seleccione **File**>**Guardar** para guardar los cambios.

## <a name="deploy-the-template"></a>Implementación de la plantilla

Existen muchos métodos para la implementación de plantillas.  En esta guía de inicio rápido, va a utilizar Cloud Shell desde Azure Portal. Cloud Shell es compatible con la CLI de Azure y Azure PowerShell. Las instrucciones proporcionadas aquí usan la CLI.

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com)
2. Seleccione **Cloud Shell** en la esquina superior derecha, tal como se muestra en la siguiente imagen:

    ![Cloud Shell de Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell.png)

3. Seleccione la flecha hacia abajo y, después, seleccione **Bash** para pasar a la CLI de PowerShell.

    ![CLI de Cloud Shell de Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-cli.png)
4. Seleccione **Reiniciar** para reiniciar el shell.
5. Seleccione **Cargar/descargar archivos** y, después, seleccione **Cargar**.

    ![Archivo de carga de Cloud Shell de Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file.png)
4. Seleccione el archivo que guardó anteriormente en la guía de inicio rápido. El nombre predeterminado es **azuredeploy.json**.
5. En Cloud Shell, ejecute el comando **ls** para comprobar que el archivo se ha cargado correctamente. También puede usar el comando **cat** para comprobar el contenido de la plantilla.

    ![Archivo de lista de Cloud Shell de Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file.png)
6. En Cloud Shell, ejecute los comandos siguientes:

    ```cli
    az group create --name <ResourceGroupName> --location <AzureLocation>

    az group deployment create --name <DeploymentName> --resource-group <ResourceGroupName> --template-file <TemplateFileName>
    ```
    Esta es la captura de pantalla de una implementación de ejemplo:

    ![Plantilla de implementación de Cloud Shell de Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template.png)

    En la captura de pantalla, se utilizan estos valores:

    - **&lt;ResourceGroupName>**: myresourcegroup0709. Hay dos apariciones del parámetro.  Asegúrese de utilizar el mismo valor.
    - **&lt;AzureLocation>**: eastus2
    - **&lt;DeployName>**: mydeployment0709
    - **&lt;TemplateFile>**: azuredeploy.json

    En la salida de la captura de pantalla, el nombre de cuenta de almacenamiento es *3tqebj3slyfyestandardsa*. 

7. Ejecute el siguiente comando de CLI para enumerar la cuenta de almacenamiento recién creada:

    ```cli
    az storage account show --resource-group <ResourceGroupName> --name <StorageAccountName>
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
2. Escriba el nombre del grupo de recursos en el campo **Filtrar por nombre**.
3. Seleccione el nombre del grupo de recursos.  Verá un total de seis recursos en el grupo de recursos.
4. Seleccione **Eliminar grupo de recursos** del menú superior.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a crear una plantilla con Visual Studio Code y cómo implementarla mediante el Cloud Shell de Azure Portal. En el siguiente tutorial, va a aprender a desarrollar una plantilla y a usar la referencia de plantilla.

> [!div class="nextstepaction"]
> [Creación de una cuenta de almacenamiento cifrada](./resource-manager-tutorial-create-encrypted-storage-accounts.md)
