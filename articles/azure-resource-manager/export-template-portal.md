---
title: Exportación de la plantilla de Azure Resource Manager mediante Azure Portal
description: Use Azure Portal para exportar una plantilla de Azure Resource Manager desde los recursos de la suscripción.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: tomfitz
ms.openlocfilehash: e482bf99013b9bec9dfbf64c4e8ad5a8a43ff540
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296306"
---
# <a name="single-and-multi-resource-export-to-template-in-azure-portal"></a>Exportación de uno y varios recursos a la plantilla en Azure Portal

Para ayudar con la creación de plantillas de Azure Resource Manager, puede exportar una plantilla desde los recursos existentes. La plantilla exportada le ayudará a comprender la sintaxis y las propiedades JSON que implementan los recursos. Para automatizar las implementaciones futuras, comience con la plantilla exportada y modifíquela para su escenario.

Resource Manager permite elegir uno o más recursos para exportarlos a una plantilla. Se puede centrar en los recursos exactos que necesita en la plantilla.

En este artículo se muestra cómo exportar plantillas mediante el portal. También puede usar la [CLI de Azure](manage-resource-groups-cli.md#export-resource-groups-to-templates), [Azure PowerShell](manage-resource-groups-powershell.md#export-resource-groups-to-templates) o la [API REST](/rest/api/resources/resourcegroups/exporttemplate).

## <a name="choose-the-right-export-option"></a>Elección de la opción de exportación correcta

Hay dos maneras de exportar una plantilla:

* **Exportar desde el grupo de recursos o el recurso**. Esta opción genera una plantilla nueva a partir de los recursos existentes. La plantilla exportada es una "instantánea" del estado actual del grupo de recursos. Puede exportar un grupo de recursos completo o recursos específicos dentro de ese grupo de recursos.

* **Exportar antes de la implementación o desde el historial**. Esta opción recupera una copia exacta de una plantilla usada para la implementación.

Según la opción que elija, las plantillas exportadas tendrán distintas calidades.

| Desde el grupo de recursos o el recurso | Antes de la implementación o desde el historial |
| --------------------- | ----------------- |
| La plantilla es una instantánea del estado actual de los recursos. Incluye todos los cambios manuales que se hayan realizado después de la implementación. | En la plantilla solo se muestra el estado de los recursos en el momento de la implementación. No se incluyen los cambios manuales que haya realizado después de la implementación. |
| Puede seleccionar los recursos de un grupo de recursos que se van a exportar. | Se incluyen todos los recursos para una implementación específica. No se puede seleccionar un subconjunto de esos recursos o agregar recursos que se hayan agregado en otro momento. |
| En la plantilla se incluyen todas las propiedades de los recursos, incluidas algunas que normalmente no se establecerían durante la implementación. Es posible que quiera quitar o borrar estas propiedades antes de volver a usar la plantilla. | En la plantilla solo se incluyen las propiedades necesarias para la implementación. La plantilla está lista para usarse. |
| Probablemente en la plantilla no se incluyan todos los parámetros que necesita para su reutilización. La mayoría de los valores de propiedad están codificados de forma rígida en la plantilla. Para volver a implementar la plantilla en otros entornos, tendrá que agregar parámetros que aumenten la capacidad de configurar los recursos. | En la plantilla se incluyen parámetros que facilitan la tarea de volver a implementar en otros entornos. |

Exporte la plantilla desde un grupo de recursos o un recurso cuando:

* Tenga que capturar cambios de los recursos que se han realizado después de la implementación original.
* Quiera seleccionar los recursos que se van a exportar.

Exporte la plantilla antes de la implementación o desde el historial cuando:

* Quiera una plantilla fácil de reutilizar.
* No necesite incluir los cambios realizados después de la implementación original.

## <a name="export-template-from-resource-group"></a>Exportación de la plantilla desde el grupo de recursos

Para exportar uno o varios recursos de un grupo de recursos:

1. Seleccione el grupo de recursos que contiene los recursos que quiere exportar.

1. Para exportar todos los recursos del grupo de recursos, seleccione Todos y después **Exportar plantilla**. La opción **Exportar plantilla** solo se habilita después de seleccionar al menos un recurso.

   ![Exportar todos los recursos](./media/export-template-portal/select-all-resources.png)

1. Para elegir recursos específicos para la exportación, active las casillas situadas junto a esos recursos. Después, seleccione **Exportar plantilla**.

   ![Seleccionar recursos para exportar](./media/export-template-portal/select-resources.png)

1. Se mostrará la plantilla exportada, que estará disponible para descargar.

   ![Mostrar la plantilla](./media/export-template-portal/show-template.png)

## <a name="export-template-from-resource"></a>Exportación de la plantilla desde el recurso

Para exportar un recurso:

1. Seleccione el grupo de recursos que contiene el recurso que quiere exportar.

1. Seleccione el recurso que se va a exportar.

   ![Seleccionar recurso](./media/export-template-portal/select-link-resource.png)

1. Para ese recurso, seleccione **Exportar plantilla** en el panel de la izquierda.

   ![Exportación del recurso](./media/export-template-portal/export-single-resource.png)

1. Se mostrará la plantilla exportada, que estará disponible para descargar. La plantilla solo contiene ese único recurso.

## <a name="export-template-before-deployment"></a>Exportación de la plantilla antes de la implementación

1. Seleccione el servicio de Azure que quiera implementar.

1. Rellene los valores para el nuevo servicio.

1. Después de pasar la validación, pero antes de iniciar la implementación, seleccione **Descargar una plantilla para la automatización**.

   ![Descarga de una plantilla](./media/export-template-portal/download-before-deployment.png)

1. Se mostrará la plantilla y estará disponible para descargarla.

   ![Mostrar la plantilla](./media/export-template-portal/show-template-before-deployment.png)

## <a name="export-template-after-deployment"></a>Exportación de la plantilla después de la implementación

Puede exportar la plantilla que se ha usado para implementar los recursos existentes. La plantilla que se obtiene es exactamente la que se ha usado para la implementación.

1. Seleccione el grupo de recursos que quiera exportar.

1. Seleccione el vínculo situado bajo **Implementaciones**.

   ![Selección del historial de implementación](./media/export-template-portal/select-deployment-history.png)

1. Seleccione una de las implementaciones del historial de implementación.

   ![Selección de la implementación](./media/export-template-portal/select-details.png)

1. Seleccione **Plantilla**. Se mostrará la plantilla que se ha usado para esta implementación y estará disponible para descargarla.

   ![Seleccionar plantilla](./media/export-template-portal/show-template-from-history.png)

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a exportar plantillas con la [CLI de Azure](manage-resource-groups-cli.md#export-resource-groups-to-templates), [Azure PowerShell](manage-resource-groups-powershell.md#export-resource-groups-to-templates) o la [API REST](/rest/api/resources/resourcegroups/exporttemplate).
- Para obtener información sobre la sintaxis de las plantillas de Resource Manager, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](./resource-group-authoring-templates.md).
- Para obtener información sobre cómo desarrollar plantillas, consulte los [tutoriales paso a paso](/azure/azure-resource-manager/).
- Para ver los esquemas de plantilla de Azure Resource Manager, vea la [referencia de plantilla](/azure/templates/).