---
title: Creación e implementación de una plantilla de Azure Resource Manager mediante Azure Portal | Microsoft Docs
description: Aprenda cómo crear su primera plantilla de Azure Resource Manager mediante Azure Portal, y cómo implementarla.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 01/11/2019
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: f989a006251313a8439432861477dc133374af35
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2019
ms.locfileid: "54304679"
---
# <a name="quickstart-create-and-deploy-azure-resource-manager-templates-by-using-the-azure-portal"></a>Inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal

Aprenda a generar una plantilla de Azure Resource Manager desde Azure Portal, así como el proceso de editar e implementar la plantilla desde el portal. Las plantillas de Resource Manager son archivos JSON que definen los recursos que necesita para implementar la solución. Para entender los conceptos asociados a la implementación y administración de sus soluciones de Azure, consulte [Introducción a Azure Resource Manager](resource-group-overview.md).

Tras completar el tutorial implemente una cuenta de Azure Storage. El mismo proceso se puede usar para implementar otros recursos de Azure.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="generate-a-template-using-the-portal"></a>Generación de una plantilla con el portal

La creación de una plantilla de Resource Manager desde cero no es una tarea fácil, sobre todo es la primera vez que se realiza una implementación de Azure y no se conoce el formato JSON. Desde Azure Portal se puede configurar un recurso, por ejemplo una cuenta de Azure Storage. Antes de implementar el recurso, puede exportar la configuración a una plantilla de Resource Manager. Puede guardar la plantilla y volver a usarla en el futuro.

Muchos desarrolladores de plantillas experimentados usan este método para generar plantillas de trabajo cuando intentan implementar recursos de Azure con los que no están familiarizados.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Crear un recurso** > **Storage** > **Cuenta de almacenamiento: blob, archivo, tabla, cola**.

    ![Creación de una cuenta de Azure Storage con Azure Portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-portal.png)
3. Escriba la siguiente información:

    - **Grupo de recursos**: seleccione **Crear nuevo** y especifique el nombre que prefiera para el grupo de recursos. En la captura de pantalla, el nombre del grupo de recursos es *mystorage1016rg*. Un grupo de recursos es un contenedor para recursos de Azure. El grupo de recursos facilita la administración de recursos de Azure.
    - **Nombre**: escriba un nombre único para la cuenta de almacenamiento. En la captura de pantalla, el nombre es *mystorage1016*.

    Puede usar los valores predeterminados para el resto de propiedades.

    ![Creación de una configuración de la cuenta de Azure Storage con Azure Portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account.png)

    > [!NOTE]
    > Algunas de las plantillas exportadas requieren algunas modificaciones antes de poder implementarlas.

4. Seleccione **Revisar y crear** en la parte inferior de la pantalla.
5. Seleccione **Descargar una plantilla para la automatización** en la parte inferior de la pantalla. El portal muestra la plantilla generada:

    ![Generación de una plantilla desde el portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template.png)

    El panel principal muestra la plantilla. Es un archivo JSON con cuatro elementos de nivel superior: `schema`, `contentVersion`, `parameters`, `variables`, `resources` y `output`. Para más información, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](./resource-group-authoring-templates.md).

    Hay seis parámetros definidos. Uno de ellos se denomina **storageAccountName**. La segunda parte resaltada de la captura de pantalla anterior muestra cómo hacer referencia a este parámetro en la plantilla. En la sección siguiente, edite la plantilla para usar un nombre generado para la cuenta de almacenamiento.

    En la plantilla, se define un recurso de Azure. El tipo es [Microsoft.Storage/storageAccounts]. Vea cómo se define el recurso y la estructura de definición.
6. Seleccione **Descargar**. Guarde **template.json** del paquete descargado en el equipo. En la siguiente sección, usará una herramienta de implementación de plantilla para modificar la plantilla.
7. Seleccione la pestaña **Parámetro** para ver los valores proporcionados para los parámetros. Anote estos valores, porque los necesitará en la sección siguiente al implementar la plantilla.

    ![Generación de una plantilla desde el portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template-parameters.png)

    Mediante la plantilla y los archivos de parámetros, puede crear un recurso, que en este tutorial será una cuenta de Azure Storage.

## <a name="edit-and-deploy-the-template"></a>Edición e implementación de la plantilla

Azure Portal puede utilizarse para realizar algunas modificaciones básicas de la plantilla. En esta guía de inicio rápido, va a usar una herramienta del portal denominada *Template Deployment*. *Template Deployment* se utiliza en este tutorial, por lo que puede completar todo el tutorial mediante una interfaz: Azure Portal. Para editar una plantilla más compleja, considere la posibilidad de usar [Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md), que proporciona mejores funcionalidades de edición.

Azure requiere que cada servicio de Azure tenga un nombre único. Se podría producir un error en la implementación si ha escrito un nombre de cuenta de almacenamiento que ya existe. Para evitar este problema, modifique la plantilla para que use una llamada de función de plantilla `uniquestring()` para generar un nombre de cuenta de almacenamiento único.

