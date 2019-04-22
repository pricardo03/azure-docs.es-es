---
title: Intercambios de autoservicio y los reembolsos para las reservas de Azure | Microsoft Docs
description: Obtenga información sobre cómo pueden intercambiar o reembolso reservas de Azure.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/5/2019
ms.author: banders
ms.openlocfilehash: aa1a218fbf0bc7eacac65b50e4ee1f86791e2b3b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59281988"
---
# <a name="self-service-exchanges-and-refunds-for-azure-reservations"></a>Intercambios de autoservicio y los reembolsos para las reservas de Azure

Las reservas de Azure proporcionan flexibilidad para ayudar a satisfacer sus necesidades en constante evolución. Puede intercambiar una reserva de la reserva otra del mismo tipo. También puede reembolsar una reserva, hasta 50.000 USD al año, si ya no lo necesita.

Capacidad de exchange y cancelación de autoservicio no está disponible para los clientes de US Government Enterprise Agreement. Se admiten otros tipos de suscripción de US Government incluidos pago por uso y CSP.

## <a name="exchange-an-existing-reserved-instance"></a>Una instancia reservada existente de Exchange

Puede intercambiar la reserva con tres pasos rápidos en el [portal Azure](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).

1. Seleccione las reservas de direcciones que desea de reembolso y haga clic en **Exchange**.  
    ![Imagen de ejemplo que muestra las reservas de direcciones para devolver](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-return.png)
2. Seleccione el producto de la máquina virtual que desea comprar y escriba una cantidad. Asegúrese de que el nuevo total de compra es mayor que el total de devolución. [Determinar el tamaño adecuado antes de adquirir](../virtual-machines/windows/prepay-reserved-vm-instances.md#determine-the-right-vm-size-before-you-buy).  
    ![Imagen de ejemplo que muestra el producto de la máquina virtual para comprar con un intercambio](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-select-purchase.png)
3. Revise y complete la transacción.  
    ![Imagen de ejemplo que muestra el producto de la máquina virtual para comprar con un intercambio, completar el valor devuelto](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-confirm-exchange.png)

Una reserva de reembolso, vaya a **detalles de la reserva** y haga clic en **reembolso**.

## <a name="how-return-and-exchange-transactions-are-processed"></a>¿Cómo se devuelven y se procesan las transacciones de exchange

En primer lugar, Microsoft cancela la reserva existente y reembolsos de la cantidad proporcional para esa reserva. Si se produce un intercambio, se procesa la compra nuevo. Microsoft procesa reembolsos mediante uno de los métodos siguientes, según el tipo de cuenta y el método de pago:

### <a name="enterprise-agreement-customers"></a>Clientes con contrato Enterprise

Dinero se agrega para el compromiso monetario para los intercambios y reembolsos si se realizó la compra original con uno. Se usa ninguna factura por encima del límite, ya que las compras originales se vuelve a abrir y rerated para asegurarse de que el compromiso monetario. Si ya no está activo se compró la reserva de uso de término de compromiso monetario, crédito se agrega a su plazo de compromiso monetario del contrato enterprise actual.

Si se realizó la compra original como un exceso, Microsoft publica una nota de crédito.

### <a name="pay-as-you-go-invoice-payment-customers-and-cloud-solution-provider-program"></a>Los clientes de pago de factura de pago por uso y el programa de proveedor de soluciones en la nube

Se ha cancelado la factura de compra de reserva original y, a continuación, se crea una nueva factura para obtener el reembolso. Para los intercambios, muestra la nueva factura el reembolso y la compra nuevo. La cantidad de reembolso se ajusta con la compra. Si solo se devolverá una reserva, la cantidad prorrateada permanece con Microsoft y se ajusta con una compra de reserva futuras.

### <a name="pay-as-you-go-credit-card-customers"></a>Clientes de tarjeta de crédito de pago por uso

Se ha cancelado la factura original y se crea una nueva factura. Se devuelve el dinero a la tarjeta de crédito que se usó para la compra original. Si ha cambiado la tarjeta, [póngase en contacto con soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="exchange-policies"></a>Directivas de Exchange

- Puede devolver varias de las reservas existentes para comprar una reserva nueva del mismo tipo. No se puede intercambiar las reservas de un tipo por otro. Por ejemplo, no puede devolver una reserva de la máquina virtual para comprar una reserva de SQL.
- Solo los propietarios de reserva pueden procesar un intercambio. [Obtenga información sobre cómo agregar o cambiar los usuarios pueden administrar una reserva](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance#add-or-change-users-who-can-manage-a-reservation).
- Un intercambio se procesa como un reembolso y recompra: distintas transacciones se crean para la cancelación y la compra nuevo. El importe de reserva prorrateado se devuelve para las reservas se permuta. Se le cobrará por completo para la compra nuevo. La cantidad de reserva prorrateado es el valor residual prorrateo diario de la reserva que se devuelve.
- Puede intercambiar o reembolso reservas incluso si usa el contrato enterprise para comprar la reserva expira y se ha renovado como un nuevo contrato.
- Puede cambiar cualquier propiedad de reserva, como el tamaño, región, quantity y término con un intercambio.
- El nuevo total de compra debe igual o ser mayor que la cantidad devuelta.
- La reserva de nuevas adquirida como parte de exchange tiene un nuevo término a partir de la hora de exchange.
- Hay ninguna penalización o límites anuales para los intercambios.

## <a name="refund-policies"></a>Directivas de reembolso

- La cantidad total de reembolso no puede superar los 50.000 USD en una ventana con desplazamiento 12 meses.
- Solo los propietarios de reserva pueden procesar un reembolso. [Obtenga información sobre cómo agregar o cambiar los usuarios pueden administrar una reserva](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
- Microsoft reserva el derecho a cobrar una reducción del 12% para las devoluciones, aunque actualmente no se le cobrará la penalización.

## <a name="exchange-a-non-premium-storage-vm-reservation-for-a-premium-storage-reservation"></a>Intercambiar una reserva de la máquina virtual para una reserva de almacenamiento premium de sin premium storage

Puede intercambiar una reserva de compra para un tamaño de máquina virtual que no es compatible con premium storage para un tamaño de máquina virtual correspondiente que hace. Por ejemplo, un _F1_ para un _F1s_. Para hacer el intercambio, vaya a detalles de la reserva y haga clic en **Exchange**. El intercambio no restablece el término de la instancia reservada o cree una nueva transacción.

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre cómo administrar una reserva, consulte [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md).
- Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:
    - [¿Qué es Azure Reservations?](billing-save-compute-costs-reservations.md)
    - [Administración de reservas en Azure](billing-manage-reserved-vm-instance.md)
    - [Información sobre cómo se aplica el descuento por la reserva](billing-understand-vm-reservation-charges.md)
    - [Información sobre el uso de reservas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
    - [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
    - [Costos de software de Windows no incluidos con reservas](billing-reserved-instance-windows-software-costs.md)
    - [Azure Reservations en el programa del Proveedor de soluciones en la nube (CSP) del Centro de partners](/partner-center/azure-reservations)
