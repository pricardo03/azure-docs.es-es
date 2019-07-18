---
title: Descripción del uso y los cargos detallados | Microsoft Docs
description: Aprenda a leer y comprender el uso y los cargos detallados
author: bandersmsft
manager: micflan
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/24/2019
ms.author: banders
ms.openlocfilehash: 2eb9f8e19be2a7b6220bc34bf4ce0c72c4ac0b4f
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275058"
---
# <a name="understand-the-terms-in-your-azure-usage-and-charges-file"></a>Comprenda los términos en el archivo de uso y cargos de Azure

El archivo de uso y cargos detallados contiene el uso valorado diario según las tarifas negociadas, las compras (por ejemplo, reservas de direcciones, precios de Marketplace) y los reembolsos durante el período especificado.
Los precios no incluyen créditos, impuestos ni otros cargos o descuentos.
En la tabla siguiente se cubren los cargos que se incluyen para cada tipo de cuenta.

Tipo de cuenta | Uso de Azure | Uso de Marketplace | Compras | Reembolsos
--- | --- | --- | --- | ---
Contrato Enterprise (EA) | Sí | Sí | Sí | Sin
Contrato de cliente de Microsoft (MCA) | Sí | Sí | Sí | Sí
Pay-as-you-go (PAYG) | Sí | No | No | Sin

Para más información sobre los pedidos de Marketplace (también conocidos como servicios externos), consulte [Descripción de los gastos de servicios externos de Azure](billing-understand-your-azure-marketplace-charges.md).

Para obtener instrucciones de descarga, consulte [Obtención de los datos de uso diario y la factura de Azure](billing-download-azure-invoice-daily-usage-date.md).
El archivo de uso y cargos está disponible en formato de valores separados por comas (.csv), que puede abrir en una aplicación de hoja de cálculo.

## <a name="list-of-terms-and-descriptions"></a>Lista de términos y descripciones

En la tabla siguiente se describen los términos importantes que se usan en la versión más reciente del archivo de uso y cargos de Azure.
En la lista se incluyen las cuentas de pago por uso, Contrato Enterprise (EA) y Contrato de cliente de Microsoft (MCA).

