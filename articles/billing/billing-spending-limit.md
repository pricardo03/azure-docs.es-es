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
ms.openlocfilehash: 97e6e04bf7fab3a48f1ce0552e1b653b208be77b
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72375430"
---
# <a name="azure-spending-limit"></a>Límite de gasto de Azure

El límite de gasto de Azure evita los gastos por encima del importe de crédito. Todos los clientes nuevos que se registren para obtener una cuenta nueva de Azure o los tipos de suscripción que incluyan créditos para varios meses tienen activado el límite de gasto de forma predeterminada. El límite de gasto es igual a la cantidad de crédito y no se puede cambiar. Por ejemplo, si se ha registrado para obtener una cuenta gratuita de Azure, el límite de gasto es 200 dólares y no se puede cambiar a 500 dólares, pero sí puede quitarlo. Por consiguiente, no tiene límite o tiene un límite igual a la cantidad de crédito. Así se evita la mayoría de los tipos de gasto. El límite de gasto no está disponible para las suscripciones con planes de compromiso o con precios de pago por uso. Vea la [lista completa de los tipos de suscripción de Azure y la disponibilidad del límite de gasto](https://azure.microsoft.com/support/legal/offer-details/).

## <a name="reaching-a-spending-limit"></a>Alcanzar un límite de gasto

Cuando el uso genere cargos que agoten el límite de gasto, los servicios que ha implementado se deshabilitan durante el resto de ese período de facturación.

Por ejemplo, si gasta todo el crédito de su cuenta gratuita de Azure, los recursos de Azure que ha implementado se quitan de la producción y las máquinas virtuales de Azure se detienen y se desasignan. Los datos de las cuentas de almacenamiento están disponibles únicamente en modo de solo lectura.

Si su tipo de suscripción incluye créditos durante varios meses, la suscripción se vuelve a habilitar automáticamente al principio del siguiente período de facturación. Después, puede volver a implementar los recursos de Azure y tener acceso total a las cuentas de almacenamiento y bases de datos.

Azure envía notificaciones por correo electrónico cuando se alcanza el límite de gasto. Inicie sesión en [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) para ver notificaciones acerca de las suscripciones que han alcanzado el límite de gasto.

Si se ha registrado para obtener una cuenta gratuita de Azure y alcanza el límite de gasto, puede actualizar a un precio de [pago por uso](billing-upgrade-azure-subscription.md) para quitar el límite de gasto y que la suscripción de vuelva a habilitar automáticamente.

<a id="remove"></a>

## <a name="remove-the-spending-limit-in-azure-portal"></a>Eliminación del límite de gasto en Azure Portal

Puede quitar el límite de gasto en cualquier momento siempre que haya un método de pago válido asociado a la suscripción de Azure. En el caso de los tipos de suscripción con crédito en varios meses, como Visual Studio Enterprise y Visual Studio Professional, también puede habilitar el límite de gasto al principio del siguiente período de facturación.

Para quitar el límite de gasto, siga estos pasos:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Seleccione una suscripción. Si se alcanza el límite de gasto de la suscripción, se deshabilitará.
1. Seleccione **Quitar el límite del gasto** en la parte superior de la página.
1. Seleccione una opción que sea apropiada para usted.

![Selección de una opción para quitar el límite de gasto](./media/billing-spending-limit/remove-spending-limit.PNG)

| Opción | Efecto |
| --- | --- |
| Quitar el límite de gasto indefinidamente | Quita el límite de gasto sin activarlo automáticamente al principio del siguiente período de facturación. |
| Quitar el límite de gasto para el período de facturación actual | Quita el límite de gasto de manera que se active automáticamente al principio del siguiente período de facturación. |

## <a name="why-you-might-want-to-remove-the-spending-limit"></a>¿Por qué querría quitar el límite de gasto?

El límite de gasto podría impedirle implementar o usar ciertos servicios de Microsoft o de terceros. A continuación, se indican las situaciones donde debería quitar el límite de gasto en su suscripción.

-  Planea implementar imágenes de primera entidad como Oracle o servicios como Azure DevOps Services. Esta situación hace que alcance su límite de gasto casi inmediatamente y que la suscripción se deshabilite.
- Tiene servicios que no quiere que se interrumpan. Al alcanzar el límite de gasto, los recursos de Azure que implementó se eliminan de producción y las máquinas virtuales de Azure se detienen y se desasignan. Si tiene servicios que no desea que se interrumpan, debe quitar el límite de gasto.
- Tiene servicios y recursos con configuraciones como direcciones IP virtuales que no quiere perder. Estos valores se pierden cuando se alcanza el límite de gasto y se desasignan los servicios y recursos.

## <a name="turn-on-the-spending-limit-after-removing"></a>Activación del límite de gasto después de quitarlo

Esta característica solo está disponible cuando el límite de gasto se ha eliminado de forma indefinida en los tipos de suscripción que incluyen créditos en varios meses. Puede usar esta característica para activar el límite de gasto automáticamente al principio del siguiente período de facturación.

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
