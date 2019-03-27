---
title: Uso de la referencia de plantilla de Azure Resource Manager | Microsoft Docs
description: Use la referencia de plantilla de Azure Resource Manager para crear una plantilla para implementar una cuenta de almacenamiento cifrada.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 03/04/2019
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: 68199083369018cab1c9f83dd6781653a8cae37c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57849302"
---
# <a name="tutorial-utilize-the-azure-resource-manager-template-reference"></a>Tutorial: Uso de la referencia de plantilla de Azure Resource Manager

Aprenda a encontrar la información del esquema de la plantilla y a usar dicha información para crear plantillas de Azure Resource Manager.

En este tutorial se usa una plantilla base de las plantillas de inicio rápido de Azure. Utilice la documentación de referencia de la plantilla para personalizar la plantilla para crear una cuenta de Storage cifrada.

![Cuenta de almacenamiento cifrada de implementación de referencia de plantilla de Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-tutorial-deploy-encrypted-storage-account.png)

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Abra una plantilla de inicio rápido.
> * Descripción de la plantilla
> * Búsqueda de la referencia de la plantilla
> * Edición de la plantilla
> * Implementación de la plantilla

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

* [Visual Studio Code](https://code.visualstudio.com/) con la [extensión Resource Manager Tools](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="open-a-quickstart-template"></a>Abra una plantilla de inicio rápido.

[Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/) es un repositorio de plantillas de Resource Manager. En lugar de crear una plantilla desde cero, puede buscar una plantilla de ejemplo y personalizarla. La plantilla usada en esta guía de inicio rápido se denomina [Crear una cuenta de almacenamiento estándar](https://azure.microsoft.com/resources/templates/101-storage-account-create/). La plantilla define un recurso de la cuenta de almacenamiento de Azure.

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
2. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Seleccione **Abrir** para abrir el archivo.
4. Seleccione **Archivo**>**Guardar como** para guardar el archivo como **azuredeploy.json** en el equipo local.

## <a name="understand-the-schema"></a>Información sobre el esquema

1. En VS Code, contraiga la plantilla al nivel raíz. Tiene la estructura más sencilla con los siguientes elementos:

    ![La estructura más sencilla de la plantilla de Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-simplest-structure.png)

    * **$schema**: especifique la ubicación del archivo de esquema JSON que describe la versión del lenguaje de plantilla.
    * **contentVersion**: especifique cualquier valor para este elemento para documentar cambios importantes en la plantilla.
    * **parámetros**: especifique los valores que se proporcionan cuando se ejecuta la implementación para personalizar la implementación de recursos.
    * **variables**: especifique los valores que se usan como fragmentos JSON en la plantilla para simplificar las expresiones de lenguaje de plantilla.
    * **recursos**: especifique los tipos de recursos que se implementan o actualizan en un grupo de recursos.
    * **salidas**: especifique los valores que se devuelven después de la implementación.

2. Expanda **recursos**. Hay un recurso `Microsoft.Storage/storageAccounts` definido. La plantilla crea una cuenta de Storage sin cifrar.

    ![Definición de cuenta de almacenamiento de la plantilla de Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-resource.png)

## <a name="find-the-template-reference"></a>Búsqueda de la referencia de la plantilla

1. Vaya a la [referencia sobre las plantillas de Azure](https://docs.microsoft.com/azure/templates/).
2. En el cuadro **Filtrar por título**, escriba **cuentas de almacenamiento**.
3. Seleccione **Reference/Template reference/Storage/&lt;Version>/Storage Accounts** como se muestra en la captura de pantalla siguiente:

    ![Cuenta de almacenamiento de referencia de plantilla de Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-resources-reference-storage-accounts.png)

    Si no sabe qué versión elegir, use la versión más reciente.

4. Búsqueda de la información de definición relacionada con el cifrado.  

    ```json
    "encryption": {
      "services": {
        "blob": {
          "enabled": boolean
        },
        "file": {
          "enabled": boolean
        }
      },
      "keySource": "string",
      "keyvaultproperties": {
        "keyname": "string",
        "keyversion": "string",
        "keyvaulturi": "string"
      }
    },
    ```

    En la misma página web, la descripción siguiente confirma que el objeto `encryption` se usa para crear una cuenta de almacenamiento cifrada.

    ![Cifrado de la cuenta de almacenamiento de referencia de la plantilla de Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-resources-reference-storage-accounts-encryption.png)

    Y hay dos maneras de administrar la clave de cifrado. Puede usar las claves de cifrado administradas por Microsoft con el Cifrado del servicio Storage, o puede usar sus propias claves de cifrado. Para simplificar este tutorial, use la opción `Microsoft.Storage`, con el fin de que no tenga que crear una instancia de Azure Key Vault.

    ![Objeto de cifrado de la cuenta de almacenamiento de referencia de la plantilla de Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-resources-reference-storage-accounts-encryption-object.png)

    El objeto de cifrado debe ser como este:

    ```json
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
    ```

## <a name="edit-the-template"></a>Edición de la plantilla

Desde Visual Studio Code, modifique la plantilla para que el elemento resources sea como este:

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

En este tutorial, aprendió a usar la referencia de plantilla para personalizar una plantilla existente. Para información sobre cómo crear varias instancias de cuenta de almacenamiento, consulte:

> [!div class="nextstepaction"]
> [Creación de varias instancias](./resource-manager-tutorial-create-multiple-instances.md)
