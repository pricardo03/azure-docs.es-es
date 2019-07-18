---
author: yashesvi
ms.author: banders
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 07/01/2019
ms.openlocfilehash: 9e0caa8b98133dad3af083e8910d0603bbd2563b
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67489928"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances-ri"></a>Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances

Pague las máquinas virtuales por adelantado y ahorre dinero con Azure Reserved Virtual Machine (VM) Instances. Para más información, consulte las [ofertas de Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Puede comprar una instancia reservada de máquina virtual en [Azure Portal](https://portal.azure.com). Para comprar una instancia:

- Debe tener un rol de propietario en al menos una suscripción Enterprise o en una suscripción con una tarifa de pago por uso.
- En el caso de las suscripciones Enterprise, la opción **Agregar instancias reservadas** debe estar habilitada en el [portal de EA](https://ea.azure.com). O bien, si esa opción está deshabilitada, debe ser un administrador de EA en la suscripción.
- En el caso del programa Proveedor de soluciones en la nube (CSP), solo los agentes de administración o de ventas pueden adquirir reservas.

El descuento de la reserva se aplica automáticamente el número de máquinas virtuales en ejecución que coincidan con el ámbito y los atributos de la reserva. Puede actualizar el ámbito de la reserva a través de [Azure Portal](https://portal.azure.com), PowerShell, CLI o a través de la API.

## <a name="determine-the-right-vm-size-before-you-buy"></a>Determinación del tamaño adecuado de una máquina virtual antes de su adquisición

Antes de adquirir una reserva, debe determinar el tamaño de la máquina virtual que necesita. Las siguientes secciones le ayudarán a determinar el tamaño correcto de la máquina virtual.

### <a name="use-reservation-recommendations"></a>Usar recomendaciones de reserva

Puede consultar las recomendaciones de reserva para averiguar las reservas que debe adquirir.

- Se mostrarán recomendaciones de compra y la cantidad recomendada al adquirir una instancia reservada de máquina virtual en Azure Portal.
- Azure Advisor proporciona recomendaciones de compra de suscripciones individuales.  
- Puede usar las API para obtener recomendaciones de compra relativas tanto a los ámbitos de suscripción tanto compartida como única. Para más información, vea [Recommendations API de compra de instancia reservada para clientes empresariales](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation).
- En el caso de los clientes de EA, las recomendaciones de compra de los ámbitos de suscripción tanto única como compartida solo están disponibles con el [paquete de contenido de Power BI de Azure Consumption Insights](/power-bi/service-connect-to-azure-consumption-insights).

### <a name="classic-vms-and-cloud-services"></a>Servicios en la nube y máquinas virtuales clásicas

Las instancias reservadas de máquinas virtuales se aplican automáticamente tanto a las máquinas virtuales clásicas como a los servicios en la nube cuando la flexibilidad de tamaño de instancia está habilitada. No existen SKU especiales para las máquinas virtuales clásicas o servicios en la nube. Se les aplica la misma SKU de máquina virtual.

Por ejemplo, podría convertir sus máquinas virtuales clásicas o servicios en la nube en máquinas virtuales basadas en Azure Resource Manager. En este ejemplo, el descuento por reserva se aplica automáticamente a las máquinas virtuales coincidentes. No hay necesidad de *intercambiar* una instancia reservada existente: se aplica automáticamente.

### <a name="analyze-your-usage-information"></a>Analizar la información de uso
Debe analizar su información de uso para averiguar qué reservas debe adquirir.

Los datos de uso están disponibles en el archivo de uso y en las API. Úselos de manera conjunta para determinar qué reserva adquirir. Para determinar la cantidad de reservas que necesita adquirir, debe comprobar si hay instancias de máquina virtual que tengan un uso elevado diario.

No tenga en cuenta la subcategoría `Meter` ni los campos `Product` de los datos de uso, ya que no distinguen los distintos tamaños de máquina virtual que usan Premium Storage. Si usa estos campos para determinar el tamaño de máquina virtual al adquirir la reserva, puede correr el riesgo de adquirir un tamaño equivocado, y no obtendrá el descuento de reserva que espera. En su lugar, consulte el campo `AdditionalInfo` del archivo o la API de uso para determinar el tamaño apropiado de la máquina virtual.

### <a name="purchase-restriction-considerations"></a>Consideraciones sobre restricciones de compra

Las instancias reservadas de máquinas virtuales están disponibles para la mayoría de los tamaños de máquina virtual, a excepción de los indicados aquí. El descuento de la reserva no se aplica en las siguientes máquinas virtuales:

- **Series de máquinas virtuales**: serie A, serie Av2 o serie G.

- **Máquinas virtuales en versión preliminar**: cualquier serie o tamaño de máquina virtual que esté en versión preliminar.

- **Nubes**: las reservas no están disponibles para la compra en las regiones de Alemania o China.

- **Cuota insuficiente**: una reserva cuyo ámbito sea de una sola suscripción debe tener cuota de vCPU disponible en la suscripción para la nueva instancia reservada. Por ejemplo, si la suscripción de destino tiene un límite de cuota de 10 vCPU para la serie D, no podrá comprar una reserva para 11 instancias Standard_D1. La comprobación de cuota para las reservas incluye las máquinas virtuales ya implementadas en la suscripción. Por ejemplo, si la suscripción tiene una cuota de 10 vCPU para la serie D y tiene implementadas dos instancias standard_D1, podrá comprar una reserva para 10 instancias standard_D1 en esta suscripción. También puede [cursar una solicitud de aumento de la cuota](../articles/azure-supportability/resource-manager-core-quotas-request.md) para resolver este problema.

- **Restricciones de capacidad**: en algunas circunstancias poco frecuentes, Azure limita la compra de nuevas reservas a un subconjunto de tamaños de máquina virtual debido a que la capacidad en una región es baja.

## <a name="buy-a-reserved-vm-instance"></a>Compra de una instancia reservada de máquina virtual

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** > **Reservations**.
3. Haga clic en **Agregar** para comprar una nueva reserva.
4. Rellene los campos obligatorios. Las instancias de máquina virtual en ejecución que coinciden con los atributos seleccionados cumplen los requisitos para obtener el descuento de la reserva. El número real de las instancias de máquina virtual que obtienen el descuento depende el ámbito y la cantidad seleccionada.

    | Campo      | DESCRIPCIÓN|
    |------------|--------------|
    |NOMBRE        |Nombre de esta reserva.|
    |Subscription|Suscripción que se usa para pagar la reserva. Los costos anticipados de la reserva se cobran en el método de pago de la suscripción. El tipo de suscripción debe ser Contrato Enterprise (números de oferta: MS-AZR-0017P o MS-AZR-0148P) o una suscripción individual con tarifas de pago por uso (números de oferta: MS-AZR-0003P o MS-AZR-0023P). Para una suscripción Enterprise, los cargos se deducen del saldo de compromiso monetario de la inscripción o se cobran como uso por encima del límite. En una suscripción con tarifas de pago por uso, los cargos se cobran con el método de pago de factura o la tarjeta de crédito de la suscripción.|    
    |Ámbito       |El ámbito de la reserva puede cubrir una o varias suscripciones (ámbito compartido). Si selecciona: <ul><li>Suscripción única: el descuento de la reserva se aplica a las máquinas virtuales de esta suscripción. </li><li>Compartido: el descuento de la reserva se aplica a las máquinas virtuales en ejecución en cualquiera de las suscripciones en el contexto de facturación. Para los clientes Enterprise, el ámbito compartido es la inscripción e incluye todas las suscripciones que esta contiene. En el caso de los clientes que tienen suscripciones con tarifas de pago por uso, el ámbito compartido abarca todas las suscripciones con tarifas de pago por uso creadas por el administrador de la cuenta.</li></ul>|
    |Region    |Región de Azure que está cubierta por la reserva.|    
    |Tamaño de VM     |Tamaño de las instancias de máquina virtual.|
    |Optimizar para     |La opción de flexibilidad del tamaño de instancia de máquina virtual aplicará el descuento por la reserva a otras máquinas virtuales del mismo [grupo](https://aka.ms/RIVMGroups). La prioridad de capacidad da preferencia a la capacidad del centro de datos para las implementaciones. Esto ofrece una mayor confianza en su capacidad para iniciar instancias de máquinas virtuales cuando las necesite. La prioridad de capacidad solo está disponible si el ámbito de la reserva es de suscripción única. |
    |Término        |Un año o tres años.|
    |Cantidad    |Número de instancias que se compran dentro de la reserva. La cantidad es el número de instancias de máquina virtual en ejecución a las que se aplica el descuento de facturación. Por ejemplo, si tiene 10 máquinas virtuales Standard_D2 en la región Este de EE. UU., debería especificar 10 como cantidad para maximizar el beneficio de todas las máquinas en ejecución. |

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2PjmT]

## <a name="change-a-reservation-after-purchase"></a>Cambiar una reserva después de la compra

Puede realizar los siguientes tipos de cambios en una reserva después de haberla comprado:

- Actualización del ámbito de la reserva
- Flexibilidad del tamaño de instancia (si procede)
- Propiedad

También puede dividir una reserva en fragmentos más pequeños y combinar reservas ya divididas. Ninguno de estos cambios conlleva una nueva transacción comercial ni un cambio en la fecha de finalización de la reserva.

No se pueden realizar los siguientes tipos de cambios tras la compra:

- La región de una reserva existente
- SKU
- Cantidad
- Duration

Pero sí se puede *intercambiar* una reserva si quiere realizar cambios.

## <a name="cancellations-and-exchanges"></a>Cancelaciones e intercambios

Si necesita cancelar la reserva, podría aplicarse una tasa de terminación anticipada del 12 %. Los reembolsos se calculan conforme al precio de compra o el precio actual de la reserva, el que sea más bajo. Los reembolsos están limitados a 50 000 dólares al año. El reembolso que recibe es el saldo prorrateado restante menos la tarifa de terminación anticipada del 12 %. Para solicitar una cancelación, vaya a la hoja Reserva de Azure Portal y seleccione **Reembolso** para crear una solicitud de soporte técnico.

Si necesita cambiar la reserva de las instancias de máquinas virtuales reservadas a otra región, otro grupo de tamaños de máquina virtual u otro plazo, puede cambiarla por otra reserva de igual o mayor valor. La fecha de inicio del período de la nueva reserva no se extiende desde la reserva intercambiada. Al crear la nueva reserva empieza el período de 1 o 3 años. Para solicitar un intercambio, vaya a la reserva en Azure Portal y seleccione **Cambio** para crear una solicitud de soporte técnico.

Para más información acerca de cómo cambiar o reembolsar las reservas, consulte [Cambios de reserva y reembolsos](../articles/billing/billing-azure-reservations-self-service-exchange-and-refund.md).

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre cómo administrar una reserva, consulte [Administración de Azure Reservations](../articles/billing/billing-manage-reserved-vm-instance.md).
- Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:
    - [¿Qué es Azure Reservations?](../articles/billing/billing-save-compute-costs-reservations.md)
    - [Administración de reservas en Azure](../articles/billing/billing-manage-reserved-vm-instance.md)
    - [Información sobre cómo se aplica el descuento por la reserva](../articles/billing/billing-understand-vm-reservation-charges.md)
    - [Información sobre el uso de reservas de Azure para suscripciones de pago por uso](../articles/billing/billing-understand-reserved-instance-usage.md)
    - [Información sobre el uso de reservas para la inscripción Enterprise](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
    - [Costos de software de Windows no incluidos con reservas](../articles/billing/billing-reserved-instance-windows-software-costs.md)
    - [Azure Reservations en el programa del Proveedor de soluciones en la nube (CSP) del Centro de partners](https://docs.microsoft.com/partner-center/azure-reservations)
