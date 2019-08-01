---
title: Cancelación de su suscripción de Azure | Microsoft Docs
description: Describe cómo cancelar la suscripción de Azure, como la suscripción a la evaluación gratuita
author: bandersmsft
manager: amberb
tags: billing
ms.assetid: 3051d6b0-179f-4e3a-bda4-3fee7135eac5
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: banders
ms.openlocfilehash: 8f4279d9ac085cdd1ded0dfdda4fad9d3fe12fb8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66480224"
---
# <a name="cancel-your-subscription-for-azure"></a>Cancelación de la suscripción de Azure

Solo una suscripción de Azure de [Administrador de cuenta](billing-subscription-transfer.md#whoisaa) puede cancelar una suscripción de Azure. Un administrador de suscripciones de Azure también puede [asignar otro usuario como administrador de suscripciones ](billing-add-change-azure-subscription-administrator.md#assign-a-user-as-an-administrator-of-a-subscription) para que pueda cancelar una suscripción. Después de cancelar la suscripción, el acceso a los servicios y recursos de Azure finalizará.

Antes de cancelar la suscripción:

* Realice una copia de seguridad de los datos. Por ejemplo, si está almacenando datos en Azure Storage o SQL, descargue una copia. Si tiene una máquina virtual, guarde una imagen de la misma localmente.
* Cierre los servicios. Vaya a la [página de recursos en el portal de administración](https://ms.portal.azure.com/?flight=1#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2Fresources) y **detenga** todas las máquinas virtuales, aplicaciones u otros servicios en ejecución.
* Considere la posibilidad de migrar los datos. Consulte [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](../azure-resource-manager/resource-group-move-resources.md).
* Debe eliminar todos los recursos y todos los grupos de recursos. Es necesario eliminarlos para poder cancelar una suscripción. Los grupos de recursos deben eliminarse uno por uno. Durante la eliminación del grupo de recursos, debe confirmar la eliminación; para ello, escriba el nombre de este.
* Si tiene algún rol personalizado que haga referencia a esta suscripción en `AssignableScopes`, debe actualizar este rol personalizado para quitar la suscripción. Si intenta actualizar un rol personalizado después de cancelar una suscripción, es posible que obtenga un error. Para más información, consulte [Problemas con roles personalizados](../role-based-access-control/troubleshooting.md#problems-with-custom-roles) y [Roles personalizados en los recursos de Azure](../role-based-access-control/custom-roles.md).

Si cancela un plan de soporte técnico de Azure de pago, se le seguirá facturando mensualmente el resto del período de la suscripción. Para más información, vea [Soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

## <a name="cancel-subscription-using-the-azure-portal"></a>Cancelación de la suscripción a través de Azure Portal

1. Seleccione su suscripción en la [página Suscripciones de Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Seleccione la suscripción que quiere cancelar.
3. Seleccione **Introducción** y, luego, seleccione **Cancelar suscripción**.

    ![Captura de pantalla que muestra el botón Cancelar](./media/billing-how-to-cancel-azure-subscription/cancel_ibiza.png)
3. Siga las indicaciones y finalice la cancelación.

## <a name="what-happens-after-i-cancel-my-subscription"></a>¿Qué ocurre después de cancelar la suscripción?

Una vez que se cancela, la facturación se detiene inmediatamente. Sin embargo, pueden transcurrir hasta 10 minutos hasta que la cancelación se muestre en el portal. Si cancela en medio de un período de facturación, enviamos la factura final en la fecha de factura típica una vez finalizado el período.

Después de cancelar, los servicios se deshabilitan. Esto significa que las máquinas virtuales se desasignan, las direcciones IP temporales se liberan y el almacenamiento es de solo lectura.

Esperamos 90 días antes de eliminar permanentemente los datos, por si necesita acceder a ellos o cambia de opinión. No se le cobrará por conservar los datos. Para más información, consulte el [Microsoft Trust Center: How we manage your data](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409) (Centro de confianza de Microsoft: Administración de los datos).

## <a name="reactivate-subscription"></a>Reactivación de la suscripción

Si cancela su suscripción de pago por uso accidentalmente, puede [volver a activarla en el Centro de cuentas](billing-subscription-become-disable.md).

Si su suscripción no es de pago por uso, para reactivar la suscripción, póngase en contacto con el soporte técnico en los noventa días posteriores a la cancelación.

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).