Término | Tipo de cuenta | DESCRIPCIÓN
--- | --- | ---
AccountName | EA | Nombre para mostrar de la cuenta de inscripción.
AccountOwnerId | EA | Identificador único para la cuenta de inscripción.
AdditionalInfo | Todo | Metadatos específicos del servicio. Por ejemplo, un tipo de imagen de una máquina virtual.
BillingAccountId | EA, MCA | Identificador único de la cuenta de facturación raíz.
BillingAccountName | EA, MCA | Nombre de la cuenta de facturación.
BillingCurrency | EA, MCA | Moneda asociada con la cuenta de facturación.
BillingPeriod | EA | El período de facturación del cargo.
BillingPeriodEndDate | EA, MCA | La fecha de finalización del período de facturación.
BillingPeriodStartDate | EA, MCA | La fecha de finalización del período de facturación.
BillingProfileId | EA, MCA | Identificador único de la inscripción de EA o el perfil de facturación de MCA.
BillingProfileName | EA, MCA | Nombre de la inscripción de EA o el perfil de facturación de MCA.
ChargeType | EA, MCA | Indica si el cargo representa la utilización (**uso**), una compra (**Compra**) o un reembolso (**Reembolso**).
ConsumedQuantity | PAYG | Véase Cantidad.
ConsumedService | Todo | Nombre del servicio al que está asociado el cargo.
Coste | EA | Véase CostInBillingCurrency.
CostCenter | EA, MCA | Centro de coste definido en la suscripción para el seguimiento de los costes (disponible solo en períodos de facturación abiertos para cuentas de MCA).
CostInBillingCurrency | MCA | Coste del cargo en la moneda de facturación antes de créditos o impuestos.
CostInPricingCurrency | MCA | Coste del cargo en la moneda de precios antes de créditos o impuestos.
Moneda | PAYG | Véase BillingCurrency.
Date | EA, MCA | La fecha de utilización o compra del cargo.
ExchangeRateDate | MCA | Fecha en la que se estableció el tipo de cambio.
ExchangeRatePricingToBilling | MCA | Tipo de cambio que se usa para convertir el coste en la moneda de precios a la moneda de facturación.
Frecuencia | EA, MCA | Indica si se espera que un cargo se repita. Los cargos pueden producirse una vez (**Una vez**), repetirse mensual o anualmente (**Periódico**) o basado en la utilización (**Basado en uso**).
IncludedQuantity | PAYG | El importe del medidor incluido sin cargo en el período de facturación actual.
InstanceId | PAGY | Véase ResourceId.
InvoiceId | EA, MCA | Identificador único del id. de documento que se muestra en el archivo PDF de la factura.
InvoiceSection | MCA | Véase InvoiceSectionName.
InvoiceSectionId | EA, MCA | Identificador único para el departamento de EA o la sección de la factura de MCA.
InvoiceSectionName | EA, MCA | Nombre del departamento de EA o la sección de la factura de MCA.
IsAzureCreditEligible | EA, MCA | Indica si el pago del cargo es elegible por usar créditos de Azure (valores: True, False).
IsEstimated | Todo | Indica si el período de facturación está cerrado o finalizado. Los datos de utilización pueden cambiar a lo largo del período de facturación hasta que se genere la factura. Estos registros se marcan como "estimado".
Location | EA, MCA | Ubicación del centro de datos donde se ejecuta el recurso.
MeterCategory | Todo | Nombre de la categoría de clasificación para el medidor. Por ejemplo, *Servicios en la nube* o *Redes*.
Id. del medidor | Todo | Identificador único del medidor.
MeterName | Todo | Nombre del medidor.
MeterRegion | Todo | Nombre de la ubicación del centro de datos para los servicios cuyo precio se basa en la ubicación. Véase Ubicación.
MeterSubCategory | Todo | Nombre de la categoría de subclasificación del medidor.
OfferId | EA, MCA | Nombre de la oferta comprada.
PartNumber | EA | Identificador que se usa para obtener los precios específicos de medidor.
PlanName | EA | Nombre del plan de Marketplace.
PreviousInvoiceId | MCA | Referencia a una factura original si este elemento de línea es un reembolso.
PricingCurrency | MCA | Moneda que se usa cuando la clasificación se basa en precios negociados.
Producto | MCA | Véase ProductName.
ProductId | EA, MCA | Identificador único del producto.
ProductName | EA | Nombre del producto.
ProductOrderId | EA, MCA | Identificador único del pedido de producto.
ProductOrderName | EA, MCA | Nombre único para el pedido del producto.
PublisherName | EA, MCA | Editor de los servicios de Marketplace.
PublisherType | EA, MCA | Tipo de editor (valores: firstParty, thirdPartyReseller, thirdPartyAgency).
Cantidad | EA, MCA | Número de unidades compradas o consumidas.
Tarifa | PAYG | Véase UnitPrice.
ReservationId | EA, MCA | Identificador único de la instancia de la reserva comprada.
ReservationName | EA, MCA | Nombre de la instancia de la reserva comprada.
ResourceGroupId | EA, MCA | Identificador único del [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) en el que se encuentra el recurso.
ResourceGroupName | EA, MCA | Nombre del [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) en el que se encuentra el recurso.
ResourceId | EA, MCA | Identificador único del recurso de [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/resources).
ResourceLocation | EA, MCA | Ubicación del centro de datos donde se ejecuta el recurso. Véase Ubicación.
ResourceName | EA | Nombre del recurso.
ResourceType | MCA | Tipo de instancia del recurso.
ServiceFamily | EA, MCA | Familia de servicios a la que pertenece el servicio.
ServiceInfo1 | Todo | Metadatos específicos del servicio.
ServiceInfo2 | Todo | Campo heredado con metadatos específicos del servicio opcional.
ServicePeriodEndDate | MCA | Fecha de finalización del período de calificación que ha definido y bloqueado los precios para el servicio consumido o adquirido.
ServicePeriodStartDate | MCA | Fecha de inicio del período de calificación que ha definido y bloqueado los precios para el servicio consumido o adquirido.
SubscriptionId | Todo | Identificador único de la suscripción.
SubscriptionName | Todo | Nombre de la suscripción.
Etiquetas | Todo | Etiquetas asignadas al recurso. No incluye etiquetas de grupo de recursos. Se puede usar para agrupar o distribuir los costes para el contracargo interno. Para más información, consulte [Organize your Azure resources with tags](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) (Organización de los recursos de Azure con etiquetas).
Unidad | PAYG | Véase UnitOfMeasure.
UnitOfMeasure | Todo | Unidad de medida para facturar por el servicio Por ejemplo, los servicios de proceso se facturan por hora.
UnitPrice | EA | El precio unitario del cargo.
UsageDate | PAYG | Véase Fecha.

Tenga en cuenta que algunos campos pueden diferir en cuanto a mayúsculas, minúsculas y espacios entre un tipo de cuenta y otro.
Las versiones anteriores de archivos de utilización de pago por uso tienen secciones independientes para el resumen de cuenta y el uso diario.

## <a name="ensure-that-your-charges-are-correct"></a>Asegurarse de que los cargos sean correctos

Para más información sobre la utilización y los cargos detallados, lea la descripción de la factura de [pago por uso](./billing-understand-your-bill.md) o [contrato del cliente de Microsoft](billing-mca-understand-your-bill.md).

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- [Visualización y descarga de la factura de Microsoft Azure](billing-download-azure-invoice.md)
- [Visualización y descarga de los datos de uso y los cargos de Microsoft Azure](billing-download-azure-daily-usage.md)
