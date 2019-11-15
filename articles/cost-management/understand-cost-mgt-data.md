---
title: Descripción de los datos de Azure Cost Management | Microsoft Docs
description: Este artículo le ayudará a comprender mejor qué datos se incluyen en Azure Cost Management y con qué frecuencia se procesan, recopilan, muestran y cierran.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/06/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: 681ccc768b1fa3d5a968847d11987fbd83898b59
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73721374"
---
# <a name="understand-cost-management-data"></a>Descripción de los datos de Cost Management

Este artículo lo ayudará a comprender mejor los datos de costo y uso de Azure que se incluyen en Azure Cost Management. En él se explica con qué frecuencia se procesan, se recopilan, se muestran y se cierran los datos. Cada mes, se le factura el uso que haga de Azure. Si bien los ciclos de facturación son períodos mensuales, las fechas de inicio y finalización de los ciclos varían según el tipo de suscripción. La frecuencia con que Cost Management recibe datos de uso varía en función de diferentes factores. Por ejemplo, cuánto tarda el procesamiento de los datos y cada cuánto transmiten los servicios de Azure el uso al sistema de facturación.

Cost Management incluye todo el uso y todas las compras, incluidas reservas y ofertas de terceros para cuentas de Contrato Enterprise (EA). Las cuentas de Contrato de cliente de Microsoft y las suscripciones individuales con tarifas de pago por uso solo incluyen el uso de los servicios de Azure y Marketplace. No se incluyen los costos de soporte técnico ni otros costos. Los costos se calculan hasta que se genera una factura y no tienen en cuenta los créditos.

## <a name="supported-microsoft-azure-offers"></a>Ofertas compatibles de Microsoft Azure

