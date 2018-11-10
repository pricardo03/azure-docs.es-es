---
title: Descripción del ciclo de vida de una instancia de Azure Blueprint
description: Información acerca del ciclo de vida de un plano técnico y detalles sobre cada fase.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 882279019a5f321c6af9beab1f4d0f220781bc5c
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50094166"
---
# <a name="understand-the-life-cycle-of-an-azure-blueprint"></a>Descripción del ciclo de vida de una instancia de Azure Blueprint

Al igual que muchos otros recursos de Azure, un plano técnico de Azure Blueprint tiene un ciclo de vida típico y natural. Se crean, se implementan y, finalmente, se eliminan cuando ya no son necesarios.
Blueprints admite operaciones de ciclo de vida estándar. A continuación, se basa en ellas para proporcionar niveles adicionales de estado que admiten una integración continua y canalizaciones de implementación continua comunes para las organizaciones que administran su infraestructura como código, un elemento clave de DevOps.

Para entender un plano técnico por completo y sus fases, vamos a ver un ciclo de vida estándar:

> [!div class="checklist"]
> - Creación y edición de un plano técnico
> - Publicación de un plano técnico
> - Creación y edición de una nueva versión del plano técnico
> - Publicación de una nueva versión del plano técnico
> - Eliminación de una versión específica del plano técnico
> - Eliminación del plano técnico

## <a name="creating-and-editing-a-blueprint"></a>Creación y edición de un plano técnico

Al crear un plano técnico, agréguele artefactos, guárdelo en un grupo de administración y proporcione un nombre y una versión que sean únicos. En este momento, el plano técnico está en modo **borrador** y aún no se puede asignar.
Sin embargo, mientras se encuentre en el modo **borrador** se puede seguir actualizando y modificando.

Un plano técnico que no se haya publicado nunca en el modo **borrador** muestra un icono diferente en la página **Definiciones del plano técnico** del icono de aquellos que se hayan **publicado**. La **versión más reciente** también aparece como **borrador** para estos planos técnicos que no se han publicado nunca.

