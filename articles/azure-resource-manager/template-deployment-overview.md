---
title: Plantillas del Administrador de recursos de Azure
description: Describe cómo usar las plantillas de Azure Resource Manager para la implementación de recursos.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/07/2019
ms.author: tomfitz
ms.openlocfilehash: 61e9bbabee969280c07521edb05d67ba68c0c58e
ms.sourcegitcommit: b7b0d9f25418b78e1ae562c525e7d7412fcc7ba0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/08/2019
ms.locfileid: "70801982"
---
# <a name="azure-resource-manager-templates"></a>Plantillas del Administrador de recursos de Azure

Con el traslado a la nube, muchos equipos han adoptado métodos de desarrollo más ágiles. Estos equipos se iteran rápidamente. Necesitan implementar repetidamente sus soluciones en la nube y saber que su infraestructura se encuentra en un estado confiable. Dado que la infraestructura se ha convertido en parte del proceso iterativo, ha desaparecido la división entre las operaciones y el desarrollo. Por ello, los equipos necesitan administrar la infraestructura y el código de aplicación a través de un proceso unificado.

Para cumplir estos desafíos, puede automatizar las implementaciones y usar la práctica de infraestructura como código. En el código, debe definir la infraestructura que va a implementar. Así pues, el código de infraestructura se convierte en parte del proyecto. Al igual que el código de la aplicación, puede almacenar el código de infraestructura en un repositorio de origen y agregarle un número de versión. Cualquier miembro del equipo podrá ejecutar el código e implementar entornos similares.

Para implementar la infraestructura como código para las soluciones de Azure, use las plantillas de Azure Resource Manager. La plantilla es un archivo de notación de objetos JavaScript (JSON) que contiene la infraestructura y la configuración del proyecto. La plantilla usa sintaxis declarativa, lo que permite establecer lo que pretende implementar sin tener que escribir la secuencia de comandos de programación para crearla. En la plantilla se especifican los recursos que se van a implementar y las propiedades de esos recursos.

## <a name="benefits-of-resource-manager-templates"></a>Ventajas de las plantillas de Resource Manager

Las plantillas de Resource Manager también proporcionan las siguientes ventajas:

* Implementar, administrar y supervisar todos los recursos de la solución en grupo, en lugar de controlarlos individualmente.

* Puede implementar la solución repetidamente a lo largo del ciclo de vida del desarrollo y tener la seguridad de que los recursos se implementan de forma coherente.

* Administrar la infraestructura mediante plantillas declarativas en lugar de scripts.

Si no sabe si usar las plantillas de Resource Manager o una de las demás infraestructuras como servicios de código, tenga en cuenta las siguientes ventajas que ofrecen las plantillas frente a los servicios:

* Los nuevos servicios y características de Azure están disponibles de forma inmediata en las plantillas. En cuanto un proveedor de recursos introduce nuevos recursos, puede implementarlos a través de plantillas. Con otra infraestructura como servicio de código, debe esperar a que terceros implementen las interfaces para los nuevos recursos.

* Las implementaciones de plantilla se administran a través de un único envío de la plantilla, en lugar de hacerlo a través de varios comandos imperativos. Resource Manager se encarga de gestionar la implementación de recursos interdependientes para que se creen en el orden correcto. Asimismo, analiza la plantilla y determina el orden correcto para la implementación en función de las referencias entre los recursos.

   ![Comparación de la implementación de plantillas](./media/template-deployment-overview/template-processing.png)

* Se realiza un seguimiento de las implementaciones de plantillas en Azure Portal. Puede revisar el historial de implementación y obtener información sobre la implementación de la plantilla. También puede ver la plantilla que se implementó, los valores de parámetro agregados y los valores de salida. Recuerde que no se realiza el seguimiento de otras infraestructuras como servicios de código a través del portal.

   ![Historial de implementación](./media/template-deployment-overview/deployment-history.png)

* Las implementaciones de plantillas se someten a una validación previa al lanzamiento del paquete piloto. Resource Manager se encarga de comprobar la plantilla antes de iniciar la implementación para asegurarse de que esta se realizará correctamente. Es menos probable que la implementación se detenga a medio acabar.

* Si usa [directivas de Azure](../governance/policy/overview.md), la corrección de la directiva se realiza en recursos no compatibles cuando se implementa a través de plantillas.

* Microsoft proporciona los [planos técnicos](../governance/blueprints/overview.md) de la implementación para cumplir con los estándares reglamentarios. Estos planos técnicos incluyen plantillas precompiladas para distintas arquitecturas.

## <a name="idempotent"></a>Idempotencia

La idempotencia significa que puede ejecutar las mismas operaciones varias veces y obtener el mismo resultado. La implementación de una plantilla de Resource Manager es idempotente. Puede implementar la misma plantilla varias veces y obtener los mismos tipos de recursos en el mismo estado. Este concepto es importante porque significa que se obtienen resultados coherentes si se vuelve a implementar una plantilla en un grupo de recursos existente, o si se implementa una plantilla en un grupo de recursos nuevo.

Supongamos que ha implementado tres recursos en un grupo de recursos y, a continuación, decide que necesita agregar un cuarto recurso. En lugar de crear una nueva plantilla que contenga solo el nuevo recurso, puede agregar el cuarto recurso a la plantilla existente. Al implementar la nueva plantilla en el grupo de recursos que ya tenía tres recursos, Resource Manager decide qué acciones deben llevarse a cabo.

