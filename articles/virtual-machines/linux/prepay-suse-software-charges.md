---
title: Pago por adelantado de planes de software - reservas de Azure | Microsoft Docs
description: Obtenga información sobre cómo puede pagar por adelantado los planes de software ahorrar dinero en los costos de pago por uso.
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2019
ms.author: banders
ms.openlocfilehash: e15dcdbbcaed32d836bb751ef93ce17e90bd6905
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60011102"
---
# <a name="prepay-for-azure-software-plans"></a>Pago por adelantado para planes de software de Azure

Cuando se pagan por adelantado el uso de software SUSE y RedHat en Azure, puede ahorrar dinero a través de los costos de pago por uso. Los descuentos se aplican solo a medidores para SUSE y RedHat y no en el uso de la máquina virtual. Puede comprar reservas para las máquinas virtuales por separado para un ahorro adicional.

Puede comprar planes de software SUSE y RedHat en Azure portal. Para comprar un plan:

- Debe tener el rol de propietario para al menos una suscripción Enterprise o pago por uso.
- Para las suscripciones de Enterprise, el **agregar instancias reservadas** opción debe estar habilitada en el [portal EA](https://ea.azure.com/). Si se deshabilita la configuración, debe ser un administrador de EA para la suscripción.
- Para el programa proveedor de soluciones en la nube (CSP), los agentes de administración o los agentes de ventas pueden comprar los planes de software.

## <a name="buy-a-software-plan"></a>Comprar un plan de software

1. Inicie sesión en Azure portal y vaya a [reservas](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).
2. Haga clic en **agregar** y, a continuación, seleccione el plan de software que desea comprar.
Rellene todos los campos obligatorios. Cualquier máquina virtual Linux de SUSE o RedHat VM que coincide con los atributos de lo que compra Obtiene el descuento. El número real de implementaciones que obtienen el descuento depende del ámbito y la cantidad seleccionada.
3. Seleccione una suscripción. Se utiliza para pagar el plan.
El método de pago de la suscripción se cobra a los costos por adelantado para la reserva. El tipo de suscripción debe ser un contrato Enterprise (números de la oferta: MS-AZR-0017P o MS-AZR-0148P) o Pago por uso (números de oferta: MS-AZR-0003P o MS-AZR-0023P).
    - Para una suscripción Enterprise, los cargos se deducen del saldo de compromiso monetario de la inscripción o se cobran como uso por encima del límite.
    - Para una suscripción de pago por uso, los cargos se facturan en método de pago de tarjeta de crédito o factura de la suscripción.
4. Seleccione un ámbito. El ámbito puede cubrir una o varias suscripciones (ámbito compartido).
    - Suscripción única: el descuento del plan se aplica a la coincidencia de uso de la suscripción.
    - Compartido: se aplica el descuento del plan para encontrar coincidencias de instancias en cualquier suscripción en el contexto de facturación. Para los clientes empresariales, el contexto de facturación es la inscripción e incluye todas las suscripciones en la inscripción. Para los clientes de pago por uso, el contexto de facturación es todas las suscripciones de pago por uso creadas por el Administrador de cuenta.
5. Seleccione un producto para elegir el tamaño de máquina virtual y el tipo de imagen. El descuento se aplica a solo el tamaño de máquina virtual seleccionado.
6. Seleccione un término de un año o tres años.
7. Elija una cantidad, que es el número de prepagados instancias de máquina virtual que se puede obtener el descuento de facturación.
8. Agregar producto al carro de compra, revisión y compra.

El descuento de reserva se aplica automáticamente en el medidor de software que paga por adelantado. Los gastos de proceso de máquina virtual no están cubiertos por el plan. Puede adquirir por separado las reservas de direcciones de la máquina virtual.

## <a name="discount-applies-to-different-suse-vm-sizes"></a>Descuento se aplica a los diferentes tamaños de VM de SUSE

Al igual que las instancias reservadas de máquina virtual, los planes de SUSE Linux ofrecen flexibilidad de tamaño de instancia. El descuento se aplica incluso cuando se implementa una máquina virtual que tenga un tamaño distinto del plan de SUSE que compró. Para obtener más información, consulte [información sobre cómo se aplica el descuento del plan de software](../../billing/billing-understand-suse-reservation-charges.md).

## <a name="redhat-plan-discount"></a>Descuento del plan de RedHat

Los planes están disponibles solo para las máquinas virtuales de Red Hat Enterprise Linux. El descuento no se aplica a las máquinas virtuales de Red Hat Enterprise Linux SAP HANA o Red Hat Enterprise Linux SAP Business Apps máquinas virtuales.

Se aplican descuentos de plan de RedHat solo para el tamaño de máquina virtual que seleccionó en el momento de la compra. RHEL planes no se reembolsarán ni intercambian después de la compra.


## <a name="cancellation-and-exchanges-not-allowed"></a>No se admiten cambios ni cancelaciones

No se puede cancelar o intercambiar un plan de SUSE o RedHat que ha comprado. Compruebe el uso para asegurarse de comprar el plan adecuado. Para que obtener ayuda identificar qué comprar, consulte [información sobre cómo se aplica el descuento del plan de software](../../billing/billing-understand-suse-reservation-charges.md).

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo administrar una reserva, consulte [Administración de reservas de Azure](../../billing/billing-manage-reserved-vm-instance.md).

Para obtener más información, consulte los artículos siguientes:

- [¿Qué es Azure Reservations?](../../billing/billing-save-compute-costs-reservations.md)
- [Administración de reservas de Azure](../../billing/billing-manage-reserved-vm-instance.md)
- [Información sobre cómo se aplica el descuento por la reserva de SUSE](../../billing/billing-understand-suse-reservation-charges.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](../../billing/billing-understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](../../billing/billing-understand-reserved-instance-usage-ea.md)
