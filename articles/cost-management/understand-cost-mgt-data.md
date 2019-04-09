---
title: Descripción de los datos de Azure Cost Management | Microsoft Docs
description: Este artículo le ayudará a comprender mejor qué datos se incluyen en Azure Cost Management y con qué frecuencia se procesan, recopilan, muestran y cierran.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 04/08/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: 1fc04ee953ea53f96adfd1e85c9a616c62268ade
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59282991"
---
# <a name="understand-cost-management-data"></a>Descripción de los datos de Cost Management

Este artículo le ayudará a comprender mejor qué datos se incluyen en Azure Cost Management. En él también se explica con qué frecuencia se procesan, se recopilan, se muestran y se cierran los datos. Cada mes, se le factura el uso que haga de Azure. Sin embargo, el tipo de suscripción de Azure determina cuándo finaliza el mes de facturación. La frecuencia con que Cost Management recibe datos de uso varía en función de diferentes factores. Por ejemplo, cuánto tarda el procesamiento de los datos y cada cuánto transmiten los servicios de Azure el uso al sistema de facturación.

## <a name="supported-microsoft-offers"></a>Ofertas de Microsoft compatibles

La siguiente información muestra las [ofertas de Microsoft Azure](https://azure.microsoft.com/support/legal/offer-details/) compatibles actualmente con Azure Cost Management.  Una oferta de Azure es el tipo de la suscripción a Azure que tiene.

| Categoría  | **Nombre de la oferta** | **Id. de cuota** | **Número de oferta** |
| --- | --- | --- | --- |
| **Azure Alemania** | [Azure Germany - Pago por uso](https://azure.microsoft.com/offers/ms-azr-de-0003p)      | PayAsYouGo_2014-09-01 | MS-AZR-DE-0003P |
| **Azure Government** | Azure Government Enterprise                                                         | EnterpriseAgreement_2014-09-01 | MS-AZR-USGOV-0017P |
| **Contrato Enterprise (EA)** | Desarrollo/pruebas - Enterprise                                                        | MSDNDevTest_2014-09-01 | MS-AZR-0148P |
| **Contrato Enterprise (EA)** | [Microsoft Azure Enterprise](https://azure.microsoft.com/offers/enterprise-agreement-support-upgrade) | EnterpriseAgreement_2014-09-01 | MS-AZR-0017P |
| **Contrato de cliente de Microsoft** | [Plan de Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0017g) | EnterpriseAgreement_2014-09-01 | N/D |
| **Contrato de cliente de Microsoft** | [Plan de Microsoft Azure para desarrollo y pruebas](https://azure.microsoft.com/offers/ms-azr-0148g)  | MSDNDevTest_2014-09-01 | N/D |
| **Microsoft Developer Network (MSDN)** | [MSDN Platforms](https://azure.microsoft.com/offers/ms-azr-0062p) | MSDN_2014-09-01 | MS-AZR-0062P |
| **Pay-As-You-Go** | [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p)                       | PayAsYouGo_2014-09-01 | MS-AZR-0003P |
| **Pay-As-You-Go** | [Desarrollo/pruebas - Pago por uso](https://azure.microsoft.com/offers/ms-azr-0023p)              | MSDNDevTest_2014-09-01 | MS-AZR-0023P |
| **Pay-As-You-Go** | [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p)           | MPN_2014-09-01 | MS-AZR-0025P |
| **Pay-As-You-Go** | [Versión de prueba gratuita](https://azure.microsoft.com/offers/ms-azr-0044p)                          | FreeTrial_2014-09-01 | MS-AZR-0044P |
| **Pay-As-You-Go** | [Azure bajo licencia Open](https://azure.microsoft.com/offers/ms-azr-0111p)                       | AzureInOpen_2014-09-01 | MS-AZR-0111P |
| **Pay-As-You-Go** | [Azure para estudiantes](https://azure.microsoft.com/offers/ms-azr-0170p)                  | AzureForStudents_2018-01-01 | MS-AZR-0170P |
| **Pay-As-You-Go** | Pase para Azure                                                                             | AzurePass_2014-09-01 | MS-AZR-0120P, MS-AZR-0122P - MS-AZR-0125P, MS-AZR-0128P - MS-AZR-0130P |
| **Visual Studio** | [Visual Studio Enterprise – MPN](https://azure.microsoft.com/offers/ms-azr-0029p)      | MPN_2014-09-01 | MS-AZR-0029P |
| **Visual Studio** | [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p)          | MSDN_2014-09-01 | MS-AZR-0059P |
| **Visual Studio** | [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p)     | MSDNDevTest_2014-09-01 | MS-AZR-0060P |
| **Visual Studio** | [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p)            | MSDN_2014-09-01 | MS-AZR-0063P |
| **Visual Studio** | [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p)  | MSDN_2014-09-01 | MS-AZR-0064P |

En la tabla siguiente se muestran las ofertas no compatibles.

| Categoría  | **Nombre de la oferta** | **Id. de cuota** | **Número de oferta** |
| --- | --- | --- | --- |
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

Los clientes con las categorías de oferta de pago por uso, MSDN y Visual Studio pueden encontrar los datos en Cost Management, a partir del 10/02/2018. Para obtener acceso a datos para su suscripción antes del 10/02/2018, puede usar el [centro de cuentas de Azure](https://account.azure.com/subscriptions) para descargar el uso de los detalles en un archivo CSV o puede usar el [API de detalles de uso](/rest/api/consumption/usagedetails).

## <a name="determine-your-offer-type"></a>Determinar el tipo de oferta
Si no ve los datos de una suscripción y desea determinar si tal suscripción se encuentra entre las ofertas admitidas, puede comprobar si realmente se admite. Para ello, inicie sesión en [Azure Portal](https://portal.azure.com). Luego, seleccione **Todos los servicios** en el panel del menú izquierdo. En la lista de servicios, seleccione **Suscripciones**. En el menú de la lista de suscripciones, elija la suscripción que desee comprobar. La suscripción se muestra en la pestaña Información general y puede ver la **Oferta** y el **Id. de oferta**. En la imagen siguiente se muestra un ejemplo:

![Ejemplo de la pestaña Información general de la suscripción mostrando la Oferta y el Id. de oferta](./media/understand-cost-mgt-data/offer-and-offer-id.png)

## <a name="costs-included-in-cost-management"></a>Costos incluidos en Cost Management

Las siguientes tablas muestran los datos que se incluyen o no se incluyen en Cost Management. Todos los costos se calculan hasta que se genera una factura. Costos que se muestran no incluyen créditos gratuitos y pagados por adelantado.

**Datos de uso y costo**

| **Se incluye** | **No se incluye** |
| --- | --- |
| Uso del servicio Azure <sup>1</sup> | Compras de reservas - Para más información, consulte [API para la automatización de reservas de Azure](../billing/billing-reservation-apis.md). |
| Uso de oferta de Marketplace<sup>2</sup> | Compras en Marketplace - Para obtener más información, consulte [Cargos por servicios de terceros](../billing/billing-understand-your-azure-marketplace-charges.md). |
|   | Cargos de soporte técnico - Para obtener más información, consulte [Explicación de los términos de facturación](../billing/billing-understand-your-invoice.md). |
|   | Impuestos - Para obtener más información, consulte [Explicación de los términos de facturación](../billing/billing-understand-your-invoice.md). |
|   | Créditos - Para obtener más información, consulte [Explicación de los términos de facturación](../billing/billing-understand-your-invoice.md). |

<sup>1</sup> El uso del servicio de Azure se basa en la reserva y la negociación de los precios.
<sup>2</sup> mercado que ofrece el uso no está disponible para pago por uso, MSDN, y Visual Studio se ofrece en este momento.

**Metadatos**

| **Se incluye** | **No se incluye** |
| --- | --- |
| Las etiquetas del recurso<sup>3</sup> | Etiquetas de grupos de recursos |

<sup>3</sup> etiquetas de recursos se aplican mientras se genera el uso de cada servicio y no están disponibles con carácter retroactivo a historial de uso.

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

Si usa las [API de Cost Management](https://aka.ms/costmgmt/docs), Power BI o Azure Portal para recuperar datos, es posible que los cargos del periodo de facturación actual se vuelvan a valorar y, consecuentemente, cambien, hasta que se cierre la factura.

## <a name="usage-data-update-frequency-varies"></a>La frecuencia de actualización de los datos de uso varía.

La disponibilidad de los datos de uso en que ha incurrido en Cost Management depende de un par de factores, como los siguientes:

- Frecuencia con que los servicios de Azure (por ejemplo, Storage, Compute, CDN y SQL) transmiten la utilización.
- El tiempo necesario para procesar los datos de uso a través del motor de valoración y las canalizaciones de administración de costos.

Algunos servicios transmiten la utilización con más frecuencia que otros. Por lo tanto, podría ver datos de Cost Management para algunos servicios antes que otros servicios que transmiten datos menos frecuentemente. Normalmente, el uso de servicios tarda 8-24 horas en aparecer en Cost Management. Tenga en cuenta que los datos de un mes abierto se actualizan a medida en que incurre en más uso porque las actualizaciones son acumulativas.

## <a name="see-also"></a>Vea también

- Si aún no ha completado la primera guía rápida de Cost Management, léala en [Start analyzing costs](quick-acm-cost-analysis.md) (Comenzar a analizar los costos).
