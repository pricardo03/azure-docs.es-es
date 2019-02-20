---
title: Descripción de los datos de Azure Cost Management | Microsoft Docs
description: Este artículo le ayudará a comprender mejor qué datos se incluyen en Azure Cost Management y con qué frecuencia se procesan, recopilan, muestran y cierran.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 02/07/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: 79cf839a23d41058e0d9583a378dc063cd220136
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2019
ms.locfileid: "55981764"
---
# <a name="understand-cost-management-data"></a>Descripción de los datos de Cost Management

Este artículo le ayudará a comprender mejor qué datos se incluyen en Azure Cost Management. En él también se explica con qué frecuencia se procesan, se recopilan, se muestran y se cierran los datos. Cada mes, se le factura el uso que haga de Azure. Sin embargo, el tipo de suscripción de Azure determina cuándo finaliza el mes de facturación. La frecuencia con que Cost Management recibe datos de uso varía en función de diferentes factores. Por ejemplo, cuánto tarda el procesamiento de los datos y cada cuánto transmiten los servicios de Azure el uso al sistema de facturación.

## <a name="supported-microsoft-offers"></a>Ofertas de Microsoft compatibles

La siguiente información muestra las [ofertas de Microsoft Azure](https://azure.microsoft.com/support/legal/offer-details/) compatibles actualmente con Azure Cost Management.  Una oferta de Azure es el tipo de la suscripción a Azure que tiene.

| Categoría  | **Nombre de la oferta** | **Número de la oferta** |
| --- | --- | --- |
| **Azure Alemania** | [Pago por uso de Azure Germany](https://azure.microsoft.com/offers/ms-azr-de-0003p/) | MS-AZR-DE-0003P |
| **Azure Government** | Azure Government Enterprise | MS-AZR-USGOV-0017P |
| **Contrato Enterprise (EA)** | Desarrollo/pruebas - Enterprise | MS-AZR-0148P |
| **Contrato Enterprise (EA)** | [Microsoft Azure Enterprise](https://azure.microsoft.com/offers/enterprise-agreement-support-upgrade/) | MS-AZR-0017P |
| **Microsoft Developer Network (MSDN)** | [Plataformas de MSDN](https://azure.microsoft.com/offers/ms-azr-0062p/) | MS-AZR-0062P |
| **Pay-As-You-Go** | [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | MS-AZR-0003P |
| **Pay-As-You-Go** | [Desarrollo/pruebas - Pago por uso](https://azure.microsoft.com/offers/ms-azr-0023p/) | MS-AZR-0023P |
| **Pay-As-You-Go** | [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p/) | MS-AZR-0025P |
| **Pay-As-You-Go** | [prueba gratuita](https://azure.microsoft.com/offers/ms-azr-0044p/) | MS-AZR-0044P |
| **Pay-As-You-Go** | [Azure bajo licencia Open](https://azure.microsoft.com/offers/ms-azr-0111p/) | MS-AZR-0111P |
| **Pay-As-You-Go** | [Azure for Students](https://azure.microsoft.com/offers/ms-azr-0170p/) | MS-AZR-0170P |
| **Pay-As-You-Go** | Pase para Azure | MS-AZR-0120P, MS-AZR-0122P - MS-AZR-0125P, MS-AZR-0128P - MS-AZR-0130P |
| **Visual Studio** | [Visual Studio Enterprise – MPN](https://azure.microsoft.com/offers/ms-azr-0029p/) | MS-AZR-0029P |
| **Visual Studio** | [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/) | MS-AZR-0059P |
| **Visual Studio** | [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/) | MS-AZR-0060P |
| **Visual Studio** | [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/) | MS-AZR-0063P |
| **Visual Studio** | [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/) | MS-AZR-0064P |

En la tabla siguiente se muestran las ofertas no compatibles.

| Categoría  | **Nombre de la oferta** | **Número de la oferta** |
| --- | --- | --- |
| **Proveedor de soluciones en la nube (CSP)** | Microsoft Azure | MS-AZR-0145P |
| **Proveedor de soluciones en la nube (CSP)** | Azure Government CSP | MS-AZR-USGOV-0145P |
| **Proveedor de soluciones en la nube (CSP)** | Azure Alemania en CSP para Microsoft Cloud Alemania | MS-AZR-DE-0145P |
| **Pay-As-You-Go** | Paquete de inicio de Azure for Students | MS-AZR-0144P |
| **Pay-As-You-Go** | [Patrocinio de Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0036p/) | MS-AZR-0036P |
| **Planes de soporte técnico** | Soporte técnico Standard | MS-AZR-0041P |
| **Planes de soporte técnico** | Soporte técnico Professional Direct | MS-AZR-0042P |
| **Planes de soporte técnico** | Soporte técnico Developer | MS-AZR-0043P |
| **Planes de soporte técnico** | Planes de soporte técnico de Alemania | MS-AZR-DE-0043P |
| **Planes de soporte técnico** | Soporte técnico Standard de Azure Government | MS-AZR-USGOV-0041P |
| **Planes de soporte técnico** | Soporte técnico Pro-Direct de Azure Government | MS-AZR-USGOV-0042P |
| **Planes de soporte técnico** | Soporte técnico para desarrolladores de Azure Government | MS-AZR-USGOV-0043P |

Si no ve los datos de una suscripción y desea determinar si tal suscripción se encuentra entre las ofertas admitidas, puede comprobar si realmente se admite. Para ello, inicie sesión en [Azure Portal](https://portal.azure.com). Luego, seleccione **Todos los servicios** en el panel del menú izquierdo. En la lista de servicios, seleccione **Suscripciones**. En el menú de la lista de suscripciones, elija la suscripción que desee comprobar. La suscripción se muestra en la pestaña Información general y puede ver la **Oferta** y el **Id. de oferta**. En la imagen siguiente se muestra un ejemplo:

![Ejemplo de la pestaña Información general de la suscripción mostrando la Oferta y el Id. de oferta](./media/understand-cost-mgt-data/offer-and-offer-id.png)

## <a name="costs-included-in-cost-management"></a>Costos incluidos en Cost Management

Las siguientes tablas muestran los datos que se incluyen o no se incluyen en Cost Management.

**Datos de costo y uso**

| **Se incluye** | **No se incluye** |
| --- | --- |
| Uso del servicio Azure <sup>1</sup> | Compras de reservas - Para más información, consulte [API para la automatización de reservas de Azure](../billing/billing-reservation-apis.md). |
| Uso de la oferta de Marketplace | Compras en Marketplace - Para obtener más información, consulte [Cargos por servicios de terceros](../billing/billing-understand-your-azure-marketplace-charges.md). |
|   | Cargos de soporte técnico - Para obtener más información, consulte [Explicación de los términos de facturación](../billing/billing-understand-your-invoice.md). |
|   | Impuestos - Para obtener más información, consulte [Explicación de los términos de facturación](../billing/billing-understand-your-invoice.md). |
|   | Créditos - Para obtener más información, consulte [Explicación de los términos de facturación](../billing/billing-understand-your-invoice.md). |

<sup>1</sup> El uso del servicio de Azure se basa en la reserva y la negociación de los precios.

**Metadata**

| **Se incluye** | **No se incluye** |
| --- | --- |
| Etiquetas de recursos <sup>2</sup> | Etiquetas de grupos de recursos |

<sup>2</sup> Las etiquetas de recursos se aplican mientras se transmite el uso de cada servicio y no están disponibles con carácter retroactivo para el historial del uso.

## <a name="rated-usage-data-refresh-schedule"></a>Programación de actualización de datos de uso valorados

Los datos de uso y costes están disponibles en Administración de costos + facturación en Azure Portal y [las API de apoyo](https://aka.ms/costmgmt/docs). Tenga en cuenta los puntos siguientes al revisar los costes:

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

## <a name="see-also"></a>Otras referencias

- Si aún no ha completado la primera guía rápida de Cost Management, léala en [Start analyzing costs](quick-acm-cost-analysis.md) (Comenzar a analizar los costos).
