---
title: Descripción de los efectos de Azure Policy
description: La definición de Azure Policy tiene varios efectos que determinan cómo se administra y notifica el cumplimiento.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 23bbbe9cf86268f93ae1f8fcec9303efa8a673de
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796723"
---
# <a name="understanding-policy-effects"></a>Descripción de los efectos de directivas

Cada definición de directiva en Azure Policy tiene un efecto único que determina lo que ocurre durante el análisis cuando el segmento **if** de la regla de directiva se evalúa para que coincida con el recurso que se está examinando. Los efectos también pueden comportarse de manera diferente si son para un nuevo recurso, un recurso actualizado o un recurso existente.

Actualmente son cinco los efectos que se admiten en una definición de directiva:

- Append
- Auditoría
- AuditIfNotExists
- Denegar
- DeployIfNotExists

## <a name="order-of-evaluation"></a>Orden de evaluación

Cuando se realiza una solicitud para crear o actualizar un recurso a través de Azure Resource Manager, la directiva procesa algunos de los efectos antes de entregar la solicitud al proveedor de recursos adecuados.
De este modo, se evita el procesamiento innecesario por un proveedor de recursos cuando un recurso no cumple con los controles de regulación diseñada de Policy. Policy crea una lista de todas las definiciones de directiva asignadas, por una directiva o asignación de iniciativa, que se aplica por ámbito (menos las exclusiones) al recurso y se prepara para evaluar el recurso en cada definición.

- **Append** se evalúa primero. Dado que append ya podría alterar la solicitud, un cambio realizado por append podría evitar la activación de un efecto audit o deny.
- Luego se evalúa **deny**. La evaluación de deny antes de la audit impide el doble registro de un recurso no deseado.
- A continuación, se evalúa **audit** antes de que la solicitud vaya al proveedor de recursos.

Una vez que la solicitud se proporciona al proveedor de recursos y el proveedor de recursos devuelve un código de estado correcto, se evalúan **AuditIfNotExists** y **DeployIfNotExists** para determinar si se requiere registro o acción de cumplimiento de seguimiento.

## <a name="append"></a>Append

Append se utiliza para agregar campos adicionales al recurso solicitado durante la creación o actualización. Puede ser útil para agregar etiquetas en recursos como costCenter o especificar direcciones IP para un recurso de almacenamiento.

### <a name="append-evaluation"></a>Evaluación de append

Como se mencionó, append evalúa antes de que la solicitud sea procesada por un proveedor de recursos durante la creación o actualización de un recurso. Append agrega campos al recurso cuando se cumple la condición **if** de la regla de directiva. Si el efecto de append invalidara un valor de la solicitud original con un valor diferente, actúa como un efecto de deny y rechaza la solicitud.

Cuando una definición de directiva que utiliza el efecto de append se ejecuta como parte de un ciclo de evaluación, no realiza cambios en los recursos que ya existen. En su lugar, marca cualquier recurso que cumple la condición **if** como no conforme.

### <a name="append-properties"></a>Propiedades de append

Un efecto append solo tiene una matriz **details**, que es necesaria. Como **details** es una matriz, puede tomar un único par **campo-valor** o varios. Consulte la [definición de directiva](policy-definition.md#fields) para obtener la lista de campos aceptables.

### <a name="append-examples"></a>Ejemplos de append

Ejemplo 1: único par **campo/valor** para anexar una etiqueta.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "tags.myTag",
        "value": "myTagValue"
    }]
}
```

Ejemplo 2: varios pares **campo/valor** para anexar un conjunto de etiquetas.

```json
"then": {
    "effect": "append",
    "details": [{
            "field": "tags.myTag",
            "value": "myTagValue"
        },
        {
            "field": "tags.myOtherTag",
            "value": "myOtherTagValue"
        }
    ]
}
```

Ejemplo 3: único par **campo/valor** que usa un [alias](policy-definition.md#aliases) con una matriz **valor** para establecer reglas IP en una cuenta de almacenamiento.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]",
        "value": [{
            "action": "Allow",
            "value": "134.5.0.0/21"
        }]
    }]
}
```

## <a name="deny"></a>Denegar

Deny se utiliza para evitar una solicitud de recursos que no coincida con los estándares deseados a través de una definición de directiva y se produce un error en la solicitud.

### <a name="deny-evaluation"></a>Evaluación de deny

Al crear o actualizar un recurso, deny evita la solicitud antes de que se envíe al proveedor de recursos. La solicitud se devuelve como un código 403 (prohibido). En el portal, este estado de prohibido puede verse como un estado en la implementación que se evitó debido a la asignación de directiva.