La siguiente información muestra las [ofertas de Microsoft Azure](https://azure.microsoft.com/support/legal/offer-details/) compatibles actualmente con Azure Cost Management. Una oferta de Azure es el tipo de la suscripción a Azure que tiene. Los datos están disponibles en Cost Management a partir de la fecha **Datos disponibles desde**. Si una suscripción cambia las ofertas, los costos anteriores a la fecha de cambio de la oferta no estarán disponibles.

| **Categoría**  | **Nombre de la oferta** | **Identificador de la cuota** | **Número de la oferta** | **Datos disponibles desde** |
| --- | --- | --- | --- | --- |
| **Azure Government** | Azure Government Enterprise                                                         | EnterpriseAgreement_2014-09-01 | MS-AZR-USGOV-0017P | Mayo de 2014<sup>1</sup> |
| **Contrato Enterprise (EA)** | Desarrollo/pruebas - Enterprise                                                        | MSDNDevTest_2014-09-01 | MS-AZR-0148P | Mayo de 2014<sup>1</sup> |
| **Contrato Enterprise (EA)** | [Microsoft Azure Enterprise](https://azure.microsoft.com/offers/enterprise-agreement-support-upgrade) | EnterpriseAgreement_2014-09-01 | MS-AZR-0017P | Mayo de 2014<sup>1</sup> |
| **Contrato de cliente de Microsoft** | [Plan de Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0017g) | EnterpriseAgreement_2014-09-01 | N/D | Marzo de 2019<sup>3</sup> |
| **Contrato de cliente de Microsoft** | [Plan de Microsoft Azure para Desarrollo/pruebas](https://azure.microsoft.com/offers/ms-azr-0148g) | MSDNDevTest_2014-09-01 | N/D | Marzo de 2019<sup>3</sup> |
| **Contrato de cliente de Microsoft admitido por los asociados** | Plan de Microsoft Azure | CSP_2015-05-01, CSP_MG_2017-12-01 y CSPDEVTEST_2018-05-01<br><br>El identificador de cuota se reutiliza para el Contrato de cliente de Microsoft y las suscripciones de CSP heredadas. Actualmente, solo se admiten las suscripciones de Contrato de cliente de Microsoft. | N/D | Octubre de 2019 |
| **Microsoft Developer Network (MSDN)** | [Plataformas de MSDN](https://azure.microsoft.com/offers/ms-azr-0062p)<sup>4</sup> | MSDN_2014-09-01 | MS-AZR-0062P | 2 de octubre de 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p)                  | PayAsYouGo_2014-09-01 | MS-AZR-0003P | 2 de octubre de 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Desarrollo/pruebas - Pago por uso](https://azure.microsoft.com/offers/ms-azr-0023p)         | MSDNDevTest_2014-09-01 | MS-AZR-0023P | 2 de octubre de 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p)      | MPN_2014-09-01 | MS-AZR-0025P | 2 de octubre de 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Evaluación gratuita](https://azure.microsoft.com/offers/ms-azr-0044p)<sup>4</sup>         | FreeTrial_2014-09-01 | MS-AZR-0044P | 2 de octubre de 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Azure bajo licencia Open](https://azure.microsoft.com/offers/ms-azr-0111p)<sup>4</sup>      | AzureInOpen_2014-09-01 | MS-AZR-0111P | 2 de octubre de 2018<sup>2</sup> |
| **Pay-As-You-Go** | Pase para Azure<sup>4</sup>                                                            | AzurePass_2014-09-01 | MS-AZR-0120P, MS-AZR-0122P - MS-AZR-0125P, MS-AZR-0128P - MS-AZR-0130P | 2 de octubre de 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Enterprise – MPN](https://azure.microsoft.com/offers/ms-azr-0029p)<sup>4</sup>     | MPN_2014-09-01 | MS-AZR-0029P | 2 de octubre de 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p)<sup>4</sup>         | MSDN_2014-09-01 | MS-AZR-0059P | 2 de octubre de 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p)<sup>4</sup>    | MSDNDevTest_2014-09-01 | MS-AZR-0060P | 2 de octubre de 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p)<sup>4</sup>           | MSDN_2014-09-01 | MS-AZR-0063P | 2 de octubre de 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p)<sup>4</sup> | MSDN_2014-09-01 | MS-AZR-0064P | 2 de octubre de 2018<sup>2</sup> |

_<sup>**1**</sup> Para los datos antes de mayo de 2014, visite [Azure Enterprise Portal](https://ea.azure.com)._

_<sup>**2**</sup> Para los datos antes del 2 de octubre de 2018, visite el [Centro de cuentas de Azure](https://account.azure.com/subscriptions)._

_<sup>**3**</sup> Los contratos de cliente de Microsoft comenzaron en marzo de 2019 y no tienen ningún dato histórico antes de este punto._

_<sup>**4**</sup> Es posible que los datos históricos de las suscripciones basadas en crédito y pagadas por adelantado no coincidan con la factura. Consulte [Los datos históricos pueden no coincidir con la factura](#historical-data-might-not-match-invoice) a continuación._

Las siguientes ofertas todavía no se admiten:

| Category  | **Nombre de la oferta** | **Identificador de la cuota** | **Número de la oferta** |
| --- | --- | --- | --- |
| **Azure Alemania** | [Pago por uso de Azure Germany](https://azure.microsoft.com/offers/ms-azr-de-0003p) | PayAsYouGo_2014-09-01 | MS-AZR-DE-0003P |
| **Proveedor de soluciones en la nube (CSP)** | Microsoft Azure                                    | CSP_2015-05-01 | MS-AZR-0145P |
| **Proveedor de soluciones en la nube (CSP)** | Azure Government CSP                               | CSP_2015-05-01 | MS-AZR-USGOV-0145P |
| **Proveedor de soluciones en la nube (CSP)** | Azure Alemania en CSP para Microsoft Cloud Alemania   | CSP_2015-05-01 | MS-AZR-DE-0145P |
| **Pay-As-You-Go**                 | Paquete de inicio de Azure for Students | DreamSpark_2015-02-01 | MS-AZR-0144P |
| **Pay-As-You-Go** | [Azure for Students](https://azure.microsoft.com/offers/ms-azr-0170p)<sup>4</sup> | AzureForStudents_2018-01-01 | MS-AZR-0170P |
| **Pay-As-You-Go**                 | [Patrocinio de Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0036p/) | Sponsored_2016-01-01 | MS-AZR-0036P |
| **Planes de soporte técnico** | Soporte técnico Standard                    | Default_2014-09-01 | MS-AZR-0041P |
| **Planes de soporte técnico** | Soporte técnico Professional Direct         | Default_2014-09-01 | MS-AZR-0042P |
| **Planes de soporte técnico** | Soporte técnico Developer                   | Default_2014-09-01 | MS-AZR-0043P |
| **Planes de soporte técnico** | Planes de soporte técnico de Alemania                | Default_2014-09-01 | MS-AZR-DE-0043P |
| **Planes de soporte técnico** | Soporte técnico Standard de Azure Government   | Default_2014-09-01 | MS-AZR-USGOV-0041P |
| **Planes de soporte técnico** | Soporte técnico Pro-Direct de Azure Government | Default_2014-09-01 | MS-AZR-USGOV-0042P |
| **Planes de soporte técnico** | Soporte técnico para desarrolladores de Azure Government  | Default_2014-09-01 | MS-AZR-USGOV-0043P |

## <a name="determine-your-offer-type"></a>Determinación del tipo de oferta
Si no ve los datos de una suscripción y desea determinar si tal suscripción se encuentra entre las ofertas admitidas, puede comprobar si realmente se admite. Para ello, inicie sesión en [Azure Portal](https://portal.azure.com). Luego, seleccione **Todos los servicios** en el panel del menú izquierdo. En la lista de servicios, seleccione **Suscripciones**. En el menú de la lista de suscripciones, elija la suscripción que desee comprobar. La suscripción se muestra en la pestaña Información general y puede ver la **Oferta** y el **Id. de oferta**. En la imagen siguiente se muestra un ejemplo:

![Ejemplo de la pestaña Información general de la suscripción mostrando la Oferta y el Id. de oferta](./media/understand-cost-mgt-data/offer-and-offer-id.png)

## <a name="costs-included-in-cost-management"></a>Costos incluidos en Cost Management

Las siguientes tablas muestran los datos que se incluyen o no se incluyen en Cost Management. Todos los costos se calculan hasta que se genera una factura. Los costos que se muestran no incluyen los créditos gratis ni los pagados por adelantado.

**Datos de costo y uso**

| **Se incluye** | **No se incluye** |
| --- | --- |
| Uso del servicio de Azure<sup>5</sup>        | Cargos de soporte técnico - Para obtener más información, consulte [Explicación de los términos de facturación](../billing/billing-understand-your-invoice.md). |
| Uso de la oferta de Marketplace<sup>6</sup> | Impuestos - Para obtener más información, consulte [Explicación de los términos de facturación](../billing/billing-understand-your-invoice.md). |
| Compras de Marketplace<sup>6</sup>      | Créditos - Para obtener más información, consulte [Explicación de los términos de facturación](../billing/billing-understand-your-invoice.md). |
| Compras de reserva<sup>7</sup>      |  |
| Amortización de las compras de reserva<sup>7</sup>      |  |

_<sup>**5**</sup> El uso del servicio de Azure se basa en la reserva y la negociación de los precios._

_<sup>**6**</sup> Las compras de Marketplace no están disponible para las ofertas de pago por uso, MSDN ni Visual Studio por el momento._

_<sup>**7**</sup> En este momento, las compras de reserva solo están disponibles para las cuentas de Contrato Enterprise (EA)._

**Metadata**

| **Se incluye** | **No se incluye** |
| --- | --- |
| Etiquetas de recurso<sup>8</sup> | Etiquetas de grupos de recursos |

_<sup>**8**</sup> Las etiquetas de recursos se aplican mientras se transmite el uso de cada servicio y no están disponibles con carácter retroactivo para el historial del uso._

## <a name="rated-usage-data-refresh-schedule"></a>Programación de actualización de datos de uso valorados

Los datos de uso y costes están disponibles en Administración de costos + facturación en Azure Portal y [las API de apoyo](index.yml). Tenga en cuenta los puntos siguientes al revisar los costes:

- La estimación de los cargos para el período de facturación actual se actualiza seis veces al día.
- La estimación de los cargos para el período de facturación actual puede cambiar según se incurre en un uso mayor.
- Cada actualización es acumulativa e incluye todos los elementos de línea y la información de la actualización anterior.
- Azure finaliza o _cierra_ el período de facturación actual hasta 72 horas (tres días naturales) después de finalizado el período de facturación.

Los ejemplos siguientes muestran cómo podrían terminar los períodos de facturación.

Suscripciones del contrato Enterprise (EA): si el mes de facturación termina el 31 de marzo, la estimación de cargos se actualiza hasta 72 horas más tarde. En este ejemplo, a la medianoche (UTC) del 4 de abril.

Suscripciones de pago por uso: si el mes de facturación termina el 15 de mayo, la estimación de cargos podría actualizarse hasta 72 horas más tarde. En este ejemplo, a la medianoche (UTC) del 19 de mayo.

### <a name="rerated-data"></a>Nueva valoración de los datos

Si usa las [API de Cost Management](index.yml), Power BI o Azure Portal para recuperar datos, es posible que los cargos del período de facturación actual se vuelvan a valorar y, consecuentemente, cambien hasta que se cierre la factura.

## <a name="cost-management-data-fields"></a>Campos de datos de Cost Management

Los siguientes campos de datos se encuentran en los archivos de detalles de uso y las API de Cost Management. En los siguientes campos en negrita, los asociados pueden usar las características de filtro y agrupar por en el análisis de costos para analizar los costos por varios campos. Los campos en negrita solo se aplican a los Contratos de cliente de Microsoft admitidos por los asociados.

| **Nombre del campo** | **Descripción** |
| --- | --- |
| invoiceId | Identificador de factura que aparece en la factura de la transacción específica. |
| previousInvoiceID | Referencia a una factura original si hay un reembolso (costo negativo). Solo se rellena si hay un reembolso. |
| billingAccountName | Nombre de la cuenta de facturación que representa al asociado. Acumula todos los costos de los clientes que se han incorporado a un Contrato de cliente de Microsoft y los clientes de CSP que han realizado compras de derechos, como SaaS y Azure Marketplace, y reservas. |
| billingAccountID | Identificador de la cuenta de facturación que representa al asociado. |
| billingProfileID | Identificador del perfil de facturación que agrupa los costos de las facturas en una única moneda de facturación de los clientes que se han incorporado a un Contrato de cliente de Microsoft y los clientes de CSP que han realizado compras de derechos, como SaaS y Azure Marketplace, y reservas. |
| billingProfileName | Nombre del perfil de facturación que agrupa los costos de las facturas en una única moneda de facturación de los clientes que se han incorporado a un Contrato de cliente de Microsoft y los clientes de CSP que han realizado compras de derechos, como SaaS y Azure Marketplace, y reservas. |
| invoiceSectionName | Nombre del proyecto que se va a cargar en la factura. No se aplica a los Contratos de cliente de Microsoft a los que se han incorporado los asociados. |
| invoiceSectionID | Identificador del proyecto que se va a cargar en la factura. No se aplica a los Contratos de cliente de Microsoft a los que se han incorporado los asociados. |
| **CustomerTenantID** | Identificador del inquilino de Azure Active Directory de la suscripción del cliente. |
| **CustomerName** | Nombre del inquilino de Azure Active Directory de la suscripción del cliente. |
| **CustomerTenantDomainName** | Nombre de dominio del inquilino de Azure Active Directory de la suscripción del cliente. |
| **PartnerTenantID** | Identificador del inquilino de Azure Active Directory del asociado. |
| **PartnerName** | Nombre del inquilino de Azure Active Directory del asociado. |
| **ResellerMPNID** | MPNID del revendedor asociado a la suscripción. |
| costCenter | Centro de costo asociado a la suscripción. |
| billingPeriodStartDate | Fecha de inicio del período de facturación, como se muestra en la factura. |
| billingPeriodEndDate | Fecha de finalización del período de facturación, como se muestra en la factura. |
| servicePeriodStartDate | Fecha de inicio del período de valoración en que se han valorado los cargos por el uso del servicio. Los precios de los servicios de Azure se determinan según el período de valoración. |
| servicePeriodEndDate | Fecha de finalización del período en que se han valorado los cargos por el uso del servicio. Los precios de los servicios de Azure se determinan en función del período de valoración. |
| date | En el caso de los datos de consumo de Azure, muestra la fecha de uso. En el caso de la instancia reservada, muestra la fecha de compra. En el caso de cargos periódicos y cargos puntuales como Marketplace y soporte técnico, muestra la fecha de compra. |
| productID | Identificador del producto que ha acumulado cargos por consumo o compra. Es la clave concatenada de productID y SKuID, como se muestra en el Centro de partners. |
| product | Nombre del producto que ha acumulado cargos por consumo o compra, como se muestra en la factura. |
| serviceFamily | Muestra la familia de servicios del producto comprado o cargado. Por ejemplo, Storage o Compute. |
| productOrderID | Identificador del recurso o nombre del plan de Azure al que pertenece la suscripción. Por ejemplo, plan de Azure. |
| productOrderName | Nombre del plan de Azure al que pertenece la suscripción. Por ejemplo, plan de Azure. |
| consumedService | Servicio consumido (taxonomía heredada) tal como se usa en los detalles de uso de EA heredados. |
| meterID | Identificador medido para el consumo medido. |
| meterName | Identifica el nombre del medidor para el consumo medido. |
| meterCategory | Identifica el servicio de nivel superior para el uso. |
| meterSubCategory | Define el tipo o subcategoría de servicio de Azure que puede afectar a la tarifa. |
| meterRegion | Identifica la ubicación del centro de datos para ciertos servicios cuyos precios se establecen según la ubicación del centro de datos. |
| Id. de suscripción | Identificador único generado por Microsoft para la suscripción de Azure. |
| subscriptionName | Nombre de la suscripción de Azure. |
| Término | Muestra el plazo de validez de la oferta. Por ejemplo, las instancias reservadas muestran 12 meses de un plazo anual de la instancia reservada. Para compras de una sola vez o compras periódicas, el término muestra un mes para SaaS, Azure Marketplace y soporte técnico. Esto no se aplica al consumo de Azure. |
| publisherType (firstParty, thirdPartyReseller, thirdPartyAgency) | Tipo de anunciante que identifica al anunciante como primera entidad, revendedor de terceros o agencia de terceros. |
| partNumber | Número de pieza de la instancia reservada sin usar y los servicios de Azure Marketplace. |
| publisherName | Nombre del anunciante del servicio, incluidos Microsoft o los anunciantes ajenos. |
| reservationId | Identificador de la compra de instancia reservada. |
| reservationName | Nombre de la instancia reservada. |
| reservationOrderId | Identificador de pedido de la instancia reservada. |
| frequency | Frecuencia de pago de una instancia reservada. |
| resourceGroup | Nombre del grupo de recursos de Azure usado para la administración de recursos de ciclo de vida. |
| instanceID (o) ResourceID | Identificador de la instancia del recurso. |
| resourceLocation | Nombre de la ubicación del recurso. |
| Location | Ubicación normalizada del recurso. |
| effectivePrice | Precio por unidad efectivo del servicio en la moneda de determinación del precio. Es único para cada producto, familia de servicios, medidor y oferta. Se usa con los precios de la hoja de precios de la cuenta de facturación. Cuando hay precios por niveles o una cantidad incluida, muestra el precio combinado por consumo. |
| Cantidad | Cantidad medida comprada o consumida. La cantidad del medidor usado durante el período de facturación. |
| unitOfMeasure | Identifica la unidad en que se cobra el servicio. Por ejemplo, GB y horas. |
| pricingCurrency | Moneda que define el precio unitario. |
| billingCurrency | Moneda que define el costo facturado. |
| chargeType | Define el tipo de cargo que representa el costo en Azure Cost Management, como compra y reembolso. |
| costinBillingCurrency | Costo combinado o ampliado en la moneda facturada. |
| costinPricingCurrency | Costo combinado o ampliado antes de impuestos en la moneda de los precios para poner en correlación con estos. |
| **costinUSD** | Costo combinado o ampliado estimado antes de impuestos en USD. |
| **paygCostInBillingCurrency** | Muestra los costos si los precios están en los precios minoristas. Muestra los precios de pago por uso en la moneda de facturación. Solo está disponible en los ámbitos de RBAC. |
| **paygCostInUSD** | Muestra los costos si los precios están en los precios minoristas. Muestra los precios de pago por uso en USD. Solo está disponible en los ámbitos de RBAC. |
| exchangeRate | Tipo de cambio que se usa para convertir la moneda de los precios a la moneda de facturación. |
| exchangeRateDate | Fecha del tipo de cambio que se usa para convertir la moneda de los precios a la moneda de facturación. |
| isAzureCreditEligible | Indica si el costo es válido para el pago mediante créditos de Azure. |
| serviceInfo1 | Campo heredado que captura los metadatos específicos del servicio opcional. |
| serviceInfo2 | Campo heredado que captura los metadatos específicos del servicio opcional. |
| additionalInfo | Metadatos específicos del servicio. Por ejemplo, un tipo de imagen de una máquina virtual. |
| etiquetas | Etiqueta que se asigna al medidor. Use etiquetas para agrupar los registros de facturación. Por ejemplo, puede usar etiquetas para distribuir los costos por el departamento que utiliza el medidor. |
| **partnerEarnedCreditRate** | Tarifa de descuento aplicada si hay un crédito ganado por el asociado (PEC) basado en el acceso al vínculo de administrador del asociado. |
| **partnerEarnedCreditApplied** | Indica si se ha aplicado el crédito ganado por el asociado. |


## <a name="usage-data-update-frequency-varies"></a>La frecuencia de actualización de los datos de uso varía.

La disponibilidad de los datos de uso en que ha incurrido en Cost Management depende de un par de factores, como los siguientes:

- Frecuencia con que los servicios de Azure (por ejemplo, Storage, Compute, CDN y SQL) transmiten la utilización.
- El tiempo necesario para procesar los datos de uso a través del motor de valoración y las canalizaciones de administración de costos.

Algunos servicios transmiten la utilización con más frecuencia que otros. Por lo tanto, podría ver datos de Cost Management para algunos servicios antes que otros servicios que transmiten datos menos frecuentemente. Normalmente, el uso de servicios tarda 8-24 horas en aparecer en Cost Management. Tenga en cuenta que los datos de un mes abierto se actualizan a medida en que incurre en más uso porque las actualizaciones son acumulativas.

## <a name="historical-data-might-not-match-invoice"></a>Los datos históricos pueden no coincidir con la factura

Es posible que los datos históricos de las ofertas basadas en crédito y pagadas por adelantado no coincidan con la factura. Algunas ofertas de pago por uso, MSDN y Visual Studio para Azure pueden tener créditos de Azure y pagos por adelantado aplicados en la factura. Sin embargo, los datos históricos que se muestran en Cost Management se basan solo en los cargos de consumo calculados. Los datos históricos de Cost Management no incluyen pagos ni créditos. Como resultado, es posible que los datos históricos que se muestran para estas ofertas no coincidan exactamente con la factura.

- Azure for Students (MS-AZR-0170P)
- Azure bajo licencia Open (MS-AZR-0111P)
- Pase para Azure (MS-AZR-0120P, MS-AZR-0123P, MS-AZR-0125P, MS-AZR-0128P, MS-AZR-0129P)
- Evaluación gratuita (MS-AZR-0044P)
- MSDN (MS-AZR-0062P)
- Visual Studio (MS-AZR-0029P, MS-AZR-0059P, MS-AZR-0060P, MS-AZR-0063P, MS-AZR-0064P)

## <a name="see-also"></a>Otras referencias

- Si aún no ha completado la primera guía rápida de Cost Management, léala en [Start analyzing costs](quick-acm-cost-analysis.md) (Comenzar a analizar los costos).
