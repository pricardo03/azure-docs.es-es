---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/15/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 89586f932ee358664a2869c87ced72594336b404
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76850523"
---
Ahorre en el uso de la unidad de estado sólido (SSD) Premium con capacidad reservada, junto con instancias reservadas de máquina virtual, puede reducir los costos totales de las máquinas virtuales. El descuento de la reserva se aplica automáticamente a los discos coincidentes en el ámbito de reserva seleccionado; no es necesario asignar una reserva a un disco administrado para obtener los descuentos. Los descuentos se aplican cada hora en el uso del disco y no se lleva a cabo ninguna capacidad reservada sin usar. El descuento de la reserva de discos administrados no se aplica a los discos no administrados, discos ultra o consumo de blobs en páginas.

## <a name="determine-your-storage-needs"></a>Determinación de las necesidades de almacenamiento

Antes de adquirir una reserva, debe determinar cuáles son sus necesidades de almacenamiento. Actualmente, la reserva de discos solo está disponible para las SKU de discos SSD Premium. La SKU de un disco SSD Premium determina el tamaño y el rendimiento del disco. Al determinar las necesidades de almacenamiento, no se recomienda pensar en los discos como una capacidad total; no se puede usar una reserva para un disco más grande (por ejemplo, un P40) y usarlo para pagar dos discos más pequeños (P30). Al adquirir una reserva, solo se compra el número total de discos por SKU.

La reserva de discos se realiza por SKU de disco; por lo tanto, el consumo de reserva se basa en la unidad de las SKU de disco en lugar del tamaño proporcionado. Por ejemplo, si ha reservado un disco P40 de capacidad aprovisionada de 2 TiB pero solo tiene asignado dos discos P30, el consumo de los dos P30 no se tendrá en cuenta para la reserva del disco P40 y pagará la tarifa de pago por uso.



