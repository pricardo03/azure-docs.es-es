---
title: Comprender los cargos y el uso detallado | Microsoft Docs
description: Obtenga información sobre cómo leer y comprender los cargos y el uso detallado
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
ms.openlocfilehash: 9ff9b6b5313026d2102b98659183fa97c6a5ef84
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64683986"
---
# <a name="understand-the-terms-in-your-azure-usage-and-charges-file"></a>Comprender los términos en el archivo de uso y los cargos de Azure

El archivo de uso y los cargos detallado contiene uso valorada diaria según las tarifas negociadas, compras (por ejemplo, las reservas de direcciones, las cuotas de Marketplace) y reembolsos durante el período especificado.
Tarifas no incluyen créditos, impuestos, u otros cargos o descuentos.
La tabla siguiente se cubre qué cargos se incluyen para cada tipo de cuenta.

Tipo de cuenta | Uso de Azure | Uso de Marketplace | Compras | Reembolsos
--- | --- | --- | --- | ---
Contrato Enterprise (EA) | Sí | Sí | Sí | Sin 
Contrato de cliente de Microsoft (MCA) | Sí | Sí | Sí | Sí
Pay-as-you-go (PAYG) | Sí | No | No | Sin 

Para obtener más información sobre pedidos de Marketplace (también conocidos como servicios externos), consulte [comprender los gastos de servicios externos Azure](billing-understand-your-azure-marketplace-charges.md).

Consulte [cómo obtener los datos de uso diario y la factura de facturación de Azure](billing-download-azure-invoice-daily-usage-date.md) para las instrucciones de descarga.
El archivo de uso y los cargos está disponible en un formato de archivo de valores separados por comas (.csv), que puede abrir en una aplicación de hoja de cálculo.

## <a name="list-of-terms-and-descriptions"></a>Lista de términos y descripciones

En la tabla siguiente se describe los términos importantes usados en la versión más reciente del archivo de uso y los cargos de Azure.
La lista trata pago por uso (PAYG), el contrato Enterprise (EA) y las cuentas de contrato de cliente de Microsoft (MCA).

