---
title: Escenarios de automatización de la administración de costos y facturación de Azure | Microsoft Docs
description: Obtenga información sobre cómo se asignan escenarios comunes de administración de costos y facturación a diferentes API.
services: billing
documentationcenter: ''
author: Erikre
manager: dougeby
editor: ''
tags: billing
ms.assetid: 204b15b2-6667-4b6c-8ea4-f32c06f287fd
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 6/13/2018
ms.author: erikre
ms.openlocfilehash: 064f141680e62d7102d7c3332e4d93efd6c28037
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53258753"
---
# <a name="billing-and-cost-management-automation-scenarios"></a>Escenarios de automatización de administración de costos y facturación

A continuación, se identifican escenarios comunes de facturación y administración de costos que se asignan a diferentes API que se pueden usar en tales escenarios. Encontrará un resumen de todas las API disponibles y la funcionalidad que ofrecen debajo del escenario para la asignación de API.

## <a name="common-scenarios"></a>Escenarios comunes

Puede usar las API de administración de costos y facturación en distintos escenarios para responder a las preguntas relacionadas con los costos y el uso.  A continuación se proporciona un resumen de los escenarios comunes.

- **Conciliación de facturas**: ¿Microsoft me cobró el importe correcto?  ¿Cuál es mi factura y puedo calcularla yo mismo?

- **Cargos cruzados**: ahora que ya sé cuándo se me va a cobrar, ¿quién debe encargarse de pagar en mi organización?

- **Optimización de costos**: sé cuánto se me ha cobrado. Pero, ¿cómo puedo obtener mayor rendimiento del dinero que gasto en Azure?

- **Seguimiento de costos**: me gustaría consultar lo que gasto y uso en Azure a lo largo del tiempo. ¿Cuáles son las tendencias? ¿Cómo podría hacerlo mejor?

- **Gasto en Azure durante el mes**: ¿cuánto llevo gastado este mes? ¿Debo realizar algún cambio en lo que gasto o uso en Azure? ¿En qué momento del mes registro más consumo de Azure?

- **Configurar alertas**: me gustaría configurar alertas basadas en el gasto o el consumo de recursos.

## <a name="scenario-to-api-mappings"></a>Escenario para las asignaciones de API

|         API/Escenario        | Conciliación de facturas    | Cargos cruzados    | Optimización de costos    | Seguimiento de costos    | Gasto a mediados de mes    | Alertas    |
|:---------------------------:|:-------------------------:|:----------------:|:--------------------:|:----------------:|:------------------:|:---------:|
| Presupuestos                     |                           |                  |           X          |                  |                    |     X     |
| Catálogos de soluciones                |             X             |         X        |           X          |         X        |          X         |     X     |
| Hoja de precios                 |             X             |         X        |           X          |         X        |          X         |           |
| Recomendaciones de reserva |                           |                  |           X          |                  |                    |           |
| Detalles de la reserva         |                           |                  |           X          |         X        |                    |           |
| Resúmenes de reservas       |                           |                  |           X          |         X        |                    |           |
| Detalles de uso               |             X             |         X        |           X          |         X        |          X         |     X     |
| Períodos de facturación             |             X             |         X        |           X          |         X        |                    |           |
| Facturas                    |             X             |         X        |           X          |         X        |                    |           |
| RateCard                    |             X             |                  |           X          |         X        |          X         |           |
| Uso sin calificar               |             X             |                  |           X          |                  |          X         |           |

> [!NOTE]
> El escenario para las asignaciones de API que se muestra a continuación no incluye las API de consumo empresarial. Siempre que sea posible, use las API de consumo generales para los nuevos escenarios de desarrollo.

## <a name="api-summaries"></a>Resúmenes de API

### <a name="consumption"></a>Consumo
(*Clientes de Web Direct + Enterprise para todas las API, excepto las que se indican a continuación*)

