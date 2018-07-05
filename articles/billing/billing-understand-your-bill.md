---
title: Descripción de la factura de Azure | Microsoft Docs
description: Aprenda a leer y entender el uso y la factura de la suscripción de Azure
services: ''
documentationcenter: ''
author: tonguyen10
manager: tonguyen
editor: ''
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/14/2018
ms.author: tonguyen
ms.openlocfilehash: 689ea9e0d029bb65bc579fc914c6ed3073b4a96b
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064696"
---
# <a name="understand-your-bill-for-microsoft-azure"></a>Comprender la factura de Microsoft Azure
Para entender la factura de Azure, compare la factura con el archivo de uso diario detallado y los informes de administración de costos en Azure Portal.

>[!NOTE]
>Este artículo no es aplicable para clientes del Contrato Enterprise (EA). Si usted es un cliente EA, [puede encontrar la documentación de las facturas en Enterprise Portal](https://ea.azure.com/helpdocs/understandingYourInvoice).  

Para obtener la factura en PDF y una copia de la descarga del archivo CSV de uso diario detallado, consulte [Obtención de los datos de uso diario y la factura de Azure](billing-download-azure-invoice-daily-usage-date.md). 

Para términos y descripciones detallados de su factura y el archivo de uso diario detallado, vea [Descripción de los términos en su factura de Microsoft Azure](billing-understand-your-invoice.md) y [Understand terms on your Microsoft Azure detailed usage](billing-understand-your-usage.md) (Descripción de los términos sobre el uso detallado de Microsoft Azure). 

Para obtener información sobre los informes de administración de costes, vea [Azure portal cost management](https://docs.microsoft.com/azure/billing/billing-getting-started) (Administración de costos de Azure Portal).

## <a name="charges"></a>¿Cómo puedo asegurarme de que los cargos de mi factura son correctos?

>[!VIDEO https://www.youtube.com/embed/3YegFD769Pk]

Si hay un cargo en la factura del que quiera tener más detalles, hay un par de opciones.

### <a name="option-1-review-your-invoice-and-compare-the-usage-and-costs-with-the-detailed-usage-csv-file"></a>Opción 1: Revisión de la factura y comparación del uso y los costos con el archivo CSV de uso detallado

El archivo CSV de uso detallado proporciona los cargos por período de facturación y el uso diario. Para obtener el archivo CSV de uso detallado, consulte [Obtención de los datos de uso diario y la factura de Azure](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date).

Los cargos de uso se muestran en el nivel de medidor. Los siguientes términos significan lo mismo en la factura y en el archivo de uso detallado. Por ejemplo, el ciclo de facturación en la factura es equivalente al período de facturación que se muestra en el archivo de uso detallado.

 | Factura (PDF) | Uso detallado (CSV)|
 | --- | --- |
|Ciclo de facturación | Período de facturación |
 |NOMBRE |Categoría de medidor |
 |Escriba |Subcategoría de medidor |
 |Recurso |Nombre de medidor |
 |Region |Medidor de la región |
 |Consumida |Cantidad consumida |
 |Se incluye |Cantidad incluida |
 |Facturable |Cantidad de superávit |

La sección de **cargos de uso** de la factura tiene el valor total de cada medidor que se consumió durante el período de facturación. Por ejemplo, la captura de pantalla siguiente muestra un cargo de uso para el servicio del programador de Azure.

![Cargos de uso de la factura](./media/billing-understand-your-bill/1.png)

Se muestra el mismo cargo en la sección de **resumen de cuenta** del archivo CSV de uso detallado. Tanto la cantidad *consumida* como el *valor* coinciden con los de la factura.

![Cargos de uso del archivo CSV](./media/billing-understand-your-bill/2.png)

Para ver un desglose de este cargo a diario, vaya a la sección **Uso diario** del archivo CSV. Filtre por "Scheduler" en *Categoría de medidor* y podrá ver qué días se usó el medidor y cuánto se consumió. La información de *recursos* y de *grupo de recursos* también se incluye para poder realizar una comparación. Los valores *consumidos* deben corresponderse con los que aparecen en la factura.

![Sección de uso diario en el archivo CSV](./media/billing-understand-your-bill/3.png)

Para obtener el costo por día, multiplique las cantidades *consumidas* por el valor de la *tarifa* en la sección **Resumen de cuenta**.

Para más información acerca de la factura, consulte [Conocimiento de la factura de Azure](billing-understand-your-invoice.md).

Para más información sobre cada una de las columnas del archivo CSV, consulte [Conocimiento del uso detallado de Azure](billing-understand-your-invoice.md).

### <a name="option-2-review-your-invoice-and-compare-with-the-usage-and-costs-in-the-azure-portal"></a>Opción 2: Revisión de la factura y comparación con el uso y los costos en Azure Portal

Azure Portal también puede ayudar a comprobar los costos, ya que proporciona gráficos de administración de costos para una visualización rápida del uso y de los cargos de la factura.

Para continuar con el ejemplo anterior, visite la [página de suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), seleccione su suscripción y, a continuación, elija **Análisis de costos**. Desde allí, puede especificar el intervalo de tiempo y ver los cargos por uso para el servicio Azure Scheduler.

![Vista de análisis de costos en Azure Portal](./media/billing-understand-your-bill/4.png)

Para ver el desglose de costo diario en **Historial de costos**, haga clic en la fila.

![Visualización del historial de costos en Azure Portal](./media/billing-understand-your-bill/5.png)

Para más información, vea [Prevención de costes inesperados con la administración de costes y facturación de Azure](billing-getting-started.md#costs).

## <a name="external"></a>¿Qué son los cargos de servicios externos?
Los servicios externos (también conocidos como pedidos de Azure Marketplace) se proporcionan mediante proveedores de servicios independientes y se facturan por separado. Los cargos no se muestran en la factura de Azure. Para aprender más, consulte [Descripción de los gastos de servicios externos de Azure](billing-understand-your-azure-marketplace-charges.md).

## <a name="payment"></a>¿Cómo puedo realizar un pago?

Si ha configurado una tarjeta de crédito o de débito como forma de pago, el pago se cargará automáticamente en un plazo de diez días después de finalizar el período de facturación. En el extracto de la tarjeta de crédito, el elemento de línea diría **MSFT Azure**.

Si [paga mediante factura](billing-how-to-pay-by-invoice.md), envíe el pago a la ubicación indicada en la parte inferior de la factura. Para obtener ayuda, [póngase en contacto con el equipo de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="how-do-i-check-the-status-of-a-payment-made-by-credit-card"></a>¿Cómo puedo comprobar el estado de un pago realizado con tarjeta de crédito?

[Cree una incidencia de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para solicitar el estado de su pago. 

## <a name="are-there-different-azure-customer-types-how-do-i-know-what-customer-type-i-am"></a>¿Hay diferentes tipos de clientes de Azure? ¿Cómo se puede saber qué tipo de cliente soy?
Hay diferentes tipos de clientes de Azure. Para entender mejor los precios y la facturación, consulte las siguientes descripciones de tipos de clientes.

- **Enterprise**: los clientes de empresa han firmado un Contrato Enterprise con Azure para negociar compromisos monetarios y acceder a precios personalizados para los recursos de Azure.
- **Web Direct**: los clientes de Web Direct no han firmado ningún contrato personalizado con Azure. Estos clientes se han registrado en Azure a través de azure.com y reciben precios públicos para todos los recursos de Azure.
- **Proveedor de servicios en la nube**: los proveedores de servicios de nube suelen ser empresas que ha contratado un cliente final para crear soluciones en Azure.

## <a name="why-dont-i-see-the-cost-the-resource-i-have-created-in-my-bill"></a>¿Por qué no se ve el costo del recurso que he creado en mi factura?
Azure no factura directamente en función del costo de los recursos. La facturación se realiza a partir de uno o más medidores que se utilizan para realizar un seguimiento de la utilización de un recurso a lo largo de su vigencia. Estos medidores se usan entonces para calcular la factura. Obtenga más información sobre los medidores de Azure a continuación.

## <a name="how-does-azure-charge-metering-work"></a>¿Cómo cobra Azure el trabajo de medición?
Cuando se usa un único recurso de Azure, como una máquina virtual, también se crearán instancias de uno o varios medidores. Estos medidores se utilizan para realizar el seguimiento de la utilización del recurso con el tiempo. Cada medidor emite registros de uso que utiliza después Azure en nuestro sistema de medición de costos para calcular la factura. 

Por ejemplo, una sola máquina virtual creada en Azure puede tener los siguientes medidores creados para realizar el seguimiento de su uso:

- Horas de proceso
- Horas de dirección IP
- Transferencia de datos de entrada
- Transferencia de datos de salida
- Disco administrado estándar
- Operaciones de disco administrado estándar
- E/S estándar: Disco
- E/S estándar: Lectura de blob en bloques
- E/S estándar: Escritura de blob en bloques
- E/S estándar: Eliminación de blob en bloques

Una vez que se crea la máquina virtual, cada uno de los medidores anteriores comenzará a emitir registros de utilización. Esta utilización se utilizará en el sistema de medición de Azure junto con el precio del medidor para determinar cuánto se le cobra al cliente.

> [!Note]
> Los medidores de ejemplo anteriores pueden ser solo un subconjunto de los medidores que se crean por una máquina virtual creada.

## <a name="what-is-the-difference-between-azure-1st-party-charges-and-azure-marketplace-charges"></a>¿Cuál es la diferencia entre los cargos de Azure y los cargos de Azure Marketplace?
Los cargos de Azure son para aquellos recursos que Azure desarrolla y ofrece directamente. Los cargos de Azure Marketplace son para aquellos recursos creados por proveedores de software de otros fabricantes que están disponibles para su uso en Azure Marketplace. Por ejemplo, un firewall Barracuda es un recurso de Azure Marketplace que ofrecen otros fabricantes. Todos los cargos por el firewall y sus medidores correspondientes aparecerán como cargos de Marketplace. 

## <a name="tips-for-cost-management"></a>Sugerencias de administración de costes
- Realice una estimación de los costos mediante la [calculadora de precios](https://azure.microsoft.com/pricing/calculator/) y la [calculadora de costo total de propiedad](https://aka.ms/azure-tco-calculator) y obtenga la [información detallada sobre los precios de cada servicio](https://azure.microsoft.com/pricing/).
- [Configure las alertas de facturación](billing-set-up-alerts.md).
- [Revise el uso y los costos con regularidad en Azure Portal](billing-getting-started.md#costs).

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Póngase en contacto con el servicio de soporte técnico.

Si sigue necesitando ayuda, [póngase en contacto con el servicio de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.
