---
title: Creación de plantillas de Azure Resource Manager
description: Describe el proceso de creación de una plantilla de Azure Resource Manager.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/27/2018
ms.author: tomfitz
ms.openlocfilehash: b198f860c10bd207a191057e21a8a159c5ebde26
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60551037"
---
# <a name="create-azure-resource-manager-template"></a>Creación de una plantilla de Azure Resource Manager

En este artículo se describe el proceso y las decisiones que debe tomar al crear una plantilla de Azure Resource Manager. Proporciona información general de ejemplos y características que pueden resultar útiles al crear la plantilla. En este artículo se presupone que va a implementar recursos en un grupo de recursos. Si necesita implementar recursos en su suscripción de Azure, como crear directivas de Azure o asignaciones de control de acceso basado en roles, consulte [Creación de grupos de recursos y otros recursos para una suscripción de Azure](deploy-to-subscription.md).

## <a name="select-json-editor"></a>Selección del editor JSON

La plantilla de Resource Manager es un archivo JSON. Necesita una buena herramienta de creación para trabajar en el archivo JSON. Existen muchas opciones, pero si todavía no tiene un editor preferido, instale [Visual Studio Code (VS Code)](https://code.visualstudio.com/). 

Tras instalar VS Code, agregue la [extensión Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools). Esta extensión agrega muchas características que simplifican la creación de plantillas.

![Plantilla en Visual Studio Code](./media/how-to-create-template/template-visual-studio-code.png)

La captura de pantalla muestra una plantilla de Resource Manager abierta en Visual Studio Code. 

Para ver un tutorial de instalación de la extensión de herramientas de Resource Manager y cómo utilizar VS Code, consulte [Inicio rápido: Creación de plantillas de Azure Resource Manager mediante Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md).

## <a name="understand-the-template-structure"></a>Descripción de la estructura de la plantilla

Vamos a revisar las partes de la plantilla, para comprender su funcionamiento. Es posible que su plantilla no tenga exactamente las mismas secciones. Las secciones en las que vamos a centrarnos son las siguientes:

* La sección [parameters](resource-group-authoring-templates.md#parameters) (parámetros), que muestra los valores que se pueden especificar durante la implementación para personalizar la infraestructura que se implementa. 

* La sección [variables](resource-group-authoring-templates.md#variables), que muestra los valores que se usan a lo largo de la plantilla.

* La sección [functions](resource-group-authoring-templates.md#functions) (funciones), que muestra las expresiones de plantilla personalizadas que se usan en la plantilla.

* La sección [resources](resource-group-authoring-templates.md#resources) (recursos), que muestra los recursos de Azure que se implementan en la suscripción.

* La sección [outputs](resource-group-authoring-templates.md#outputs) (resultados), que muestra los valores que se devuelven una vez finalizada la implementación.

## <a name="look-for-similar-templates"></a>Búsqueda de plantillas similares

A menudo, puede encontrar una plantilla existente que implementa una solución similar a la que necesita. En [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/) encontrará cientos de plantillas de colaboradores de la comunidad.

![Repositorio de plantillas de inicio rápido](./media/how-to-create-template/template-quickstart-repo.png)

Busque a través de ese repositorio para encontrar una plantilla con una solución similar a la que necesita. Es correcto que la plantilla no haga exactamente lo que usted necesita, puede personalizarla.

Tras encontrar una plantilla, seleccione **Explorar en Github**y, a continuación, copie el archivo **azuredeploy.json** del repositorio. En VS Code, cree un nuevo archivo denominado **azuredeploy.json** y agregue el contenido del archivo de plantilla que ha copiado desde el repositorio de inicio rápido.

## <a name="add-resources"></a>Adición de recursos

Es posible que quiera personalizar la plantilla para asegurarse de que hace exactamente lo que usted desea. Primero, revise los recursos implementados. Es posible que tenga que agregar, quitar o cambiar recursos en la plantilla. Para obtener descripciones y sintaxis de los recursos, consulte la [referencia de plantillas de Azure Resource Manager](/azure/templates/).

![Referencia de plantilla](./media/how-to-create-template/template-reference.png)

Después de revisar las propiedades, realice los cambios necesarios. Para obtener recomendaciones sobre cómo definir los recursos, consulte las [prácticas recomendadas sobre los recursos](template-best-practices.md#resources).

## <a name="add-or-remove-parameters"></a>Incorporación o eliminación de parámetros

También es posible que deba ajustar los parámetros de la plantilla. Puede agregar o quitar parámetros basándose en el grado de personalización que desee habilitar durante la implementación. Para obtener recomendaciones sobre cómo usar los parámetros, consulte las [prácticas recomendadas sobre los parámetros](template-best-practices.md#parameters).

## <a name="add-tags"></a>Incorporación de etiquetas

Puede agregar etiquetas a los recursos para organizarlos de forma lógica por categorías, y dividir los costes de facturación. Agregar etiquetas es fácil, puede aplicarlas en el archivo JSON para el recurso. Por ejemplo, la cuenta de almacenamiento siguiente tiene dos etiquetas:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
      {
        "apiVersion": "2016-01-01",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[concat('storage', uniqueString(resourceGroup().id))]",
        "location": "[resourceGroup().location]",
        "tags": {
          "Dept": "Finance",
          "Environment": "Production"
        },
        "sku": {
          "name": "Standard_LRS"
        },
        "kind": "Storage",
        "properties": { }
      }
    ]
}
```

También puede aplicar dinámicamente las etiquetas de los parámetros. Para obtener más información, consulte la información sobre las [etiquetas en la plantilla](resource-group-using-tags.md#templates).

## <a name="review-template-functions"></a>Revisión de funciones de la plantilla

Es posible que observe expresiones que están entre corchetes en la plantilla, como `"[some-expression]"`. Estas expresiones utilizan las funciones de plantilla para generar dinámicamente valores durante la implementación.

Por ejemplo, a menudo verá una expresión como la siguiente:

```json
"name": "[parameters('siteName')]"
```

Esta expresión obtiene el valor de un parámetro. El valor se asigna a la propiedad de nombre.

O bien, es posible que vea una expresión más compleja que usa varias funciones, como la siguiente:

```json
"[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2018-07-01')]"
```

Esa expresión obtiene un objeto con las propiedades de una cuenta de almacenamiento.

Para comprender qué hacen las funciones, revise la documentación de [referencia de las funciones de las plantillas](resource-group-template-functions.md).

## <a name="create-more-than-one-instance"></a>Creación de más de una instancia

A veces querrá crear más de una instancia de un recurso. Por ejemplo, puede que necesite varias cuentas de almacenamiento. En lugar de repetir el recurso a través de la plantilla, se puede utilizar la sintaxis `copy` para especificar más de una instancia.

En el ejemplo siguiente se crean tres cuentas de almacenamiento:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        }
    ],
    "outputs": {}
}
```

También se puede especificar el número de instancias dinámicamente a partir de un parámetro. Para obtener más información, consulte [Implementación de varias instancias de un recurso o una propiedad en plantillas de Azure Resource Manager](resource-group-create-multiple.md).

## <a name="conditionally-deploy-a-resource"></a>Implementación de un recurso de forma condicional

En ocasiones necesitará especificar si se implementa un recurso en la plantilla, durante la implementación. Por ejemplo, es posible que quiera tener la flexibilidad de poder implementar un recurso nuevo o usar un recurso existente. El elemento `condition` le ofrece la capacidad de activar o desactivar la implementación de un recurso. Cuando la expresión en el elemento de la condición está establecida como "true" (verdadero), se implementa el recurso. Si es "false" (falso), el recurso se omite durante la implementación.

En el ejemplo siguiente se implementa una cuenta de almacenamiento de forma condicional:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

Para obtener más información, consulte el [elemento de condición](resource-group-authoring-templates.md#condition).

## <a name="review-dependencies"></a>Revisión de las dependencias

Algunos recursos de la plantilla deben implementarse antes que otros recursos. Por ejemplo, el servidor SQL debe existir antes de que se cree la base de datos SQL. Implícitamente, Resource Manager determina el orden de implementación de los recursos cuando se usa la [función de referencia](resource-group-template-functions-resource.md#reference). Sin embargo, en algunos casos, deberá definir explícitamente las dependencias mediante el elemento `dependsOn`. Revise la plantilla para ver si debe agregarse alguna dependencia. Tenga la precaución de no agregar dependencias innecesarias, ya que pueden ralentizar la implementación o crear referencias circulares.

En la siguiente imagen se muestra el orden de dependencia de distintos recursos de App Service:

![Dependencias de la aplicación web](./media/how-to-create-template/web-dependencies.png)

En el ejemplo siguiente se muestra parte de una plantilla que define dependencias.

```json
{
    "name": "[parameters('appName')]",
    "type": "Microsoft.Web/Sites",
    ...
    "resources": [
      {
          "name": "MSDeploy",
          "type": "Extensions",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('appName'))]",
            "[concat('Microsoft.Sql/servers/', parameters('dbServerName'), '/databases/', parameters('dbName'))]",
          ],
          ...
      },
      {
          "name": "connectionstrings",
          "type": "config",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('appName'), '/Extensions/MSDeploy')]"
          ],
          ...
      }
    ]
}
```

Para más información consulte [Definición del orden de implementación de recursos en plantillas de Azure Resource Manager](resource-group-define-dependencies.md).

## <a name="review-recommended-practices"></a>Revisión de los procedimientos recomendados

Antes de implementar la plantilla, revise [Procedimientos recomendados de plantillas de Azure Resource Manager](template-best-practices.md) para ver si existe algún enfoque recomendado que quiera implementar en ella.

Si tiene que usar la plantilla en entornos de nube de Azure distintos, consulte [Desarrollo de plantillas de Azure Resource Manager para mantener la coherencia en la nube](templates-cloud-consistency.md).

## <a name="next-steps"></a>Pasos siguientes

* Para implementar una plantilla, consulte el tema de [implementación con la CLI de Azure](resource-group-template-deploy-cli.md) o [implementación con PowerShell](resource-group-template-deploy.md).
* Para obtener instrucciones de inicio rápido detalladas sobre cómo crear una plantilla, consulte [Creación de plantillas de Azure Resource Manager mediante Visual Studio Code](resource-manager-quickstart-create-templates-use-visual-studio-code.md).
* Para obtener una lista de las funciones disponibles en una plantilla, consulte [Funciones de plantilla](resource-group-template-functions.md).
