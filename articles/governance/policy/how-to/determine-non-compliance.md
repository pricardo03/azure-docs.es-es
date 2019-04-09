---
title: Determinación de las causas de incumplimiento
description: Cuando un recurso no es compatible, hay muchas razones posibles. Aprenda a averiguar la causa el incumplimiento.
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/26/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 8a593e92d7f24885c35043b874528e881d2e021e
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59276106"
---
# <a name="determine-causes-of-non-compliance"></a>Determinación de las causas de incumplimiento

Cuando un recurso de Azure se determina como no conforme a una regla de directiva, es útil entender qué parte de la regla no es compatible con el recurso. También es útil para comprender qué cambio modifica un recurso compatible previamente para que sea no compatible. Hay dos maneras de buscar esta información:

> [!div class="checklist"]
> - [Detalles de cumplimiento](#compliance-details)
> - [Historial de cambios (versión preliminar)](#change-history-preview)

## <a name="compliance-details"></a>Detalles de cumplimiento

Cuando un recurso no es compatible, los detalles de cumplimiento para ese recurso estarán disponibles en el **cumplimiento de directivas** página. El panel de detalles de cumplimiento incluye la información siguiente:

- Detalles de recursos como el identificador de recurso, tipo, ubicación y nombre
- Estado de cumplimiento y la marca de tiempo de la última evaluación para la asignación de directiva actual
- Una lista de _motivos_ el recurso en caso de incumplimiento

> [!IMPORTANT]
> Como los detalles de cumplimiento para un _no compatible_ recurso muestra el valor actual de propiedades en ese recurso, el usuario debe tener **leer** operación a la **tipo** de recurso. Por ejemplo, si la _no compatible_ recurso es **Microsoft.COMPUTE/virtualmachines** , a continuación, el usuario debe tener el **Microsoft.Compute/virtualMachines/read** operación. Si el usuario no tiene la operación necesaria, se muestra un error de acceso.

Para ver los detalles de cumplimiento, siga estos pasos:

1. Inicie el servicio Azure Policy en Azure Portal. Para ello, haga clic en **Todos los servicios** y, a continuación, busque y seleccione **Directiva**.

1. En el **información general sobre** o **cumplimiento** , seleccione una directiva en un **estado de cumplimiento** decir _no compatible_.

1. En el **cumplimiento recursos** pestaña de la **cumplimiento de directivas** página, haga doble clic o seleccione los puntos suspensivos de un recurso en un **estado de cumplimiento** que es  _No conforme_. A continuación, seleccione **ver los detalles de cumplimiento**.

   ![Opción de vista de detalles de cumplimiento](../media/determine-non-compliance/view-compliance-details.png)

1. El **detalles de cumplimiento** panel muestra información de la evaluación más reciente del recurso para la asignación de directiva actual. En este ejemplo, el campo **Microsoft.Sql/servers/version** se encuentre _12.0_ mientras se esperaba la definición de directiva _14.0_. Si el recurso no es compatible por varios motivos, cada una se muestra en este panel.

   ![Panel de detalles de cumplimiento y las razones de incumplimiento](../media/determine-non-compliance/compliance-details-pane.png)

   Para un **auditIfNotExists** o **deployIfNotExists** definición de directiva, los detalles incluyen el **details.type** propiedad y las propiedades opcionales. Para obtener una lista, consulte [auditIfNotExists propiedades](../concepts/effects.md#auditifnotexists-properties) y [deployIfNotExists propiedades](../concepts/effects.md#deployifnotexists-properties). **Última evaluación recursos** es un recurso relacionado desde la **detalles** sección de la definición.

   Ejemplo parcial **deployIfNotExists** definición:

   ```json
   {
       "if": {
           "field": "type",
           "equals": "[parameters('resourceType')]"
       },
       "then": {
           "effect": "DeployIfNotExists",
           "details": {
               "type": "Microsoft.Insights/metricAlerts",
               "existenceCondition": {
                   "field": "name",
                   "equals": "[concat(parameters('alertNamePrefix'), '-', resourcegroup().name, '-', field('name'))]"
               },
               "existenceScope": "subscription",
               "deployment": {
                   ...
               }
           }
       }
   }
   ```

   ![Panel de detalles de cumplimiento - * ifNotExists](../media/determine-non-compliance/compliance-details-pane-existence.png)

> [!NOTE]
> Para proteger los datos, cuando un valor de propiedad es un _secreto_ el valor actual muestra asteriscos.

Estos detalles explican por qué un recurso actualmente no es compatible, pero no muestran cuando se realizó el cambio en el recurso que ha provocado que se convierten en no conformes. Para obtener esa información, consulte [(versión preliminar) historial de cambios](#change-history-preview) a continuación.

### <a name="compliance-reasons"></a>Motivos de cumplimiento

La siguiente matriz asigna cada posible _motivo_ a los responsables [condición](../concepts/definition-structure.md#conditions) en la definición de directiva:

|Motivo | Condición |
|-|-|
|El valor actual debe contener el valor de destino como clave. |containsKey o **no** notContainsKey |
|El valor actual debe contener el valor de destino. |contiene o **no** notContains |
|El valor actual debe ser igual que el valor de destino. |es igual a o **no** notEquals |
|El valor actual debe existir. |exists |
|El valor actual debe estar en el valor de destino. |en o **no** notIn |
|El valor actual debe ser como el valor de destino. |al igual que o **no** notLike |
|El valor actual debe coincidir con distinción de mayúsculas y minúsculas con el valor de destino. |coinciden o **no** notMatch |
|El valor actual debe coincidir sin distinción de mayúsculas y minúsculas con el valor de destino. |matchInsensitively o **no** notMatchInsensitively |
|El valor actual no debe contener el valor de destino como clave. |notContainsKey o **no** containsKey|
|El valor actual no debe contener el valor de destino. |notContains o **no** contiene |
|El valor actual no debe ser igual que el valor de destino. |notEquals o **no** es igual a |
|El valor actual no debe existir. |**no** existe  |
|El valor actual no debe estar en el valor de destino. |notIn o **no** en |
|El valor actual no debe ser como el valor de destino. |notLike o **no** como |
|El valor actual no debe coincidir con distinción de mayúsculas y minúsculas con el valor de destino. |notMatch o **no** coincide con |
|El valor actual no debe coincidir sin distinción de mayúsculas y minúsculas con el valor de destino. |notMatchInsensitively o **no** matchInsensitively |
|Ninguno de los recursos relacionados coincide con los detalles de vigencia de la definición de directiva. |Un recurso del tipo definido en **then.details.type** y relacionados con el recurso definido en el **si** parte de la regla de directiva no existe. |

## <a name="change-history-preview"></a>Historial de cambios (versión preliminar)

Como parte de un nuevo **versión preliminar pública**, los últimos 14 días de cambio está disponible para todos los recursos de Azure que admiten historial [completar la eliminación de modo](../../../azure-resource-manager/complete-mode-deletion.md). El historial de cambios proporciona información acerca de cuándo se detectó un cambio y una _diferencia visual_ para cada cambio. La detección de cambios se desencadena cuando se agregan, quitan o modificar las propiedades del Administrador de recursos.

1. Inicie el servicio Azure Policy en Azure Portal. Para ello, haga clic en **Todos los servicios** y, a continuación, busque y seleccione **Directiva**.

1. En el **Introducción** o **cumplimiento** , seleccione una directiva en cualquier **estado de cumplimiento**.

1. En el **cumplimiento recursos** pestaña de la **cumplimiento de directivas** página, seleccione un recurso.

1. Seleccione la pestaña **Historial de cambios (versión preliminar)** en la página **Compatibilidad de recursos**. Se muestra una lista de cambios detectados, si existe alguna.

   ![Ficha Historial de cambios de directiva en la página de compatibilidad de recursos](../media/determine-non-compliance/change-history-tab.png)

1. Seleccione uno de los cambios detectados. El _diff visual_ para el recurso se presenta en el **historial de cambios** página.

   ![Cambio de directiva historial Diff Visual en la página del historial de cambios](../media/determine-non-compliance/change-history-visual-diff.png)

Las _diferencias visuales_ ayudan a identificar los cambios de un recurso. Los cambios detectados no pueden estar relacionadas con el estado de compatibilidad actual del recurso.

## <a name="next-steps"></a>Pasos siguientes

- Puede consultar ejemplos en [Ejemplos de Azure Policy](../samples/index.md)
- Consulte la [Estructura de definición de directivas](../concepts/definition-structure.md)
- Consulte [Descripción de los efectos de directivas](../concepts/effects.md)
- Entender cómo se pueden [crear directivas mediante programación](programmatically-create.md)
- Obtenga información sobre cómo [obtener datos de cumplimiento](getting-compliance-data.md)
- Más información sobre cómo [corregir recursos no compatibles](remediate-resources.md)
- En [Organización de los recursos con grupos de administración de Azure](../../management-groups/overview.md), obtendrá información sobre lo que es un grupo de administración.