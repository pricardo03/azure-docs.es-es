---
title: Recursos de la plantilla de Azure Resource Manager | Microsoft Docs
description: Describe la sección de recursos de plantillas de Azure Resource Manager mediante la sintaxis declarativa de JSON.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2019
ms.author: tomfitz
ms.openlocfilehash: 01aacf8815ce4150eb1c243d4337f52c4e0b03e9
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2019
ms.locfileid: "55697074"
---
# <a name="resources-section-of-azure-resource-manager-templates"></a>Sección de recursos de plantillas de Azure Resource Manager

En la sección de recursos, se define que los recursos se implementan o se actualizan. Esta sección se puede complicar porque debe comprender los tipos que va a implementar para proporcionar los valores adecuados.

## <a name="available-properties"></a>Propiedades disponibles

Defina recursos con la siguiente estructura:

```json
"resources": [
  {
      "condition": "<true-to-deploy-this-resource>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": <number-of-iterations>,
          "mode": "<serial-or-parallel>",
          "batchSize": <number-to-deploy-serially>
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "sku": {
          "name": "<sku-name>",
          "tier": "<sku-tier>",
          "size": "<sku-size>",
          "family": "<sku-family>",
          "capacity": <sku-capacity>
      },
      "kind": "<type-of-resource>",
      "plan": {
          "name": "<plan-name>",
          "promotionCode": "<plan-promotion-code>",
          "publisher": "<plan-publisher>",
          "product": "<plan-product>",
          "version": "<plan-version>"
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Nombre del elemento | Obligatorio | DESCRIPCIÓN |
|:--- |:--- |:--- |
| condition | Sin  | Valor booleano que indica si el recurso se aprovisionará durante esta implementación. Si es `true`, el recurso se crea durante la implementación. Si es `false`, el recurso se omite para esta implementación. |
| apiVersion |Sí |Versión de la API de REST que debe usar para crear el recurso. |
| Tipo |Sí |Tipo de recurso. Este valor es una combinación del espacio de nombres del proveedor de recursos y el tipo de recurso (como **Microsoft.Storage/storageAccounts**). |
| Nombre |Sí |Nombre del recurso. El nombre debe cumplir las restricciones de componente URI definidas en RFC3986. Además, los servicios de Azure que exponen el nombre del recurso a partes externas validan el nombre para asegurarse de que no es un intento de suplantar otra identidad. |
| location |Varía |Ubicaciones geográficas compatibles del recurso proporcionado. Puede seleccionar cualquiera de las ubicaciones disponibles, pero normalmente tiene sentido elegir aquella que esté más cerca de los usuarios. Normalmente, también tiene sentido colocar los recursos que interactúan entre sí en la misma región. La mayoría de los tipos de recursos requieren una ubicación, pero algunos (por ejemplo, una asignación de roles) no la necesitan. |
| etiquetas |Sin  |Etiquetas asociadas al recurso. Aplique etiquetas para organizar de forma lógica los recursos en su suscripción. |
| comentarios |Sin  |Notas para documentar los recursos de la plantilla. Para más información, consulte [Comentarios en plantillas](resource-group-authoring-templates.md#comments). |
| copia |Sin  |Si se necesita más de una instancia, el número de recursos que se crearán. El modo predeterminado es paralelo. Si no desea que todos los recursos se implementen al mismo tiempo, especifique el modo serie. Para obtener más información, consulte [Creación de varias instancias de recursos en Azure Resource Manager](resource-group-create-multiple.md). |
| dependsOn |Sin  |Recursos que se deben implementar antes de implementar este. Resource Manager evalúa las dependencias entre recursos y los implementa en su orden correcto. Cuando no hay recursos dependientes entre sí, se implementan en paralelo. El valor puede ser una lista separada por comas de nombres de recursos o identificadores de recursos únicos. Solo los recursos de lista que se implementan en esta plantilla. Deben existir los recursos que no estén definidos en esta plantilla. Evite agregar dependencias innecesarias, ya que pueden ralentizar la implementación y crear dependencias circulares. Para obtener instrucciones sobre la configuración de dependencias, consulte [Definición de dependencias en plantillas de Azure Resource Manager](resource-group-define-dependencies.md). |
| propiedades |Sin  |Opciones de configuración específicas de recursos. Los valores de las propiedades son exactamente los mismos valores que se especifican en el cuerpo de la solicitud de la operación de API de REST (método PUT) para crear el recurso. También puede especificar una matriz de copia para crear varias instancias de una propiedad. |
| sku | Sin  | Algunos recursos permiten valores que definen la SKU que se va a implementar. Por ejemplo, puede especificar el tipo de redundancia para una cuenta de almacenamiento. |
| kind | Sin  | Algunos recursos permiten un valor que define el tipo de recurso que va a implementar. Por ejemplo, puede especificar el tipo de instancia de Cosmos DB que va a crear. |
| plan | Sin  | Algunos recursos permiten valores que definen el plan que se va a implementar. Por ejemplo, puede especificar la imagen de Marketplace para una máquina virtual. | 
| resources |Sin  |Recursos secundarios que dependen del recurso que se está definiendo. Proporcione solo tipos de recursos que permita el esquema del recurso principal. El tipo completo del recurso secundario incluye el tipo del recurso principal, por ejemplo, **Microsoft.Web/sites/extensions**. La dependencia del recurso principal no está implícita. Debe definirla explícitamente. |

## <a name="condition"></a>Condición

Si durante la implementación debe decidir si crear o no un recurso, use el elemento `condition`. El valor de este elemento se resuelve como true o false. Cuando el valor es true, el recurso se crea. Cuando el valor es false, el recurso no se crea. El valor solo se puede aplicar a todo el recurso.

Por lo general, este valor se usa cuando desea crear un nuevo recurso o usar uno existente. Por ejemplo, para especificar si se implementa una nueva cuenta de almacenamiento o se usa una cuenta de almacenamiento existente, use:

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

Para una plantilla de ejemplo completo que usa el elemento `condition`, consulte [VM with a new or existing Virtual Network, Storage, and Public IP](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions) (Máquina virtual con una red virtual nueva o existente, almacenamiento y dirección IP pública).

## <a name="resource-specific-values"></a>Valores específicos de los recursos

Los elementos **apiVersion**, **type** y **properties** son diferentes para cada tipo de recurso. Los elementos **sku**, **kind** y **plan** están disponibles para algunos tipos de recursos, pero no para todos. Para determinar los valores de estas propiedades, consulte la [referencia de plantillas](/azure/templates/).

## <a name="resource-names"></a>Nombres de recurso

Por lo general, trabaja con tres tipos de nombres de recursos en Resource Manager:

* Nombres de recurso que deben ser únicos.
* Nombres de recurso que no tienen que ser únicos, pero decide proporcionarles un nombre que pueda ayudarle a identificarlos.
* Nombres de recurso que pueden ser genéricos.

### <a name="unique-resource-names"></a>Nombres de recurso únicos

Asigne un nombre de recurso único para cualquier tipo de recurso que tenga un punto de conexión de acceso a datos. Entre los tipos de recursos comunes que requieren un nombre único se incluyen los siguientes:

* Azure Storage<sup>1</sup> 
* Característica Web Apps de Azure App Service
* SQL Server
* Azure Key Vault
* Azure Cache for Redis
* Azure Batch
* Administrador de tráfico de Azure
* Azure Search
* HDInsight de Azure

<sup>1</sup> Los nombres de las cuentas de almacenamiento deben estar en minúsculas, tener 24 caracteres o menos y no incluir guiones.

Al establecer el nombre, puede crear un nombre único de forma manual o usar la función [uniqueString()](resource-group-template-functions-string.md#uniquestring) para generarlo. También puede que quiera agregar un prefijo o sufijo al resultado **uniqueString**. Modificar el nombre único podría ayudarlo a identificar más fácilmente el tipo de recurso a partir del nombre. Por ejemplo, puede generar un nombre único para una cuenta de almacenamiento si usa la variable siguiente:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Nombres de recurso para la identificación
Algunos tipos de recursos a los que podría querer asignar un nombre, pero no es necesario que los nombres sean únicos. En el caso de estos tipos de recursos, puede proporcionar un nombre que identifique tanto el contexto del recurso como el tipo de recurso.

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "The name of the VM to create."
        }
    }
}
```

