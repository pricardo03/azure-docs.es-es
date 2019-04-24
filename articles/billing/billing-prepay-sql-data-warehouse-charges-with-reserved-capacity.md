---
title: Pago por adelantado de los cargos de SQL Data Warehouse con capacidad reservada de Azure | Microsoft Docs
description: Obtenga información sobre cómo puede pagar los cargos de SQL Data Warehouse con capacidad reservada para ahorrar dinero.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 04/13/2019
ms.author: banders
ms.openlocfilehash: 3d56ee99f8056139a234477b6eed93d9fcbe2bbb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60371196"
---
# <a name="prepay-for-sql-data-warehouse-charges-with-reserved-capacity"></a>Pago por adelantado de los cargos de SQL Data Warehouse con capacidad reservada

Puede ahorrar dinero con Azure SQL Data Warehouse por adelantado para el uso de cDWU con una duración de uno o tres años. Para comprar capacidad reservada de SQL Data Warehouse, deberá elegir Azure región y término. A continuación, agregue la SKU de almacenamiento de datos de SQL a su carro de compra y elija la cantidad de unidades de cDWU que desea comprar.

Al comprar una reserva, SQL Data Warehouse ya no se cobra el uso que coincide con los atributos de reserva en el pago a medida que vaya a las tasas.

No tratan una reserva de almacenamiento o los cargos de red asociados con el uso de SQL Data Warehouse.

Cuando expira la capacidad reservada, las instancias de SQL Data Warehouse continúan ejecutándose, pero se facturan según la tarifa vaya paga a medida que. Las reservas no se renuevan automáticamente.

Para obtener más información, consulte el [oferta de capacidad de reserva de SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/gen2/).

Puede comprar capacidad reservada de Azure SQL Data Warehouse en el [portal Azure](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade). Para adquirir capacidad reservada:

- Debe tener el rol de propietario de al menos una de las empresas o de suscripción de pago por uso.
- Para las suscripciones de Enterprise, el **agregar instancias reservadas** opción debe estar habilitada en el [portal EA](https://ea.azure.com/). Si se deshabilita la configuración, debe ser un administrador de EA.
- Para el programa proveedor de soluciones en la nube (CSP), solo los agentes de administración o los agentes de ventas pueden adquirir capacidad reservada de SQL Data Warehouse.

Para obtener más información acerca de cómo los clientes empresariales y los clientes de pago por uso se le cobrará por las compras de reserva, consulte [entender el uso de Azure de reserva para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md) y [comprender de Azure uso de reserva para su suscripción de pago por uso](billing-understand-reserved-instance-usage.md).

## <a name="choose-the-right-size-before-purchase"></a>Elija el tamaño adecuado antes de la compra

El tamaño de reserva debe basarse en el total de SQL Data Warehouse compute unidades de almacenamiento de datos (cDWU) que consumen. Realizar compras en incrementos de 100 cDWU.

Por ejemplo, suponga que el consumo total de SQL Data Warehouse es DW3000c. Que desea comprar una capacidad reservada para todo. Por lo tanto, debe comprar 30 unidades de capacidad de cDWU reservado.

## <a name="buy-sql-data-warehouse-reserved-capacity"></a>Comprar capacidad reservada de SQL Data Warehouse

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/).
2. Seleccione **Todos los servicios** > **Reservations**.
3. Seleccione una suscripción. Use la lista de suscripciones para elegir la suscripción que se usa para pagar la capacidad reservada. El método de pago de la suscripción se cobra a los costos por adelantado por la capacidad reservada. El tipo de suscripción debe ser Contrato Enterprise (números de oferta: MS-AZR-0017P o MS-AZR-0148P) o Pago por uso (números de oferta: MS-AZR-0003P o MS-AZR-0023P).
  - Para una suscripción Enterprise, los cargos se deducen del saldo de compromiso monetario de la inscripción o se cobran como uso por encima del límite.
  - Para la suscripción Pago por uso, los cargos se cobran en el método de pago de tarjeta de crédito o factura de la suscripción.
4. Seleccione un ámbito. Use la lista de ámbitos para elegir un ámbito de la suscripción.
  - Con el **único** opción, el descuento de reserva se aplica a SQL Data Warehouse implementados en la suscripción seleccionada.
  - Con el **Shared** opción, el descuento de reserva se aplica a instancias que se ejecutan en todas las suscripciones en el contexto de facturación.
    - Para los clientes empresariales, el contexto de facturación es la inscripción de EA.
    - Para los clientes de Pago por uso, el ámbito compartido incluye todas las suscripciones de Pago por uso creadas por el administrador de la cuenta.
5. Seleccione una región para elegir una región de Azure que está cubierta por la capacidad reservada.
6. Elija una cantidad. Especifique la cantidad de 100 unidades de almacenamiento de datos (cDWU) que desea comprar.    
  Por ejemplo, una cantidad de 30 le aportara 3.000 cDWU de capacidad reservada cada hora.
7. Revisión de SQL Data Warehouse reservado el costo de reserva capacidad en el **costos** sección.
8. Seleccione **Comprar**.
9. Seleccione **ver esta reserva** para ver el estado de la compra.

## <a name="cancellations-and-exchanges"></a>Cancelaciones e intercambios

Si necesita cancelar su SQL Data Warehouse capacidad reservada, es posible que haya una cuota de finalización anticipada del 12%. Los reembolsos se basan en el precio más bajo de su precio de compra o en el precio actual de la reserva. Reembolsos están limitados a 50,000.00 $ al año. El reembolso que recibe es el balance prorrateado restante, menos la tasa de finalización anticipada del 12%. Para solicitar una cancelación, vaya a la reserva de Azure Portal y seleccione **Reembolso** para crear una solicitud de soporte técnico.

Si necesita cambiar la capacidad reservada de SQL Data Warehouse a otra región o un término, puede cambiarlo por otro reserva de valor igual o mayor. La fecha de inicio del período de la nueva reserva no se extiende desde la reserva intercambiada. El uno o tres años se inicia cuando se crea la reserva de nuevas. Para solicitar un intercambio, abra la reserva en el portal de Azure y seleccione **Exchange** para crear una solicitud de soporte técnico.

Para obtener más información acerca de cómo las reservas de direcciones de exchange o un reembolso, consulte [intercambios de reserva y los reembolsos](billing-azure-reservations-self-service-exchange-and-refund.md).

El descuento de reserva se aplica automáticamente al número de instancias de SQL Data Warehouse que coinciden con el ámbito de capacidad reservada de SQL Data Warehouse y la región. Puede actualizar el ámbito de la capacidad reservada de SQL Data Warehouse con la [portal Azure](https://portal.azure.com/), PowerShell, CLI o la API.

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información acerca de cómo se aplican descuentos de reserva a Azure SQL Data Warehouse, consulte [cómo se aplican descuentos de reserva a Azure SQL Data Warehouse](billing-prepay-sql-data-warehouse-charges-with-reserved-capacity.md).

- Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:
  - [¿Qué es Azure Reservations?](billing-save-compute-costs-reservations.md)
  - [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md)
  - [Información sobre el descuento de Azure Reservations](billing-understand-reservation-charges.md)
  - [Información sobre el uso de reservas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
  - [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
