---
title: ¿Qué es Azure Reservations?
description: Obtenga información sobre Azure Reservations y precios para ahorrar en máquinas virtuales, SQL Database, Azure Cosmos DB y otros costos de recursos.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: banders
ms.openlocfilehash: c1cced862bee1ec4579f3e6e5145968dec7613e4
ms.sourcegitcommit: 0a9419aeba64170c302f7201acdd513bb4b346c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2020
ms.locfileid: "77500573"
---
# <a name="what-are-azure-reservations"></a>¿Qué es Azure Reservations?

Con Azure Reservations ahorrará gastos al confirmar los planes de un año o tres para las máquinas virtuales, la capacidad de proceso de SQL Database, Azure Blob Storage o Azure Data Lake Storage Gen2, Azure Disk Storage, el rendimiento de Azure Cosmos DB u otros recursos de Azure. La confirmación le permite obtener un descuento en los recursos que utiliza. Reservations puede reducir significativamente los costos de los recursos hasta un 72 % en los precios de pago por uso. Reservations ofrece un descuento en la facturación y no afecta al estado del entorno de ejecución de los recursos.

Puede pagar una reserva por adelantado o mensualmente. El costo total de las reservas por adelantado y mensuales es el mismo y no se pagan cargos adicionales por elegir el pago mensual. El pago mensual está disponible para las reservas de Azure, no para los productos de terceros.

Puede comprar una reserva en [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).

## <a name="why-buy-a-reservation"></a>¿Por qué comprar una reserva?

Si tiene máquinas virtuales, datos de Blob Storage, bases de datos SQL o Azure Cosmos DB que usen una capacidad o una cantidad de procesos significativas, o bien se ejecuten durante largos períodos, la compra de una reserva le proporcionará la opción más rentable. Por ejemplo, si ejecuta continuamente cuatro instancias de un servicio sin una reserva, se le cobra según las tarifas de pago por uso. Cuando compre una reserva para esos recursos, obtendrá inmediatamente el descuento por reserva. Ya no se aplicarán a los recursos las tarifas de pago por uso.

## <a name="charges-covered-by-reservation"></a>Cargos cubiertos por la reserva

Planes de servicio:

- **Instancia reservada de máquina virtual**: una reserva solo cubre el costo de proceso de máquina virtual. No cubre cargos por software adicional, administración de redes ni almacenamiento.
- **Capacidad reservada de Azure Storage**: una reserva cubre la capacidad de almacenamiento de las cuentas de almacenamiento estándar para el almacenamiento de blobs o el almacenamiento de Azure Data Lake Gen2. La reserva no cubre las tasas de ancho de banda o de transacción.
- **Reservas de Azure Disk Storage**: Una reserva solo cubre las SSD Premium de tamaño P30 o superior. No cubre ningún otro tipo de disco ni tamaños menores que P30.
- **Capacidad reservada de Azure Cosmos DB**: una reserva abarca el rendimiento aprovisionado de los recursos. No cubre los cargos de almacenamiento y redes.
- **Núcleo virtual reservado de SQL Database**: solo se incluyen con una reserva los costos de proceso. La licencia se factura por separado.
- **SQL Data Warehouse**: una reserva cubre el uso de cDWU. No cubre los cargos de almacenamiento o de red asociados con el uso de SQL Data Warehouse.
- **Impuesto sobre el timbre de App Service**: una reserva cubre el uso del timbre. No se aplica a los trabajos, por lo que los demás recursos asociados con el timbre se cobran por separado.
- **Azure Databricks**: una reserva solo cubre el uso de DBU. Otros cargos, tales como proceso, almacenamiento y redes, se aplican por separado.
- **Azure Database for MySQL**: solo se incluyen con una reserva los costos de proceso. La reserva no cubre los cargos por software, redes o almacenamiento asociados al servidor de bases de datos de MySQL.
- **Azure Database for PostgreSQL**: solo se incluyen con una reserva los costos de proceso. La reserva no cubre los cargos por software, redes o almacenamiento asociados al servidor de bases de datos de PostgreSQL.
- **Azure Database for MariaDB**: solo se incluyen con una reserva los costos de proceso. La reserva no cubre los cargos por software, redes o almacenamiento asociados al servidor de bases de datos de MariaDB.
- **Azure Data Explorer**: una reserva cubre los cargos de margen de beneficio. Una reserva no cubre los cargos de proceso, de red o de almacenamiento asociados con los clústeres.
- **Discos administrados SSD Premium**: se realiza una reserva para una SKU de disco especificada. 

