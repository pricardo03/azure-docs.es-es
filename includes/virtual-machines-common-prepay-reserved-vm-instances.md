---
author: yashesvi
ms.author: yashar
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 09/05/2018
ms.openlocfilehash: 40551fa60aa17e9a46d24ffdf17bb61a884a48f2
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52585834"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances

Pague las máquinas virtuales por adelantado y ahorre dinero con Azure Reserved Virtual Machine (VM) Instances. Para más información, consulte las [ofertas de Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Puede comprar una instancia reservada de máquina virtual en [Azure Portal](https://portal.azure.com). Para comprar una instancia:

- Debe tener un rol de propietario al menos en suscripción Enterprise o de Pago por uso.
- Para las suscripciones Enterprise, las compras de reserva deben habilitarse en el [portal de EA](https://ea.azure.com).
- En el caso del programa Proveedor de soluciones en la nube (CSP), solo los agentes de administración o de ventas pueden adquirir reservas.

## <a name="determine-the-right-vm-size-before-you-buy"></a>Determinación del tamaño adecuado de una máquina virtual antes de su adquisición

Los campos Subcategoría del medidor y Producto de los datos de uso no distinguen los tamaños de máquina virtual que utilizan almacenamiento premium de los tamaños que no lo hacen. Si usa estos campos para determinar el tamaño de la máquina virtual que se va a utilizar para la reserva, podría adquirir el tamaño equivocado y no obtendría el descuento esperado en la reserva. Utilice uno de los métodos siguientes para determinar el tamaño de la máquina virtual correcto al adquirir la reserva:

- Consulte el campo Información adicional del archivo o la API de uso para determinar el tamaño apropiado de la máquina virtual. No utilice los valores de los campos Subcategoría del medidor o Producto. Estos campos no distinguen entre las versiones S y no S de una máquina virtual.
- También puede obtener información precisa sobre el tamaño de la máquina virtual con Powershell, Azure Resource Manager o en los datos de la máquina virtual de Azure Portal.

Las instancias reservadas de máquinas virtuales están disponibles para la mayoría de los tamaños de máquina virtual, a excepción de los siguientes:

- El descuento de la reserva no se aplica en las siguientes máquinas virtuales:
  - Servicios en la nube y máquinas virtuales clásicas
  - Tamaños de vCPU restringidos
  - Series de máquinas virtuales: serie A, serie Av2 o serie G
  - Máquinas virtuales en versión preliminar: cualquier serie o tamaño de máquina virtual que esté en versión preliminar
- Nubes: las reservas no están disponibles para la compra en las regiones de Alemania o China.
- Cuota insuficiente: una reserva cuyo ámbito sea de una sola suscripción debe tener cuota de vCPU disponible en la suscripción para la nueva instancia reservada. Por ejemplo, si la suscripción de destino tiene un límite de cuota de 10 vCPU para la serie D, no podrá comprar una reserva para 11 instancias Standard_D1. La comprobación de cuota para las reservas incluye las máquinas virtuales ya implementadas en la suscripción. Por ejemplo, si la suscripción tiene una cuota de 10 vCPU para la serie D y tiene implementadas dos instancias standard_D1, podrá comprar una reserva para 10 instancias standard_D1 en esta suscripción. 
- Restricciones de capacidad: en algunas circunstancias poco frecuentes, Azure limita la compra de nuevas reservas para un subconjunto de tamaños de máquina virtual debido a que la capacidad en una región es baja.

## <a name="buy-a-reserved-vm-instance"></a>Compra de una instancia reservada de máquina virtual

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** > **Reservations**.
3. Haga clic en **Agregar** para comprar una nueva reserva.
4. Rellene todos los campos obligatorios. Las instancias de máquina virtual en ejecución que coinciden con los atributos seleccionados cumplen los requisitos para obtener el descuento de la reserva. El número real de las instancias de máquina virtual que obtienen el descuento depende el ámbito y la cantidad seleccionada.

    | Campo      | DESCRIPCIÓN|
    |:------------|:--------------|
    |NOMBRE        |Nombre de esta reserva.| 
    |Subscription|Suscripción que se usa para pagar la reserva. Los costos anticipados de la reserva se cobran en el método de pago de la suscripción. El tipo de suscripción debe ser un contrato Enterprise (número de la oferta: MS-AZR-0017P) o de Pago por uso (número de la oferta: MS-AZR-0003P). Para una suscripción Enterprise, los cargos se deducen del saldo de compromiso monetario de la inscripción o se cobran como uso por encima del límite. Para la suscripción Pago por uso, los cargos se cobran en el método de pago de tarjeta de crédito o factura de la suscripción.|    
    |Ámbito       |El ámbito de la reserva puede cubrir una o varias suscripciones (ámbito compartido). Si selecciona: <ul><li>Suscripción única: el descuento de la reserva se aplica a las máquinas virtuales de esta suscripción. </li><li>Compartido: el descuento de la reserva se aplica a las máquinas virtuales en ejecución en cualquiera de las suscripciones en el contexto de facturación. Para los clientes de Enterprise, el ámbito compartido es la inscripción e incluye todas las suscripciones (excepto las suscripciones de desarrollo y pruebas) dentro de la inscripción. Para los clientes de Pago por uso, el ámbito compartido incluye todas las suscripciones de Pago por uso creadas por el administrador de la cuenta.</li></ul>|
    |Region    |Región de Azure que está cubierta por la reserva.|    
    |Tamaño de VM     |Tamaño de las instancias de máquina virtual.|
    |Optimizar para     |La opción de flexibilidad del tamaño de instancia de máquina virtual aplicará el descuento por la reserva a otras máquinas virtuales del mismo [grupo](https://aka.ms/RIVMGroups). La prioridad de capacidad da preferencia a la capacidad del centro de datos para las implementaciones. Esto ofrece una mayor confianza en su capacidad para iniciar instancias de máquinas virtuales cuando las necesite. La prioridad de capacidad solo está disponible si el ámbito de la reserva es de suscripción única. |
    |Término        |Un año o tres años.|
    |Cantidad    |Número de instancias que se compran dentro de la reserva. La cantidad es el número de instancias de máquina virtual en ejecución a las que se aplica el descuento de facturación. Por ejemplo, si tiene 10 máquinas virtuales Standard_D2 en la región Este de EE. UU., debería especificar 10 como cantidad para maximizar el beneficio de todas las máquinas en ejecución. |
5. Puede ver el costo de la reserva al hacer clic en **Calcular costo**.

    ![Captura de pantalla antes de enviar la compra de reserva](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Seleccione **Comprar**.
7. Haga clic en **Ver esta reserva** para conocer el estado de la compra.

    ![Captura de pantalla después de enviar la compra de reserva](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="cancellations-and-exchanges"></a>Cancelaciones e intercambios

Si necesita cancelar la reserva, podría aplicarse una tasa de terminación anticipada del 12 %. Los reembolsos se calculan conforme al precio de compra o el precio actual de la reserva, el que sea más bajo. Los reembolsos están limitados a 50 000 dólares al año. El reembolso que recibe es el saldo prorrateado restante menos la tarifa de terminación anticipada del 12 %. Para solicitar una cancelación, vaya a la hoja Reserva de Azure Portal y seleccione **Reembolso** para crear una solicitud de soporte técnico.

Si necesita cambiar la reserva de las instancias de máquinas virtuales reservadas a otra región, otro grupo de tamaños de máquina virtual u otro plazo, puede cambiarla por otra reserva de igual o mayor valor. La fecha de inicio del plazo de la nueva reserva no se traslada desde la reserva cambiada. Al crear la nueva reserva empieza el período de 1 o 3 años. Para solicitar un intercambio, vaya a la reserva en Azure Portal y seleccione **Cambio** para crear una solicitud de soporte técnico.

## <a name="next-steps"></a>Pasos siguientes

El descuento de la reserva se aplica automáticamente el número de máquinas virtuales en ejecución que coincidan con el ámbito y los atributos de la reserva. Puede actualizar el ámbito de la reserva a través de [Azure Portal](https://portal.azure.com), PowerShell, CLI o a través de la API.

Para más información sobre cómo administrar una reserva, consulte [Administración de Azure Reservations](../articles/billing/billing-manage-reserved-vm-instance.md).

Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:

- [¿Qué es Azure Reservations?](../articles/billing/billing-save-compute-costs-reservations.md)
- [Administración de reservas en Azure](../articles/billing/billing-manage-reserved-vm-instance.md)
- [Información sobre cómo se aplica el descuento por la reserva](../articles/billing/billing-understand-vm-reservation-charges.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](../articles/billing/billing-understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
- [Costos de software de Windows no incluidos con reservas](../articles/billing/billing-reserved-instance-windows-software-costs.md)
- [Azure Reservations en el programa del Proveedor de soluciones en la nube (CSP) del Centro de partners](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