Cree y edite un plano técnico con [Azure Portal](../create-blueprint-portal.md#create-a-blueprint) o la [API REST](../create-blueprint-rest-api.md#create-a-blueprint).

## <a name="publishing-a-blueprint"></a>Publicación de un plano técnico

Una vez realizados todos los cambios deseados en un plano técnico en modo **borrador**, este se puede **publicar** y pasar a estar disponible para la asignación. La versión del plano técnico con el estado **Publicado** no se puede modificar.
Una vez **publicado**, el plano técnico aparece con un icono que es diferente al de los planos técnicos con el estado **Borrador** y muestra el número de versión que se proporcionó en la columna **Versión más reciente**.

Publique un plano técnico con [Azure Portal](../create-blueprint-portal.md#publish-a-blueprint) o la [API REST](../create-blueprint-rest-api.md#publish-a-blueprint).

## <a name="creating-and-editing-a-new-version-of-the-blueprint"></a>Creación y edición de una nueva versión del plano técnico

Una versión del plano técnico con el estado **publicado** no se puede modificar. Sin embargo, una nueva versión del plano técnico se puede agregar al plano ya existente y modificarse según sea necesario. Para hacer cambios en un plano técnico existente, edítelo. Cuando se guardan los cambios nuevos, el plano técnico tiene el estado **cambios sin publicar**. Estos cambios constituyen una nueva versión de **borrador** del plano técnico.

Edite un plano técnico con [Azure Portal](../create-blueprint-portal.md#edit-a-blueprint).

## <a name="publishing-a-new-version-of-the-blueprint"></a>Publicación de una nueva versión del plano técnico

Cada versión editada de un plano técnico debe ser **publicada** para que se pueda asignar. Si se han realizado **cambios sin publicar** en un plano técnico, pero aún no se han **publicado**, el botón **Publicar plano técnico** está disponible en la página de edición del plano técnico. Si el botón no está visible, el plano técnico ya se ha **publicado** y no tiene ningún **cambio sin publicar**.

> [!NOTE]
> Un único plano técnico puede incluir varias versiones **publicadas** que se pueden asignar cada una a las suscripciones.

Para publicar un plano técnico con **cambios sin publicar**, siga los mismos pasos que para publicar otro plano técnico.

## <a name="deleting-a-specific-version-of-the-blueprint"></a>Eliminación de una versión específica del plano técnico

Cada versión de un plano técnico es un objeto único que se puede **publicar** individualmente. Por tanto, cada versión del plano técnico también se puede eliminar. La eliminación de una versión de un plano técnico no tiene ningún efecto en otras versiones de ese plano.

> [!NOTE]
> No es posible eliminar un plano que tiene asignaciones activas. Elimine primero las asignaciones y, después, elimine la versión que desea quitar.

1. Haga clic en **Todos los servicios** y busque y seleccione **Directiva** en el panel izquierdo. En la página **Directiva**, haga clic en **Planos técnicos**.

1. Seleccione **Definiciones del plano técnico** de la página de la izquierda y use las opciones de filtro para buscar el plano técnico del que desea eliminar una versión. Haga clic en él para abrir la página de edición.

1. Haga clic en la pestaña **Versiones publicadas** y busque la versión que desea eliminar.

1. Haga clic con el botón derecho en la versión que quiere eliminar y seleccione **Eliminar esta versión**.

## <a name="deleting-the-blueprint"></a>Eliminación del plano técnico

También se puede eliminar el plano técnico principal. Si se elimina el plano técnico principal, también se eliminan todas las versiones de planos técnicos que incluye, tanto si son planos técnicos en **borrador** como si son **publicados**. Al igual que en la eliminación de una versión de un plano técnico, eliminar el plano técnico principal no quita las asignaciones existentes de cualquiera de las versiones del plano técnico.

> [!NOTE]
> No es posible eliminar un plano que tiene asignaciones activas. Elimine primero las asignaciones y, después, elimine la versión que desea quitar.

Elimine un plano técnico con [Azure Portal](../create-blueprint-portal.md#delete-a-blueprint) o la [API REST](../create-blueprint-rest-api.md#delete-a-blueprint).

## <a name="assignments"></a>Assignments

Hay varios puntos durante el ciclo de vida en los que un plano técnico se puede asignar a una suscripción. Una versión del plano técnico se puede asignar a una suscripción siempre y cuando su estado sea **publicado**. Este ciclo de vida permite que las versiones de un plano técnico se puedan usar y asignar activamente mientras se desarrolla una versión más reciente.

A medida que se asignan las versiones de los planos técnicos, es importante comprender dónde se asignan y con qué parámetros se realiza la asignación. Los parámetros pueden ser estáticos o dinámicos. Para más información, consulte [parámetros estáticos y dinámicos](parameters.md).

### <a name="updating-assignments"></a>Actualización de asignaciones

Cuando se asigna un plano técnico, se puede actualizar la asignación. Hay varias razones para actualizar una asignación, entre las que se incluyen:

- Agregar o quitar un [bloqueo de recursos](resource-locking.md)
- Cambiar el valor de [parámetros dinámicos](parameters.md#dynamic-parameters)
- Actualizar la asignación por una versión **publicada** más reciente del plano técnico

Para más información, consulte [actualización de las asignaciones existentes](../how-to/update-existing-assignments.md).

## <a name="next-steps"></a>Pasos siguientes

- Descubra cómo utilizar [parámetros estáticos y dinámicos](parameters.md).
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](sequencing-order.md).
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](resource-locking.md).
- Aprenda a [actualizar las asignaciones existentes](../how-to/update-existing-assignments.md).
- Puede consultar la información de [solución de problemas generales](../troubleshoot/general.md) para resolver los problemas durante la asignación de un plano técnico.