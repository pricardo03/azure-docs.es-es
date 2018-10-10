---
title: Descripción del ciclo de vida de una instancia de Azure Blueprint
description: Información acerca del ciclo de vida de un plano técnico y detalles sobre cada fase.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: e0790168a8b9590aaa440a04cd99f26c2ece2818
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991551"
---
# <a name="understand-the-life-cycle-of-an-azure-blueprint"></a>Descripción del ciclo de vida de una instancia de Azure Blueprint

Al igual que muchos otros recursos de Azure, un plano técnico de Azure Blueprint tiene un ciclo de vida típico y natural. Se crean, se implementan y, finalmente, se eliminan cuando ya no son necesarios.
Blueprint admite operaciones con ciclo de vida CRUD (crear, leer, actualizar, eliminar), pero también las supera para proporcionar niveles adicionales de estados que admiten canalizaciones de integración e implementación continuas (CI/CD) para el uso de aquellos que administran su infraestructura mediante código: un elemento clave en DevOps al que se hace referencia como Infraestructura como código (IaC).

Para entender un plano técnico por completo y sus fases, vamos a ver un ciclo de vida estándar:

> [!div class="checklist"]
> - Creación y edición de un plano técnico
> - Publicación de un plano técnico
> - Creación y edición de una nueva versión del plano técnico
> - Publicación de una nueva versión del plano técnico
> - Eliminación de una versión específica del plano técnico
> - Eliminación del plano técnico

## <a name="creating-and-editing-a-blueprint"></a>Creación y edición de un plano técnico

Al crear un plano técnico, agréguele artefactos, guárdelo en un grupo de administración y proporcione un nombre y una versión que sean únicos. En este momento, el plano técnico está en modo **borrador** y aún no se puede asignar. Sin embargo, mientras se encuentre en el modo **borrador** se puede seguir actualizando y modificando.

Un plano técnico en el modo **borrador** que nunca se haya publicado mostrará un icono diferente en la página **Definiciones del plano técnico** al icono de aquellos que se hayan **publicado**. La **versión más reciente** también aparecerá como **Borrador** para estos planos técnicos que no se han publicado nunca.

Cree y edite un plano técnico con [Azure Portal](../create-blueprint-portal.md#create-a-blueprint) o la [API REST](../create-blueprint-rest-api.md#create-a-blueprint).

## <a name="publishing-a-blueprint"></a>Publicación de un plano técnico

Una vez realizados todos los cambios deseados en un plano en modo **borrador**, este se puede **publicar** y pasar a estar disponible para la asignación. La versión del plano técnico con el estado **Publicado** no se puede modificar.
Una vez **publicado**, el plano técnico aparece con un icono que es diferente al de los planos técnicos con el estado **Borrador** y muestra el número de versión que se proporcionó en la columna **Versión más reciente**.

Publique un plano técnico con [Azure Portal](../create-blueprint-portal.md#publish-a-blueprint) o la [API REST](../create-blueprint-rest-api.md#publish-a-blueprint).

## <a name="creating-and-editing-a-new-version-of-the-blueprint"></a>Creación y edición de una nueva versión del plano técnico

Aunque una versión con estado **Publicado** de un plano técnico no se puede alterar, se puede agregar una nueva versión del plano técnico al plano ya existente y modificarla según sea necesario. Para ello, realice cambios en un plano técnico ya existente. Si el plano técnico ya está **publicado**, cuando se guarden estos cambios, estos aparecerán como **Cambios sin publicar** en la lista de definiciones del plano técnico. Al guardar los cambios, se guarda una versión **borrador** del plano técnico.

Edite un plano técnico con [Azure Portal](../create-blueprint-portal.md#edit-a-blueprint).

## <a name="publishing-a-new-version-of-the-blueprint"></a>Publicación de una nueva versión del plano técnico

Al igual que había que **publicar** la primera versión de un plano técnico para asignarlo, todas las versiones posteriores de ese mismo plano técnico se deben **publicar** para poder asignarlas. Si se han realizado **Cambios sin publicar** en un plano técnico, pero aún no se han **publicado**, el botón **Publicar plano técnico** está disponible en la página de edición del plano técnico. Si el botón no está visible, el plano técnico ya se ha **publicado**, pero no tiene ningún **cambio sin publicar**.

> [!NOTE]
> Un único plano técnico puede incluir varias versiones **publicadas** que se pueden asignar cada una a las suscripciones.

Los pasos para publicar un plano técnico con **cambios sin publicar** como una nueva versión de un plano ya existente son los mismos que los pasos para publicar uno nuevo.

## <a name="deleting-a-specific-version-of-the-blueprint"></a>Eliminación de una versión específica del plano técnico

Cada versión de un plano técnico es un objeto único que se puede **publicar** individualmente. Esto también significa que se puede eliminar individualmente. La eliminación de una versión de un plano técnico no tiene ningún efecto en otras versiones de ese plano.

> [!NOTE]
> No es posible eliminar un plano que tiene asignaciones activas. Elimine primero las asignaciones y, después, elimine la versión que desea quitar.

1. Inicie el servicio Azure Blueprint en Azure Portal; para ello, haga clic en **Todos los servicios** y busque y seleccione **Directiva** en el panel izquierdo. En la página **Directiva**, haga clic en **Planos técnicos**.

1. Seleccione **Definiciones del plano técnico** de la página de la izquierda y use las opciones de filtro para buscar el plano técnico del que desea eliminar una versión. Haga clic en él para abrir la página de edición.

1. Haga clic en la pestaña **Versiones publicadas** y busque la versión que desea eliminar.

1. Haga clic con el botón derecho en la versión que quiere eliminar y seleccione **Eliminar esta versión**.

## <a name="deleting-the-blueprint"></a>Eliminación del plano técnico

También se puede eliminar el plano técnico principal. Si se elimina el plano técnico principal también se eliminarán todas las versiones de planos técnicos que incluye, independientemente de si su estado es **Borrador** o **Publicado**. Al igual que en la eliminación de una versión de un plano técnico, eliminar el plano técnico principal no quita las asignaciones existentes de cualquiera de las versiones del plano técnico.

> [!NOTE]
> No es posible eliminar un plano que tiene asignaciones activas. Elimine primero las asignaciones y, después, elimine la versión que desea quitar.

Elimine un plano técnico con [Azure Portal](../create-blueprint-portal.md#delete-a-blueprint) o la [API REST](../create-blueprint-rest-api.md#delete-a-blueprint).

## <a name="assignments"></a>Assignments

Hay varios puntos durante el ciclo de vida en el que se puede asignar un plano técnico a una suscripción.
Una versión del plano técnico se puede asignar a una suscripción siempre y cuando su estado sea **Publicado**. Incluso cuando hay una versión **Borrador** del plano técnico, si hay una o varias versiones del plano técnico en un modo **Publicado**, cada una de ellas está disponible para su asignación. Esto permite que las versiones de un plano se puedan usar y asignar activamente mientras se desarrolla una versión más reciente.

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