Durante un ciclo de evaluación, las definiciones de directiva con un efecto deny que coinciden con los recursos se marcan como no conformes, pero no se realiza ninguna acción en dicho recurso.

### <a name="deny-properties"></a>Propiedades de deny

El efecto deny no tiene propiedades adicionales para su uso en la condición **then** de la definición de directiva.

### <a name="deny-example"></a>Ejemplo de deny

Ejemplo: uso del efecto deny.

```json
"then": {
    "effect": "deny"
}
```

## <a name="audit"></a>Auditoría

El efecto audit se utiliza para crear un evento de advertencia en el registro de auditoría cuando se evalúa un recurso no compatible, pero no se detiene la solicitud.

### <a name="audit-evaluation"></a>Evaluación de audit

El efecto audit es el último en ejecutarse durante la creación o actualización de un recurso antes de que el recurso se envíe al proveedor de recursos. Audit funciona igual para una solicitud de recursos y un ciclo de evaluación, y ejecuta una operación `Microsoft.Authorization/policies/audit/action` en el registro de actividad. En ambos casos, el recurso se marca como no conforme.

### <a name="audit-properties"></a>Propiedades de audit

El efecto audit no tiene propiedades adicionales para su uso en la condición **then** de la definición de directiva.

### <a name="audit-example"></a>Ejemplo de audit

Ejemplo: uso del efecto audit.

```json
"then": {
    "effect": "audit"
}
```

## <a name="auditifnotexists"></a>AuditIfNotExists

AuditIfNotExists habilita la auditoría en un recurso que coincide con la condición **if**, pero no tiene los componentes especificados en la propiedad **details** de la condición **then**.

### <a name="auditifnotexists-evaluation"></a>Evaluación de AuditIfNotExists

AuditIfNotExists se ejecuta después de que un proveedor de recursos haya operado con una solicitud de creación o actualización de un recurso y haya devuelto un código de estado correcto. El efecto se desencadena si no hay recursos relacionados o si los recursos definidos por **ExistenceCondition** no se evalúan como true. Cuando se desencadena el efecto, una operación `Microsoft.Authorization/policies/audit/action` en el registro de actividad se ejecuta de la misma manera que el efecto audit. Cuando se activa, el recurso que cumple la condición **if** es el recurso que está marcado como no conforme.

### <a name="auditifnotexists-properties"></a>Propiedades de AuditIfNotExists

La propiedad **details** de los efectos de AuditIfNotExists tiene todas las subpropiedades que definen los recursos relacionados para coincidir.

- **Type** [obligatorio]
  - Especifica el tipo del recurso relacionado para coincidir.
  - Comienza tratando de recuperar un recurso debajo del recurso de condición **if** , luego consulta dentro del mismo grupo de recursos que el recurso de condición **if**.
- **Nombre** (opcional)
  - Especifica el nombre exacto del recurso para coincidir y hace que la directiva recupere un recurso específico en lugar de todos los recursos del tipo especificado.
- **ResourceGroupName** (opcional)
  - Permite que la coincidencia del recurso relacionado provenga de un grupo de recursos diferente.
  - No se aplica si **type** es un recurso que estaría debajo del recurso de condición **if**.
  - El valor predeterminado es el grupo de recursos del recurso de la condición **if**.
- **ExistenceScope** (opcional)
  - Los valores permitidos son _Subscription_ y _ResourceGroup_.
  - Establece el ámbito de donde obtener el recurso relacionado para que coincida.
  - No se aplica si **type** es un recurso que estaría debajo del recurso de condición **if**.
  - Para _ResourceGroup_, se limitaría al grupo de recursos del recurso de la condición **if** o al grupo de recursos especificado en **ResourceGroupName**.
  - Para _Subscription_, se consulta toda la suscripción para el recurso relacionado.
  - El valor predeterminado es _ResourceGroup_.
- **ExistenceCondition** (opcional)
  - Si no se especifica, todo recurso relacionado de **type** cumple con el efecto y no desencadena la auditoría.
  - Utiliza el mismo lenguaje que la regla de directiva para la condición **if**, pero se evalúa individualmente en relación con cada recurso relacionado.
  - Si algún recurso relacionado coincidente se evalúa como true, se cumple la condición del efecto y no se desencadena la auditoría.
  - Puede utilizar [field()] para comprobar la equivalencia con los valores en la condición **if**.
  - Por ejemplo, podría usarse para validar que el recurso primario (en la condición **if**) está en la misma ubicación de recursos que el recurso relacionado coincidente.