Si el recurso existe en el grupo de recursos y la solicitud no contiene ninguna actualización de las propiedades, no se realiza ninguna acción. Si el recurso existe pero las propiedades han cambiado, el recurso existente se actualiza. Si el recurso no existe, se crea el nuevo recurso.

Así pues, tendrá la certeza de que, cuando finalice la implementación, los recursos estarán siempre en el estado esperado.

## <a name="template-file"></a>Archivo de plantilla

Dentro de la plantilla, puede escribir [expresiones de plantilla](template-expressions.md) que aumentan las capacidades de JSON. Estas expresiones hacen uso de las [funciones](resource-group-template-functions.md) que proporciona Resource Manager.

La plantilla contiene las secciones siguientes:

* [Parámetros](template-parameters.md): proporcione valores durante la implementación que permitan usar la misma plantilla con entornos diferentes.

* [Variables](template-variables.md): defina los valores que se reutilizan en las plantillas. Se pueden crear a partir de valores de parámetro.

* [Funciones definidas por el usuario](template-user-defined-functions.md): cree funciones personalizadas que simplifiquen la plantilla.

* [Recursos](resource-group-authoring-templates.md#resources): especifique los recursos que se van a implementar.

* [Salidas](template-outputs.md): devuelva valores de los recursos implementados.

## <a name="template-features"></a>Características de plantilla

Resource Manager analiza las dependencias para asegurarse de que los recursos se crean en el orden correcto. La mayoría de las dependencias se determinan de forma implícita. Sin embargo, puede establecer explícitamente una dependencia para asegurarse de que se implementa un recurso antes que otro. Para obtener más información, consulte [Definición de dependencias en plantillas del Administrador de recursos de Azure](resource-group-define-dependencies.md).

Puede agregar un recurso a la plantilla y, opcionalmente, implementarlo. Normalmente, se pasa un valor de parámetro que indica si es necesario implementar el recurso. Para obtener más información, consulte [Implementación condicional con plantillas de Resource Manager](conditional-resource-deployment.md).

En lugar de repetir bloques de JSON varias veces en la plantilla, puede usar un elemento de copia para especificar más de una instancia de una variable, propiedad o recurso. Para obtener más información, consulte [Recurso, propiedad o iteración de variables en las plantillas de Azure Resource Manager](resource-group-create-multiple.md).

## <a name="export-templates"></a>Exportar plantillas

Puede recuperar una plantilla de un grupo de recursos existente mediante la exportación del estado actual del grupo de recursos o la visualización de la plantilla de una implementación determinada. Una buena estrategia para aprender sobre la sintaxis de una plantilla es consultar la [plantilla exportada](export-template-portal.md).

Cuando crea una solución desde el Portal, la solución incluye automáticamente una plantilla de implementación. No tiene que crear la plantilla desde cero; puede empezar con la plantilla para la solución y personalizarla para satisfacer sus necesidades específicas. Para obtener un ejemplo, consulte [Inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](./resource-manager-quickstart-create-templates-use-the-portal.md).

## <a name="template-deployment-process"></a>Proceso de implementación de plantillas

Cuando se implementa una plantilla, Resource Manager la convierte en operaciones de la API de REST. Por ejemplo, cuando Resource Manager recibe una plantilla con la siguiente definición de recursos:

```json
"resources": [
  {
    "apiVersion": "2016-01-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "mystorageaccount",
    "location": "westus",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {
    }
  }
]
```

Convierte la definición en la siguiente operación de API de REST, que se envía al proveedor de recursos Microsoft.Storage:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/mystorageaccount?api-version=2016-01-01
REQUEST BODY
{
  "location": "westus",
  "properties": {
  }
  "sku": {
    "name": "Standard_LRS"
  },
  "kind": "Storage"
}
```

## <a name="template-design"></a>Diseño de plantilla

La definición de plantillas y grupos de recursos depende únicamente de usted, al igual que la administración de la solución. Por ejemplo, puede implementar su aplicación de tres niveles a través de una única plantilla en un único grupo de recursos.

![plantilla de tres niveles](./media/template-deployment-overview/3-tier-template.png)

No obstante, no es necesario que defina toda la infraestructura en una sola plantilla. A menudo, tiene sentido dividir los requisitos de implementación en un conjunto de plantillas seleccionadas, específicas para un propósito. Estas plantillas se pueden reutilizar fácilmente para distintas soluciones. Para implementar una solución concreta, cree una plantilla maestra que vincule todas las plantillas necesarias. La imagen siguiente muestra cómo implementar una solución de tres niveles mediante una plantilla principal que incluye tres plantillas anidadas.

![plantilla de niveles anidada](./media/template-deployment-overview/nested-tiers-template.png)

Si desea que sus niveles tengan ciclos de vida independientes, puede implementar los tres niveles en grupos de recursos independientes. Observe que todavía se pueden vincular los recursos a los recursos de otros grupos.

![plantilla de niveles](./media/template-deployment-overview/tier-templates.png)

Para más información acerca de las plantillas anidadas, consulte [Uso de plantillas vinculadas con Azure Resource Manager](resource-group-linked-templates.md).

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre las propiedades en los archivos de plantilla, consulte [Información sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](resource-group-authoring-templates.md).
* Para comenzar a desarrollar una plantilla, consulte [Uso de Visual Studio Code para crear plantillas de Azure Resource Manager](resource-manager-tools-vs-code.md).
* Para ver una introducción al servicio Resource Manager, incluidas sus características de administración, consulte [Información general de Azure Resource Manager](resource-group-overview.md).