Planes de software:

- **SUSE Linux**: una reserva cubre los costos de los planes de software. Los descuentos solo se aplican a los medidores SUSE y no en al uso de la máquina virtual.
- **Planes de Red Hat**: una reserva cubre los costos del plan de software. Los descuentos solo se aplican a los medidores RedHat, y no al uso de la máquina virtual.
- **Azure VMware Solution by CloudSimple**: una reserva cubre los nodos de CloudSimple de VMWare. Es posible que todavía se apliquen costos de software adicionales.
- **Red Hat OpenShift en Azure**: una reserva se aplica a los costos de OpenShift, no a los costos de infraestructura de Azure.

Para las máquinas virtuales de Windows y SQL Database, puede cubrir los costos de licencia de Windows con la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>¿Quién puede comprar una reserva?

Para comprar un plan, debe tener un rol de propietario de la suscripción en una suscripción Enterprise (MS-AZR-0017P o MS-AZR-0148P) o de pago por uso (MS-AZR-0003P o MS-AZR-0023P), o bien una suscripción de Microsoft Customer Agreement. Los proveedores de soluciones en la nube pueden usar Azure Portal o el  [Centro de partners](/partner-center/azure-reservations) para comprar reservas de Azure.

Los clientes de Contrato Enterprise (EA) pueden limitar sus compras a los administradores de EA mediante la desactivación de la opción **Add Reserved Instances** (Agregar instancias reservadas) en EA Portal. Para poder comprar una reserva, los administradores de Contrato Enterprise deben ser propietarios de la suscripción para al menos una suscripción de Contrato Enterprise. La opción es útil para empresas que requieren un equipo centralizado para comprar reservas para distintos centros de coste. Después de la compra, los equipos centralizados pueden agregar propietarios de centros de coste a las reservas. Los propietarios pueden entonces ampliar la reserva a sus suscripciones. El equipo central no necesita tener acceso al propietario de la suscripción donde se compra la reserva.

Los descuentos por reserva se aplican solo a los recursos asociados con las suscripciones compradas a través de un contrato Enterprise, Proveedor de soluciones en la nube (CSP), Microsoft Customer Agreement y de planes individuales con tarifas de pago por uso.

## <a name="scope-reservations"></a>Ámbito de las reservas

Puede limitar el ámbito de una reserva a una suscripción o a grupos de recursos. Al limitar el ámbito de una reserva, se selecciona el lugar en el que se aplican los ahorros de la reserva. Al limitar el ámbito de la reserva a un grupo de recursos, los descuentos por reserva solo se aplican al grupo de recursos, no a la suscripción completa.

### <a name="reservation-scoping-options"></a>Opciones de limitación del ámbito de la reserva

Con la limitación del ámbito del grupo de recursos tiene tres opciones para limitar el ámbito de una reserva, según sus necesidades:

- **Single resource group scope** (Ámbito de grupo de recursos único): aplica el descuento por reserva a los recursos coincidentes solo en el grupo de recursos seleccionado.
- **Single subscription scope** (Ámbito de suscripción única): aplica el descuento por reserva a los recursos coincidentes de la suscripción seleccionada.
- **Ámbito compartido**: aplica el descuento por reserva a los recursos coincidentes en suscripciones aptas que están en el contexto de facturación. Para los clientes con Contrato Enterprise, el contexto de facturación es la inscripción. En el caso de los clientes con contrato Microsoft Customer Agreement, el ámbito de facturación es el perfil de facturación. En el caso de suscripciones individuales con tarifas de pago por uso, el ámbito de facturación son todas las suscripciones aptas creadas por el administrador de la cuenta.

