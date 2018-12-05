---
title: 'Comprar planes SUSE Linux: Azure Reservations | Microsoft Docs'
description: Obtenga información sobre cómo puede pagar por adelantado su uso de SUSE y ahorrar dinero en los costos de pago por uso.
services: virtual-machines-linux
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2018
ms.author: yashar
ms.openlocfilehash: 44d23cf38eb1dca9ade94c5a8fd0ae495300be44
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582198"
---
# <a name="prepay-for-suse-software-plans-from-azure-reservations"></a>Pago por adelantado para planes de software SUSE con Azure Reservations

Pague por adelantado su uso de SUSE y ahorre dinero en los costos de pago por uso. Los descuentos solo se aplican a los medidores SUSE y no en al uso de la máquina virtual. Puede comprar reservas para máquinas virtuales por separado para ahorrar todavía más.

Puede comprar planes de software SUSE en Azure Portal. Para comprar un plan:

- Debe tener un rol de propietario al menos en suscripción Enterprise o de Pago por uso.
- Para las suscripciones Enterprise, las compras de reserva deben habilitarse en el [portal de EA](https://ea.azure.com).
- En el caso del programa del Proveedor de soluciones en la nube (CSP), solo los agentes de administración o de ventas pueden comprar planes SUSE.

## <a name="buy-a-suse-software-plan"></a>Comprar un plan de software SUSE

1. Desde Azure Portal, vaya a [Reservations](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).
1. Seleccione **Agregar** y, a continuación, SUSE Linux.
1. Rellene todos los campos obligatorios. Cualquier VM SUSE Linux de SUSE que coincida con los atributos de lo que compra obtiene el descuento. El número real de implementaciones que obtienen el descuento depende del ámbito y la cantidad seleccionada.

    | Campo      | DESCRIPCIÓN|
    |:------------|:--------------|
    |NOMBRE        |Nombre de esta compra.|
    |Subscription|Suscripción que se usa para pagar este plan. Los costos anticipados de la reserva se cobran en el método de pago de la suscripción. El tipo de suscripción debe ser un contrato Enterprise (número de la oferta: MS-AZR-0017P) o de Pago por uso (número de la oferta: MS-AZR-0003P). Para una suscripción Enterprise, los cargos se deducen del saldo de compromiso monetario de la inscripción o se cobran como uso por encima del límite. Para la suscripción Pago por uso, los cargos se cobran en el método de pago de tarjeta de crédito o factura de la suscripción.|
    |Ámbito       |El ámbito puede cubrir una o varias suscripciones (ámbito compartido). Si selecciona: <ul><li>Suscripción única: el descuento del plan se aplica al uso de SUSE Linux en esta suscripción. </li><li>Compartido: el descuento del plan se aplica al uso de SUSE Linux en cualquier suscripción dentro del contexto de facturación. Para los clientes de Enterprise, el ámbito compartido es la inscripción e incluye todas las suscripciones (excepto las suscripciones de desarrollo y pruebas) dentro de la inscripción. Para los clientes de Pago por uso, el ámbito compartido incluye todas las suscripciones de Pago por uso creadas por el administrador de la cuenta.</li></ul>|
    |Plan de software     |Seleccione el plan SUSE Linux. Para obtener ayudar para identificar lo que debe comprar, consulte [Understand how the SUSE Linux Enterprise software reservation discount is applied](../../billing/billing-understand-suse-reservation-charges.md) (Información sobre cómo se aplica el descuento de reserva de software de SUSE Linux Enterprise).|
    |Tamaño de VM     |Los precios de SUSE Linux dependen del número de vCPU en la VM. Seleccione la opción que representa el número de vCPU de las VM SUSE Linux.|
    |Término        |Un año o tres años.|
    |Cantidad    |El número de VM para el que compra este plan SUSE Linux. La cantidad es el número de instancias de SUSE Linux en ejecución a las que se aplica el descuento de facturación.|
1. Seleccione **Comprar**.
1. Haga clic en **Ver esta reserva** para conocer el estado de la compra.

El descuento de reserva se aplica automáticamente a las máquinas virtuales SUSE ejecución que coincidan con la reserva. El descuento se aplica solo en el medidor de SUSE. Este plan no cubre los cargos de proceso de VM.

## <a name="discount-applies-to-different-vm-sizes-with-instance-size-flexibility"></a>El descuento se aplica a diferentes tamaños de VM con flexibilidad de tamaño de instancia

Del mismo modo que con las instancias de VM reservadas, los planes SUSE Linux ofrecen flexibilidad de tamaño de instancia. Esto significa que el descuento se aplica incluso cuando se implementa una VM con un tamaño distinto al del plan SUSE que compró. Para obtener más información, consulte [Understand how the SUSE Linux Enterprise software reservation discount is applied](../../billing/billing-understand-suse-reservation-charges.md) (Información sobre cómo se aplica el descuento de reserva de software de SUSE Linux Enterprise).

## <a name="cancellation-and-exchanges-not-allowed"></a>No se admiten cambios ni cancelaciones

Una vez comprado, un plan SUSE no se puede cancelar ni cambiar. Compruebe el uso para asegurarse de comprar el plan adecuado. Para obtener ayudar para identificar lo que debe comprar, consulte [Understand how the SUSE Linux Enterprise software reservation discount is applied](../../billing/billing-understand-suse-reservation-charges.md) (Información sobre cómo se aplica el descuento de reserva de software de SUSE Linux Enterprise).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo administrar una reserva, consulte [Administración de reservas de Azure](../../billing/billing-manage-reserved-vm-instance.md).

Para obtener más información, consulte los artículos siguientes:

- [¿Qué es Azure Reservations?](../../billing/billing-save-compute-costs-reservations.md)
- [Administración de reservas de Azure](../../billing/billing-manage-reserved-vm-instance.md)
- [Información sobre cómo se aplica el descuento por la reserva de SUSE](../../billing/billing-understand-suse-reservation-charges.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](../../billing/billing-understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](../../billing/billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).