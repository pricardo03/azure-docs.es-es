---
title: Implementación de varias instancias de recursos
description: Use la operación de copia y matrices en una plantilla del Administrador de recursos de Azure para iterar varias veces al implementar recursos.
ms.topic: conceptual
ms.date: 09/27/2019
ms.openlocfilehash: 0250f5ee64c91d8d75ad246271ab31324a2553f8
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76836936"
---
# <a name="resource-property-or-variable-iteration-in-azure-resource-manager-templates"></a>Recurso, propiedad o iteración de variables en las plantillas de Azure Resource Manager

En este artículo se muestra cómo crear más de una instancia de un recurso, variable o propiedad en una plantilla de Azure Resource Manager. Para crear varias instancias, agregue el objeto `copy` a la plantilla.

Cuando se usa con un recurso, el objeto copy tiene el formato siguiente:

```json
"copy": {
  "name": "<name-of-loop>",
  "count": <number-of-iterations>,
  "mode": "serial" <or> "parallel",
  "batchSize": <number-to-deploy-serially>
}
```

Cuando se usa con una variable o una propiedad, el objeto copy tiene el formato siguiente:

```json
"copy": [
  {
    "name": "<name-of-loop>",
    "count": <number-of-iterations>,
    "input": <values-for-the-property-or-variable>
  }
]
```

Ambos usos se describen con más detalle en este artículo. Para obtener un tutorial, consulte [Tutorial: create multiple resource instances using Resource Manager templates](template-tutorial-create-multiple-instances.md) (Tutorial: Creación de varias instancias de recursos con plantillas de Resource Manager).

Si tiene que especificar si un recurso se implementa, consulte [Elemento condition](conditional-resource-deployment.md).

## <a name="copy-limits"></a>Límites de copia

Para especificar el número de iteraciones, proporcione un valor para la propiedad count. El valor de count no puede superar 800.

El valor de count no puede ser un número negativo. Si implementa una plantilla con Azure PowerShell 2.6 o posterior, la CLI de Azure 2.0.74 o posterior, o bien con la API REST, versión **2019-05-10** o posterior, puede establecer el valor de count en cero. Las versiones anteriores de PowerShell, la CLI y API REST no admiten un valor de count de cero.

Tenga cuidado al usar la [implementación de modo completo](deployment-modes.md) con copia. Si vuelve a implementar con el modo completo en un grupo de recursos, se eliminan todos los recursos que no se especifican en la plantilla después de resolver el bucle de copia.

Los límites del valor de count son los mismos si se usa con un recurso, variable o propiedad.

## <a name="resource-iteration"></a>Iteración de recursos

Si desea crear más de una instancia de un recurso en una implementación, agregue un elemento `copy` al tipo de recurso. En el elemento copy, especifique el número de iteraciones y un nombre para este bucle.

El recurso para crear varias veces tiene el formato siguiente:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-01-01",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "copy": {
        "name": "storagecopy",
        "count": 3
      },
      "properties": {}
    }
  ],
  "outputs": {}
}
```

Tenga en cuenta que el nombre de cada recurso incluye la función `copyIndex()`, que devuelve la iteración actual del bucle. `copyIndex()` es de base cero. Así, en el ejemplo siguiente:

```json
"name": "[concat('storage', copyIndex())]",
```

Crea estos nombres:

* storage0
* storage1
* storage2.

Para desplazar el valor de índice, puede pasar un valor de la función copyIndex(). El número de iteraciones que se especifica en el elemento copy, pero el valor de copyIndex se desplaza el valor especificado. Así, en el ejemplo siguiente:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Crea estos nombres:

* storage1
* storage2
* storage3

La operación de copia es útil al trabajar con matrices, ya que puede iterar a través de cada elemento de la matriz. Use la función `length` en la matriz para especificar el número de iteraciones, y `copyIndex` para recuperar el índice actual de la matriz. Así, en el ejemplo siguiente:

```json
"parameters": {
  "org": {
    "type": "array",
    "defaultValue": [
      "contoso",
      "fabrikam",
      "coho"
    ]
  }
},
"resources": [
  {
    "name": "[concat('storage', parameters('org')[copyIndex()])]",
    "copy": {
      "name": "storagecopy",
      "count": "[length(parameters('org'))]"
    },
    ...
  }
]
```

Crea estos nombres:

* storagecontoso
* storagefabrikam
* storagecoho

Resource Manager crea los recursos en paralelo de forma predeterminada. No se aplica ningún límite en el número de recursos implementados en paralelo, aparte del límite total de 800 recursos de la plantilla. No se garantiza el orden en el que se crean.

Sin embargo, es posible que quiera especificar que los recursos se implementen en secuencia. Por ejemplo, al actualizar un entorno de producción, puede que quiera escalonar las actualizaciones para que solo una cantidad determinada se actualice al mismo tiempo. Para implementar en serie varias instancias de un recurso, establezca `mode` en el valor **serial** y `batchSize` en el número de instancias que se implementarán a la vez. Con mode establecido en serial, Resource Manager crea una dependencia en las instancias anteriores del bucle, por lo que no se inicia ningún lote hasta que se completa el lote anterior.

Por ejemplo, para implementar en serie dos cuentas de almacenamiento a la vez, use lo siguiente:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-01-01",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "copy": {
        "name": "storagecopy",
        "count": 4,
        "mode": "serial",
        "batchSize": 2
      },
      "properties": {}
    }
  ],
  "outputs": {}
}
```

