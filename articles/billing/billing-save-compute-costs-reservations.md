---
title: ¿Qué es Azure Reservations?
description: Obtenga información sobre Azure Reservations y precios para ahorrar en máquinas virtuales, SQL Database, Azure Cosmos DB y otros costos de recursos.
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: banders
ms.openlocfilehash: 2e595fbee90b710ec6b8090a770d93e688a04818
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68839982"
---
# <a name="what-are-azure-reservations"></a>¿Qué es Azure Reservations?

Con Azure Reservations ahorrará gastos al pagar por adelantado por un año o tres años de máquinas virtuales, capacidad de proceso de SQL Database, rendimiento de Azure Cosmos DB u otros recursos de Azure. El pago adelantado le permite obtener un descuento en los recursos que utiliza. Reservations puede reducir considerablemente los costos en máquinas virtuales, capacidad de proceso de SQL Database, Azure Cosmos DB u otros recursos en hasta un 72 % en precios de pago por uso. Reservations ofrece un descuento en la facturación y no afecta al estado del entorno de ejecución de los recursos.

Puede comprar una reserva en [Azure Portal](https://ms.portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).

## <a name="why-buy-a-reservation"></a>¿Por qué comprar una reserva?

Si tiene máquinas virtuales, Azure Cosmos DB o SQL Database que se ejecuten durante largos períodos, la compra de una reserva le proporcionará la opción más rentable. Por ejemplo, si ejecuta continuamente cuatro instancias de un servicio sin una reserva, se le cobra según las tarifas de pago por uso. Cuando compre una reserva para esos recursos, obtendrá inmediatamente el descuento por reserva. Ya no se aplicarán a los recursos las tarifas de pago por uso.

## <a name="charges-covered-by-reservation"></a>Cargos cubiertos por la reserva

Planes de servicio:

- **Instancia reservada de máquina virtual**: una reserva solo cubre el costo de proceso de máquina virtual. No cubre cargos por software adicional, administración de redes ni almacenamiento.
- **Capacidad reservada de Azure Cosmos DB**: una reserva abarca el rendimiento aprovisionado de los recursos. No cubre los cargos de almacenamiento y redes.
- **Núcleo virtual reservado de SQL Database**: solo se incluyen con una reserva los costos de proceso. La licencia se factura por separado.

Para las máquinas virtuales de Windows y SQL Database, puede cubrir los costos de licencia de Windows con la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>¿Quién puede comprar una reserva?

Para comprar un plan, debe tener un rol de propietario de la suscripción en una suscripción Enterprise (MS-AZR-0017P o MS-AZR-0148P) o Pago por uso (MS-AZR-003P o MS-AZR-0023P). Los proveedores de soluciones en la nube pueden usar Azure Portal o el  [Centro de partners](/partner-center/azure-reservations) para comprar reservas de Azure.

Los clientes de Contrato Enterprise (EA) pueden limitar sus compras a los administradores de EA mediante la desactivación de la opción **Add Reserved Instances** (Agregar instancias reservadas) en EA Portal. Para poder comprar una reserva, los administradores de Contrato Enterprise deben ser propietarios de la suscripción para al menos una suscripción de Contrato Enterprise. La opción es útil para empresas que requieren un equipo centralizado para comprar reservas para distintos centros de coste. Después de la compra, los equipos centralizados pueden agregar propietarios de centros de coste a las reservas. Los propietarios pueden entonces ampliar la reserva a sus suscripciones. El equipo central no necesita tener acceso al propietario de la suscripción donde se compra la reserva.

Un descuento por reserva se aplica solo a los recursos asociados con las suscripciones compradas a través de Enterprise, Proveedor de soluciones en la nube (CSP) y planes individuales con tarifas de pago por uso.

## <a name="scope-reservations"></a>Ámbito de las reservas

Puede limitar el ámbito de una reserva a una suscripción o a grupos de recursos. Al limitar el ámbito de una reserva, se selecciona el lugar en el que se aplican los ahorros de la reserva. Al limitar el ámbito de la reserva a un grupo de recursos, los descuentos por reserva solo se aplican al grupo de recursos, no a la suscripción completa.

### <a name="reservation-scoping-options"></a>Opciones de limitación del ámbito de la reserva

Con la limitación del ámbito del grupo de recursos tiene tres opciones para limitar el ámbito de una reserva, según sus necesidades:

- **Single resource group scope** (Ámbito de grupo de recursos único): aplica el descuento por reserva a los recursos coincidentes solo en el grupo de recursos seleccionado.
- **Single subscription scope** (Ámbito de suscripción única): aplica el descuento por reserva a los recursos coincidentes de la suscripción seleccionada.
- **Ámbito compartido**: aplica el descuento por reserva a los recursos coincidentes en suscripciones aptas que están en el contexto de facturación. Para los clientes con Contrato Enterprise, el contexto de facturación es la inscripción. En el caso de suscripciones individuales con tarifas de pago por uso, el ámbito de facturación son todas las suscripciones aptas creadas por el administrador de la cuenta.

Al aplicar los descuentos por reserva sobre su uso, Azure procesa la reserva en el orden siguiente:

1. Reservas cuyo ámbito es un grupo de recursos
2. Reservas de ámbito único
3. Reservas de ámbito compartido

Un único grupo de recursos puede obtener descuentos por reserva de varias reservas, en función de cómo se establezca el ámbito de las reservas.

### <a name="scope-a-reservation-to-a-resource-group"></a>Limitación del ámbito de una reserva a un grupo de recursos

Puede limitar el ámbito de la reserva a un grupo de recursos al comprar la reserva o bien hacerlo después. Para limitar el ámbito de la reserva a un grupo de recursos, debe ser el propietario de la suscripción.

Para establecer el ámbito, vaya a la página [Comprar reservas](https://ms.portal.azure.com/#blade/Microsoft\_Azure\_Reservations/CreateBlade/referrer/Browse\_AddCommand) en Azure Portal. Seleccione el tipo de reserva que quiere comprar. En el formulario de selección **Seleccione el producto que quiere comprar**, cambie el valor de Ámbito a Grupo de recursos único. Después, seleccione un grupo de recursos.

![Ejemplo que muestra la selección de compra de reservas de máquina virtual](./media/billing-save-compute-costs-reservations/select-product-to-purchase.png)

Se muestran las recomendaciones de compra del grupo de recursos en la reserva de máquina virtual. Las recomendaciones se calculan mediante el análisis del uso durante los últimos 30 días. Si el costo de la ejecución de recursos con instancias reservadas es más barato que el costo de la ejecución de recursos con tarifas de pago por uso, se realiza una recomendación de compra. Para más información sobre las recomendaciones de compra de reservas, consulte la entrada de blog [Get Reserved Instance purchase recommendations based on usage pattern](https://azure.microsoft.com/blog/get-usage-based-reserved-instance-recommendations) (Obtener recomendaciones de compra de instancias reservadas basadas en el patrón de uso).

También puede actualizar el ámbito después de comprar una reserva. Para ello, vaya a la reserva, haga clic en **Configuración** y limite de nuevo el ámbito de la reserva. Volver a limitar el ámbito de una reserva no es una transacción comercial. No se cambian las condiciones de la reserva. Para más información sobre cómo actualizar el ámbito, consulte [Update the scope after you purchase a reservation](billing-manage-reserved-vm-instance.md#change-the-reservation-scope) (Actualización del ámbito después de comprar una reserva).

![Ejemplo que muestra un cambio en el ámbito de la reserva](./media/billing-save-compute-costs-reservations/rescope-reservation-resource-group.png)

### <a name="monitor-and-optimize-reservation-usage"></a>Supervisión y optimización del uso de reservas

Puede supervisar el uso de la reserva de varias maneras: mediante Azure Portal, las API o los datos de uso. Para ver todas las reservas a las que tiene acceso, vaya a **Reservas** en Azure Portal. La cuadrícula de reservas muestra el último porcentaje de uso registrado para la reserva. Haga clic en la reserva para ver su uso a largo plazo.

También puede conocer el uso de la reserva mediante las [API](billing-reservation-apis.md#see-reservation-usage) y a partir de los [datos de uso](billing-understand-reserved-instance-usage-ea.md#common-cost-and-usage-tasks) si es un cliente con contrato Enterprise.

Si observa que el uso de la reserva con ámbito de grupo de recursos es bajo, puede actualizar el ámbito de la reserva a una suscripción única o compartirlo en el contexto de facturación. También puede dividir la reserva y aplicar las reservas resultantes a distintos grupos de recursos.

### <a name="other-considerations"></a>Otras consideraciones

Si no tiene recursos que coincidan en un grupo de recursos, la reserva estará infrautilizada. La reserva no se aplica automáticamente a un grupo de recursos o una suscripción diferentes en los que haya un uso bajo.

Un ámbito de reserva no se actualiza automáticamente si se mueve el grupo de recursos de una suscripción a otra. El ámbito no se actualiza si se elimina el grupo de recursos. Tendrá que [volver a limitar el ámbito de la reserva](billing-manage-reserved-vm-instance.md#change-the-reservation-scope). Si no, la reserva estará infrautilizada.

## <a name="discounted-subscription-and-offer-types"></a>Tipos de ofertas y suscripciones con descuento

Los descuentos por reserva se aplican a las siguientes suscripciones aptas y tipos de ofertas.

- Contrato Enterprise (números de oferta: MS-AZR-0017P o MS-AZR-0148P)
- Planes individuales con tarifas de pago por uso (números de oferta: MS-AZR-0003P o MS-AZR-0023P)
- Suscripciones de CSP

Los recursos que se ejecutan en una suscripción con otros tipos de oferta no recibirán el descuento por la reserva.

## <a name="how-is-a-reservation-billed"></a>¿Cómo se factura una reserva?

La reserva se cobra en el método de pago vinculado a la suscripción. Si tiene una suscripción Enterprise, el costo de la reserva se deducirá del saldo del compromiso monetario. Cuando este no sea suficiente para cubrir el costo de la reserva, se le cobrará el monto restante. Si tiene una suscripción de un plan individual con tarifas de pago por uso, se factura inmediatamente en la tarjeta de crédito que tiene en su cuenta. Cuando se le facture por factura, verá los cargos la próxima vez que reciba una.

## <a name="how-reservation-discount-is-applied"></a>Aplicación del descuento por reserva

El descuento por reserva se aplica a la utilización de recursos que coincide con los atributos seleccionados al comprar la reserva. Los atributos incluyen el ámbito en el que se ejecutan las máquinas virtuales, SQL Database, Azure Cosmos DB u otras ejecuciones de recursos. Por ejemplo, si quiere un descuento por reserva para las cuatro máquinas virtuales Estándar D2 en la región Oeste de EE. UU., seleccione la suscripción en la que se ejecutan las máquinas virtuales.

Un descuento de reserva significa "*usarlo o perderlo*". Si no tiene recursos coincidentes para ninguna hora, perderá una cantidad de reserva para esa hora. No se pueden arrastrar las horas reservadas no utilizadas.

Al cerrar un recurso, el descuento por reserva se aplica automáticamente a otro recurso que coincida con el ámbito especificado. Si no se encuentran recursos coincidentes en el ámbito especificado, entonces las horas reservadas se *pierden*.

Por ejemplo, más adelante podría crear un recurso y tener una reserva que coincida con la que está infrautilizada. El descuento por reserva se aplica automáticamente al nuevo recurso coincidente.

Si, por el contrario, sus máquinas virtuales se ejecutan en diferentes suscripciones dentro de su cuenta o inscripción, seleccione el ámbito compartido. El ámbito compartido le permite aplicar el descuento de la reserva en varias suscripciones. También puede cambiar el ámbito después de la compra de la reserva. Para más información, consulte [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md).

Un descuento por reserva solo se aplica a los recursos asociados con Enterprise, CSP o suscripciones con tarifas de pago por uso. Los recursos que se ejecutan en una suscripción con otros tipos de oferta no recibirán el descuento por la reserva.

## <a name="when-the-reservation-term-expires"></a>Cuando expira el plazo de reserva

Al final del plazo de reserva, el descuento de facturación expira. La máquina virtual, la base de datos SQL y Azure Cosmos DB u otros recursos se factura según el precio de pago por uso. Azure Reservations no se renueva de manera automática. Para seguir disfrutando del descuento de facturación, debe comprar una reserva nueva para los servicios y el software aplicables.

## <a name="discount-applies-to-different-sizes"></a>El descuento se aplica a los diferentes tamaños

Al comprar una reserva, puede aplicar el descuento a otras instancias con atributos que están dentro del mismo grupo de tamaño. Esta característica se conoce como flexibilidad de tamaño de instancia. La flexibilidad de la cobertura del descuento depende del tipo de reserva y los atributos que elija al comprar la reserva.

Planes de servicio:

- Instancias reservadas de máquina virtual: al comprar la reserva y seleccionar **Optimizado para: flexibilidad de tamaño de instancia**, la cobertura de descuento dependerá del tamaño de máquina virtual que elija. La reserva se puede aplicar a los tamaños de máquinas virtuales (VM) en el mismo grupo de series de tamaño. Si quiere saber más, vea [Flexibilidad en el tamaño de las máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).
- Capacidad reservada de SQL Database: la cobertura del descuento depende del nivel de rendimiento elegido. Para más información, vea [Aprenda cómo se aplica un descuento en la reserva de Azure a las instancias de SQL Database](billing-understand-reservation-charges.md).
- Capacidad reservada de Azure Cosmos DB: la cobertura del descuento depende del rendimiento aprovisionado. Para más información, vea [Aplicación del descuento por reserva a Azure Cosmos DB](billing-understand-cosmosdb-reservation-charges.md).

## <a name="reservation-notifications"></a>Notificaciones de reserva

En función de cómo paga la suscripción de Azure, enviaremos notificaciones de reservas por correo electrónico a los siguientes usuarios de su organización. Se envían notificaciones para varios eventos, entre los que se incluyen:

- Purchase
- Próxima expiración de la reserva
- Expiry
- Renovación
- Cancelación
- Cambio de ámbito

Para clientes con suscripciones de EA:
- Se envía una notificación de compra al comprador y a los contactos de notificación de EA.
- Otras notificaciones del ciclo de vida de la reserva solo se envían a los contactos de notificación de EA.
- Los usuarios que se agregan a una reserva a través del permiso de RBAC (IAM) no reciben ninguna notificación por correo electrónico.

Para clientes con suscripciones individuales:
- El comprador recibe una notificación de compra.
- En el momento de la compra, el propietario de la cuenta de facturación de suscripción recibe una notificación de compra.
- El propietario de la cuenta recibe todas las demás notificaciones.


## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- Para más información acerca de Azure Reservations, consulte los siguientes artículos:
    - [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md)
    - [Información sobre el uso de reservas de Azure para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
    - [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
    - [Costos de software de Windows no incluidos con reservas](billing-reserved-instance-windows-software-costs.md)
    - [Azure Reservations en el programa del Proveedor de soluciones en la nube (CSP) del Centro de partners](/partner-center/azure-reservations)

- Más información sobre las reservas para los planes de servicio:
    - [Máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
    - [Recursos de Azure Cosmos DB con capacidad reservada de Azure Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md)
    - [Recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database](../sql-database/sql-database-reserved-capacity.md) Más información sobre las reservas para planes de software:
    - [Planes de software de Red Hat con reservas de Azure](../virtual-machines/linux/prepay-rhel-software-charges.md)
    - [Planes de software SUSE con reservas de Azure](../virtual-machines/linux/prepay-suse-software-charges.md)
