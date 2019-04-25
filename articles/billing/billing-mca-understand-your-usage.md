---
title: Descripción de los términos en los CSV de cargos y el uso de Azure para un contrato de cliente de Microsoft | Microsoft Docs
description: Obtenga información sobre cómo leer y entender las secciones del uso de Azure y los cargos de CSV para el perfil de facturación
services: ''
documentationcenter: ''
author: bandersmsft
manager: alherz
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: banders
ms.openlocfilehash: 8f71f42386ce49d4d7178cb03d28d74edacd7e39
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60371315"
---
# <a name="understand-terms-on-your-azure-usage-and-charges-csv-for-a-microsoft-customer-agreement"></a>Descripción de los términos en los CSV de cargos y el uso de Azure para un contrato de cliente de Microsoft

En este artículo se aplica a una cuenta de facturación para un contrato de cliente de Microsoft. [Compruebe si tiene acceso a un contrato de cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement).

El archivo CSV de uso y los cargos de Azure contiene los cargos de nivel de uso diario y por medidor del período de facturación actual.

Para obtener el archivo de uso y los cargos de Azure, consulte [ver y descargar uso de Azure y los cargos de su contrato de cliente de Microsoft](billing-download-azure-daily-usage.md).
Está disponible en un formato de archivo de valores separados por comas (.csv) que se puede abrir en una aplicación de hoja de cálculo.

Los cargos de uso son los cargos **mensuales** totales de una suscripción. Los cargos de uso no tienen en cuenta los créditos o descuentos.

## <a name="changes-in-the-enterprise-agreement-azure-usage-and-charges-csv"></a>Cambios en el CSV de cargos y el uso de Azure para contratos Enterprise

Si fuese un cliente EA, observará que los términos en el perfil de facturación archivo CSV de uso de Azure son diferentes de los términos en el archivo CSV de uso de EA Azure. Esta es una asignación de términos de uso EA en términos de uso de perfil de facturación:

| Uso de Azure EA CSV | Uso de contrato de cliente de Microsoft Azure y los cargos de CSV |
| --- | --- |
| Date | date |
| Mes| date |
| Día | date |
| Year | date |
| Producto | product |
| Id. del medidor | meterID |
| Categoría del medidor | meterCategory |
| Subcategoría del medidor | meterSubCategory |
| Región del medidor | meterRegion |
| Nombre del medidor | meterName |
| ConsumedQuantity | quantity |
| Tasa de recursos | effectivePrice | <!-- this was highlighted -->
| Coste ampliado | cost |
| Ubicación de los recursos | resourceLocation |
| Servicio consumido | consumedService |
| InstanceId | instanceId |
| Información del servicio 1 | serviceInfo1 |
| Información del servicio 2 | serviceInfo2 |
| Información adicional | additionalInfo |
| Etiquetas | etiquetas |
| Identificador del servicio del almacén | N/D |
| DepartmentName | invoiceSection | <!-- this was highlighted -->
| CostCenter | costCenter |
| Unidad de medida | unidad de medida |
| ResourceGroup | resourceGroup |
| ChargesBilledSeparately | isAzureCreditEligible | <!-- this was highlighted -->

<!-- TO DO: Marketplace CSV? -->

## <a name="detailed-terms-and-descriptions-in-your-azure-usage-and-charges-file"></a>Términos y descripciones de los archivos de Azure de uso y los cargos detallados

La siguiente sección describe los términos importantes que se muestra en el archivo de uso y los cargos de Azure.

