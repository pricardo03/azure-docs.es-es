---
title: Comprar planes de Red Hat Enterprise Linux - reservas de Azure | Microsoft Docs
description: Obtenga información sobre cómo puede pagar por adelantado para el uso de Red Hat y ahorrar dinero en los costos de pago por uso.
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
ms.date: 01/18/2019
ms.author: yashar
ms.openlocfilehash: e90241d9da140ca33d1aae8ffc5636f8eb2fbc74
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58653146"
---
# <a name="prepay-for-red-hat-enterprise-linux-plans-from-azure-reservations"></a>Pago por adelantado para Red Hat Enterprise Linux planes de las reservas de Azure

Pago por adelantado para el uso de Red Hat y ahorrar dinero en los costos de pago por uso. Los descuentos solo se aplican a los medidores de Red Hat y no en el uso de la máquina virtual. Puede comprar reservas para máquinas virtuales por separado para ahorrar todavía más.

Puede comprar planes de software de Red Hat en Azure portal. Para comprar un plan:

- Debe tener un rol de propietario al menos en suscripción Enterprise o de Pago por uso.
- En el caso de las suscripciones Enterprise, la opción **Agregar instancias reservadas** debe estar habilitada en el [portal de EA](https://ea.azure.com). O bien, si esa opción está deshabilitada, debe ser un administrador de EA en la suscripción.
- Para el programa proveedor de soluciones en la nube (CSP), los agentes de administración o los agentes de ventas pueden comprar los planes de Red Hat.

## <a name="buy-a-red-hat-software-plan"></a>Comprar un plan de software de Red Hat

1. Desde Azure Portal, vaya a [Reservations](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).
1. Seleccione **agregar** y seleccione Red Hat Linux.
1. Rellene todos los campos obligatorios. Cualquier VM Red Hat Linux que coincide con los atributos de lo que compra Obtiene el descuento. El número real de implementaciones que obtienen el descuento depende del ámbito y la cantidad seleccionada.

    | Campo      | DESCRIPCIÓN|
    |:------------|:--------------|
    |NOMBRE        |Nombre de esta compra.|
    |Subscription|Suscripción que se usa para pagar este plan. Los costos anticipados de la reserva se cobran en el método de pago de la suscripción. El tipo de suscripción debe ser Contrato Enterprise (números de oferta: MS-AZR-0017P o MS-AZR-0148P) o Pago por uso (números de oferta: MS-AZR-0003P o MS-AZR-0023P). Para una suscripción Enterprise, los cargos se deducen del saldo de compromiso monetario de la inscripción o se cobran como uso por encima del límite. Para la suscripción Pago por uso, los cargos se cobran en el método de pago de tarjeta de crédito o factura de la suscripción.|
    |Ámbito       |El ámbito puede cubrir una o varias suscripciones (ámbito compartido). Si selecciona: <ul><li>Suscripción única: el descuento del plan se aplica al uso de Red Hat Linux en esta suscripción. </li><li>Compartido: el descuento del plan se aplica al uso de Red Hat Linux en cualquier suscripción en el contexto de facturación. Para los clientes Enterprise, el ámbito compartido es la inscripción e incluye todas las suscripciones que esta contiene. Para los clientes de Pago por uso, el ámbito compartido incluye todas las suscripciones de Pago por uso creadas por el administrador de la cuenta.</li></ul>|
    |Plan     |Seleccione el plan de Red Hat Linux. Para obtener ayuda en identificar qué hay que comprar, consulte comprenda cómo se aplica el descuento de reserva de software de Red Hat Linux Enterprise.|
    |Tamaño de VM     |Precios de Red Hat Linux depende del número de vCPU en la máquina virtual. Seleccione la opción que representa el número de vCPU en las máquinas virtuales de Red Hat Linux.|
    |Término        |Un año o tres años.|
    |Cantidad    |El número de máquinas virtuales que se va a comprar para este plan de Red Hat Linux. La cantidad es el número de instancias de Red Hat Linux que se pueden obtener el descuento de facturación.|
1. Seleccione **Comprar**.
1. Haga clic en **Ver esta reserva** para conocer el estado de la compra.

El descuento de reserva se aplica automáticamente a cualquier máquina virtual Red Hat ejecución que coincida con la reserva. El descuento se aplica sólo en el medidor de Red Hat. Este plan no cubre los cargos de proceso de VM.

## <a name="discount-applies-to-different-vm-sizes"></a>Descuento se aplica a los diferentes tamaños de VM

Al igual que las instancias reservadas de máquina virtual, Red Hat Linux planes ofrecen flexibilidad de tamaño de instancia. Esto significa que el descuento aplica incluso cuando se implementa una máquina virtual que tenga un tamaño distinto del plan de Red Hat que compró. Para obtener más información, consulte comprenda cómo se aplica el descuento de reserva de software de Red Hat Linux Enterprise.

## <a name="cancellation-and-exchanges-not-allowed"></a>No se admiten cambios ni cancelaciones

No se puede cancelar o intercambiar un plan de Red Hat que ha comprado. Compruebe el uso para asegurarse de comprar el plan adecuado. Para obtener ayuda en identificar qué hay que comprar, consulte comprenda cómo se aplica el descuento de reserva de software de Red Hat Linux Enterprise.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo administrar una reserva, consulte [Administración de reservas de Azure](../../billing/billing-manage-reserved-vm-instance.md).

Para obtener más información, consulte los artículos siguientes:

- [¿Qué es Azure Reservations?](../../billing/billing-save-compute-costs-reservations.md)
- [Administración de reservas en Azure](../../billing/billing-manage-reserved-vm-instance.md)
- Información sobre cómo se aplica el descuento de reserva de Red Hat
- [Información sobre el uso de reservas para suscripciones de pago por uso](../../billing/billing-understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](../../billing/billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si tiene más preguntas, [póngase en contacto con el soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.