---
title: Pago por adelantado de recursos de Azure Cosmos DB para ahorrar dinero | Microsoft Docs
description: Aprenda a comprar capacidad reservada de Azure Cosmos DB para ahorrar en los costos de proceso.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: d389b3b7e50c83246542fd6b446827055a5a002a
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52583473"
---
# <a name="prepay-for-azure-cosmos-db-resources-with-reserved-capacity"></a>Pago por adelantado de recursos de Azure Cosmos DB con capacidad reservada

La capacidad reservada de Azure Cosmos DB le ayuda a ahorrar dinero gracias al pago anticipado de los recursos de Azure Cosmos DB durante uno o tres años. Con la capacidad reservada de Azure Cosmos DB, puede obtener un descuento en el rendimiento aprovisionado para recursos de Cosmos DB. Algunos ejemplos de recursos son las bases de datos y los contenedores (tablas, colecciones y gráficos).

La capacidad reservada de Azure Cosmos DB puede reducir de forma considerable los costos de Cosmos DB: hasta un 65 % sobre los precios normales, con el acuerdo anticipado de uno o tres años.&mdash; La capacidad reservada ofrece un descuento en la facturación y no afecta el estado de tiempo de ejecución de sus recursos de Azure Cosmos DB.

La capacidad reservada de Azure Cosmos DB abarca el rendimiento aprovisionado de los recursos. No cubre los cargos de almacenamiento y redes. Tan pronto como se compra una reserva, los costos de proceso que coincidan con los atributos de la reserva dejan de pagarse según las tarifas de pago por uso. Para más información sobre las reservas, consulte el artículo [Azure Reservations](../billing/billing-save-compute-costs-reservations.md). 

