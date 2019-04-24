---
title: Comprender los términos en la hoja de precios para un contrato de cliente de Microsoft - Azure | Microsoft Docs
description: Aprenda a leer y entender el uso y la factura de la suscripción de Azure
services: ''
documentationcenter: ''
author: jureid
manager: jureid
editor: ''
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: eb6184e10d38cdcfad7070663e36f6610d009cdb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60371366"
---
# <a name="understand-the-terms-in-your-price-sheet-for-a-microsoft-customer-agreement"></a>Comprender los términos en la hoja de precios para un contrato de cliente de Microsoft

En este artículo se aplica a una cuenta de facturación para un contrato de cliente de Microsoft. [Compruebe si tiene acceso a un contrato de cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement).

Si es un perfil de facturación propietario, Colaborador, lector o administrador de facturas, puede descargar la hoja de precios de su organización desde el portal de Azure. Consulte [ver y descargar su organización de precios](billing-ea-pricing.md).

## <a name="detailed-terms-and-descriptions-in-your-microsoft-customer-agreement-price-sheet"></a>Términos y descripciones en la hoja de precios de contrato de cliente de Microsoft detallados

La siguiente sección describe los términos importantes que se muestra en la hoja de precios de contrato de cliente de Microsoft.

| **Nombre del campo**   | **Descripción**   |
| --- | --- |
| billingAccountId  | Identificador único para la cuenta de facturación.   |
| billingAccountName  | Nombre de la cuenta de facturación.  |
| billingProfileId  | Identificador único para el perfil de facturación.   |
| billingProfileName  | Nombre del perfil de facturación que está configurado para recibir facturas. Los precios en la hoja de precios están asociados con este perfil de facturación. |
| productOrderName  | Nombre del plan de producto comprado. |
| serviceFamily  | Tipo de servicio de Azure. P. ej.: COMPUTE, análisis, seguridad |
| Producto  | Nombre del producto que se acumularán los cargos. P. ej.: Implementación básica de base de datos SQL estándar SQL DB  |
| productId  | Identificador único para el producto cuyo medidor se consume. |
| unitOfMeasure  | Identifica las unidades de medida para la facturación para el servicio. Por ejemplo, los servicios de proceso se facturan por hora. |
| meterId  | Identificador único para el medidor. |
| meterName  | Nombre del medidor. El medidor representa el recurso que se pueden implementar de un servicio de Azure. |
| meterCategory  | Nombre de la categoría de clasificación para el medidor. Por ejemplo, _servicios en la nube_, _redes_, etcetera. |
| meterType  |  Nombre del tipo de medidor. |
| meterSubCategory  | Nombre de la categoría de medidor subclasificación.  |
| meterRegion  | Nombre de la región donde el medidor para el servicio está disponible. Identifica la ubicación del centro de datos para ciertos servicios cuyos precios se establecen según la ubicación del centro de datos.    |
| tierId  | Identifica el plan de tarifa cuando sea aplicable. Esto funciona conjuntamente con tierMinimumUnits para establecer en niveles precios cuando los precios varían en función del número de unidades usadas.    |
| tierMinimumUnits  | Define el límite inferior del intervalo de nivel para el que se definen los precios. Por ejemplo, si el intervalo es de 0 a 100, tierMinimumUnits sería 0.  |
| effectiveStartDate  | Cuando el precio se hace efectivo de fecha de inicio. |
| effectiveEndDate  | Fecha de finalización del precio efectivo. |
| unitPrice  | Precio por unidad en el momento de facturación (no el precio combinado efectivo) lo más específico a un nombre de medidor y el producto pedido.  Nota: El precio unitario no es el mismo como el precio efectivo en los detalles de uso de descargas en el caso de los servicios que tienen precios diferenciales entre niveles.  En el caso de servicios con varios niveles de precios, el precio efectivo es una tarifa combinada en todos los niveles y no muestra un precio de venta específicos de la capa. El precio combinada o precio efectivo es el precio neto de la expansión de cantidad consumida a través de varios niveles (donde cada nivel tiene un precio de venta específico). |
| basePrice  | El precio de mercado en el momento en que el cliente inicia sesión o el precio de mercado en el momento en el medidor de servicio se inicia si ha finalizado el inicio de sesión.   |

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobar el acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [crear una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- [Ver y descargar los precios de su organización](billing-ea-pricing.md)
