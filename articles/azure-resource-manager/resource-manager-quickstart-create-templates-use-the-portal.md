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
ms.date: 08/22/2018
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: fb59ada68d773d1ef4148c1a4c1ae35620972aca
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43093713"
---
# <a name="quickstart-create-and-deploy-azure-resource-manager-templates-by-using-the-azure-portal"></a>Guía de inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal

Aprenda cómo crear su primera plantilla de Azure Resource Manager al generar una con Azure Portal, y cómo editar e implementar la plantilla desde el portal.

Las plantillas de Resource Manager son archivos JSON que definen los recursos que necesita para implementar la solución. Para crear una plantilla, no tiene que empezar siempre desde cero. En este tutorial, aprenderá a generar una plantilla desde Azure Portal. Después, puede personalizar la plantilla e implementarla.

Las instrucciones de este tutorial crean una cuenta de Azure Storage. Puede usar el mismo proceso para crear otros recursos de Azure.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="generate-a-template-using-the-portal"></a>Generación de una plantilla con el portal

En esta sección, creará una cuenta de almacenamiento con Azure Portal. Antes de implementar la cuenta de almacenamiento, tiene la opción de explorar la plantilla generada por el portal en función de sus configuraciones. Puede guardar la plantilla y volver a usarla en el futuro.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Crear un recurso** > **Storage** > **Cuenta de almacenamiento: blob, archivo, tabla, cola**.

    ![Creación de una cuenta de Azure Storage con Azure Portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-portal.png)
3. Escriba la siguiente información. Asegúrese de seleccionar **Opciones de automatización** en lugar de **Crear** en el siguiente paso, para que pueda ver la plantilla antes de que se implemente.

    - **Nombre**: escriba un nombre único para la cuenta de almacenamiento. En la captura de pantalla, el nombre es *mystorage0626*.
    - **Grupo de recursos**: cree un nuevo grupo de recursos de Azure con el nombre que prefiera. En la captura de pantalla, el nombre del grupo de recursos es *mystorage0626rg*.

    Puede usar los valores predeterminados para el resto de propiedades.

    ![Creación de una configuración de la cuenta de Azure Storage con Azure Portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account.png)

    > [!NOTE]
    > Algunas de las plantillas exportadas requieren algunas modificaciones antes de poder implementarlas.

4. Seleccione **Opciones de automatización** en la parte inferior de la pantalla. El portal muestra la plantilla en la pestaña **Plantilla**:

    ![Generación de una plantilla desde el portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template.png)

    El panel principal muestra la plantilla. Es un archivo JSON con cuatro elementos de nivel superior. Para más información, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](./resource-group-authoring-templates.md).

    En el elemento **Parámetro**, hay cinco parámetros definidos. Para ver los valores que ha proporcionado durante la implementación, seleccione la pestaña **Parámetros**.

    ![Generación de una plantilla desde el portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template-parameters.png)

    Estos valores son los que haya configurado en la sección anterior. Con la plantilla y los archivos de parámetros, puede crear una cuenta de almacenamiento de Azure.

5. En la parte superior de las pestañas, hay tres elementos de menú:

    - **Descargar**: descargue la plantilla y el archivo de parámetros en el equipo local.
    - **Agregar a la biblioteca**: agregue la plantilla a la biblioteca para volverla a usar en el futuro.
    - **Implementar**: implemente la cuenta de almacenamiento de Azure en Azure.

    En este tutorial, usará la opción **Agregar a la biblioteca**.

6. Seleccione **Agregar a la biblioteca**.
7. Escriba el **nombre** y la **descripción** y, a continuación, seleccione **Guardar**.

> [!NOTE]
> La característica de biblioteca de plantillas se encuentra en versión preliminar. Mayoría de los usuarios eligen guardar sus plantillas en un equipo local o en un almacenamiento público como Github.  

## <a name="edit-and-deploy-the-template"></a>Edición e implementación de la plantilla

En esta sección, va a abrir la plantilla guardada en la biblioteca de plantillas, va a editar la plantilla dentro del portal y, después, va a implementar la plantilla revisada. Para editar una plantilla más compleja, considere el uso de [Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md) que proporciona mejores funcionalidades de edición.

Azure requiere que cada servicio de Azure tenga un nombre único. Se producirá un error en la implementación si escribe un nombre de cuenta de almacenamiento que ya existe. Para evitar este problema, puede usar una llamada de función de plantilla uniquestring() para generar un nombre de cuenta de almacenamiento único.

1. En Azure Portal, seleccione **Todos los servicios** en el menú de la izquierda, escriba **plantilla** en el cuadro de filtro y, a continuación, seleccione **Plantillas (Vista previa)**.

    ![Plantillas del Administrador de recursos de Azure](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-library.png)
