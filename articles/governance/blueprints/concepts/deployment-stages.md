---
title: Etapas de implementación de un plano técnico
description: Obtenga información sobre los pasos que los servicios de Azure Blueprint sigue durante la implementación.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: d7000813b51fb9c9aae9a21cbded3ae0028e83f4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59796640"
---
# <a name="stages-of-a-blueprint-deployment"></a>Etapas de implementación de un plano técnico

Cuando se implementa un plano, una serie de acciones es tomada por el servicio de plano técnico de Azure para implementar los recursos definidos en el proyecto. En este artículo proporciona detalles sobre cada paso implica.

Implementación de la instancia de blueprint se desencadena mediante la asignación de un plano a una suscripción o [actualizando una asignación existente](../how-to/update-existing-assignments.md). Durante la implementación, planos toma los siguientes pasos generales:

> [!div class="checklist"]
> - Planos técnicos le conceden derechos de propietario
> - Se crea el objeto de asignación de plano técnico
> - Opcional: planos crea **asignado por el sistema** identidad administrada
> - Artefactos de plano técnico implementa la identidad administrada
> - Plano de servicio y **asignado por el sistema** se revocan los permisos de identidad administrada

## <a name="blueprints-granted-owner-rights"></a>Planos técnicos le conceden derechos de propietario

La entidad de servicio planos técnicos de Azure se le conceden derechos de propietario a la suscripción asignada o suscripciones. El rol concedido permite planos técnicos crear y, más adelante se revoca el [asignado por el sistema de identidad administrada](../../../active-directory/managed-identities-azure-resources/overview.md).

Los derechos se conceden automáticamente si la asignación se realiza a través del portal. Sin embargo, si la asignación se realiza a través de la API de REST, conceder los derechos debe ser con una API independiente llamar. Azure Blueprint AppId es `f71766dc-90d9-4b7d-bd9d-4499c4331c3f`, pero la entidad de servicio varía según el inquilino. Use [API Graph de Azure Active Directory](../../../active-directory/develop/active-directory-graph-api.md) y punto de conexión REST [servicePrincipals](/graph/api/resources/serviceprincipal) para obtener la entidad de servicio. A continuación, conceda los planos de Azure la _propietario_ rol a través de la [Portal](../../../role-based-access-control/role-assignments-portal.md), [CLI de Azure](../../../role-based-access-control/role-assignments-cli.md), [Azure PowerShell](../../../role-based-access-control/role-assignments-powershell.md), [REST API](../../../role-based-access-control/role-assignments-rest.md), o un [plantilla de Resource Manager](../../../role-based-access-control/role-assignments-template.md).

El servicio de proyectos no implementa directamente los recursos.

## <a name="the-blueprint-assignment-object-is-created"></a>Se crea el objeto de asignación de plano técnico

Un usuario, grupo o entidad de servicio asigna un plano a una suscripción. El objeto de asignación existe en el nivel de suscripción que se asignó la instancia de blueprint. Los recursos creados por la implementación no se realiza en contexto de la entidad de implementación.

Al crear la asignación del plano técnico, el tipo de [identidad administrada](../../../active-directory/managed-identities-azure-resources/overview.md) está seleccionada. El valor predeterminado es un **asignado por el sistema** identidad administrada. Un **asignada por el usuario** se puede elegir la identidad administrada. Cuando se usa un **asignada por el usuario** administra identidades, deben definirse y los permisos concedido antes de crea la asignación del plano técnico.

## <a name="optional---blueprints-creates-system-assigned-managed-identity"></a>Opcional: planos crea asignado por el sistema de identidad administrada

Cuando [asignado por el sistema de identidad administrada](../../../active-directory/managed-identities-azure-resources/overview.md) se selecciona durante la asignación, planos crea la identidad y la identidad administrada se concede el [propietario](../../../role-based-access-control/built-in-roles.md#owner) rol. Si un [asignación existente se actualiza](../how-to/update-existing-assignments.md), planos se utiliza la identidad administrada creada anteriormente.

La identidad administrada relacionados con la asignación del plano técnico se usa para implementar o volver a implementar los recursos definidos en el proyecto. Este diseño evita las asignaciones de forma inadvertida interfieren unas con otras.
Este diseño también admite la [bloqueo de recursos](./resource-locking.md) característica mediante el control de la seguridad de cada recurso implementado desde el proyecto.

## <a name="the-managed-identity-deploys-blueprint-artifacts"></a>Artefactos de plano técnico implementa la identidad administrada

La identidad administrada, a continuación, desencadena las implementaciones de Resource Manager de los artefactos en el proyecto en definido [orden secuenciación](./sequencing-order.md). El orden se puede ajustar para garantizar que los artefactos dependientes de otros artefactos se implementan en el orden correcto.

Un error de acceso de una implementación suele ser el resultado del nivel de acceso concedido a la identidad administrada. El servicio de planos administra el ciclo de vida de la seguridad de la **asignado por el sistema** identidad administrada. Sin embargo, el usuario es responsable de administrar los derechos y ciclo de vida de un **asignada por el usuario** identidad administrada.

## <a name="blueprint-service-and-system-assigned-managed-identity-rights-are-revoked"></a>Servicio de instancia de blueprint y derechos de identidad administrada asignado por el sistema se revocan

Una vez que se completan las implementaciones, planos revoca los derechos de la **asignado por el sistema** identidad administrada de la suscripción. A continuación, el servicio de planos revoca sus derechos de la suscripción. Eliminación de derechos evita planos un propietario permanente en una suscripción.

## <a name="next-steps"></a>Pasos siguientes

- Descubra cómo utilizar [parámetros estáticos y dinámicos](parameters.md).
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](sequencing-order.md).
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](resource-locking.md).
- Aprenda a [actualizar las asignaciones existentes](../how-to/update-existing-assignments.md).
- Puede consultar la información de [solución de problemas generales](../troubleshoot/general.md) para resolver los problemas durante la asignación de un plano técnico.