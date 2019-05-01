---
title: Determinación de las causas de incumplimiento
description: Cuando un recurso no es compatible, hay muchas razones posibles. Aprenda a averiguar la causa el incumplimiento.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 2f856e9c42b26d4e286493e2eb5d019a8cff6c23
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64868722"
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

## <a name="compliance-details-for-guest-configuration"></a>Detalles de cumplimiento para la configuración de invitado

Para _auditoría_ directivas en el _configuración de invitado_ categoría, podría haber varias opciones de configuración evaluadas dentro de la máquina virtual y necesita ver los detalles de cada configuración. Por ejemplo, si está realizando una auditoría para obtener una lista de aplicaciones instaladas y el estado de asignación es _no compatible_, deberá saber qué aplicaciones específicas que faltan.

También es posible que no tiene acceso para iniciar sesión en la máquina virtual directamente, pero deberá informar sobre por qué la máquina virtual es _no compatible_. Por ejemplo, podría auditar que las máquinas virtuales están unidas al dominio correcto e incluyen la pertenencia al dominio actual en los detalles del informe.

### <a name="azure-portal"></a>Azure Portal

1. Inicie el servicio Azure Policy en Azure Portal. Para ello, haga clic en **Todos los servicios** y, a continuación, busque y seleccione **Directiva**.

1. En el **Introducción** o **cumplimiento** , seleccione una asignación de directiva para las iniciativas que contiene una definición de directiva de configuración de invitado de _no compatible_.

1. Seleccione un _auditoría_ directiva en la iniciativa de _no compatible_.

   ![Vista Detalles de la definición de auditoría](../media/determine-non-compliance/guestconfig-audit-compliance.png)

1. En el **cumplimiento recursos** pestaña, se proporciona la siguiente información:

   - **Nombre** -el nombre de las asignaciones de configuración de invitado.
   - **El recurso primario** -la máquina virtual en un _no compatible_ estado para la asignación seleccionada de la configuración de invitado.
   - **Tipo de recurso** : la _guestConfigurationAssignments_ nombre completo.
   - **Última evaluación** : la última vez que el servicio de configuración de invitado de una notificación a Azure Policy sobre el estado de la máquina virtual de destino.

   ![Ver detalles de cumplimiento.](../media/determine-non-compliance/guestconfig-assignment-view.png)

1. Seleccione el nombre del trabajo de configuración de invitado en el **nombre** columna para abrir el **recursos cumplimiento** página.

1. Seleccione el **ver recurso** situado en la parte superior de la página para abrir el **invitado asignación** página.

El **invitado asignación** página muestra todos los detalles de cumplimiento disponible. Cada fila de la vista representa una evaluación que se realizó en la máquina virtual. En el **motivo** columna, una frase que describe por qué es la asignación de invitado _no compatible_ se muestra. Por ejemplo, si está realizando una auditoría que las máquinas virtuales deben estar unidas a un dominio, el **motivo** columna mostraría el texto incluido en la pertenencia al dominio actual.

![Ver detalles de cumplimiento.](../media/determine-non-compliance/guestconfig-compliance-details.png)

### <a name="azure-powershell"></a>Azure PowerShell

También puede ver los detalles de cumplimiento de Azure PowerShell. En primer lugar, asegúrese de que tiene instalado el módulo de configuración de invitado.

```azurepowershell-interactive
Install-Module Az.GuestConfiguration
```

Puede ver el estado actual de todas las asignaciones de invitado para una máquina virtual mediante el comando siguiente:

```azurepowershell-interactive
Get-AzVMGuestPolicyReport -ResourceGroupName <resourcegroupname> -VMName <vmname>
```

```output
PolicyDisplayName                                                         ComplianceReasons
-----------------                                                         -----------------
Audit that an application is installed inside Windows VMs                 {[InstalledApplication]bwhitelistedapp}
Audit that an application is not installed inside Windows VMs.            {[InstalledApplication]NotInstalledApplica...
```

Para ver solo el _motivo_ frase que describe por qué la máquina virtual es _no compatible_, devolver sólo la propiedad child de motivo.

```azurepowershell-interactive
Get-AzVMGuestPolicyReport -ResourceGroupName <resourcegroupname> -VMName <vmname> | % ComplianceReasons | % Reasons | % Reason
```

```output
The following applications are not installed: '<name>'.
```

También puede generar un historial de cumplimiento de las asignaciones de invitado en el ámbito de la máquina virtual. La salida de este comando incluye los detalles de cada informe para la máquina virtual.

