---
title: 'Tutorial: Uso de un archivo de parámetros para implementar la plantilla'
description: Use archivos de parámetros que contengan los valores que se usarán para implementar la plantilla de Azure Resource Manager.
author: mumian
ms.date: 10/04/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 6a12d92c0cfb9d86ebf4c335c351944997f79b4e
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2020
ms.locfileid: "76773158"
---
# <a name="tutorial-use-parameter-files-to-deploy-your-resource-manager-template"></a>Tutorial: Uso de archivos de parámetros para implementar la plantilla de Resource Manager

En este tutorial, aprenderá a usar [archivos de parámetros](parameter-files.md) para almacenar los valores que se pasan durante la implementación. En los tutoriales anteriores, usó parámetros insertados con el comando de implementación. Este enfoque funcionó para probar la plantilla, pero al automatizar las implementaciones puede ser más fácil pasar un conjunto de valores del entorno. Los archivos de parámetros facilitan el empaquetado de los valores de los parámetros de un entorno específico. En este tutorial, creará archivos de parámetros para entornos de desarrollo y producción. Su tiempo de realización es de unos **12 minutos**.

## <a name="prerequisites"></a>Prerequisites

Aunque no es obligatorio, se recomienda realizar el [tutorial sobre etiquetas](template-tutorial-add-tags.md).

Debe tener Visual Studio Code con la extensión Herramientas de Resource Manager y Azure PowerShell o la CLI de Azure. Para más información, consulte las [herramientas de plantilla](template-tutorial-create-first-template.md#get-tools).

## <a name="review-template"></a>Revisión de la plantilla

La plantilla tiene muchos parámetros que puede proporcionar durante la implementación. Al final del tutorial anterior, la plantilla tenía el siguiente aspecto:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.json":::

Esta plantilla funciona bien, pero ahora desea administrar fácilmente los parámetros que pasa a la plantilla.

## <a name="add-parameter-files"></a>Adición de archivos de parámetros

Los archivos de parámetros son archivos JSON con una estructura similar a la plantilla. En el archivo, proporcione los valores de los parámetros que desea pasar durante la implementación.

En VS Code, cree un nuevo archivo con el siguiente contenido. Guarde el archivo con el nombre **azuredeploy.parameters.dev.json**.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.parameters.dev.json":::

Este archivo es el archivo de parámetros para el entorno de desarrollo. Observe que usa Standard_LRS para la cuenta de almacenamiento, asigna un nombre a los recursos con el prefijo **dev** y establece la etiqueta **Environment** en **Dev**.

De nuevo, cree un nuevo archivo con el siguiente contenido. Guarde el archivo con el nombre **azuredeploy.parameters.prod.json**.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.parameters.prod.json":::

Este archivo es el archivo de parámetros para el entorno de producción. Observe que usa Standard_GRS para la cuenta de almacenamiento, asigna un nombre a los recursos con el prefijo **contoso** y establece la etiqueta **Environment** en **Production**. En un entorno de producción real, también deseará usar una instancia de App Service con una SKU que no sea gratuita, pero seguiremos con esa SKU para este tutorial.

## <a name="deploy-template"></a>Implementar plantilla

Use la CLI de Azure o Azure PowerShell para implementar la plantilla.

Como prueba final de la plantilla, vamos a crear dos nuevos grupos de recursos. Uno para el entorno de desarrollo y otro para el entorno de producción.

En primer lugar, se implementará en el entorno de desarrollo.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$templateFile = "{provide-the-path-to-the-template-file}"
$parameterFile="{path-to-azuredeploy.parameters.dev.json}"
New-AzResourceGroup `
  -Name myResourceGroupDev `
  -Location "East US"
New-AzResourceGroupDeployment `
  -Name devenvironment `
  -ResourceGroupName myResourceGroupDev `
  -TemplateFile $templateFile `
  -TemplateParameterFile $parameterFile
```

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli
templateFile="{provide-the-path-to-the-template-file}"
az group create \
  --name myResourceGroupDev \
  --location "East US"
az group deployment create \
  --name devenvironment \
  --resource-group myResourceGroupDev \
  --template-file $templateFile \
  --parameters azuredeploy.parameters.dev.json
```

---

Ahora, se implementará en el entorno de producción.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$parameterFile="{path-to-azuredeploy.parameters.prod.json}"
New-AzResourceGroup `
  -Name myResourceGroupProd `
  -Location "West US"
New-AzResourceGroupDeployment `
  -Name prodenvironment `
  -ResourceGroupName myResourceGroupProd `
  -TemplateFile $templateFile `
  -TemplateParameterFile $parameterFile
```

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli
az group create \
  --name myResourceGroupProd \
  --location "West US"
az group deployment create \
  --name prodenvironment \
  --resource-group myResourceGroupProd \
  --template-file $templateFile \
  --parameters azuredeploy.parameters.prod.json
```

---

## <a name="verify-deployment"></a>Comprobación de la implementación

Para comprobar la implementación, explore el grupo de recursos desde Azure Portal.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. En el menú izquierdo, seleccione **Grupos de recursos**.
1. Verá los dos nuevos grupos de recursos que implementó en este tutorial.
1. Seleccione cualquier grupo de recursos y consulte los recursos implementados. Observe que coinciden con los valores especificados en el archivo de parámetros para ese entorno.

## <a name="clean-up-resources"></a>Limpieza de recursos

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
2. Escriba el nombre del grupo de recursos en el campo **Filtrar por nombre**. Si ha completado esta serie, tiene tres grupos de recursos que eliminar: myResourceGroup, myResourceGroupDev y myResourceGroupProd.
3. Seleccione el nombre del grupo de recursos.
4. Seleccione **Eliminar grupo de recursos** del menú superior.

## <a name="next-steps"></a>Pasos siguientes

Enhorabuena, ha terminado esta introducción a la implementación de plantillas en Azure. Si tiene comentarios y sugerencias, háganoslo saber en la sección de comentarios. Gracias.

Está listo para pasar a conceptos más avanzados sobre las plantillas. En el tutorial siguiente se explica con más detalle cómo usar la documentación de referencia de la plantilla para ayudar a definir los recursos que se van a implementar.

> [!div class="nextstepaction"]
> [Utilización de la referencia de la plantilla](template-tutorial-create-encrypted-storage-accounts.md)