-   **Budgets** (*SOLO clientes Enterprise*): use [Budgets API](https://docs.microsoft.com/rest/api/consumption/budgets) para crear presupuestos de costos o uso para recursos, grupos de recursos o medidores de facturación. Una vez creados los presupuestos, se pueden configurar alertas para notificar al usuario cuando se exceden umbrales de presupuestos que haya definido. Las acciones también se pueden configurar para que se ejecuten cuando se alcanzan los importes del presupuesto.
-   **Marketplaces**: Use la [API de cargos de Marketplace](https://docs.microsoft.com/rest/api/consumption/marketplaces) para obtener datos de los cobros y el uso de todos los recursos de Marketplace (ofertas de terceros para Azure). Estos datos se pueden utilizar para sumar los costos de todos los recursos de catálogos de soluciones o investigar los costos o el uso de recursos específicos.
-   **Price Sheet** (*SOLO clientes Enterprise*): Los clientes Enterprise pueden usar esta [Price Sheet API](https://docs.microsoft.com/rest/api/consumption/pricesheet) para recuperar sus precios personalizados para todos los medidores. Las empresas pueden utilizar estos datos en combinación con los detalles de uso y la información de uso de las instancias de Marketplace para realizar cálculos de costos en función de los datos de uso y de Marketplace. 
-   **Reservation recommendations**: use [Reservation Recommendations API](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations) para obtener recomendaciones de compra de instancias reservadas de máquina virtual. Las recomendaciones están diseñadas para permitir a los clientes analizar las cantidades compra y de ahorros en costos esperadas. Para más información, consulte [APIs for Azure reservation automation](billing-reservation-apis.md) (API de automatización de reservas de Azure).
-   **Reservation details**: use [Reservations Details API](https://docs.microsoft.com/rest/api/consumption/reservationsdetails) para consultar la información sobre las reservas de máquinas virtuales adquiridas anteriormente, como cuánto consumo se ha reservado y cuánto se ha utilizado realmente. Puede consultar datos con detalles de cada máquina virtual. Para más información, consulte [APIs for Azure reservation automation](billing-reservation-apis.md) (API de automatización de reservas de Azure).
-   **Reservation summaries**: use [Reservations Summaries API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries) para ver la información agregada sobre las reservas de máquina virtual que adquirió su organización, como cuánto consumo está reservado en comparación con la cantidad usada del agregado. Para más información, consulte [APIs for Azure reservation automation](billing-reservation-apis.md) (API de automatización de reservas de Azure).
-   **Usage details**: use [Usage Details API](https://docs.microsoft.com/rest/api/consumption/usagedetails) para obtener los cargos y la información del uso de todos los recursos propios de Azure. La información presenta la forma de registros de detalles de uso, que actualmente se emiten una vez por cada medidor al día. La información se puede utilizar para sumar los costos de todos los recursos o investigar los costos o el uso de recursos específicos.
-   **RateCard**: los clientes de Web Direct pueden usar [RateCard API](https://msdn.microsoft.com/library/azure/mt219005.aspx) para obtener las tasas de los medidores. Posteriormente, pueden usar la información devuelta con la información de uso de los recursos para calcular manualmente la factura esperada. 
-   **Unrated usage**: puede usar [Unrated Usage API](https://msdn.microsoft.com/library/azure/mt219003.aspx) para obtener información del uso sin procesar antes de que Azure realice cualquier medición o aplique algún cargo.

### <a name="billing"></a>Facturación
-   **Billing Periods**: use [Billing Periods API](https://docs.microsoft.com/rest/api/billing/billingperiods) para determinar un período de facturación para analizar, junto con el identificador de la factura de ese período. Los identificadores de factura pueden usarse con la instancia siguiente de Invoice API. 
-   **Invoices**: use [Invoices API](https://docs.microsoft.com/rest/api/billing/invoices) para obtener la dirección URL de descarga de una factura para un período de facturación determinado en formato PDF.

### <a name="enterprise-consumption"></a>Consumo de Enterprise
*(SOLO todas las API de Enterprise)*

-   **Balance summary**: use [Balance Summary API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) para obtener un resumen mensual de la información sobre los saldos, las nuevas compras, los gastos de los servicios de Azure Marketplace, los ajustes y los gastos de uso por encima del límite. Puede obtener esta información para el período de facturación actual o cualquier período en el pasado. Las empresas pueden utilizar estos datos para compararlos con los cargos del resumen calculados manualmente. Esta API no proporciona información específica de recursos o una vista agregada de los costos.
-   **Usage details**: use [Usage Details API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) para obtener información detallada del uso de recursos propios de Azure para el mes actual, un período de facturación específico o un período de fechas personalizado. Las empresas pueden usar estos datos para calcular manualmente la factura en función de la velocidad y el consumo de y también pueden utilizar la información de departamento u organización disponible para los costos de atributo entre organizaciones. Los datos proporcionan una vista específica de recursos de uso/costo.
-   **Marketplace Store Charge**: use [Marketplace Store Charge API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) para obtener información detallada del uso de Azure de terceros para el mes actual, un período de facturación específico o un período de fechas personalizado. Las empresas pueden usar estos datos para calcular manualmente la factura en función de la velocidad y el consumo de y también pueden utilizar la información de departamento u organización disponible para los costos de atributo entre organizaciones. La API de cargos de la tienda de Marketplace ofrece una vista específica de recursos de uso/costo.
-   **Price Sheet**: [Price Sheet API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) proporciona la tasa aplicable de cada medidor para la inscripción y el período de facturación determinados. Esta información de tasa puede usarse junto con los detalles de uso y la información de uso de catálogos de soluciones para calcular manualmente la factura esperada.
-   **Billing Periods**: use [Billing Periods API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) para obtener una lista de períodos de facturación junto con una propiedad que apunte a la ruta de la API para los cuatro conjuntos de datos de la API Enterprise que pertenecen a dicho período de facturación: BalanceSummary, UsageDetails, Marketplace Charges y PriceSheet.
-   **Reservation Recommendations**: [Reservation Recommendations API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation) examina el uso de las máquinas virtuales del cliente correspondiente a 7 días, 30 días o 60 días y ofrece recomendaciones de compra única y compartida. La API de instancia reservada permite a los clientes analizar las cantidades de compra recomendadas y los ahorros en costos esperados. Para más información, consulte [APIs for Azure reservation automation](billing-reservation-apis.md) (API de automatización de reservas de Azure).

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="what-is-the-difference-between-the-enterprise-reporting-apis-and-the-consumption-apis-when-should-i-use-each"></a>¿Cuál es la diferencia entre las API de Enterprise Reporting y las API de consumo? ¿Cuándo se debe usar cada una?
Estas API tienen un conjunto similar de funcionalidades y pueden responder al mismo conjunto amplio de preguntas en el ámbito de administración de costos y facturación. Sin embargo, cada API se dirige a distintas audiencias: 

- **API de informes para Enterprise**: estas API están disponibles para los clientes que han firmado un contrato Enterprise con Microsoft que les concede acceso a compromisos monetarios negociados y precios personalizados. Para poder usarse, las API necesitan una clave que se obtiene de [Enterprise Portal](https://ea.azure.com). Para obtener una descripción de estas API, vea [Información general de API de informes para clientes de Enterprise](billing-enterprise-api.md).

- **API de consumo**: estas API están disponibles para todos los clientes, con algunas excepciones. Para más información, consulte [Información general sobre la API de consumo de Azure](billing-consumption-api-overview.md) y la [referencia de la API de consumo de Azure](https://docs.microsoft.com/rest/api/consumption/). Las API proporcionadas son la solución recomendada para los escenarios de desarrollo más recientes. 

### <a name="what-is-the-difference-between-the-usage-details-api-and-the-usage-api"></a>¿Cuál es la diferencia entre la API de detalles de uso y la API de uso?
Estas API ofrecen básicamente distintos datos:

- **Usage Details**: [Usage Details API](https://docs.microsoft.com/rest/api/consumption/usagedetails) proporciona información sobre los costos y el uso de Azure por instancia del medidor. Los datos proporcionados ya han pasado por el sistema de medición de costos de Azure y no se les había aplicado costos junto con otros posibles cambios:

    - Cambios que tienen en cuenta el uso de compromisos monetarios de prepago
    - Cambios que tienen en cuenta las discrepancias de uso detectadas por Azure

- **Uso** [Usage API](https://msdn.microsoft.com/library/Mt219003.aspx) proporciona información de uso de Azure sin procesar antes de pasar por el sistema de medición de costos de Azure. Estos datos no pueden tener ninguna correlación con la cantidad de uso o el importe de los cargos que se ven después del sistema de medición de cargos de Azure.

### <a name="what-is-the-difference-between-the-invoice-api-and-the-usage-details-api"></a>¿Cuál es la diferencia entre la API de facturación y la API de detalles de uso?
Estas API ofrecen una vista diferente de los mismos datos. [Invoice API](https://docs.microsoft.com/rest/api/billing/invoices) es solamente para los clientes de Web Direct y proporciona una acumulación mensual de su factura según los cargos agregados por cada tipo de medidor. La [API de detalles de uso](https://docs.microsoft.com/rest/api/consumption/usagedetails) proporciona una vista detallada de los registros de uso y costo diarios y la pueden usar tanto clientes Enterprise como Web Direct.

### <a name="what-is-the-difference-between-the-price-sheet-api-and-the-ratecard-api"></a>¿Cuál es la diferencia entre la API de hoja de precios y la API RateCard?
Estas API ofrecen conjuntos similares de datos, pero tienen distintas audiencias. A continuación se proporciona información.

- Price Sheet API: [Price Sheet API](https://docs.microsoft.com/rest/api/consumption/pricesheet) proporciona los precios personalizados que se han negociado para un cliente Enterprise.
- RateCard API: [RateCard API](https://msdn.microsoft.com/library/mt219005.aspx) proporciona los precios públicos que se aplican a los clientes de Web Direct.

## <a name="next-steps"></a>Pasos siguientes

- Para información sobre el uso de API de Azure para consultar información detallada mediante programación, consulte [Información general sobre la API de consumo de Azure](billing-consumption-api-overview.md) e [Información general sobre la API de facturación](billing-usage-rate-card-overview.md).
- Para comparar la factura con el archivo de uso diario detallado y los informes de administración de costos en Azure Portal, consulte [Comprender la factura de Microsoft Azure](billing-understand-your-bill.md)
- Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