### <a name="generic-resource-names"></a>Nombres de recurso genéricos
En los tipos de recurso a los que tiene acceso mayoritariamente por medio de otro recurso, puede usar un nombre genérico que esté codificado de forma rígida en la plantilla. Por ejemplo, puede establecer un nombre genérico estándar para reglas de firewall en un servidor SQL:

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="location"></a>Ubicación
Al implementar una plantilla, debe proporcionar una ubicación para cada recurso. Se admiten diferentes tipos de recursos en diferentes ubicaciones. Para obtener las ubicaciones admitidas para un tipo de recurso, consulte [Tipos de recursos y proveedores de recursos de Azure](resource-manager-supported-services.md).

Use un parámetro para especificar la ubicación de recursos y establecer el valor predeterminado en `resourceGroup().location`.

En el ejemplo siguiente se muestra una cuenta de almacenamiento que está implementada en una ubicación especificada como parámetro:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
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
  "variables": {
    "storageAccountName": "[concat('store', uniquestring(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "apiVersion": "2018-07-01",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "properties": {}
    }
  ],
  "outputs": {
    "storageAccountName": {
      "type": "string",
      "value": "[variables('storageAccountName')]"
    }
  }
}
```

Si tiene que codificar la ubicación en la plantilla, indique el nombre de una de las regiones admitidas. En el ejemplo siguiente se muestra una cuenta de almacenamiento que está siempre implementada en Centro-Norte de EE. UU:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "North Central US",
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

## <a name="tags"></a>Etiquetas
[!INCLUDE [resource-manager-governance-tags](../../includes/resource-manager-governance-tags.md)]

### <a name="add-tags-to-your-template"></a>Agregar etiquetas a la plantilla

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="child-resources"></a>Recursos secundarios

En cada tipo recurso puede definir también una matriz de recursos secundarios. Los recursos secundarios son recursos que solo existen en el contexto de otro recurso. Por ejemplo, una base de datos SQL no puede existir sin un servidor SQL, por lo que se trata de un elemento secundario del servidor. Puede definir la base de datos dentro de la definición del servidor.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

Cuando está anidado, el tipo se establece en `databases`, pero el tipo de recurso completo es `Microsoft.Sql/servers/databases`. No se proporciona `Microsoft.Sql/servers/` porque se supone que viene del tipo de recurso primario. El nombre del recurso secundario se establece en `exampledatabase`, pero el nombre completo incluye el primario. No se proporciona `exampleserver` porque se supone que viene del recurso primario.

El formato del tipo de recurso secundario es: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

El formato del nombre de recurso secundario es: `{parent-resource-name}/{child-resource-name}`

Sin embargo, no es necesario definir la base de datos en el servidor. Puede definir el recurso secundario en el nivel superior. Este enfoque puede usarse si el recurso primario no está implementado en la misma plantilla o si quiere usar `copy` para crear más de un recurso secundario. En este enfoque, es necesario proporcionar el tipo de recurso completo, así como incluir el nombre del recurso primario en el del secundario.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

Al construir una referencia completa a un recurso, el orden para combinar los segmentos a partir del tipo y el nombre no es simplemente una concatenación de los dos. En su lugar, después del espacio de nombres, use una secuencia de pares *tipo/nombre* de menos a más específico:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Por ejemplo: 

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` es correcto `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` no es correcto



## <a name="next-steps"></a>Pasos siguientes
* Para ver plantillas completas de muchos tipos diferentes de soluciones, consulte [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/documentation/templates/).
* Para obtener información detallada sobre las funciones que se pueden usar dentro de una plantilla, consulte [Funciones de plantilla de Azure Resource Manager](resource-group-template-functions.md).
* Para más recomendaciones sobre creación de platillas, consulte [Azure Resource Manager template best practices](template-best-practices.md) (Procedimientos recomendados para plantillas de Azure Resource Manager).
* Puede que necesite usar los recursos que existen dentro de un grupo de recursos diferente. Este escenario es habitual al trabajar con cuentas de almacenamiento o redes virtuales que se comparten entre varios grupos de recursos. Para obtener más información, vea la [función resourceId](resource-group-template-functions-resource.md#resourceid).
* Para obtener información sobre las restricciones de los nombres de recurso, consulte [Recommended naming conventions for Azure resources](../guidance/guidance-naming-conventions.md)(Convenciones de nomenclatura recomendadas para los recursos de Azure).