Término | Tipo de cuenta | DESCRIPCIÓN
--- | --- | ---
AccountName | EA | Nombre para mostrar de la cuenta de inscripción.
Id. del propietario de la cuenta | EA | Identificador único para la cuenta de inscripción.
Información adicional | Todo | Metadatos específicos del servicio. Por ejemplo, un tipo de imagen de una máquina virtual.
BillingAccountId | EA, MCA | Identificador único para la raíz de la cuenta de facturación.
BillingAccountName | EA, MCA | Nombre de la cuenta de facturación.
BillingCurrency | EA, MCA | Moneda asociada con la cuenta de facturación.
BillingPeriod | EA | El período de facturación del cargo.
BillingPeriodEndDate | EA, MCA | La fecha de finalización del período de facturación.
BillingPeriodStartDate | EA, MCA | La fecha de inicio del período de facturación.
BillingProfileId | EA, MCA | Identificador único de la inscripción de EA o MCA el perfil de facturación.
BillingProfileName | EA, MCA | Nombre de la inscripción de EA o MCA el perfil de facturación.
ChargeType | EA, MCA | Indica si el gasto representa uso (**uso**), una compra (**comprar**), o un reembolso (**reembolso**).
ConsumedQuantity | PAYG | Ver la cantidad.
Servicio consumido | Todo | Nombre del servicio, el cargo está asociado.
Coste | EA | Consulte CostInBillingCurrency.
CostCenter | EA, MCA | El centro de costo definido para la suscripción para el seguimiento de los costos (disponibles solo en períodos de facturación abiertos para las cuentas MCA).
CostInBillingCurrency | MCA | Costo del gasto en la moneda de facturación antes de créditos o impuestos.
CostInPricingCurrency | MCA | Costo del gasto en la moneda precios antes de créditos o impuestos.
Moneda | PAYG | Consulte BillingCurrency.
Date | EA, MCA | La fecha del cargo de uso o la compra.
ExchangeRateDate | MCA | Fecha en que se estableció la tasa de cambio.
ExchangeRatePricingToBilling | MCA | Tasa de cambio que se usa para convertir el costo en la moneda de precios en la moneda de facturación.
Frecuencia | EA, MCA | Indica si se espera un cargo a repetir. Cargos cualquiera sucede una vez (**OneTime**), repetición mensual o anual (**periódica**), o basarse en uso (**UsageBased**).
IncludedQuantity | PAYG | La cantidad del medidor que se incluye sin cargo en el período de facturación actual.
InstanceId | PAGY | Consulte ResourceId.
InvoiceId | EA, MCA | El identificador único del documento aparecen en la factura PDF.
InvoiceSection | MCA | Consulte InvoiceSectionName.
InvoiceSectionId | EA, MCA | Identificador único para el departamento de EA o la sección de la factura MCA.
InvoiceSectionName | EA, MCA | Nombre del departamento de EA o la sección de factura MCA.
IsAzureCreditEligible | EA, MCA | Indica si el cargo es elegible para pagar por usar créditos de Azure (valores: True, False).
Location | EA, MCA | Ubicación de centro de datos donde se ejecuta el recurso.
Categoría del medidor | Todo | Nombre de la categoría de clasificación para el medidor. Por ejemplo, *servicios en la nube* y *redes*.
Id. del medidor | Todo | El identificador único para el medidor.
Nombre del medidor | Todo | El nombre del medidor.
Región del medidor | Todo | Nombre de la ubicación de centro de datos de servicios cuyo precio se basa en la ubicación. Ver la ubicación.
Subcategoría del medidor | Todo | Nombre de la categoría de medidor subclasificación.
OfferId | EA, MCA | Nombre de la oferta adquirida.
PartNumber | EA | Identificador utilizado para obtener los precios específico medidor.
PlanName | EA | Nombre del plan de Marketplace.
PreviousInvoiceId | MCA | Referencia a una factura original si este elemento es un reembolso.
pricingCurrency | MCA | Moneda usada cuando la clasificación se basa en los precios negociados.
Producto | MCA | Vea ProductName.
ProductId | EA, MCA | Identificador único para el producto.
ProductName | EA | Nombre del producto.
ProductOrderId | EA, MCA | Identificador único para el pedido de producto.
ProductOrderName | EA, MCA | Nombre único para el pedido de producto.
PublisherName | EA, MCA | Publicador para los servicios de Marketplace.
PublisherType | EA, MCA | Tipo de publicador (valores: firstParty, thirdPartyReseller, thirdPartyAgency).
Cantidad | EA, MCA | El número de unidades adquiridas o consumidos.
Tarifa | PAYG | Consulte UnitPrice.
ReservationId | EA, MCA | Identificador único para la instancia de reserva que adquirió.
ReservationName | EA, MCA | Nombre de la instancia de reserva que adquirió.
ResourceGroupId | EA, MCA | Identificador único para el [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) el recurso está en.
ResourceGroupName | EA, MCA | Nombre de la [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) el recurso está en.
ResourceId | EA, MCA | Identificador único de la [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/resources) recursos.
Ubicación de los recursos | EA, MCA | Ubicación de centro de datos donde se ejecuta el recurso. Ver la ubicación.
ResourceName | EA | Nombre del recurso.
ResourceType | MCA | Tipo de instancia del recurso.
ServiceFamily | EA, MCA | Familia de servicio al que pertenece el servicio.
Información del servicio 1 | Todo | Metadatos específicos del servicio.
Información del servicio 2 | Todo | Campo heredado con metadatos específicos del servicio opcional.
ServicePeriodEndDate | MCA | La fecha de finalización del período de clasificación que define y bloquea los precios del servicio consumido o comprado.
ServicePeriodStartDate | MCA | La fecha de inicio del período de clasificación que define y bloquea los precios del servicio consumido o comprado.
SubscriptionId | Todo | Identificador único para la suscripción.
SubscriptionName | Todo | Nombre de la suscripción.
Etiquetas | Todo | Etiquetas asignadas a los recursos. No incluye etiquetas de grupo de recursos. Puede utilizarse para agrupar o distribuir los costos para la anulación interna. Para más información, consulte [Organize your Azure resources with tags](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) (Organización de los recursos de Azure con etiquetas).
Unidad | PAYG | Ver la unidad de medida.
Unidad de medida | Todo | La unidad de medida para la facturación para el servicio. Por ejemplo, los servicios de proceso se facturan por hora.
precio por unidad | EA | El precio unitario de la carga.
UsageDate | PAYG | Ver la fecha.

Tenga en cuenta que algunos campos pueden diferir en las mayúsculas y minúsculas y el espaciado entre tipos de cuenta.
Las versiones anteriores de archivos de uso de pago por uso tienen secciones independientes para la instrucción y el uso diario.

## <a name="ensure-that-your-charges-are-correct"></a>Asegúrese de que los cargos son correctos

Para obtener más información sobre los cargos y el uso detallado, lea sobre cómo comprender su [pago por uso](./billing-understand-your-bill.md) o [contrato del cliente de Microsoft](billing-mca-understand-your-bill.md) factura.

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- [Ver y descargar la factura de Microsoft Azure](billing-download-azure-invoice.md)
- [Ver y descargar los cargos y el uso de Microsoft Azure](billing-download-azure-daily-usage.md)