Puede comprar capacidad reservada de Azure Cosmos DB en [Azure Portal](https://portal.azure.com). Para adquirir capacidad reservada:

* Debe tener rol de propietario al menos en una suscripción Enterprise o de Pago por uso.  
* Para las suscripciones Enterprise, las compras de reserva de Azure deben habilitarse en el [portal de EA](https://ea.azure.com/).  
* En el caso del programa del Proveedor de soluciones en la nube (CSP), solo los agentes de administración o de ventas pueden comprar capacidad reservada de Azure Cosmos DB.

## <a name="determine-the-required-throughput-before-purchase"></a>Determinación del rendimiento necesario antes de la compra

El tamaño de la reserva debe basarse en la cantidad total de rendimiento que usarán los recursos de Azure Cosmos DB existentes o que se van a implementar. Puede determinar el rendimiento necesario de las maneras siguientes:

* Obtenga los datos históricos del rendimiento total aprovisionado a través de sus cuentas, bases de datos y colecciones de Azure Cosmos DB en todas las regiones. Por ejemplo, puede evaluar el rendimiento aprovisionado medio diario si descarga el extracto de uso diario desde `https://account.azure.com`.

* Si es un cliente de Contrato Enterprise (EA), puede descargar el archivo de uso para obtener los detalles de rendimiento de Azure Cosmos DB. Consulte el valor **Tipo de servicio** de la sección **Información adicional** en el archivo de uso.

* Puede sumar el rendimiento medio de todas las cargas de trabajo de las cuentas de Azure Cosmos DB que espera ejecutar durante el próximo período de uno o tres años. A continuación, puede usar esa cantidad para la reserva.

## <a name="buy-azure-cosmos-db-reserved-capacity"></a>Compra de capacidad reservada de Azure Cosmos DB

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).  

2. Seleccione **Todos los servicios** > **Reservations** > **Agregar**.  

3. En el panel **Seleccione el tipo de producto**, elija **Azure Cosmos DB** > **Seleccionar** para comprar una nueva reserva.  

4. Rellene los campos obligatorios tal como se describe en la tabla siguiente:

   ![Cumplimentación del formulario de capacidad reservada](./media/cosmos-db-reserved-capacity/fill_reserved_capacity_form.png) 

   |Campo  |DESCRIPCIÓN  |
   |---------|---------|
   |NOMBRE   |    Nombre de la reserva. Este campo se rellena automáticamente con `CosmosDB_Reservation_<timeStamp>`. Puede proporcionar un nombre diferente al crear la reserva. O bien, puede cambiarlo después de crear la reserva.      |
   |Subscription  |   Suscripción usada para pagar la capacidad reservada de Azure Cosmos DB. El método de pago en la suscripción seleccionada se usa al cargar los costos por adelantado. El tipo de suscripción debe ser uno de los siguientes: <br/><br/>  [Contrato Enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/) (número de oferta: MS-AZR-0017P): en una suscripción Enterprise, los cargos se deducen del saldo de compromiso monetario de la inscripción o se cobra como uso por encima del límite. <br/><br/> [Pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/) (número de oferta: MS-AZR-0003P): en una suscripción de pago por uso, los cargos se cobran mediante el método de pago de tarjeta de crédito o factura de la suscripción.    |
   |Ámbito   |   Opción que controla el número de suscripciones que pueden usar la ventaja de facturación asociada con la reserva. También controla cómo se aplica la reserva a suscripciones concretas.   <br/><br/>  Si selecciona **Suscripción única**, el descuento de reserva se aplica a las instancias de Azure Cosmos DB de la suscripción seleccionada. <br/><br/>  Si selecciona **Compartido**, el descuento de la reserva se aplica a las instancias de Azure Cosmos DB que se ejecutan en cualquier suscripción en el contexto de facturación. El contexto de facturación se basa en cómo se haya suscrito a Azure. Para los clientes de Enterprise, el ámbito compartido es la inscripción e incluye todas las suscripciones (excepto las suscripciones de desarrollo y pruebas) dentro de la inscripción. Para los clientes de Pago por uso, el ámbito compartido incluye todas las suscripciones de Pago por uso creadas por el administrador de la cuenta.  <br/><br/> Puede cambiar el ámbito de reserva después de comprar la capacidad reservada.  |
   |Tipo de capacidad reservada   |  Rendimiento aprovisionado como unidades de solicitud.|
   |Unidades de capacidad reservada  |      Cantidad de rendimiento que quiere reservar. Puede calcular este valor si determina el rendimiento necesario para todos los recursos de Cosmos DB (por ejemplo, las bases de datos o contenedores) por región. A continuación, multiplique esa cifra por el número de regiones que asociará a la base de datos de Cosmos DB.  <br/><br/> Por ejemplo, si tiene cinco regiones con un millón de RU/s en todas las regiones, seleccione cinco millones de RU/s para la compra de capacidad de reserva.    |
   |Término  |   Un año o tres años.   |

5. Revise el descuento y el precio de la reserva en la sección **Costos**. Este precio de reserva se aplica a los recursos de Azure Cosmos DB con rendimiento aprovisionado en todas las regiones.  

6. Seleccione **Comprar**. Cuando se realiza correctamente la compra, se muestra la siguiente página: 

   ![Cumplimentación del formulario de capacidad reservada](./media/cosmos-db-reserved-capacity/reserved_capacity_successful.png) 

Después de comprar una reserva, se aplica inmediatamente a cualquier recurso de Azure Cosmos DB existente que coincida con los términos de la reserva. Si no tiene recursos de Azure Cosmos DB ya existentes, la reserva se aplica al implementar una nueva instancia de Cosmos DB que coincida con los términos de la reserva. En ambos casos, el período de la reserva empieza inmediatamente después de que una compra se ha realizado correctamente. 

Cuando expira la reserva, las instancias de Azure Cosmos DB se siguen ejecutando y se facturan según las tarifas habituales de pago por uso.

## <a name="next-steps"></a>Pasos siguientes

El descuento de la reserva se aplica automáticamente a los recursos de Microsoft Azure Cosmos DB que coincidan con el ámbito y los atributos de la reserva. Puede actualizar el ámbito de la reserva mediante Azure Portal, PowerShell, la CLI de Azure o la API.

*  Para saber cómo se aplican los descuentos de capacidad reservada a Azure Cosmos DB, consulte [Aplicación del descuento por reserva a Azure Cosmos DB](../billing/billing-understand-cosmosdb-reservation-charges.md).

* Para más información acerca de las reservas de Azure, consulte los siguientes artículos:

   * [¿Qué son las reservas de Azure?](../billing/billing-save-compute-costs-reservations.md)  
   * [Administración de reservas de Azure](../billing/billing-manage-reserved-vm-instance.md)  
   * [Información sobre el uso de reservas para la inscripción Enterprise](../billing/billing-understand-reserved-instance-usage-ea.md)  
   * [Información sobre el uso de reservas para suscripciones de pago por uso](../billing/billing-understand-reserved-instance-usage.md)
   * [Reservas de Azure en el programa CSP del Centro de partners](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

