---
title: Límite de gasto de Azure | Microsoft Docs
description: En este artículo se describe cómo funciona un límite de gasto de Azure y cómo quitarlo.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/28/2019
ms.author: banders
ms.openlocfilehash: 1324b60de05805cd409975358ff78e5b2b27eef1
ms.sourcegitcommit: 8e1fb03a9c3ad0fc3fd4d6c111598aa74e0b9bd4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70114447"
---
# <a name="azure-spending-limit"></a>Límite de gasto de Azure

El límite de gasto de Azure evita los gastos por encima del importe de crédito. Todos los clientes nuevos que se registren para la evaluación de Azure o las ofertas que incluyen créditos para varios meses, tienen activado el límite de gasto de forma predeterminada. El límite de gasto es 0 USD y no se puede cambiar. Por ejemplo, no puede cambiar el límite de gasto a 100 USD, pero sí puede quitarlo. Por lo tanto, no tiene límite o tiene un límite de cero que impide la mayoría de los tipos de gasto. El límite de gasto no está disponible para las suscripciones en algunos planes, como los planes de compromiso y los planes con precios de pago por uso. Vea la [lista completa de ofertas de Azure y la disponibilidad del límite de gasto](https://azure.microsoft.com/support/legal/offer-details/).

## <a name="reaching-a-spending-limit"></a>Alcanzar un límite de gasto

Cuando el uso dé lugar a cobros que superen las cantidades mensuales de la oferta incluidas con su suscripción de Azure, los servicios que implementó se deshabilitan durante el resto de ese período de facturación.

Por ejemplo, cuando gasta todo el crédito incluido con su suscripción, los recursos de Azure que implementó se quitan de la producción y las máquinas virtuales de Azure se detienen y se desasignan. Los datos de las cuentas de almacenamiento están disponibles únicamente en modo de solo lectura.

Al principio del siguiente período de facturación, si su oferta de suscripción incluyera créditos durante varios meses, la suscripción se volvería a habilitar automáticamente. Después, puede volver a implementar los recursos de Azure y tener acceso total a las cuentas de almacenamiento y bases de datos.

Azure le enviará notificaciones por correo electrónico cuando alcance el límite de gasto de su suscripción. Inicie sesión en el [Centro de cuentas](https://account.windowsazure.com/Subscriptions) para ver notificaciones sobre las suscripciones que han alcanzado el límite de gasto.

Si tiene una suscripción de evaluación gratuita y alcanza el límite de gasto, puede actualizar a un plan con precios de [pago por uso](billing-upgrade-azure-subscription.md) para quitar el límite de gasto y habilitar automáticamente la suscripción.

<a id="remove"></a>

## <a name="remove-the-spending-limit-in-account-center"></a>Eliminación del límite de gasto en el Centro de cuentas

Puede quitar el límite de gasto en cualquier momento siempre que haya un método de pago válido asociado a la suscripción de Azure. Para las ofertas que tienen crédito para varios meses, también puede habilitar el límite de gasto al principio del siguiente período de facturación.

Para quitar el límite de gasto, siga estos pasos:

1. Inicie sesión en el [Centro de cuentas](https://account.windowsazure.com/Subscriptions).
1. Seleccione una suscripción. Si la suscripción se ha deshabilitado porque se ha alcanzado el límite de gasto, haga clic en la notificación: **La suscripción alcanzó el límite de gasto y se deshabilitó para evitar que se realicen más cargos**. De lo contrario, haga clic en **Quitar límite de gasto** en el área **ESTADO DE LA SUSCRIPCIÓN**.
1. Seleccione una opción que sea apropiada para usted.

![Selección de una opción para quitar el límite de gasto](./media/billing-spending-limit/remove-spending-limit.PNG)

| Opción | Efecto |
| --- | --- |
| Quitar el límite de gasto indefinidamente | Quita el límite de gasto sin activarlo automáticamente al principio del siguiente período de facturación. |
| Quitar el límite de gasto para el período de facturación actual | Quita el límite de gasto de manera que se active automáticamente al principio del siguiente período de facturación. |

## <a name="why-you-might-want-to-remove-the-spending-limit"></a>¿Por qué querría quitar el límite de gasto?

El límite de gasto podría impedirle implementar o usar ciertos servicios de Microsoft o de terceros. A continuación, se indican las situaciones donde debería quitar el límite de gasto en su suscripción.

-  Planea implementar imágenes de primera entidad como Oracle y servicios como Azure DevOps Services. Esta situación hace que se supere el límite de gasto prácticamente de forma inmediata y que la suscripción se deshabilite.
- Tiene servicios que no quiere que se interrumpan.
- Tiene servicios y recursos con configuraciones como direcciones IP virtuales que no quiere perder. Estas configuraciones se pierden cuando se desasignan los servicios y recursos.

## <a name="turn-on-the-spending-limit-after-removing"></a>Activación del límite de gasto después de quitarlo

Esta característica solo está disponible cuando se ha quitado el límite de gasto indefinidamente. Cámbielo para activarlo automáticamente al principio del siguiente período de facturación.

1. Inicie sesión en el [Centro de cuentas](https://account.windowsazure.com/Subscriptions).
1. Haga clic aquí en el banner amarillo para cambiar la opción de límite de gasto.
1. Elija **Activar el límite de gasto para el siguiente período de facturación \<fecha inicial del período de facturación\>** .

## <a name="custom-spending-limit"></a>Límite de gasto personalizado

Los límites de gasto personalizados no están disponibles.

## <a name="a-spending-limit-doesnt-prevent-all-charges"></a>Un límite de gasto no evita todos los cargos

[Algunos servicios externos publicados en Azure Marketplace](billing-understand-your-azure-marketplace-charges.md) no se pueden usar con los créditos de una suscripción y pueden generar cargos independientes incluso si se ha establecido un límite de gasto. Como ejemplos se pueden citar las licencias de Visual Studio, Azure Active Directory Premium, los planes de soporte técnico y la mayoría de los servicios de marca de terceros. Al aprovisionar un nuevo servicio externo, se muestra una advertencia en la que se le informa de que los servicios se facturan por separado:

![Advertencia de compra de Marketplace](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes
- Actualización a un plan con precios de [pago por uso](billing-upgrade-azure-subscription.md).
