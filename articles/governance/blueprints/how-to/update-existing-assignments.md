---
title: Procedimiento para actualizar una asignación de Azure Blueprint
description: Información acerca del mecanismo para actualizar una asignación existente en Azure Blueprint.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: ecac0fb21a6691874d5e8db49eadd7114d41845f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956207"
---
# <a name="how-to-update-an-existing-blueprint-assignment"></a>Procedimiento para actualizar una asignación de plano técnico

Cuando se asigna un plano técnico, se puede actualizar la asignación. Hay varias razones para actualizar una asignación, entre las que se incluyen:

- Agregar o quitar un [bloqueo de recursos](../concepts/resource-locking.md)
- Cambiar el valor de [parámetros dinámicos](../concepts/parameters.md#dynamic-parameters)
- Actualizar la asignación por una versión **publicada** más reciente del plano técnico

## <a name="updating-assignments"></a>Actualización de asignaciones

1. Inicie el servicio Azure Blueprint en Azure Portal. Para ello, haga clic en **Todos los servicios** y, a continuación, busque y seleccione **Directiva** en el panel de la izquierda. En la página **Directiva**, haga clic en **Planos técnicos**.

1. Seleccione **Planos técnicos asignados** desde la página de la izquierda.

1. En la lista de planos técnicos, haga clic con el botón izquierdo en la asignación del plano técnico y, a continuación, haga clic en el botón **Actualizar asignación** o haga clic con el botón derecho en la asignación del plano técnico y seleccione **Actualizar asignación**.

   ![Actualización de la asignación](../media/update-existing-assignments/update-assignment.png)

1. La página **Asignar plano técnico** se cargará con todos los valores rellenados de la asignación original. Puede cambiar la **versión de la definición de plano técnico**, el estado de **Asignación de bloqueo** y cualquiera de los parámetros dinámicos que existan en la definición del plano técnico. Haga clic en **Asignar** cuando haya terminado de realizar los cambios.

1. En la página de detalles de la asignación actualizada, consulte el nuevo estado. En este ejemplo, hemos agregado el **bloqueo** a la asignación.

   ![Asignación actualizada: bloqueada](../media/update-existing-assignments/updated-assignment.png)

1. Explore los detalles acerca de otras **operaciones de asignación** con la lista desplegable. La tabla de **recursos administrados** se actualiza mediante la operación de asignación seleccionada.

   ![Operaciones de asignación](../media/update-existing-assignments/assignment-operations.png)

## <a name="rules-for-updating-assignments"></a>Reglas de la actualización de asignaciones

La implementación de las asignaciones actualizadas sigue unas reglas importantes. Estas reglas determinan lo que le sucede a un recurso existente según el cambio solicitado y el tipo de recurso de artefacto que se va a implementar o actualizar.

- Asignaciones de roles
  - Si cambia el rol o el encargado de rol (usuario, grupo o aplicación), se crea una nueva asignación de roles. La asignación de roles implementada previamente se deja en su lugar.
- Asignaciones de directiva
  - Si cambian los parámetros de la asignación de directiva, se actualiza la asignación existente.
  - Si cambia la definición de la asignación de directiva, se crea una nueva. La asignación de directiva implementada anteriormente se deja en su lugar.
  - Si se quita el artefacto de asignación de directiva del plano técnico, la asignación de directiva implementada anteriormente se deja en su lugar.
- Plantillas del Administrador de recursos de Azure
  - La plantilla se procesa mediante Resource Manager como **PUT**. Como cada tipo de recurso se trata de forma distinta, revise la documentación de cada recurso incluido para determinar el impacto de esta acción cuando la ejecuta Azure Blueprint.

## <a name="possible-errors-on-updating-assignments"></a>Posibles errores en la actualización de asignaciones

Al actualizar las asignaciones, es posible realizar cambios que se interrumpan cuando se ejecutan. Un ejemplo se da al cambiar la ubicación de un grupo de recursos una vez que se haya implementado. Puede realizarse cualquier cambio que [Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md) admita, pero, si este produjera un error con Azure Resource Manager, también lo generaría en la asignación.

No hay ningún límite en el número de veces que una asignación se puede actualizar. Por lo tanto, si se produce un error, ya sea debido a un parámetro incorrecto, un objeto ya existente o un cambio no permitido por Azure Resource Manager, determine el error y actualice otra vez la asignación.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre el [ciclo de vida de los planos técnicos](../concepts/lifecycle.md).
- Descubra cómo utilizar [parámetros estáticos y dinámicos](../concepts/parameters.md)
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](../concepts/sequencing-order.md)
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](../concepts/resource-locking.md)
- Puede consultar la información de [solución de problemas generales](../troubleshoot/general.md) para resolver los problemas durante la asignación de un plano técnico
