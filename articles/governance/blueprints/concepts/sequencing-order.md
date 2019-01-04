---
title: Orden de la secuencia de implementación
description: Información acerca del ciclo de vida de un plano técnico y detalles sobre cada fase.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 11/12/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: bd12aabf0ca8f82261e6b3c677d7306ee46c4171
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53308624"
---
# <a name="understand-the-deployment-sequence-in-azure-blueprints"></a>Información sobre la secuencia de implementación en Azure Blueprint

Azure Blueprint usa un **orden de secuenciación** para determinar el orden de creación de recursos al procesar la asignación de un plano técnico. En este artículo se explican los conceptos siguientes:

- El orden de secuenciación predeterminado que se usa
- Cómo personalizar el orden
- Cómo se procesa el orden personalizado

Hay variables en los ejemplos JSON que se deben reemplazar por los suyos propios:

- `{YourMG}`: reemplácelo por el nombre del grupo de administración

## <a name="default-sequencing-order"></a>Orden de secuenciación predeterminado

Si el plano técnico no contiene ninguna directiva para el orden al implementar los artefactos o si la directiva es null, se utiliza el orden siguiente:

- Artefactos de **asignación de roles** de nivel de suscripción ordenados por nombre de artefacto
- Artefactos de **asignación de directiva** de nivel de suscripción ordenados por nombre de artefacto
- Artefactos de **plantilla de Azure Resource Manager** de nivel de suscripción ordenados por nombre de artefacto
- Artefactos de **grupo de recursos** (incluidos los secundarios) ordenados por nombre de marcador de posición

Dentro de cada artefacto de **grupo de recursos**, se usa el orden de secuencia siguiente para crear los artefactos dentro de ese grupo de recursos:

- Artefactos secundarios de **asignación de roles** de grupo de recursos ordenados por nombre de artefacto
- Artefactos de **asignación de directiva** secundarios de grupo de recursos ordenados por nombre de artefacto
- Artefactos de **plantilla de Azure Resource Manager** secundarios de grupo de recursos ordenados por nombre de artefacto

## <a name="customizing-the-sequencing-order"></a>Personalización del orden de secuenciación

Al crear planos técnicos de gran tamaño, puede ser necesario que los recursos se creen en un orden específico. El patrón de uso más común de este escenario se da cuando un plano técnico incluye varias plantillas de Azure Resource Manager. Para que Blueprints controle este patrón, permite definir el orden de secuenciación.

La ordenación se logra definiendo una propiedad `dependsOn` en JSON. Solo los objetos de artefacto y del plano técnico (para grupos de recursos) admiten esta propiedad. `dependsOn` es una matriz de cadenas de nombres de artefacto que el artefacto en particular debe crear antes de su propia creación.

> [!NOTE]
> Los artefactos de **grupo de recursos** son compatibles con la propiedad `dependsOn`, pero no pueden ser el destino de la propiedad `dependsOn` de cualquier tipo de artefacto.

### <a name="example---blueprint-with-ordered-resource-group"></a>Ejemplo: plano técnico con el grupo de recursos ordenado

Este plano técnico de ejemplo tiene un grupo de recursos que ha definido un orden personalizado de secuenciación declarando un valor para `dependsOn`, junto con un grupo de recursos estándar. En este caso, el artefacto denominado **assignPolicyTags** se procesará antes que el grupo de recursos **ordered-rg**. **standard-rg** se procesará en el orden de secuenciación predeterminado.

```json
{
    "properties": {
        "description": "Example blueprint with custom sequencing order",
        "resourceGroups": {
            "ordered-rg": {
                "dependsOn": [
                    "assignPolicyTags"
                ],
                "metadata": {
                    "description": "Resource Group that waits for 'assignPolicyTags' creation"
                }
            },
            "standard-rg": {
                "metadata": {
                    "description": "Resource Group that follows the standard sequence ordering"
                }
            }
        },
        "targetScope": "subscription"
    },
    "id": "/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/mySequencedBlueprint",
    "type": "Microsoft.Blueprint/blueprints",
    "name": "mySequencedBlueprint"
}
```

### <a name="example---artifact-with-custom-order"></a>Ejemplo de artefacto con orden personalizado

Este ejemplo es un artefacto de directiva que depende de una plantilla de Azure Resource Manager. Con el orden predeterminado, se crearía un artefacto de directiva antes que la plantilla de Azure Resource Manager. Esta ordenación permite que el artefacto de directiva espere a que se cree la plantilla de Azure Resource Manager.

```json
{
    "properties": {
        "displayName": "Assigns an identifying tag",
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
        "resourceGroup": "standard-rg",
        "dependsOn": [
            "customTemplate"
        ]
    },
    "kind": "policyAssignment",
    "id": "/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/mySequencedBlueprint/artifacts/assignPolicyTags",
    "type": "Microsoft.Blueprint/artifacts",
    "name": "assignPolicyTags"
}
```

## <a name="processing-the-customized-sequence"></a>Procesamiento de la secuencia personalizada

Durante el proceso de creación, se usa una ordenación topológica para crear el grafo de dependencias de los artefactos del plano técnico. La comprobación garantiza que se admita cada nivel de dependencia entre los grupos de recursos y los artefactos.

Si se declara una dependencia de artefacto que no alteraría el orden predeterminado, no se realiza ningún cambio. Un ejemplo es un grupo de recursos que depende de una directiva de nivel de suscripción. Otro ejemplo es una asignación de una directiva secundaria "standard-rg" de un grupo de recursos que dependa de la asignación de roles secundarios de un grupo de recursos "standard-rg". En ambos casos, el `dependsOn` no habría alterado el orden de secuenciación predeterminado y no se habría realizado ningún cambio.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre el [ciclo de vida del plano técnico](lifecycle.md)
- Descubra cómo utilizar [parámetros estáticos y dinámicos](parameters.md)
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](resource-locking.md)
- Aprenda a [actualizar las asignaciones existentes](../how-to/update-existing-assignments.md).
- Puede consultar la información de [solución de problemas generales](../troubleshoot/general.md) para resolver los problemas durante la asignación de un plano técnico.