Término | DESCRIPCIÓN
--- | ---
invoiceId | El identificador único del documento aparecen en la factura PDF
previousInvoiceId | Referencia a una factura original si este elemento es un reembolso
billingAccountName | Nombre de la cuenta de facturación
billingAccountId | Identificador único para la raíz de la cuenta de facturación
billingProfileId | Nombre del perfil de facturación que se acumularán los cargos que se facturan
billingProfileName | Identificador único para el perfil de facturación que se acumularán los cargos que se facturan
invoiceSectionId | Identificador único para la sección de factura
invoiceSectionName | Nombre de la sección de factura
costCenter | El centro de costo definido en la suscripción para el seguimiento de los costos (disponibles solo en períodos de facturación abiertos)
billingPeriodStartDate | La fecha de inicio del período de facturación para el que se genera la factura
billingPeriodEndDate | La fecha de finalización del período de facturación para el que se genera la factura
servicePeriodStartDate | La fecha de inicio del período de evaluación que tiene definido y bloquea los precios del servicio consumido o adquirido
servicePeriodEndDate | La fecha de finalización del período de evaluación que tiene definido y bloquea los precios del servicio consumido o adquirido
date | Para Azure y Marketplace cargos de basada en uso, esta es la fecha de clasificación. Para las compras de un solo uso (las reservas de direcciones, Marketplace) o los cargos periódicos fijos (ofertas de soporte técnico), esta es la fecha de compra.
serviceFamily | Familia de servicio que pertenece el servicio
productOrderId | Identificador único para el pedido de producto
productOrderName | Nombre único para el pedido de producto
consumedService | Nombre del servicio consumido
meterId | El identificador único para el medidor
meterName | El nombre del medidor
meterCategory | Nombre de la categoría de clasificación para el medidor. Por ejemplo, *servicios en la nube*, *redes*, etcetera.
meterSubCategory | Nombre de la categoría de medidor subclasificación
meterRegion | Nombre de la región donde el medidor para el servicio está disponible. Identifica la ubicación del centro de datos para ciertos servicios cuyos precios se establecen según la ubicación del centro de datos.
offer | Nombre de la oferta adquirida
productId | Identificador único para el producto que se acumularán los cargos
product | Nombre del producto que se acumularán los cargos
Id. de suscripción | Identificador único para la suscripción que se acumularán los cargos
subscriptionName | Nombre de la suscripción que se acumularán los cargos
reservationId | Identificador único para la instancia de la reserva adquirida
reservationName | Nombre de la instancia de la reserva adquirida
publisherType | Tipo de publicador (valores: firstParty, thirdPartyReseller, thirdPartyAgency)
publisherName | Publicador para los servicios de Marketplace
resourceGroupId | Identificador único para el grupo de recursos asociado al recurso
resourceGroupName | Nombre del grupo de recursos asociado al recurso
resourceId | Identificador único para la instancia del recurso
resourceType | Tipo de instancia del recurso
resourceLocation | Identifica la ubicación del centro de datos donde se ejecuta el recurso.
location | Ubicación normalizada del recurso si se configuran las ubicaciones de recursos diferente para las mismas regiones
quantity | El número de unidades adquiridas o consumidos
unitOfMeasure | La unidad de medida para la facturación para el servicio. Por ejemplo, los servicios de proceso se facturan por hora.
chargeType | El tipo de gasto. Valores: <ul><li>AsCharged-Usage: Gastos acumulados en función del uso de un servicio de Azure. Esto incluye el uso con máquinas virtuales que no se le cobrará por las instancias reservadas.</li><li>AsCharged-PurchaseMarketplace: Un solo uso o fijos los cargos periódicos de las compras de Marketplace</li><li>AsCharged-UsageMarketplace: Cargos para los servicios de Marketplace que se cobran en función de las unidades de consumo</li></ul>
isAzureCreditEligible | Marca que indica si la carga en el servicio es elegible para pagar por usar créditos de Azure (valores: True, False)
serviceInfo1 | Metadatos específicos del servicio
serviceInfo2 | Campo heredado que captura los metadatos específicos del servicio opcional.
additionalInfo | Metadatos específicos del servicio adicional.
etiquetas | Etiquetas que se asigna al recurso

### <a name="how-do-i-make-sure-that-the-charges-in-my-azure-usage-and-charges-file-are-correct"></a>¿Cómo asegurarme de que los cargos de mi archivo de uso y los cargos de Azure son correctos?

Si hay un cargo en el archivo de uso detallado que le gustaría obtener más información, consulte [comprender los cargos de facturación de su perfil de facturación](billing-mca-understand-your-bill.md)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobar el acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [crear una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- [Ver y descargar la factura de Microsoft Azure](billing-download-azure-invoice.md)
- [Ver y descargar los cargos y el uso de Microsoft Azure](billing-download-azure-daily-usage.md)