Al aplicar los descuentos por reserva sobre su uso, Azure procesa la reserva en el orden siguiente:

1. Reservas cuyo ámbito es un grupo de recursos
2. Reservas de ámbito único
3. Reservas de ámbito compartido

Un único grupo de recursos puede obtener descuentos por reserva de varias reservas, en función de cómo se establezca el ámbito de las reservas.

### <a name="scope-a-reservation-to-a-resource-group"></a>Limitación del ámbito de una reserva a un grupo de recursos

Puede limitar el ámbito de la reserva a un grupo de recursos al comprar la reserva o bien hacerlo después. Para limitar el ámbito de la reserva a un grupo de recursos, debe ser el propietario de la suscripción.

Para establecer el ámbito, vaya a la página [Comprar reservas](https://ms.portal.azure.com/#blade/Microsoft\_Azure\_Reservations/CreateBlade/referrer/Browse\_AddCommand) en Azure Portal. Seleccione el tipo de reserva que quiere comprar. En el formulario de selección **Seleccione el producto que quiere comprar**, cambie el valor de Ámbito a Grupo de recursos único. Después, seleccione un grupo de recursos.

![Ejemplo que muestra la selección de compra de reservas de máquina virtual](./media/save-compute-costs-reservations/select-product-to-purchase.png)

Se muestran las recomendaciones de compra del grupo de recursos en la reserva de máquina virtual. Las recomendaciones se calculan mediante el análisis del uso durante los últimos 30 días. Si el costo de la ejecución de recursos con instancias reservadas es más barato que el costo de la ejecución de recursos con tarifas de pago por uso, se realiza una recomendación de compra. Para más información sobre las recomendaciones de compra de reservas, consulte la entrada de blog [Get Reserved Instance purchase recommendations based on usage pattern](https://azure.microsoft.com/blog/get-usage-based-reserved-instance-recommendations) (Obtener recomendaciones de compra de instancias reservadas basadas en el patrón de uso).

También puede actualizar el ámbito después de comprar una reserva. Para ello, vaya a la reserva, haga clic en **Configuración** y limite de nuevo el ámbito de la reserva. Volver a limitar el ámbito de una reserva no es una transacción comercial. No se cambian las condiciones de la reserva. Para más información sobre cómo actualizar el ámbito, consulte [Update the scope after you purchase a reservation](manage-reserved-vm-instance.md#change-the-reservation-scope) (Actualización del ámbito después de comprar una reserva).

![Ejemplo que muestra un cambio en el ámbito de la reserva](./media/save-compute-costs-reservations/rescope-reservation-resource-group.png)

### <a name="monitor-and-optimize-reservation-usage"></a>Supervisión y optimización del uso de reservas

Puede supervisar el uso de la reserva de varias maneras: mediante Azure Portal, las API o los datos de uso. Para ver todas las reservas a las que tiene acceso, vaya a **Reservas** en Azure Portal. La cuadrícula de reservas muestra el último porcentaje de uso registrado para la reserva. Haga clic en la reserva para ver su uso a largo plazo.

También puede obtener el uso de la reserva mediante las [API](reservation-apis.md#see-reservation-usage) y a partir de los [datos de uso](understand-reserved-instance-usage-ea.md#common-cost-and-usage-tasks) si es un cliente con contrato Enterprise o con contrato Microsoft Customer Agreement.

Si observa que el uso de la reserva con ámbito de grupo de recursos es bajo, puede actualizar el ámbito de la reserva a una suscripción única o compartirlo en el contexto de facturación. También puede dividir la reserva y aplicar las reservas resultantes a distintos grupos de recursos.

### <a name="other-considerations"></a>Otras consideraciones

Si no tiene recursos que coincidan en un grupo de recursos, la reserva estará infrautilizada. La reserva no se aplica automáticamente a un grupo de recursos o una suscripción diferentes en los que haya un uso bajo.

Un ámbito de reserva no se actualiza automáticamente si se mueve el grupo de recursos de una suscripción a otra. El ámbito no se actualiza si se elimina el grupo de recursos. Tendrá que [volver a limitar el ámbito de la reserva](manage-reserved-vm-instance.md#change-the-reservation-scope). Si no, la reserva estará infrautilizada.

## <a name="discounted-subscription-and-offer-types"></a>Tipos de ofertas y suscripciones con descuento

Los descuentos por reserva se aplican a las siguientes suscripciones aptas y tipos de ofertas.

- Contrato Enterprise (números de oferta: MS-AZR-0017P o MS-AZR-0148P)
- Suscripciones del contrato Microsoft Customer Agreement.
- Planes individuales con tarifas de pago por uso (números de oferta: MS-AZR-0003P o MS-AZR-0023P)
- Suscripciones de CSP

Los recursos que se ejecutan en una suscripción con otros tipos de oferta no recibirán el descuento por la reserva.

## <a name="how-is-a-reservation-billed"></a>¿Cómo se factura una reserva?

La reserva se cobra en el método de pago vinculado a la suscripción. El costo de la reserva se deducirá del saldo del compromiso monetario, si está disponible. Cuando este no sea suficiente para cubrir el costo de la reserva, se le cobrará el monto restante. Si tiene una suscripción de un plan individual con tarifas de pago por uso, se factura inmediatamente en la tarjeta de crédito que tiene en su cuenta por las compras por adelantado. Los pagos mensuales aparecen en la factura y la tarjeta de crédito se cobra mensualmente. Cuando se le facture por factura, verá los cargos la próxima vez que reciba una.

## <a name="how-reservation-discount-is-applied"></a>Aplicación del descuento por reserva

El descuento por reserva se aplica a la utilización de recursos que coincide con los atributos seleccionados al comprar la reserva. Los atributos incluyen el ámbito en el que se ejecutan las máquinas virtuales, SQL Database, Azure Cosmos DB u otras ejecuciones de recursos. Por ejemplo, si quiere un descuento por reserva para las cuatro máquinas virtuales Estándar D2 en la región Oeste de EE. UU., seleccione la suscripción en la que se ejecutan las máquinas virtuales.

Un descuento de reserva significa "*usarlo o perderlo*". Si no tiene recursos coincidentes para ninguna hora, perderá una cantidad de reserva para esa hora. No se pueden arrastrar las horas reservadas no utilizadas.

Al cerrar un recurso, el descuento por reserva se aplica automáticamente a otro recurso que coincida con el ámbito especificado. Si no se encuentran recursos coincidentes en el ámbito especificado, las horas reservadas se *pierden*.

Por ejemplo, más adelante podría crear un recurso y tener una reserva que coincida con la que está infrautilizada. El descuento por reserva se aplica automáticamente al nuevo recurso coincidente.

Si, por el contrario, sus máquinas virtuales se ejecutan en diferentes suscripciones dentro de su cuenta o inscripción, seleccione el ámbito compartido. El ámbito compartido le permite aplicar el descuento de la reserva en varias suscripciones. También puede cambiar el ámbito después de la compra de la reserva. Para más información, consulte [Administración de Azure Reservations](manage-reserved-vm-instance.md).

Los descuentos por reserva solo se aplican a los recursos asociados con Enterprise, Microsoft Customer Agreement, CSP o a las suscripciones con tarifas de pago por uso. Los recursos que se ejecutan en una suscripción con otros tipos de oferta no recibirán el descuento por la reserva.

## <a name="when-the-reservation-term-expires"></a>Cuando expira el plazo de reserva

Al final del plazo de reserva, el descuento de facturación expira y los recursos se facturan según los precios de pago por uso. De forma predeterminada, las reservas no se establecen para renovarse automáticamente. Puede optar por habilitar la renovación automática de una reserva mediante la selección de la opción de la configuración de renovación. Con la renovación automática, se comprará una reserva de reemplazo cuando expire la reserva existente. De forma predeterminada, la reserva de reemplazo tiene los mismos atributos que la reserva que expira y, opcionalmente, puede cambiar la frecuencia de facturación, el plazo o la cantidad en la configuración de renovación. Cualquier usuario con acceso de propietario en la reserva y la suscripción que se usa para la facturación puede configurar la renovación.  

## <a name="discount-applies-to-different-sizes"></a>El descuento se aplica a los diferentes tamaños

Al comprar una reserva, puede aplicar el descuento a otras instancias con atributos que están dentro del mismo grupo de tamaño. Esta característica se conoce como flexibilidad de tamaño de instancia. La flexibilidad de la cobertura del descuento depende del tipo de reserva y los atributos que elija al comprar la reserva.

Planes de servicio:

- Instancias reservadas de máquina virtual: al comprar la reserva y seleccionar **Optimizado para: flexibilidad de tamaño de instancia**, la cobertura de descuento dependerá del tamaño de máquina virtual que elija. La reserva se puede aplicar a los tamaños de máquinas virtuales (VM) en el mismo grupo de series de tamaño. Si quiere saber más, vea [Flexibilidad en el tamaño de las máquinas virtuales con Azure Reserved VM Instances](../../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).
- Capacidad reservada de Azure Storage: puede adquirir capacidad reservada para cuentas de Azure Storage estándar en unidades de 100 TiB o de 1 PiB al mes. La capacidad reservada de Azure Storage está disponible en todas las regiones para cualquier nivel de acceso (recuente, esporádico o archivo) y para cualquier opción de replicación (LRS, GRS o ZRS).
- Capacidad reservada de SQL Database: la cobertura del descuento depende del nivel de rendimiento elegido. Para más información, vea [Aprenda cómo se aplica un descuento en la reserva de Azure a las instancias de SQL Database](understand-reservation-charges.md).
- Capacidad reservada de Azure Cosmos DB: la cobertura del descuento depende del rendimiento aprovisionado. Para más información, vea [Aplicación del descuento por reserva a Azure Cosmos DB](understand-cosmosdb-reservation-charges.md).

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
    - [Administración de Azure Reservations](manage-reserved-vm-instance.md)
    - [Información sobre el uso de reservas de Azure para suscripciones de pago por uso](understand-reserved-instance-usage.md)
    - [Información sobre el uso de reservas para la inscripción Enterprise](understand-reserved-instance-usage-ea.md)
    - [Costos de software de Windows no incluidos con reservas](reserved-instance-windows-software-costs.md)
    - [Azure Reservations en el programa del Proveedor de soluciones en la nube (CSP) del Centro de partners](/partner-center/azure-reservations)

- Más información sobre las reservas para los planes de servicio:
    - [Máquinas virtuales con Azure Reserved VM Instances](../../virtual-machines/windows/prepay-reserved-vm-instances.md)
    - [Recursos de Azure Cosmos DB con capacidad reservada de Azure Cosmos DB](../../cosmos-db/cosmos-db-reserved-capacity.md)
    - [Recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database](../../sql-database/sql-database-reserved-capacity.md) Más información sobre las reservas para planes de software:
    - [Planes de software de Red Hat con reservas de Azure](../../virtual-machines/linux/prepay-rhel-software-charges.md)
    - [Planes de software SUSE con reservas de Azure](../../virtual-machines/linux/prepay-suse-software-charges.md)
