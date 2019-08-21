---
title: Descripción de los datos de Azure Cost Management | Microsoft Docs
description: Este artículo le ayudará a comprender mejor qué datos se incluyen en Azure Cost Management y con qué frecuencia se procesan, recopilan, muestran y cierran.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 07/01/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: e44acfc66a8faf76068af9c31233e89c45ee02f7
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/12/2019
ms.locfileid: "68953167"
---
# <a name="understand-cost-management-data"></a>Descripción de los datos de Cost Management

Este artículo lo ayudará a comprender mejor los datos de costo y uso de Azure que se incluyen en Azure Cost Management. En él se explica con qué frecuencia se procesan, se recopilan, se muestran y se cierran los datos. Cada mes, se le factura el uso que haga de Azure. Si bien los ciclos de facturación son períodos mensuales, las fechas de inicio y finalización de los ciclos varían según el tipo de suscripción. La frecuencia con que Cost Management recibe datos de uso varía en función de diferentes factores. Por ejemplo, cuánto tarda el procesamiento de los datos y cada cuánto transmiten los servicios de Azure el uso al sistema de facturación.

Cost Management incluye todo el uso y todas las compras, incluidas reservas y ofertas de terceros para cuentas de Contrato Enterprise (EA). Las cuentas de contrato de cliente de Microsoft (MCA) y las suscripciones individuales con tarifas de pago por uso solo incluyen el uso de los servicios de Azure y de Marketplace. No se incluyen los costos de soporte técnico ni otros costos. Los costos se calculan hasta que se genera una factura y no tienen en cuenta los créditos.

## <a name="supported-microsoft-azure-offers"></a>Ofertas compatibles de Microsoft Azure

La siguiente información muestra las [ofertas de Microsoft Azure](https://azure.microsoft.com/support/legal/offer-details/) compatibles actualmente con Azure Cost Management. Una oferta de Azure es el tipo de la suscripción a Azure que tiene. Los datos están disponibles en Cost Management a partir de la fecha **Datos disponibles desde**. Si una suscripción cambia las ofertas, los costos anteriores a la fecha de cambio de la oferta no estarán disponibles. 

| **Categoría**  | **Nombre de la oferta** | **Identificador de la cuota** | **Número de la oferta** | **Datos disponibles desde** |
| --- | --- | --- | --- | --- |
| **Azure Alemania** | [Pago por uso de Azure Germany](https://azure.microsoft.com/offers/ms-azr-de-0003p)      | PayAsYouGo_2014-09-01 | MS-AZR-DE-0003P | 2 de octubre de 2018<sup>2</sup> |
| **Azure Government** | Azure Government Enterprise                                                         | EnterpriseAgreement_2014-09-01 | MS-AZR-USGOV-0017P | Mayo de 2014<sup>1</sup> |
| **Contrato Enterprise (EA)** | Desarrollo/pruebas - Enterprise                                                        | MSDNDevTest_2014-09-01 | MS-AZR-0148P | Mayo de 2014<sup>1</sup> |
| **Contrato Enterprise (EA)** | [Microsoft Azure Enterprise](https://azure.microsoft.com/offers/enterprise-agreement-support-upgrade) | EnterpriseAgreement_2014-09-01 | MS-AZR-0017P | Mayo de 2014<sup>1</sup> |
| **Contrato de cliente de Microsoft** | [Plan de Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0017g) | EnterpriseAgreement_2014-09-01 | N/D | Marzo de 2019<sup>3</sup> |
| **Contrato de cliente de Microsoft** | [Plan de Microsoft Azure para Desarrollo/pruebas](https://azure.microsoft.com/offers/ms-azr-0148g) | MSDNDevTest_2014-09-01 | N/D | Marzo de 2019<sup>3</sup> |
| **Microsoft Developer Network (MSDN)** | [Plataformas de MSDN](https://azure.microsoft.com/offers/ms-azr-0062p)<sup>4</sup> | MSDN_2014-09-01 | MS-AZR-0062P | 2 de octubre de 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p)                  | PayAsYouGo_2014-09-01 | MS-AZR-0003P | 2 de octubre de 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Desarrollo/pruebas - Pago por uso](https://azure.microsoft.com/offers/ms-azr-0023p)         | MSDNDevTest_2014-09-01 | MS-AZR-0023P | 2 de octubre de 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p)      | MPN_2014-09-01 | MS-AZR-0025P | 2 de octubre de 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Evaluación gratuita](https://azure.microsoft.com/offers/ms-azr-0044p)<sup>4</sup>         | FreeTrial_2014-09-01 | MS-AZR-0044P | 2 de octubre de 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Azure bajo licencia Open](https://azure.microsoft.com/offers/ms-azr-0111p)<sup>4</sup>      | AzureInOpen_2014-09-01 | MS-AZR-0111P | 2 de octubre de 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Azure for Students](https://azure.microsoft.com/offers/ms-azr-0170p)<sup>4</sup> | AzureForStudents_2018-01-01 | MS-AZR-0170P | 2 de octubre de 2018<sup>2</sup> |
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

| Categoría  | **Nombre de la oferta** | **Identificador de la cuota** | **Número de la oferta** |
| --- | --- | --- | --- |
| **Azure Alemania** | [Pago por uso de Azure Germany](https://azure.microsoft.com/offers/ms-azr-de-0003p) | PayAsYouGo_2014-09-01 | MS-AZR-DE-0003P |
| **Proveedor de soluciones en la nube (CSP)** | Microsoft Azure                                    | CSP_2015-05-01 | MS-AZR-0145P |
| **Proveedor de soluciones en la nube (CSP)** | Azure Government CSP                               | CSP_2015-05-01 | MS-AZR-USGOV-0145P |
| **Proveedor de soluciones en la nube (CSP)** | Azure Alemania en CSP para Microsoft Cloud Alemania   | CSP_2015-05-01 | MS-AZR-DE-0145P |
| **Pay-As-You-Go**                 | Paquete de inicio de Azure for Students | DreamSpark_2015-02-01 | MS-AZR-0144P |
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