La propiedad mode también acepta **parallel**, que es el valor predeterminado.

Para obtener información acerca del uso de copy con plantillas anidadas, consulte [Uso de copy](linked-templates.md#using-copy).

## <a name="property-iteration"></a>Iteración de propiedades

Para crear varios valores para una propiedad de un recurso, agregue una matriz `copy` en el elemento properties. Esta matriz contiene objetos, y cada objeto tiene las siguientes propiedades:

* nombre: el nombre de la propiedad para la que se van a crear varios valores.
* recuento: el número de valores que se van a crear.
* entrada: un objeto que contiene los valores que se van a asignar a la propiedad

En el ejemplo siguiente se muestra cómo aplicar `copy` a la propiedad dataDisks en una máquina virtual:

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "name": "examplevm",
  "properties": {
    "storageProfile": {
      "copy": [{
        "name": "dataDisks",
        "count": 3,
        "input": {
          "lun": "[copyIndex('dataDisks')]",
          "createOption": "Empty",
          "diskSizeGB": "1023"
        }
      }],
      ...
```

Tenga en cuenta que, cuando se usa `copyIndex` dentro de una iteración de propiedad, debe proporcionar el nombre de la iteración. No tiene que proporcionar el nombre cuando se usa con la iteración de recursos.

> [!NOTE]
> La iteración de propiedades también admite un argumento de desplazamiento. El desplazamiento debe aparecer después del nombre de la iteración, como copyIndex ("dataDisks", 1).
>

Resource Manager expande la matriz `copy` durante la implementación. El nombre de la matriz se convierte en el nombre de la propiedad. Los valores de entrada se convierten en las propiedades del objeto. La plantilla implementada se convierte en:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
        {
          "lun": 0,
          "createOption": "Empty",
          "diskSizeGB": "1023"
        },
        {
          "lun": 1,
          "createOption": "Empty",
          "diskSizeGB": "1023"
        },
        {
          "lun": 2,
          "createOption": "Empty",
          "diskSizeGB": "1023"
        }
      ],
      ...
```

El elemento de copia es una matriz por lo que puede especificar más de una propiedad para el recurso. Agregue un objeto para cada propiedad que desee crear.

```json
{
  "name": "string",
  "type": "Microsoft.Network/loadBalancers",
  "apiVersion": "2017-10-01",
  "properties": {
    "copy": [
      {
        "name": "loadBalancingRules",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      },
      {
        "name": "probes",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      }
    ]
  }
}
```

Puede usar la iteración de recursos y propiedades conjuntamente. Haga referencia a la iteración de la propiedad por el nombre.

```json
{
  "type": "Microsoft.Network/virtualNetworks",
  "apiVersion": "2018-04-01",
  "name": "[concat(parameters('vnetname'), copyIndex())]",
  "copy":{
    "count": 2,
    "name": "vnetloop"
  },
  "location": "[resourceGroup().location]",
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "[parameters('addressPrefix')]"
      ]
    },
    "copy": [
      {
        "name": "subnets",
        "count": 2,
        "input": {
          "name": "[concat('subnet-', copyIndex('subnets'))]",
          "properties": {
            "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
          }
        }
      }
    ]
  }
}
```

## <a name="variable-iteration"></a>Iteración variable

Para crear varias instancias de una variable, use la propiedad `copy` en la sección de variables. Tiene que crear una matriz de elementos construida a partir del valor de la propiedad `input`. Puede utilizar una propiedad `copy` que esté dentro de una variable o en el nivel superior de la sección de variables. Si usa `copyIndex` dentro de una iteración de variables, debe proporcionar el nombre de la iteración.

> [!NOTE]
> La iteración de variables también admite un argumento de desplazamiento. El desplazamiento debe aparecer después del nombre de la iteración, como copyIndex ("diskNames", 1).
>

Para obtener un ejemplo sencillo de creación de una matriz de valores de cadena, consulte [Plantilla de copy array](https://github.com/bmoore-msft/AzureRM-Samples/blob/master/copy-array/azuredeploy.json).

En el ejemplo siguiente se muestran varias formas de crear las variables de matriz con elementos creados dinámicamente. Muestra cómo usar la copia dentro de una variable para crear matrices de objetos y cadenas. También muestra cómo usar la copia en el nivel superior para crear matrices de objetos, cadenas y enteros.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "disk-array-on-object": {
      "copy": [
        {
          "name": "disks",
          "count": 5,
          "input": {
            "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
            "diskSizeGB": "1",
            "diskIndex": "[copyIndex('disks')]"
          }
        },
        {
          "name": "diskNames",
          "count": 5,
          "input": "[concat('myDataDisk', copyIndex('diskNames', 1))]"
        }
      ]
    },
    "copy": [
      {
        "name": "top-level-object-array",
        "count": 5,
        "input": {
          "name": "[concat('myDataDisk', copyIndex('top-level-object-array', 1))]",
          "diskSizeGB": "1",
          "diskIndex": "[copyIndex('top-level-object-array')]"
        }
      },
      {
        "name": "top-level-string-array",
        "count": 5,
        "input": "[concat('myDataDisk', copyIndex('top-level-string-array', 1))]"
      },
      {
        "name": "top-level-integer-array",
        "count": 5,
        "input": "[copyIndex('top-level-integer-array')]"
      }
    ]
  },
  "resources": [],
  "outputs": {
    "exampleObject": {
      "value": "[variables('disk-array-on-object')]",
      "type": "object"
    },
    "exampleArrayOnObject": {
      "value": "[variables('disk-array-on-object').disks]",
      "type" : "array"
    },
    "exampleObjectArray": {
      "value": "[variables('top-level-object-array')]",
      "type" : "array"
    },
    "exampleStringArray": {
      "value": "[variables('top-level-string-array')]",
      "type" : "array"
    },
    "exampleIntegerArray": {
      "value": "[variables('top-level-integer-array')]",
      "type" : "array"
    }
  }
}
```

El tipo de variable que se crea depende el objeto de entrada. Por ejemplo, la variable denominada **top-level-object-array** del ejemplo anterior devuelve:

```json
[
  {
    "name": "myDataDisk1",
    "diskSizeGB": "1",
    "diskIndex": 0
  },
  {
    "name": "myDataDisk2",
    "diskSizeGB": "1",
    "diskIndex": 1
  },
  {
    "name": "myDataDisk3",
    "diskSizeGB": "1",
    "diskIndex": 2
  },
  {
    "name": "myDataDisk4",
    "diskSizeGB": "1",
    "diskIndex": 3
  },
  {
    "name": "myDataDisk5",
    "diskSizeGB": "1",
    "diskIndex": 4
  }
]
```

Y la variable denominada **top-level-string-array** devuelve:

```json
[
  "myDataDisk1",
  "myDataDisk2",
  "myDataDisk3",
  "myDataDisk4",
  "myDataDisk5"
]
```

## <a name="depend-on-resources-in-a-loop"></a>Dependencia de los recursos de un bucle

Especifique que un recurso se implemente después de otro recurso mediante el elemento `dependsOn`. Para implementar un recurso que dependa de la colección de recursos de un bucle, proporcione el nombre del bucle copy en el elemento dependsOn. En el ejemplo siguiente se muestra cómo implementar tres cuentas de almacenamiento antes de implementar la máquina virtual. La definición de la máquina virtual no se muestra. Tenga en cuenta que el elemento copy tiene el nombre establecido en `storagecopy` y el elemento dependsOn para las máquinas virtuales también se establece en `storagecopy`.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-01-01",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "copy": {
        "name": "storagecopy",
        "count": 3
      },
      "properties": {}
    },
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2015-06-15",
      "name": "[concat('VM', uniqueString(resourceGroup().id))]",
      "dependsOn": ["storagecopy"],
      ...
    }
  ],
  "outputs": {}
}
```

<a id="looping-on-a-nested-resource" />

## <a name="iteration-for-a-child-resource"></a>Iteración para un recurso secundario
No puede usar un bucle copy en un recurso secundario. Para crear varias instancias de un recurso que se define normalmente como anidado dentro de otro recurso, debe crear dicho recurso como uno de nivel superior. La relación con el recurso principal se define a través de las propiedades type y name.

Por ejemplo, supongamos que suele definir un conjunto de datos como un recurso secundario dentro de una factoría de datos.

```json
"resources": [
{
  "type": "Microsoft.DataFactory/datafactories",
  "name": "exampleDataFactory",
  ...
  "resources": [
    {
      "type": "datasets",
      "name": "exampleDataSet",
      "dependsOn": [
        "exampleDataFactory"
      ],
      ...
    }
  ]
```

Para crear más de un conjunto de datos, muévalos fuera de la factoría de datos. El conjunto de datos debe estar en el mismo nivel que la factoría de datos, pero seguirá siendo un recurso secundario de la factoría de datos. La relación entre el conjunto de datos y la factoría de datos se conserva a través de los parámetros type y name. Puesto que la propiedad de type ya no se puede inferir de su posición en la plantilla, debe proporcionar el nombre completo del tipo con el formato: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

Para establecer una relación de principal-secundario con una instancia de la factoría de datos, proporcione un nombre para el conjunto de datos que incluya el nombre de recurso principal. Utilice el formato: `{parent-resource-name}/{child-resource-name}`.

En el siguiente ejemplo se muestra la implementación:

```json
"resources": [
{
  "type": "Microsoft.DataFactory/datafactories",
  "name": "exampleDataFactory",
  ...
},
{
  "type": "Microsoft.DataFactory/datafactories/datasets",
  "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
  "dependsOn": [
    "exampleDataFactory"
  ],
  "copy": {
    "name": "datasetcopy",
    "count": "3"
  },
  ...
}]
```

## <a name="example-templates"></a>Plantillas de ejemplo

En los ejemplos siguientes se muestran escenarios comunes para crear más de una instancia de un recurso o propiedad.

|Plantilla  |Descripción  |
|---------|---------|
|[Almacenamiento de copias](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystorage.json) |Implementa más de una cuenta de almacenamiento con un número de índice en el nombre. |
|[Almacenamiento de copias en serie](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/serialcopystorage.json) |Implementa varias cuentas de almacenamiento, una tras otra. El nombre incluye el número de índice. |
|[Almacenamiento de copias con una matriz](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystoragewitharray.json) |Implementa varias cuentas de almacenamiento. El nombre incluye el valor de una matriz. |
|[Implementación de máquinas virtuales con un número variable de discos de datos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Implementa varios discos de datos con una máquina virtual. |
|[Variables de copia](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) |Muestra las diferentes maneras de efectuar una iteración en las variables. |
|[Varias reglas de seguridad](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) |Implementa varias reglas de seguridad en un grupo de seguridad de red. Crea las reglas de seguridad a partir de un parámetro. Para el parámetro, consulte el [archivo de parámetros de varios grupos de seguridad de red](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json). |

## <a name="next-steps"></a>Pasos siguientes

* Para seguir los pasos de un tutorial, consulte [Tutorial: create multiple resource instances using Resource Manager templates](template-tutorial-create-multiple-instances.md) (Tutorial: Creación de varias instancias de recursos con plantillas de Resource Manager).

* Para obtener información sobre las secciones de una plantilla, consulte el artículo sobre cómo [crear plantillas de Azure Resource Manager](template-syntax.md).
* Para obtener información sobre cómo implementar la plantilla, consulte [Implementación de una aplicación con la plantilla del Administrador de recursos de Azure](deploy-powershell.md).