### <a name="auditifnotexists-example"></a>Ejemplo de AuditIfNotExists

Ejemplo: evalúa las máquinas virtuales para determinar si existe la extensión Antimalware y luego audita cuando faltan.

```json
{
    "if": {
        "field": "type",
        "equals": "Microsoft.Compute/virtualMachines"
    },
    "then": {
        "effect": "auditIfNotExists",
        "details": {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "existenceCondition": {
                "allOf": [{
                        "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                        "equals": "Microsoft.Azure.Security"
                    },
                    {
                        "field": "Microsoft.Compute/virtualMachines/extensions/type",
                        "equals": "IaaSAntimalware"
                    }
                ]
            }
        }
    }
}
```

## <a name="deployifnotexists"></a>DeployIfNotExists

Similar a AuditIfNotExists, DeployIfNotExists ejecuta una implementación de plantilla cuando se cumple la condición.

### <a name="deployifnotexists-evaluation"></a>Evaluación de DeployIfNotExists

DeployIfNotExists se ejecuta también después de que un proveedor de recursos haya operado con una solicitud de creación o actualización de un recurso y haya devuelto un código de estado correcto. El efecto se desencadena si no hay recursos relacionados o si los recursos definidos por **ExistenceCondition** no se evalúan como true. Cuando se desencadena el efecto, se ejecuta una implementación de plantilla.

Durante un ciclo de evaluación, las definiciones de directiva con un efecto DeployIfNotExists que coinciden con los recursos se marcan como no conformes, pero no se realiza ninguna acción en dicho recurso.

### <a name="deployifnotexists-properties"></a>Propiedades de DeployIfNotExists

La propiedad **details** de los efectos de DeployIfNotExists tiene todas las subpropiedades que definen los recursos relacionados para coincidir y la implementación de plantilla para ejecutar.

- **Type** [obligatorio]
  - Especifica el tipo del recurso relacionado para coincidir.
  - Comienza tratando de recuperar un recurso debajo del recurso de condición **if** , luego consulta dentro del mismo grupo de recursos que el recurso de condición **if**.
- **Nombre** (opcional)
  - Especifica el nombre exacto del recurso para coincidir y hace que la directiva recupere un recurso específico en lugar de todos los recursos del tipo especificado.
- **ResourceGroupName** (opcional)
  - Permite que la coincidencia del recurso relacionado provenga de un grupo de recursos diferente.
  - No se aplica si **type** es un recurso que estaría debajo del recurso de condición **if**.
  - El valor predeterminado es el grupo de recursos del recurso de la condición **if**.
  - Si se ejecuta una implementación de plantilla, se implementa en el grupo de recursos de este valor.
- **ExistenceScope** (opcional)
  - Los valores permitidos son _Subscription_ y _ResourceGroup_.
  - Establece el ámbito de donde obtener el recurso relacionado para que coincida.
  - No se aplica si **type** es un recurso que estaría debajo del recurso de condición **if**.
  - Para _ResourceGroup_, se limitaría al grupo de recursos del recurso de la condición **if** o al grupo de recursos especificado en **ResourceGroupName**.
  - Para _Subscription_, se consulta toda la suscripción para el recurso relacionado.
  - El valor predeterminado es _ResourceGroup_.
- **ExistenceCondition** (opcional)
  - Si no se especifica, todo recurso relacionado de **type** cumple con el efecto y no desencadena la implementación.
  - Utiliza el mismo lenguaje que la regla de directiva para la condición **if**, pero se evalúa individualmente en relación con cada recurso relacionado.
  - Si algún recurso relacionado coincidente se evalúa como true, se cumple la condición del efecto y no se desencadena la implementación.
  - Puede utilizar [field()] para comprobar la equivalencia con los valores en la condición **if**.
  - Por ejemplo, podría usarse para validar que el recurso primario (en la condición **if**) está en la misma ubicación de recursos que el recurso relacionado coincidente.
- **Deployment** [obligatorio]
  - Esta propiedad debe contener la implementación de plantilla completa ya que luego se pasaría a la API PUT `Microsoft.Resources/deployments`. Para más información, consulte [Deployments REST API](/rest/api/resources/deployments) (API REST de implementaciones).

  > [!NOTE]
  > Todas las funciones dentro de la propiedad **Deployment** se evalúan como componentes de la plantilla, no la directiva. La excepción es la propiedad **parameters** que pasa los valores de la directiva a la plantilla. El **valor** en esta sección bajo el nombre de un parámetro de plantilla se usa para realizar este paso de valor (vea _fullDbName_ en el ejemplo DeployIfNotExists).

