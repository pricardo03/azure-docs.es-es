---
title: Comprensión del bloqueo de recursos en planos técnicos de Azure Blueprint
description: Obtenga más información sobre las opciones de bloqueo para proteger los recursos cuando asigne un plano técnico.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 718c23b806da5058c042b961077e51ba0d4b6245
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973259"
---
# <a name="understand-resource-locking-in-azure-blueprints"></a>Comprensión del bloqueo de recursos en planos técnicos de Azure Blueprint

La creación de entornos coherentes a escala solo es realmente valiosa si existe un mecanismo que garantice la persistencia de la coherencia. En este artículo se explica cómo funciona el bloqueo de recursos en planos técnicos de Azure Blueprint.

## <a name="locking-modes-and-states"></a>Estados y modos de bloqueos

El modo de bloqueo se aplica a la asignación de planos técnicos y solo tiene dos opciones: **Ninguno** o **Todos los recursos**. Esto se configura durante la asignación de planos técnicos y no se puede cambiar una vez que la asignación se aplica correctamente a la suscripción.

Los recursos que crean las definiciones de artefactos dentro del plano técnico asignado a la suscripción tienen tres estados: **Sin bloquear**, **Solo lectura** o **No se puede editar/eliminar**. Cualquier tipo de artefacto puede tener el estado **Sin bloquear**. Sin embargo, los artefactos de grupos que no son de recursos tienen el estado **Solo lectura** y los grupos de recursos tienen el estado **No se puede editar/eliminar**. Esta es una distinción importante en la forma en que se administran estos recursos.

El estado **Solo lectura** es exactamente tal como se define: el recurso no se puede modificar de ninguna manera, no se puede cambiar ni eliminar. El estado **No se puede editar/eliminar** es más matizado debido a la naturaleza de tipo "contenedor" de los grupos de recursos. El objeto de grupo de recursos es de solo lectura, pero es posible crear, actualizar y eliminar recursos dentro del grupo de recursos, siempre y cuando no formen parte de ninguna asignación de planos técnicos con el estado de bloqueo **Solo lectura**.

## <a name="overriding-locking-states"></a>Sustitución de los estados de bloqueo

Aunque normalmente es posible para alguien con un [control de acceso basado en rol](../../../role-based-access-control/overview.md) apropiado en la suscripción, como el rol Propietario, para poder modificar o eliminar cualquier recurso, este no es el caso cuando los planos técnicos aplican el bloqueo como parte de una asignación implementada. Si la asignación se estableció con la opción **Bloquear**, ni siquiera el propietario de la suscripción puede cambiar los recursos incluidos.

Esto protege la coherencia del plano técnico definido y el entorno en que se ha diseñado para crear de una eliminación o modificación accidental o mediante programación.

## <a name="removing-locking-states"></a>Eliminación de los estados de bloqueo

Si es necesario eliminar los recursos creados por una asignación, la única manera de eliminarlos es quitar primero la asignación. Cuando se quita la asignación, se quitan los bloqueos creados por los planos técnicos. El recurso, sin embargo, se deja atrás y tendría que eliminarse por medios normales por alguien con los permisos apropiados.

## <a name="how-blueprint-locks-work"></a>Cómo funcionan los bloqueos de los planos técnicos

Un rol de control de acceso basado en rol `denyAssignments` se aplica a los recursos de artefactos durante la asignación de un plano técnico si la asignación ha seleccionado la opción **Bloquear**. El rol lo agrega la identidad administrada de la asignación del plano técnico y solo la misma identidad administrada puede eliminar de los recursos del artefacto. Esto refuerza el mecanismo de bloqueo y evita los intentos de quitar el bloqueo del plano técnico fuera de Blueprint. La eliminación del rol y el bloqueo solo es posible mediante la eliminación de la asignación de planos técnicos, que solo lo pueden realizar usuarios con los derechos adecuados.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre el [ciclo de vida de los planos técnicos](lifecycle.md).
- Descubra cómo utilizar [parámetros estáticos y dinámicos](parameters.md)
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](sequencing-order.md)
- Más información sobre la [actualización de las asignaciones existentes](../how-to/update-existing-assignments.md).
- Puede consultar la información de [solución de problemas generales](../troubleshoot/general.md) para resolver los problemas durante la asignación de un plano técnico