---
author: yashesvi
ms.author: yashar
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 08-07-2018
ms.openlocfilehash: 19a153a5cdc9d5f878494984313baebd12dbcbb5
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39631170"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances

Pague las máquinas virtuales por adelantado y ahorre dinero con Azure Reserved Virtual Machine (VM) Instances. Para más información, consulte las [ofertas de Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Puede comprar una instancia reservada de máquina virtual en [Azure Portal](https://portal.azure.com). Para comprar una instancia:

- Debe tener un rol de propietario al menos en suscripción Enterprise o de Pago por uso.
- Para las suscripciones Enterprise, las compras de reserva deben habilitarse en el [portal de EA](https://ea.azure.com).
- En el caso del programa del Proveedor de soluciones en la nube (CSP) solo los agentes de administración o de ventas pueden adquirir las reservas.

## <a name="determine-the-right-vm-size-before-purchase"></a>Determine el tamaño adecuado de la máquina virtual antes de la compra

Los campos Subcategoría de medidor y Producto de los datos de uso no distinguen entre tamaños de máquina virtual que usan almacenamiento premium de los tamaños de las máquinas virtuales que no lo hacen. El uso de estos campos para determinar el tamaño de la máquina virtual para la adquisición de la reserva puede conducir a una compra incorrecta y a no proporcionarle descuentos por reserva. Use alguno de los métodos siguientes para determinar el tamaño correcto de la máquina virtual para la compra de la reserva.

- Consulte el campo AdditionalInfo en su archivo de uso o API de uso para determinar el tamaño adecuado de la máquina virtual para una compra de reserva. No use los valores de los campos Subcategoría de medidor o Producto, ya que estos campos no diferencian entre versiones S y no S de una máquina virtual.
- También puede obtener la información precisa del tamaño de máquina virtual con Powershell, Azure Resource Manager o los detalles de la máquina virtual en Azure Portal.

Las instancias reservadas de máquinas virtuales están disponibles para la mayoría de los tamaños de máquina virtual, a excepción de los siguientes:

- Máquinas virtuales en versión preliminar: las series de máquinas virtuales o los tamaños que estén en versión preliminar no estarán disponibles para la compra de una reserva.
- Nubes: la compra de reservas no está disponible en las regiones de Azure Gobierno de EE. UU., Alemania y China.
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
    |Ubicación    |Región de Azure que está cubierta por la reserva.|    
    |Tamaño de VM     |Tamaño de las instancias de máquina virtual.|
    |Optimizar para     |La opción de flexibilidad del tamaño de instancia de máquina virtual aplicará el descuento por la reserva a otras máquinas virtuales del mismo [grupo](https://aka.ms/RIVMGroups). La prioridad de capacidad reserva capacidad del centro de datos para sus implementaciones. Esto ofrece una mayor confianza en su capacidad para iniciar instancias de máquinas virtuales cuando las necesite. La prioridad de capacidad solo está disponible si el ámbito de la reserva es de suscripción única. |
    |Término        |Un año o tres años.|
    |Cantidad    |Número de instancias que se compran dentro de la reserva. La cantidad es el número de instancias de máquina virtual en ejecución a las que se aplica el descuento de facturación. Por ejemplo, si tiene 10 máquinas virtuales Standard_D2 en la región Este de EE. UU., debería especificar 10 como cantidad para maximizar el beneficio de todas las máquinas en ejecución. |
5. Puede ver el costo de la reserva al hacer clic en **Calcular costo**.

    ![Captura de pantalla antes de enviar la compra de reserva](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Seleccione **Comprar**.
7. Haga clic en **Ver esta reserva** para conocer el estado de la compra.

    ![Captura de pantalla después de enviar la compra de reserva](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="next-steps"></a>Pasos siguientes

El descuento de la reserva se aplica automáticamente el número de máquinas virtuales en ejecución que coincidan con el ámbito y los atributos de la reserva. Puede actualizar el ámbito de la reserva a través de [Azure Portal](https://portal.azure.com), PowerShell, CLI o a través de la API.

Para más información sobre cómo administrar una reserva, consulte [Administración de reservas de Azure](../articles/billing/billing-manage-reserved-vm-instance.md).

Para más información acerca de las reservas de Azure, consulte los siguientes artículos:

- [¿Qué son las reservas de Azure?](../articles/billing/billing-save-compute-costs-reservations.md)
- [Administración de reservas de Azure](../articles/billing/billing-manage-reserved-vm-instance.md)
- [Información sobre cómo se aplica el descuento por la reserva](../articles/billing/billing-understand-vm-reservation-charges.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](../articles/billing/billing-understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
- [Costos de software de Windows no incluidos con reservas](../articles/billing/billing-reserved-instance-windows-software-costs.md)
- [Reservas de Azure en el programa del Proveedor de soluciones en la nube (CSP) del Centro de partners](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si tiene más preguntas, [póngase en contacto con el soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.
