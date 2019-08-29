---
title: Solución de errores al mover los recursos de Azure a una nueva suscripción o grupo de recursos
description: Use Azure Resource Manager para trasladar recursos a un nuevo grupo de recursos o a una nueva suscripción.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/19/2019
ms.author: tomfitz
ms.openlocfilehash: 445ee2784a74a366089a49a0e2f2f17d51ef93bf
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2019
ms.locfileid: "69624296"
---
# <a name="troubleshoot-moving-azure-resources-to-new-resource-group-or-subscription"></a>Solución de problemas al mover los recursos de Azure a una nueva suscripción o grupo de recursos

Este artículo contiene sugerencias que le ayudarán a resolver los problemas que pueden surgir al mover recursos.

## <a name="upgrade-a-subscription"></a>Actualización de una suscripción

Si quiere actualizar la suscripción de Azure (por ejemplo, para cambiar de gratuita a de pago por uso) deberá convertir su suscripción.

* Para actualizar a una evaluación gratuita, consulte [Actualización de la suscripción de Microsoft Imagine Azure o la prueba gratuita al plan de pago por uso](../billing/billing-upgrade-azure-subscription.md).
* Para cambiar a una cuenta de pago por uso, consulte [Cambie la suscripción de pago por uso de Azure a otra oferta](../billing/billing-how-to-switch-azure-offer.md).

Si no puede convertir la suscripción, [cree una solicitud de soporte técnico de Azure](../azure-supportability/how-to-create-azure-support-request.md). Seleccione **Administración de suscripciones** para el tipo de problema.

## <a name="service-limitations"></a>Limitaciones del servicio

Algunos servicios requieren otras consideraciones a la hora de mover recursos. Si va a mover los siguientes servicios, no olvide consultar las guías y las limitaciones.

* [App Services](./move-limitations/app-service-move-limitations.md)
* [Azure DevOps Services](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
* [Modelo de implementación clásica](./move-limitations/classic-model-move-limitations.md)
* [Redes](./move-limitations/networking-move-limitations.md)
* [Recovery Services](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
* [Máquinas virtuales](./move-limitations/virtual-machines-move-limitations.md)

## <a name="large-requests"></a>Solicitudes grandes

Cuando sea posible, divida las operaciones de movimiento grandes en varias operaciones de movimiento independientes. Resource Manager inmediatamente devolverá un error si hay más de 800 recursos en una sola operación. No obstante, también se puede producir un error por agotamiento del tiempo de espera al mover menos de 800 recursos.

## <a name="resource-not-in-succeeded-state"></a>El recurso no está en el estado correcto

Si recibe un mensaje de error que indica que no se puede mover un recurso porque no se encuentra en un estado correcto, es posible que en realidad sea un recurso dependiente que esté bloqueando el movimiento. Consulte [Estado de los recursos dependientes](./move-limitations/networking-move-limitations.md#state-of-dependent-resources).

## <a name="next-steps"></a>Pasos siguientes

Para ver los comandos para trasladar recursos, vea [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](resource-group-move-resources.md).
