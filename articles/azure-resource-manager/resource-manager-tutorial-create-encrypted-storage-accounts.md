---
title: Creación de una plantilla de Azure Resource Manager para la implementación de una cuenta de almacenamiento cifrada | Microsoft Docs
description: Use Visual Studio Code para crear una plantilla para la implementación de una cuenta de almacenamiento cifrada.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 08/27/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 57d5f7039831c9fd617926f20f3ff001b22ef314
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43097892"
---
# <a name="tutorial-create-an-azure-resource-manager-template-for-deploying-an-encrypted-storage-account"></a>Tutorial: Creación de una plantilla de Azure Resource Manager para la implementación de una cuenta de almacenamiento cifrada

Aprenda a buscar información para completar una plantilla de Azure Resource Manager.

En este tutorial, usará una plantilla base a partir de las plantillas de inicio rápido de Azure para crear una cuenta de Azure Storage.  Mediante la documentación de referencia de la plantilla, personalizará la plantilla base para crear una cuenta de almacenamiento cifrada.

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Abra una plantilla de inicio rápido.
> * Descripción de la plantilla
> * Edición de la plantilla
> * Implementación de la plantilla

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

* [Visual Studio Code](https://code.visualstudio.com/)
* Extensión de herramientas de Resource Manager Para realizar la instalación, consulte [Instalación de la extensión de herramientas de Resource Manager](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="open-a-quickstart-template"></a>Abra una plantilla de inicio rápido.

La plantilla usada en esta guía de inicio rápido se denomina [Crear una cuenta de almacenamiento estándar](https://azure.microsoft.com/resources/templates/101-storage-account-create/). La plantilla define un recurso de la cuenta de almacenamiento de Azure.

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
2. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Seleccione **Abrir** para abrir el archivo.
4. Seleccione **Archivo**>**Guardar como** para guardar el archivo como **azuredeploy.json** en el equipo local.

## <a name="understand-the-format"></a>Descripción del formato

En VS Code, contraiga la plantilla al nivel raíz. Tiene la estructura más sencilla con los siguientes elementos:

![La estructura más sencilla de la plantilla de Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-simplest-structure.png)

* **$schema**: especifique la ubicación del archivo de esquema JSON que describe la versión del lenguaje de plantilla.
* **contentVersion**: especifique cualquier valor para este elemento para documentar cambios importantes en la plantilla.
* **parámetros**: especifique los valores que se proporcionan cuando se ejecuta la implementación para personalizar la implementación de recursos.
* **variables**: especifique los valores que se usan como fragmentos JSON en la plantilla para simplificar las expresiones de lenguaje de plantilla.
* **recursos**: especifique los tipos de recursos que se implementan o actualizan en un grupo de recursos.
* **salidas**: especifique los valores que se devuelven después de la implementación.

## <a name="use-parameters-in-template"></a>Uso de parámetros en la plantilla

Los parámetros le permiten personalizar la implementación al proporcionar valores que son específicos para un entorno concreto. Puede usar los parámetros definidos en la plantilla al establecer los valores de la cuenta de almacenamiento.

![Parámetros de plantilla de Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-parameters.png)

En esta plantilla, se definen dos parámetros. Tenga en cuenta que se usa una función de plantilla en location.defaultValue:

```json
"defaultValue": "[resourceGroup().location]",
```

La función resourceGroup() devuelve un objeto que representa el grupo de recursos actual. Para obtener una lista de las funciones de plantilla, consulte [Funciones de la plantilla de Azure Resource Manager](./resource-group-template-functions.md).

Para usar los parámetros definidos en la plantilla:

```json
"location": "[parameters('location')]",
"name": "[parameters('storageAccountType')]"
```

## <a name="use-variables-in-template"></a>Uso de variables en la plantilla

Las variables le permiten crear valores que pueden usarse en toda la plantilla. Las variables ayudan a reducir la complejidad de las plantillas.

![Variables de plantilla de Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-variables.png)

Esta plantilla define una variable *storageAccountName*. En la definición, se usan dos funciones de plantilla:

- **concat()**: permite concatenar cadenas. Para más información, consulte [concat](./resource-group-template-functions-string.md#concat).
- **uniqueString()**: crea una cadena de hash determinista basada en los valores proporcionados como parámetros. Cada cuenta de Azure Storage debe tener un nombre único en todo Azure. Esta función proporciona una cadena única. Para obtener más funciones de cadena, consulte [Funciones de cadena](./resource-group-template-functions-string.md).

Para usar la variable definida en la plantilla:

```json
"name": "[variables('storageAccountName')]"
```

## <a name="edit-the-template"></a>Edición de la plantilla

El objetivo de este tutorial es definir una plantilla para crear una cuenta de almacenamiento cifrada.  La plantilla de ejemplo solo crea una cuenta de almacenamiento sin cifrar básica. Para buscar la configuración relacionada con el cifrado, puede usar la referencia de plantilla de la cuenta de Azure Storage.

1. Vaya a [Plantillas de Azure](https://docs.microsoft.com/azure/templates/).
2. En el índice de la izquierda, seleccione **Reference (Referencia)**->**Storage**->**Storage Accounts (Cuentas de Storage)**. También puede escribir **storage** en el campo **Filtrar por título**.  La página contiene el esquema para definir la información de una cuenta de Storage.
3. Explore la información relacionada con el cifrado.  
4. Dentro del elemento de propiedades de la definición de recursos de la cuenta de almacenamiento, agregue el siguiente json:

    ```json
    "encryption": {
        "keySource": "Microsoft.Storage",
        "services": {
            "blob": {
                "enabled": true
            }
        }
    }
    ```
    Este elemento habilita la función de cifrado del servicio de almacenamiento de blobs.

Desde Visual Studio Code, modifique la plantilla para que el elemento resources final sea parecido a:

![Recursos de la cuenta de almacenamiento cifrada de la plantilla de Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-resources.png)

## <a name="deploy-the-template"></a>Implementación de la plantilla

Consulte la sección [Implementación de la plantilla](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) de la guía de inicio rápido de Visual Studio Code para el procedimiento de implementación.

La siguiente captura de pantalla muestra el comando de la CLI para enumerar la cuenta de almacenamiento recién creada, lo cual indica que se ha habilitado el cifrado para el almacenamiento de blobs.

![Cuenta de almacenamiento cifrada de Azure Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-account.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
2. Escriba el nombre del grupo de recursos en el campo **Filtrar por nombre**.
3. Seleccione el nombre del grupo de recursos.  Verá un total de seis recursos en el grupo de recursos.
4. Seleccione **Eliminar grupo de recursos** del menú superior.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a usar la referencia de plantilla para personalizar una plantilla existente. La plantilla usada en este tutorial solo contiene un recurso de Azure.  En el siguiente tutorial, va a desarrollar una plantilla con varios recursos. Algunos de los recursos tienen recursos dependientes.

> [!div class="nextstepaction"]
> [Creación de varios recursos](./resource-manager-tutorial-create-templates-with-dependent-resources.md)