### <a name="deployifnotexists-example"></a>Ejemplo de DeployIfNotExists

Ejemplo: evalúa las bases de datos de SQL Server para determinar si está habilitado transparentDataEncryption. De lo contrario, se ejecuta una implementación para habilitarlo.

```json
"if": {
    "field": "type",
    "equals": "Microsoft.Sql/servers/databases"
},
"then": {
    "effect": "DeployIfNotExists",
    "details": {
        "type": "Microsoft.Sql/servers/databases/transparentDataEncryption",
        "name": "current",
        "existenceCondition": {
            "field": "Microsoft.Sql/transparentDataEncryption.status",
            "equals": "Enabled"
        },
        "deployment": {
            "properties": {
                "mode": "incremental",
                "template": {
                    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {
                        "fullDbName": {
                            "type": "string"
                        }
                    },
                    "resources": [{
                        "name": "[concat(parameters('fullDbName'), '/current')]",
                        "type": "Microsoft.Sql/servers/databases/transparentDataEncryption",
                        "apiVersion": "2014-04-01",
                        "properties": {
                            "status": "Enabled"
                        }
                    }]
                },
                "parameters": {
                    "fullDbName": {
                        "value": "[field('fullName')]"
                    }
                }
            }
        }
    }
}
```

## <a name="layering-policies"></a>Directivas de distribución en capas

Un recurso puede verse afectado por varias asignaciones. Estas asignaciones pueden estar en el mismo ámbito (recurso específico, grupo de recursos, suscripción o grupo de administración) o en distintos ámbitos. Cada una de estas asignaciones también es probable que tenga un efecto diferente al definido. En cualquier caso, la condición y el efecto de cada directiva (asignada directamente o como parte de una iniciativa) se evalúan por separado. Por ejemplo, si la directiva 1 tiene una condición que limita la ubicación para que la suscripción A no se cree en "Oeste de EE. UU" con el efecto deny y la directiva 2 limita los recursos en el grupo de recursos B (que está en la suscripción A) para que no se cree en "Este de EE. UU" con el efecto audit asignado en ambos casos, el resultado sería el siguiente:

- Cualquier recurso que ya esté en el grupo de recursos B en "Este de EE. UU." está conforme con la directiva 2, pero se marca como no conforme con la directiva 1.
- Cualquier recurso que ya esté en el grupo de recursos B que no esté en "Este de EE. UU." se marcará como no conforme con la directiva 2, y también se marcaría como no conforme con la directiva 1 si no es "Oeste de EE. UU.".
- Cualquier recurso nuevo en la suscripción A que no esté en "Oeste de EE. UU." se denegaría por la directiva 1.
- Cualquier recurso nuevo en la suscripción A/grupo de recursos B en "Oeste de EE. UU." se marcaría como no conforme en la directiva 2, pero se crearía (conforme a la directiva 1 y la directiva 2 es audit y no deny).

Si tanto la directiva 1 como la directiva 2 tienen efecto deny, la situación cambiaría a:

- Cualquier recurso que ya esté en el grupo de recursos B y no en "Este de EE. UU." se marcaría como no conforme con la directiva 2.
- Cualquier recurso que ya esté en el grupo de recursos B y no en "Oeste de EE. UU." se marcaría como no conforme con la directiva 1.
- Cualquier recurso nuevo en la suscripción A que no esté en "Oeste de EE. UU." se denegaría por la directiva 1.
- Cualquier recurso nuevo en la suscripción A/grupo de recursos B se denegaría (ya que su ubicación nunca podría cumplir con la directiva 1 y la directiva 2).

Como cada asignación se evalúa individualmente, no hay oportunidad de que un recurso se cuele por una rendija provocada por las diferencias en el ámbito. Por lo tanto, el resultado neto de la distribución en capas o superposición de directivas se considera **acumulativo más restrictivo**. En otras palabras, un recurso que desea crear puede bloquearse debido a las directivas en conflicto y superpuestas como en el ejemplo anterior si la directiva 1 y la directiva 2 tuvieran un efecto deny. Si aún necesita que el recurso se cree en el ámbito de destino, revise las exclusiones en cada asignación para asegurarse de que las directivas adecuadas estén incidiendo en los ámbitos adecuados.

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene una comprensión más profunda de los efectos de la definición de directivas, revise ejemplos de directiva:

- Vea más ejemplos en [Ejemplos de Azure Policy](json-samples.md).
