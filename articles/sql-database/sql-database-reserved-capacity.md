---
title: Pago por adelantado de los núcleos virtuales de Azure SQL Database para ahorrar dinero | Microsoft Docs
description: Aprenda a comprar capacidad reservada de Azure SQL Database para ahorrar en los costos de proceso.
services: sql-database
documentationcenter: ''
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.devlang: na
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: carlrab
ms.openlocfilehash: 1d1cad4b614eb35be9235a721368db8048be050a
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39633721"
---
# <a name="prepay-for-sql-database-compute-resources-with-azure-sql-database-reserved-capacity"></a>Pago por adelantado por recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database

Ahorre dinero al pagar por adelantado los recursos de proceso de Azure SQL Database en lugar de elegir el pago por uso. Con la capacidad reservada, se compromete de antemano en Azure SQL Database durante un período de uno a tres años para obtener un descuento considerable en los costos de proceso. Para comprar capacidad reservada de SQL Database, deberá especificar la región de Azure, el tipo de implementación, el servicio y el período. 

No es necesario asignar la reserva a las instancias de SQL Database. Al igualarse las instancias de SQL Database en ejecución o las recién implementadas se obtendrá la ventaja automáticamente. Al comprar una reserva, se adelanta el pago de los costos de proceso de las instancias de SQL Database durante un período de uno a tres años. En cuando se compra una reserva, los costos de proceso de SQL Database que coincidan con los atributos de reserva dejan de pagarse según las tarifas de pago por uso. La reserva no cubre los cargos por software, redes o almacenamiento asociados a la instancia de SQL Database. Al final del plazo de reserva, la ventaja en la facturación expira y las instancias de SQL Database se facturan según los precios de pago por uso. Las reservas no se renuevan automáticamente. Para información sobre precios, consulte el artículo sobre la [oferta de capacidad reservada de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/managed/).

