---
title: Bloqueo de recursos
description: Obtenga más información sobre las opciones de bloqueo para proteger los recursos cuando asigne un plano técnico.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 0e272f7137967b545269a408b6e83552de532682
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53309440"
---
# <a name="understand-resource-locking-in-azure-blueprints"></a>Comprensión del bloqueo de recursos en planos técnicos de Azure Blueprint

La creación de entornos coherentes a escala solo es realmente valiosa si existe un mecanismo que mantenga la coherencia. En este artículo se explica cómo funciona el bloqueo de recursos en planos técnicos de Azure Blueprint.

## <a name="locking-modes-and-states"></a>Estados y modos de bloqueos

El modo de bloqueo se aplica a la asignación de planos técnicos y solo tiene dos opciones: **None** (Ninguno) o **All Resources** (Todos los recursos). Este modo de bloqueo se configura durante la asignación de planos técnicos y no se puede cambiar una vez que la asignación se aplica correctamente a la suscripción.

Los recursos creados por los artefactos en una asignación de plano técnico tienen tres estados: **Not Locked** (Sin bloquear), **Read Only** (Solo lectura) o **Cannot Edit / Delete** (No se puede editar/eliminar). Todos artefactos pueden tener el estado **Sin bloquear**. Sin embargo, los artefactos de grupos que no son de recursos tienen el estado **Solo lectura** y los grupos de recursos tienen los estados **No se puede editar/eliminar**. Esta diferencia es una distinción importante en la forma en la que se administran estos recursos.

El estado **Solo lectura** es exactamente tal como se define: el recurso no se puede modificar de ninguna manera, no se puede cambiar ni eliminar. El estado **No se puede editar/eliminar** es más matizado debido a la naturaleza de tipo "contenedor" de los grupos de recursos. El objeto del grupo de recursos es de solo lectura, pero se pueden realizar cambios en los recursos sin bloquear.

## <a name="overriding-locking-states"></a>Sustitución de los estados de bloqueo

Es típico que alguien con el[control de acceso basado en roles](../../../role-based-access-control/overview.md) (RBAC) adecuado en la suscripción, como el rol "Propietario", pueda modificar o eliminar cualquier recurso. Este acceso no es el caso cuando se aplican planos de bloqueo como parte de una asignación implementada. Si la asignación se estableció con la opción **Bloquear**, ni siquiera el propietario de la suscripción puede cambiar los recursos incluidos.

Esta medida de seguridad protege la coherencia del plano técnico definido y el entorno en el que se ha diseñado para crear a partir de una eliminación o modificación accidental o mediante programación.

## <a name="removing-locking-states"></a>Eliminación de los estados de bloqueo

Si es necesario eliminar los recursos creados por una asignación, una manera de hacerlo es quitar primero la asignación. Cuando se quita la asignación, se quitan los bloqueos creados por los planos técnicos. Sin embargo, el recurso se queda atrás y deberá eliminarse por medios normales.

## <a name="how-blueprint-locks-work"></a>Cómo funcionan los bloqueos de los planos técnicos

Un rol de control de acceso basado en rol `denyAssignments` se aplica a los recursos de artefactos durante la asignación de un plano técnico si la asignación ha seleccionado la opción **Bloquear**. El rol lo agrega la identidad administrada de la asignación del plano técnico y solo la misma identidad administrada puede eliminar de los recursos del artefacto. Esta medida de seguridad refuerza el mecanismo de bloqueo y evita que se quite el bloqueo del plano técnico fuera de Blueprint. La eliminación del rol y el bloqueo solo es posible mediante la eliminación de la asignación de planos técnicos, que solo lo pueden realizar usuarios con los derechos adecuados.

> [!IMPORTANT]
> Azure Resource Manager almacena en caché los detalles de asignación de roles durante un máximo de 30 minutos. Como resultado, puede que `denyAssignments` no funcione completamente en los recursos de plano técnico. Durante este período de tiempo es posible eliminar un recurso diseñado para estar protegido por bloqueos de plano técnico.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre el [ciclo de vida del plano técnico](lifecycle.md)
- Descubra cómo utilizar [parámetros estáticos y dinámicos](parameters.md).
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](sequencing-order.md)
- Más información sobre la [actualización de las asignaciones existentes](../how-to/update-existing-assignments.md).
- Puede consultar la información de [solución de problemas generales](../troubleshoot/general.md) para resolver los problemas durante la asignación de un plano técnico.