1. En Azure Portal, haga clic en **Crear un recurso**.
2. En **Buscar en Marketplace**, escriba **implementación de plantillas** y, después, presione **ENTRAR**.
3. Seleccione **Implementación de plantillas**.

    ![Biblioteca de plantillas de Azure Resource Manager](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-library.png)
4. Seleccione **Crear**.
5. Seleccione **Cree su propia plantilla en el editor**.
6. Seleccione **Cargar archivo** y, después, siga las instrucciones para cargar template.json que descargó en la última sección.
7. Agregue una variable, tal como se muestra en la siguiente captura de pantalla:

    ```json
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
    ```
    ![Plantillas del Administrador de recursos de Azure](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-edit-storage-account-template-revised.png)

    Aquí se usan dos funciones de plantilla: `concat()` y `uniqueString()`.

8. Quite el parámetro **storageAccountName** resaltado en la captura de pantalla anterior.
9. Actualice el elemento name del recurso **Microsoft.Storage/storageAccounts** para usar la variable recién definida en lugar del parámetro:

    ```json
    "name": "[variables('storageAccountName')]",
    ```

    La plantilla final debe parecerse a esta:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "location": {
                "type": "string"
            },
            "accountType": {
                "type": "string"
            },
            "kind": {
                "type": "string"
            },
            "accessTier": {
                "type": "string"
            },
            "supportsHttpsTrafficOnly": {
                "type": "bool"
            }
        },
        "variables": {
            "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
            {
                "name": "[variables('storageAccountName')]",
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2018-07-01",
                "location": "[parameters('location')]",
                "properties": {
                    "accessTier": "[parameters('accessTier')]",
                    "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]"
                },
                "dependsOn": [],
                "sku": {
                    "name": "[parameters('accountType')]"
                },
                "kind": "[parameters('kind')]"
            }
        ],
        "outputs": {}
    }
    ```
7. Seleccione **Guardar**.
8. Escriba los siguientes valores:

    - **Grupo de recursos**: seleccione **Crear nuevo** y asigne un nombre único a su grupo de recursos.
    - **Ubicación**: seleccione una ubicación para el grupo de recursos. Por ejemplo, **Centro de EE. UU.**. 
    - **Ubicación**: seleccione la ubicación para la cuenta de almacenamiento. Por ejemplo, **Centro de EE. UU.**.
    - **Tipo de cuenta**: escriba **Standard_LRS** para este artículo de inicio rápido.
    - **Variante**: escriba **StorageV2** para este artículo de inicio rápido.
    - **Nivel de acceso**: escriba **Frecuente** para este artículo de inicio rápido.
    - **Https Traffic Only Enabled** (Solo tráfico HTTPS habilitado).  Seleccione **true** para esta guía de inicio rápido.
    - **Acepto los términos y condiciones indicados anteriormente**: (seleccionar)

    Esta es una captura de pantalla de una implementación de ejemplo:

    ![Implementación de plantillas de Azure Resource Manager](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-deploy.png)

10. Seleccione **Comprar**.
11. Seleccione el icono de campana (notificaciones) en la parte superior de la pantalla para ver el estado de implementación. Verá **Implementación en curso**. Espere hasta que se complete la implementación.

    ![Notificación sobre la implementación de plantillas de Azure Resource Manager](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-notification.png)

12. Seleccione **Ir al grupo de recursos** desde el panel de notificación. Debería ver una pantalla parecida a:

    ![Grupo de recursos de la implementación de plantillas de Azure Resource Manager](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-deployment-resource-group.png)

    Puede ver que el estado de la implementación fue correcto y que solo hay una cuenta de almacenamiento en el grupo de recursos. El nombre de cuenta de almacenamiento es una cadena única generada por la plantilla. Para más información sobre las cuentas de almacenamiento de Azure, consulte [Inicio rápido: Carga, descarga y enumeración de blobs mediante Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
2. Escriba el nombre del grupo de recursos en el campo **Filtrar por nombre**.
3. Seleccione el nombre del grupo de recursos.  Verá la cuenta de almacenamiento en el grupo de recursos.
4. Seleccione **Eliminar grupo de recursos** en el menú superior.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido cómo generar una plantilla desde Azure Portal y cómo implementar la plantilla mediante el portal. La plantilla usada en esta guía de inicio rápido es una plantilla sencilla con un recurso de Azure. Cuando la plantilla es compleja, es más fácil de usar Visual Studio Code o Visual Studio para desarrollarla. La siguiente guía de inicio rápido también muestra cómo implementar plantillas mediante Azure PowerShell y la interfaz de la línea de comandos (CLI) de Azure.

> [!div class="nextstepaction"]
> [Creación de plantillas mediante Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md)