Puede comprar capacidad reservada de Azure SQL Database en [Azure Portal](https://portal.azure.com). Para comprar capacidad reservada de SQL Database:
- Debe tener rol de propietario al menos en una suscripción Enterprise o de Pago por uso.
- Para las suscripciones Enterprise, las compras de reserva de Azure deben habilitarse en el [portal de EA](https://ea.azure.com).
-  En el caso del programa del Proveedor de soluciones en la nube (CSP), los únicos que pueden comprar la capacidad reservada de SQL Database son los agentes de administración o de ventas.

Los detalles sobre los cargos por la compra de reserva para los clientes empresariales y los de pago por uso se tratan en las [preguntas más frecuentes](#frequently-asked-questions).

## <a name="determine-the-right-sql-size-before-purchase"></a>Determinación del tamaño adecuado de SQL antes de la compra

El tamaño de la reserva debe basarse en el proceso total de las bases de datos de SQL únicas existentes o que se van a implementar o de los grupos elásticos de una región específica y con el mismo nivel de rendimiento y la misma generación de hardware. 

Por ejemplo, supongamos que está ejecutando un grupo elástico de propósito general Gen5 de 16 núcleos virtuales y dos bases de datos únicas Gen5 de 4 núcleos virtuales críticas para la empresa. Además, supongamos que planea implementar en el próximo mes un grupo elástico Gen5 de propósito general y 16 núcleos virtuales adicional, y otro de 32 núcleos virtuales crítico para la empresa. Y también supongamos que sabe que necesitará estos recursos durante al menos 1 año. En este caso debe comprar una reserva de 1 año de una instancia de Compute Gen5 con 32 núcleos virtuales (2 × 16) para una base de datos de SQL única y un grupo elástico de uso general y otra reserva de 1 año de una instancia de Compute Gen 5 con 40 núcleos virtuales (2 × 4 + 32) para una base de datos de SQL única y un grupo elástico críticos para la empresa.

## <a name="buy-sql-database-reserved-capacity"></a>Compra de capacidad reservada de SQL Database

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** > **Reservations**.
3. Seleccione **Agregar** y, en el panel Seleccione el tipo de producto, elija **SQL Database** para adquirir una nueva reserva para SQL Database.
4. Rellene todos los campos obligatorios. Las bases de datos únicas o grupos elásticos nuevos que coincidan con los atributos seleccionados serán aptos para el descuento en la capacidad reservada. El número real de instancias de SQL Database que obtienen el descuento depende el ámbito y la cantidad seleccionados.

   ![Captura de pantalla antes del envío de la compra de capacidad reservada de SQL Database](./media/sql-database-reserved-vcores/sql-reserved-vcores-purchase.png)

    | Campo      | DESCRIPCIÓN|
    |:------------|:--------------|
    |NOMBRE        |Nombre de esta reserva.| 
    |Subscription|La usada para pagar la reserva de capacidad reservada de SQL Database. Los costos anticipados por la capacidad reservada de SQL Database se cobran mediante el método de pago de la suscripción. El tipo de suscripción debe ser un contrato Enterprise (número de la oferta: MS-AZR-0017P) o de Pago por uso (número de la oferta: MS-AZR-0003P). Para una suscripción Enterprise, los cargos se deducen del saldo de compromiso monetario de la inscripción o se cobran como uso por encima del límite. Para la suscripción Pago por uso, los cargos se cobran en el método de pago de tarjeta de crédito o factura de la suscripción.|    
    |Ámbito       |El ámbito de la reserva de núcleos virtuales puede cubrir una suscripción o varias (ámbito compartido). Si selecciona: <ul><li>Suscripción única: el descuento por la reserva de núcleos virtuales se aplica a las instancias de SQL Database de esta suscripción. </li><li>Compartido: el descuento por la reserva de núcleos virtuales se aplica a las instancias de SQL Database en ejecución en cualquiera de las suscripciones en el contexto de facturación. Para los clientes de Enterprise, el ámbito compartido es la inscripción e incluye todas las suscripciones (excepto las suscripciones de desarrollo y pruebas) dentro de la inscripción. Para los clientes de Pago por uso, el ámbito compartido incluye todas las suscripciones de Pago por uso creadas por el administrador de la cuenta.</li></ul>|
    |Region      |La región de Azure que abarca la capacidad reservada de SQL Database.|    
    |Tipo de implementación|El tipo para el que desea adquirir la reserva.|
    |Nivel de servicio|Nivel de servicio de las instancias de SQL Database.
    |Término        |Un año o tres años.|
    |Cantidad    |Número de instancias que se compran dentro de la capacidad reservada de SQL Database. La cantidad es el número de instancias de SQL Database en ejecución a las que se aplica el descuento de facturación. Por ejemplo, si tiene 10 instancias de SQL Database en la región Este de EE. UU., debería especificar 10 como cantidad para maximizar la ventaja para todas las máquinas en ejecución. |

5. Revise el costo de la reserva de capacidad reservada de SQL Database en la sección **Costos**.
6. Seleccione **Comprar**.
7. Haga clic en **Ver esta reserva** para conocer el estado de la compra.

## <a name="next-steps"></a>Pasos siguientes 
El descuento por la reserva de núcleos virtuales se aplica automáticamente al número de instancias de SQL Database que coincidan con el ámbito y los atributos de la capacidad reservada de SQL Database. Puede actualizar el ámbito de la capacidad reservada de SQL Database a través de [Azure Portal](https://portal.azure.com), PowerShell, la CLI o la API. 

Para saber cómo administrar la capacidad reservada de SQL Database, consulte el artículo sobre la [administración de capacidad reservada de SQL Database](../billing/billing-manage-reserved-vm-instance.md).

Para más información acerca de la reserva de Azure, consulte los siguientes artículos:

- [¿Qué son las reservas de Azure?](../billing/billing-save-compute-costs-reservations.md)
- [Administración de reservas de Azure](../billing/billing-manage-reserved-vm-instance.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](../billing/billing-understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](../billing/billing-understand-reserved-instance-usage-ea.md)
- [Reservas de Azure en el programa del Proveedor de soluciones en la nube (CSP) del Centro de partners](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si tiene más preguntas, [póngase en contacto con el soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.

