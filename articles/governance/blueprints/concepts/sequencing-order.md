---
title: Información sobre la secuencia de implementación en Azure Blueprint
description: Información acerca del ciclo de vida de un plano técnico y detalles sobre cada fase.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: c09fb26d8375e08281241aaed3f6f6e30acc755b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955459"
---
# <a name="understand-the-deployment-sequence-in-azure-blueprints"></a>Información sobre la secuencia de implementación en Azure Blueprint

Azure Blueprint usa un **orden de secuenciación** para determinar el orden de creación de recursos al procesar la asignación de un plano técnico. En este artículo se explica el orden de secuenciación predeterminado que se usa, cómo personalizarlo y cómo se procesa el orden personalizado.

Hay variables en los ejemplos JSON que se deben reemplazar por los suyos propios:

- `{YourMG}`: reemplácelo por el nombre del grupo de administración

## <a name="default-sequencing-order"></a>Orden de secuenciación predeterminado

Si el plano técnico no contiene ninguna directiva para el orden al implementar los artefactos o si la directiva es null, se utiliza el orden siguiente:

- Artefactos de **asignación de roles** de nivel de suscripción ordenados por nombre de artefacto
- Artefactos de **asignación de directiva** de nivel de suscripción ordenados por nombre de artefacto
- Artefactos de **plantilla de Azure Resource Manager** de nivel de suscripción ordenados por nombre de artefacto
- Artefactos de **grupo de recursos** (incluidos los secundarios) ordenados por nombre de marcador de posición

Dentro de cada artefacto de **grupo de recursos** que se procesa, se usa el orden de secuencia siguiente para crear los artefactos dentro de ese grupo de recursos:

- Artefactos secundarios de **asignación de roles** de grupo de recursos ordenados por nombre de artefacto
- Artefactos de **asignación de directiva** secundarios de grupo de recursos ordenados por nombre de artefacto
- Artefactos de **plantilla de Azure Resource Manager** secundarios de grupo de recursos ordenados por nombre de artefacto

## <a name="customizing-the-sequencing-order"></a>Personalización del orden de secuenciación

Al crear planos técnicos de gran tamaño, puede ser necesario que un recurso se cree en un orden específico en relación con otro. El patrón de uso más común es cuando un plano técnico incluye varias plantillas de Azure Resource Manager. Para ello, Azure Blueprint permite definir el orden de secuenciación.

Esto se logra definiendo una propiedad `dependsOn` en JSON. Solo los objetos de artefacto y del plano técnico (para grupos de recursos) admiten esta propiedad. `dependsOn` es una matriz de cadenas de nombres de artefacto que el artefacto en particular debe crear antes de su propia creación.

### <a name="example---blueprint-with-ordered-resource-group"></a>Ejemplo: plano técnico con el grupo de recursos ordenado

Se trata de un plano técnico de ejemplo con un grupo de recursos que ha definido un orden personalizado de secuenciación declarando un valor para `dependsOn`, junto con un grupo de recursos estándar. En este caso, el artefacto denominado **assignPolicyTags** se procesará antes que el grupo de recursos **ordered-rg**.
**standard-rg** se procesará en el orden de secuenciación predeterminado.

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

Se trata de un ejemplo de artefacto de directiva que depende de una plantilla de Azure Resource Manager. Con el orden predeterminado, se crearía un artefacto de directiva antes que la plantilla de Azure Resource Manager. Esto permite que el artefacto de directiva espere a que se cree la plantilla de Azure Resource Manager.

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

Durante el proceso de creación, se usa una ordenación topológica para crear el grafo de dependencias del plano técnico y sus artefactos. Así se garantiza que se admitan varios niveles de dependencias entre grupos de recursos y artefactos.

Si se declara una dependencia en el plano técnico o un artefacto que no alteraría el orden predeterminado, no se realiza ningún cambio en el orden de secuenciación. Algunos ejemplos de esta área son un grupo de recursos que depende de una asignación de directiva secundaria "standard-rg" de grupo de recursos o una directiva de nivel de suscripción que depende de la asignación de roles secundaria 'standard-rg". En ambos casos, el `dependsOn` no habría alterado el orden de secuenciación predeterminado y no se habría realizado ningún cambio.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre el [ciclo de vida de los planos técnicos](lifecycle.md).
- Descubra cómo utilizar [parámetros estáticos y dinámicos](parameters.md)
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](resource-locking.md)
- Más información sobre la [actualización de las asignaciones existentes](../how-to/update-existing-assignments.md)
- Puede consultar la información de [solución de problemas generales](../troubleshoot/general.md) para resolver los problemas durante la asignación de un plano técnico