> [!NOTE]
> La salida puede devolver un gran volumen de datos. Se recomienda para almacenar la salida en una variable.

```azurepowershell-interactive
$guestHistory = Get-AzVMGuestPolicyStatusHistory -ResourceGroupName <resourcegroupname> -VMName <vmname>
$guestHistory
```

```output
PolicyDisplayName                                                         ComplianceStatus ComplianceReasons StartTime              EndTime                VMName LatestRepor
                                                                                                                                                                  tId
-----------------                                                         ---------------- ----------------- ---------              -------                ------ -----------
[Preview]: Audit that an application is installed inside Windows VMs      NonCompliant                       02/10/2019 12:00:38 PM 02/10/2019 12:00:41 PM VM01  ../17fg0...
<truncated>
```

Para simplificar esta vista, use el **ShowChanged** parámetro. La salida de este comando solo incluye los informes que seguir un cambio en el estado de cumplimiento.

```azurepowershell-interactive
$guestHistory = Get-AzVMGuestPolicyStatusHistory -ResourceGroupName <resourcegroupname> -VMName <vmname> -ShowChanged
$guestHistory
```

```output
PolicyDisplayName                                                         ComplianceStatus ComplianceReasons StartTime              EndTime                VMName LatestRepor
                                                                                                                                                                  tId
-----------------                                                         ---------------- ----------------- ---------              -------                ------ -----------
Audit that an application is installed inside Windows VMs                 NonCompliant                       02/10/2019 10:00:38 PM 02/10/2019 10:00:41 PM VM01  ../12ab0...
Audit that an application is installed inside Windows VMs.                Compliant                          02/09/2019 11:00:38 AM 02/09/2019 11:00:39 AM VM01  ../e3665...
Audit that an application is installed inside Windows VMs                 NonCompliant                       02/09/2019 09:00:20 AM 02/09/2019 09:00:23 AM VM01  ../15ze1...
```

## <a name="a-namechange-historychange-history-preview"></a><a name="change-history"/>Historial de cambios (versión preliminar)

Como parte de un nuevo **versión preliminar pública**, los últimos 14 días del historial de cambios están disponibles para todos los recursos de Azure que admiten [completar la eliminación de modo](../../../azure-resource-manager/complete-mode-deletion.md). El historial de cambios proporciona información acerca de cuándo se detectó un cambio y una _diferencia visual_ para cada cambio. La detección de cambios se desencadena cuando se agregan, quitan o modificar las propiedades del Administrador de recursos.

1. Inicie el servicio Azure Policy en Azure Portal. Para ello, haga clic en **Todos los servicios** y, a continuación, busque y seleccione **Directiva**.

1. En el **Introducción** o **cumplimiento** , seleccione una directiva en cualquier **estado de cumplimiento**.

1. En el **cumplimiento recursos** pestaña de la **cumplimiento de directivas** página, seleccione un recurso.

1. Seleccione la pestaña **Historial de cambios (versión preliminar)** en la página **Compatibilidad de recursos**. Se muestra una lista de cambios detectados, si existe alguna.

   ![Ficha Historial de cambios de directiva en la página de compatibilidad de recursos](../media/determine-non-compliance/change-history-tab.png)

1. Seleccione uno de los cambios detectados. El _diff visual_ para el recurso se presenta en el **historial de cambios** página.

   ![Cambio de directiva historial Diff Visual en la página del historial de cambios](../media/determine-non-compliance/change-history-visual-diff.png)

Las _diferencias visuales_ ayudan a identificar los cambios de un recurso. Los cambios detectados no pueden estar relacionadas con el estado de compatibilidad actual del recurso.

Proporcionan datos del historial de cambios [recursos de Azure Graph](../../resource-graph/overview.md). Para consultar esta información fuera de Azure portal, consulte [obtener los cambios de recursos](../../resource-graph/how-to/get-resource-changes.md).

## <a name="next-steps"></a>Pasos siguientes

- Revise los ejemplos en [ejemplos de Azure Policy](../samples/index.md).
- Vea la [Estructura de definición de Azure Policy](../concepts/definition-structure.md).
- Vea la [Descripción de los efectos de directivas](../concepts/effects.md).
- Comprender cómo [crear mediante programación las directivas](programmatically-create.md).
- Obtenga información sobre cómo [obtener datos de cumplimiento](getting-compliance-data.md).
- Obtenga información sobre cómo [corregir recursos no compatibles](remediate-resources.md).
- Compruebe que un grupo de administración con [organizar los recursos con grupos de administración de Azure](../../management-groups/overview.md).