2. Seleccione la plantilla que guardó en la última sección. El nombre utilizado en la captura de pantalla es *mystorage0626*.
3. Seleccione **Editar** y, después, seleccione **Plantilla agregada**.

    ![Plantillas del Administrador de recursos de Azure](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-edit-storage-account-template.png)

4. Agregue un elemento **variables** y, después, agregue una variable, tal como se muestra en la siguiente captura de pantalla:

    ```json
    "variables": {
        "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
    },
    ```
    ![Plantillas del Administrador de recursos de Azure](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-edit-storage-account-template-revised.png)

    Aquí se usan dos funciones: *concat()* y *uniqueString()*. La función uniqueString() es útil para crear un nombre único para un recurso.

5. Quite el parámetro **name** resaltado en la captura de pantalla anterior.
6. Actualice el elemento name del recurso **Microsoft.Storage/storageAccounts** para usar la variable recién definida en lugar del parámetro:

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
            "httpsTrafficOnlyEnabled": {
                "type": "bool"
            }
        },
        "variables": {
            "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
            {
                "apiVersion": "2018-02-01",
                "name": "[variables('storageAccountName')]",
                "location": "[parameters('location')]",
                "type": "Microsoft.Storage/storageAccounts",
                "sku": {
                    "name": "[parameters('accountType')]"
                },
                "kind": "[parameters('kind')]",
                "properties": {
                    "supportsHttpsTrafficOnly": "[parameters('httpsTrafficOnlyEnabled')]",
                    "encryption": {
                        "services": {
                            "blob": {
                                "enabled": true
                            },
                            "file": {
                                "enabled": true
                            }
                        },
                        "keySource": "Microsoft.Storage"
                    }
                },
                "dependsOn": []
            }
        ]
    }
    ```
7. Seleccione **Aceptar** y, después, seleccione **Guardar** para guardar los cambios.
8. Seleccione **Implementar**.
9. Escriba los siguientes valores:

    - **Suscripción**: seleccione su suscripción de Azure.
    - **Grupo de recursos**: dé un nombre único a su grupo de recursos.
    - **Ubicación**: seleccione una ubicación para el grupo de recursos.
    - **Ubicación**: seleccione la ubicación para la cuenta de almacenamiento.  Puede usar la misma ubicación que el grupo de recursos.
    - **Tipo de cuenta**: escriba **Standard_LRS** para esta guía de inicio rápido.
    - **Variante**: escriba **Almacenamiento** para esta guía de inicio rápido.
    - **Https Traffic Only Enabled** (Solo tráfico HTTPS habilitado).  Seleccione **false** para esta guía de inicio rápido.
    - **Acepto los términos y condiciones indicados anteriormente**: (seleccionar)

    Esta es una captura de pantalla de una implementación de ejemplo:

    ![Implementación de plantillas de Azure Resource Manager](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-deploy.png)

10. Seleccione **Comprar**.
11. Seleccione el icono de campana (notificaciones) en la parte superior de la pantalla para ver el estado de implementación.

    ![Notificación sobre la implementación de plantillas de Azure Resource Manager](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-notification.png)

12. Seleccione **Ir al grupo de recursos** desde el panel de notificación. Debería ver una pantalla parecida a:

    ![Grupo de recursos de la implementación de plantillas de Azure Resource Manager](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-deployment-resource-group.png)

    Puede ver que el estado de la implementación fue correcto y que solo hay una cuenta de almacenamiento en el grupo de recursos. El nombre de cuenta de almacenamiento es una cadena única generada por la plantilla. Para más información sobre el uso de cuentas de Azure Storage, consulte [Inicio rápido: Carga, descarga y enumeración de blobs mediante Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
2. Escriba el nombre del grupo de recursos en el campo **Filtrar por nombre**.
3. Seleccione el nombre del grupo de recursos.  Verá la cuenta de almacenamiento en el grupo de recursos.
4. Seleccione **Eliminar grupo de recursos** en el menú superior.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido cómo generar una plantilla desde Azure Portal y cómo implementar la plantilla mediante el portal. La plantilla usada en esta guía de inicio rápido es una plantilla sencilla con un recurso de Azure. Cuando la plantilla es compleja, es más fácil de usar Visual Studio Code o Visual Studio para desarrollarla. El siguiente inicio rápido también muestra cómo implementar plantillas mediante Azure PowerShell y la interfaz de la línea de comandos (CLI) de Azure.

> [!div class="nextstepaction"]
> [Creación de plantillas mediante Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md)