[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

## <a name="purchase-considerations"></a>Consideraciones de compra

Se recomiendan los siguientes procedimientos recomendados al considerar la compra de la reserva de discos:

- Analice su información de uso para averiguar qué reservas debe adquirir. Asegúrese de que está realizando el seguimiento del uso en las SKU de disco en lugar de la capacidad de disco aprovisionada o usada. 
- Examine la reserva de discos junto con la reserva de la máquina virtual. Se recomienda encarecidamente que se realice la reserva para el uso de disco y de la máquina virtual para un ahorro máximo. Puede empezar con la determinación de la reserva de la máquina virtual correcta y, a continuación, evaluar la reserva de discos en consecuencia. Por lo general, tendrá una configuración estándar para cada una de las cargas de trabajo; por ejemplo, un servidor SQL Server puede tener dos discos de datos P40 y un disco P30 para el sistema operativo. Este tipo de patrón puede ayudarle a determinar la cantidad de reservas que puede comprar. Este enfoque puede simplificar el proceso de evaluación y asegurarse también de que dispone de un plan alineado para máquinas virtuales y discos en términos de suscripciones, regiones y otros. 

## <a name="purchase-restrictions"></a>Restricciones de compra

Los descuentos de reserva no están disponibles actualmente para los siguientes discos:
- Blobs en páginas o discos no administrados
- Discos SSD o HDD estándar
- Las SKU de un disco SSD Premium menores de P30: las reservas no están disponibles para las SKU de los discos SSD Premium P1/P2/P3/P4/P6/P10/P15/P20.
- Nubes: las reservas no están disponibles para la compra en las regiones de Azure Gov, Alemania o China.
- Restricciones de capacidad: en algunas circunstancias poco frecuentes, Azure limita la compra de nuevas reservas a un subconjunto de SKU de discos debido a que la capacidad en una región es baja.

## <a name="buy-a-disk-reservation"></a>Compra de una reserva de discos

Puede adquirir reservas de discos de Azure mediante [Azure Portal](https://portal.azure.com/). Puede pagar la reserva por adelantado o de manera mensual. Para más información sobre cómo comprar con pagos mensuales, consulte [Compra de reservas con pagos mensuales](../articles/cost-management-billing/reservations/monthly-payments-reservations.md).

Siga estos pasos para adquirir la capacidad reservada:

1. Vaya al panel [Comprar reservas](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/Browse_AddCommand) en Azure Portal.
1. Seleccione **Azure Managed Disks** para comprar una reserva.

    ![disks-reserved-purchase-reservation.png](media/disks-reserved-capacity/disks-reserved-purchase-reservation.png)    

1. Rellene los campos obligatorios tal como se describe en la tabla siguiente:

   |Campo  |Descripción  |
   |---------|---------|
   |**Ámbito**   |  Indica el número de suscripciones que pueden usar la ventaja de facturación asociada con la reserva. También controla cómo se aplica la reserva a suscripciones concretas. <br/><br/> Si selecciona **Compartido**, el descuento de la reserva se aplica a la capacidad de Azure Storage en cualquier suscripción en el contexto de facturación. El contexto de facturación se basa en cómo se haya suscrito a Azure. Para los clientes Enterprise, el ámbito compartido es la inscripción e incluye todas las suscripciones que esta contiene. Para los clientes de pago por uso, el ámbito compartido incluye todas las suscripciones con tarifas de pago por uso creadas por el administrador de la cuenta.  <br/><br/>  Si selecciona **Suscripción única**, el descuento de reserva se aplica a la capacidad de Azure Storage de la suscripción seleccionada. <br/><br/> Si selecciona **Grupo de recursos único**, el descuento de reserva se aplica a la capacidad de Azure Storage de la suscripción seleccionada y al grupo de recursos seleccionado dentro de esa suscripción. <br/><br/> Puede cambiar el ámbito de reserva después de comprar la reserva.  |
   |**Suscripción**  | La suscripción que se usa para pagar la reserva de Azure Storage. El método de pago en la suscripción seleccionada se usa al cargar los costos. La suscripción debe ser uno de los tipos siguientes: <br/><br/>  Contrato Enterprise (números de oferta: MS-AZR-0017P o MS-AZR-0148P): Para una suscripción Enterprise, los cargos se deducen del saldo de compromiso monetario de la inscripción o se cobran como uso por encima del límite. <br/><br/> Suscripción individual con tarifas de pago por uso (números de la oferta: MS-AZR-0003P o MS-AZR-0023P): en una suscripción individual con tarifas de pago por uso, los cargos se cobran en el método de pago de tarjeta de crédito o factura de la suscripción.    |
   | **Discos** | La SKU que quiere crear. |
   | **Región** | La región donde está en vigor la reserva. |
   | **Frecuencia de facturación** | Indica la frecuencia con la que se factura la cuenta para la reserva. Entre las opciones se incluyen *Mensual* o *Por adelantado*. |

    ![premium-ssd-reserved-purchase-selection.png](media/disks-reserved-capacity/premium-ssd-reserved-purchase-selection.png)

1. Después de seleccionar los parámetros para la reserva, Azure Portal muestra el costo. El portal también muestra el porcentaje de descuento en la facturación de pago por uso. Seleccione **Siguiente** para continuar con la hoja **Comprar reservas**.

1. En el panel **Comprar reservas**, puede asignar un nombre a la reserva y seleccionar la cantidad total de reservas que quiere crear. El número de reservas se asigna al número de discos. Por ejemplo, si quiere reservar cien discos, debe cambiar la **cantidad** a 100.
1. Revise el costo total de la reserva.

    ![premium-ssd-reserved-selecting-sku-total-purchase.png](media/disks-reserved-capacity/premium-ssd-reserved-selecting-sku-total-purchase.png)

Después de comprar una reserva, se aplica automáticamente a cualquier recurso de Azure Disk Storage que coincida con los términos de la reserva. Si aún no ha creado ningún recurso de Azure Disk Storage, se aplicará la reserva siempre que cree un recurso que coincida con los términos de la reserva. En cualquier caso, el período de la reserva comienza inmediatamente después de una compra correcta.

## <a name="exchange-or-refund-a-reservation"></a>Intercambio o reembolso de una reserva

Con ciertas limitaciones, puede intercambiar o reembolsar una reserva.

Para intercambiar o reembolsar una reserva, vaya a los detalles de la reserva en Azure Portal. Seleccione **Intercambiar o Reembolsar** y siga las instrucciones para enviar una solicitud de soporte técnico. Una vez procesada la solicitud, Microsoft le enviará un correo electrónico para confirmar que se completó la solicitud.

Para más información sobre las directivas de reservas de Azure, consulte [Autoservicio de cambios y reembolsos de reservas de Azure](../articles/cost-management-billing/reservations/exchange-and-refund-azure-reservations.md).

### <a name="exchange-a-reservation"></a>Intercambio de una reserva

El intercambio de una reserva le permite recibir un reembolso prorrateado en función de la parte no utilizada de la reserva. Después, puede aplicar el reembolso al precio de compra de una nueva reserva de discos de Azure.
No hay límite en el número de intercambios que puede hacer. Además, los intercambios no conllevan ninguna tarifa. La reserva nueva que compre debe ser igual o mayor que el crédito prorrateado de la reserva original. Una reserva de discos de Azure se puede intercambiar solo para otra reserva de discos de Azure y no para una reserva de ningún otro servicio de Azure.

### <a name="refund-a-reservation"></a>Reembolso de una reserva

Puede cancelar una reserva de discos de Azure en cualquier momento. Si lo hace, recibirá un reembolso prorrateado según el período restante de la reserva, menos una cuota de finalización temprana del 12 %. El reembolso máximo por año es de USD 50 000.
Al cancelar una reserva, esta se finaliza inmediatamente y se devuelven los meses restantes a Microsoft. El saldo prorrateado restante, menos la cuota, se reembolsará en la forma de compra original.

## <a name="expiration-of-a-reservation"></a>Expiración de una reserva

Cuando expira una reserva, cualquier capacidad de Azure Disk que esté usando en virtud de esa reserva se factura según la tarifa de pago por uso. Las reservas no se renuevan automáticamente.
Recibirá una notificación por correo electrónico 30 días antes de la expiración de la reserva y nuevamente en la fecha de expiración. Para seguir aprovechando el ahorro de costos que proporciona una reserva, debe renovarla a más tardar en la fecha de expiración.

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es Azure Reservations?](../articles/cost-management-billing/reservations/save-compute-costs-reservations.md)
- [Aplicación del descuento por reserva a Azure Disk Storage](../articles/cost-management-billing/reservations/understand-disk-reservations.md)