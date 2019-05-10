---
title: Exportar plantilla de Azure Resource Manager mediante el portal de Azure
description: Usar el portal de Azure para exportar una plantilla de Azure Resource Manager desde recursos de la suscripción.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: tomfitz
ms.openlocfilehash: ea9499da3dac67635a48704f439f6592c6ed467e
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65515390"
---
# <a name="single-and-multi-resource-export-to-template-in-azure-portal"></a>Exportación única y varios recurso a la plantilla en Azure portal

Para ayudar con la creación de plantillas de Azure Resource Manager, puede exportar una plantilla desde recursos existentes. La plantilla exportada le ayudará a comprender la sintaxis JSON y las propiedades que implementan los recursos. Para automatizar las implementaciones futuras, comience con la plantilla exportada y modificarlo para su escenario.

Resource Manager permite elegir uno o más recursos para exportar a una plantilla. Se puede Centrar en exactamente los recursos que necesita en la plantilla.

## <a name="choose-the-right-export-option"></a>Elija la opción de exportación correcta

Hay dos maneras de exportar una plantilla:

* **Exportar desde el grupo de recursos o recurso**. Esta opción genera una nueva plantilla desde recursos existentes. La plantilla exportada es una "instantánea" del estado actual del grupo de recursos. Puede exportar un grupo de recursos completo o recursos específicos dentro de ese grupo de recursos.

* **Exportar antes de la implementación o de historial**. Esta opción recupera una copia exacta de una plantilla usada para la implementación.

Según la opción que elija, las plantillas exportadas tienen distintas calidades.

| Desde el grupo de recursos o recurso | Antes de la implementación o de historial |
| --------------------- | ----------------- |
| Plantilla es la instantánea del estado actual de los recursos. Incluye los cambios manuales realizados después de la implementación. | Plantilla solo muestra el estado de los recursos en el momento de la implementación. Cualquier cambio realizado manualmente después de que no se incluyen la implementación. |
| Puede seleccionar los recursos de un grupo de recursos para exportar. | Se incluyen todos los recursos para una implementación específica. No se puede seleccionar un subconjunto de esos recursos o agregar recursos que se agregaron en otro momento. |
| Plantilla incluye todas las propiedades de los recursos, incluidas algunas propiedades que normalmente no sería establecer durante la implementación. Es posible que desee quitar o borrar estas propiedades antes de volver a usar la plantilla. | Plantilla incluye solo las propiedades necesarias para la implementación. La plantilla está listos para usar. |
| Plantilla probablemente no incluye todos los parámetros que necesita para su reutilización. La mayoría de los valores de propiedad están codificados en la plantilla. Para volver a implementar la plantilla en otros entornos, deberá agregar los parámetros que aumentan la capacidad de configurar los recursos. | Plantilla incluye los parámetros que facilitan la tarea volver a implementar en entornos diferentes. |

Exportar la plantilla de un grupo de recursos o un recurso, cuando:

* Deberá capturar los cambios a los recursos que se realizaron después de la implementación original.
* Desea seleccionar los recursos que se exportan.

Exportar la plantilla antes de la implementación o desde el historial, cuando:

* Desea una plantilla de reutilización fácil.
* No es necesario incluir los cambios realizados después de la implementación original.

## <a name="export-template-from-resource-group"></a>Exportación de la plantilla desde el grupo de recursos

Para exportar uno o varios recursos de un grupo de recursos:

1. Seleccione el grupo de recursos que contiene los recursos que desea exportar.

1. Para exportar todos los recursos del grupo de recursos, seleccione todos y, a continuación, **Exportar plantilla**. El **Exportar plantilla** opción solo se habilita después de seleccionar al menos un recurso.

   ![Exportar todos los recursos](./media/export-template-portal/select-all-resources.png)

1. Para elegir recursos específicos para la exportación, seleccione las casillas de verificación junto a esos recursos. A continuación, seleccione **Exportar plantilla**.

   ![Seleccionar recursos para exportar](./media/export-template-portal/select-resources.png)

1. La plantilla exportada se muestra y está disponible para descargar.

   ![Mostrar plantilla](./media/export-template-portal/show-template.png)

## <a name="export-template-from-resource"></a>Exportar plantilla de recursos

Para exportar un recurso:

1. Seleccione el grupo de recursos que contiene el recurso que desea exportar.

1. Seleccione el recurso para exportar.

   ![Seleccionar recurso](./media/export-template-portal/select-link-resource.png)

1. Para ese recurso, seleccione **Exportar plantilla** en el panel izquierdo.

   ![Exportar recursos](./media/export-template-portal/export-single-resource.png)

1. La plantilla exportada se muestra y está disponible para descargar. La plantilla solo contiene el único recurso.

## <a name="export-template-before-deployment"></a>Exportar plantilla antes de la implementación

1. Seleccione el servicio de Azure que desea implementar.

1. Rellene los valores para el nuevo servicio.

1. Después de pasar la validación, pero antes de iniciar la implementación, seleccione **descargar una plantilla para la automatización**.

   ![Descargar plantilla](./media/export-template-portal/download-before-deployment.png)

1. La plantilla se muestra y está disponible para su descarga.

   ![Mostrar plantilla](./media/export-template-portal/show-template-before-deployment.png)

## <a name="export-template-after-deployment"></a>Exportar plantilla después de la implementación

Puede exportar la plantilla que se usó para implementar los recursos existentes. La plantilla que se obtiene es exactamente lo que se usó para la implementación.

1. Seleccione el grupo de recursos que desea exportar.

1. Seleccione el vínculo situado bajo **implementaciones**.

   ![Seleccione el historial de implementación](./media/export-template-portal/select-deployment-history.png)

1. Seleccione una de las implementaciones del historial de implementación.

   ![Seleccione la implementación](./media/export-template-portal/select-details.png)

1. Seleccione **plantilla**. La plantilla usada para esta implementación se muestra y está disponible para su descarga.

   ![Seleccionar plantilla](./media/export-template-portal/show-template-from-history.png)

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre Azure Resource Manager, consulte [Introducción a Azure Resource Manager](./resource-group-overview.md).
- Para obtener información sobre la sintaxis de la plantilla de Resource Manager, consulte [entender la estructura y sintaxis de plantillas de Azure Resource Manager](./resource-group-authoring-templates.md).
- Para obtener información sobre cómo desarrollar las plantillas, consulte el [tutoriales paso a paso](/azure/azure-resource-manager/).
- Para ver los esquemas de plantilla de Azure Resource Manager, consulte [referencia de plantilla](/azure/templates/).