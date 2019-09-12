---
title: Descripción de los términos de la hoja de precios para un contrato de cliente de Microsoft en Azure
description: Aprenda a leer y entender el uso y la factura de un contrato de cliente de Microsoft.
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 4d83228fbec395d604e5ce3f988d2a6157f21eed
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "67490666"
---
# <a name="terms-in-your-microsoft-customer-agreement-price-sheet"></a>Términos de la hoja de precios del contrato de cliente de Microsoft

Este artículo se aplica a una cuenta de facturación de Azure de un contrato de cliente de Microsoft. [Compruebe si tiene acceso a un contrato de cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement).

Si es el propietario, colaborador, lector o administrador de facturación de un perfil de facturación, puede descargar la hoja de precios de la organización desde Azure Portal. Consulte [Visualización y descarga de los precios de la organización](billing-ea-pricing.md).

## <a name="terms-and-descriptions-in-your-price-sheet"></a>Términos y descripciones de la hoja de precios

En la siguiente sección se describen los términos importantes que aparecen en la hoja de precios del contrato de cliente de Microsoft.

| **Nombre del campo**   | **Descripción**   |
| --- | --- |
| billingAccountId  | Identificador único de la cuenta de facturación.   |
| billingAccountName  | Nombre de la cuenta de facturación.  |
| billingProfileId  | Identificador único del perfil de facturación.   |
| billingProfileName  | Nombre del perfil de facturación que está configurado para recibir facturas. Los precios de la hoja de precios se asocian con este perfil de facturación. |
| productOrderName  | Nombre del plan del producto comprado. |
| serviceFamily  | Tipo de servicio de Azure. Por ejemplo: proceso, análisis o seguridad |
| Producto  | Nombre del producto que acumula los cargos, por ejemplo: base de datos SQL básica frente a base de datos SQL estándar  |
| productId  | Identificador único del producto cuyo medidor se consume. |
| unitOfMeasure  | Identifica las unidades de medida para la facturación del servicio. Por ejemplo, los servicios de proceso se facturan por hora. |
| meterId  | Identificador único del medidor. |
| meterName  | Nombre del medidor. El medidor representa el recurso de un servicio de Azure que se pueden implementar. |
| meterCategory  | Nombre de la categoría de clasificación para el medidor. Por ejemplo, _servicios en la nube_ o _redes_. |
| meterType  |  Nombre del tipo de medidor. |
| meterSubCategory  | Nombre de la categoría de subclasificación del medidor.  |
| meterRegion  | Nombre de la región donde está disponible el medidor para el servicio. Identifica la ubicación del centro de datos para ciertos servicios cuyos precios se establecen según la ubicación del centro de datos.    |
| tierId  | Identifica el plan de tarifa cuando proceda. Esto funciona conjuntamente con tierMinimumUnits para establecer precios por niveles cuando los precios varían en función del número de unidades consumidas.    |
| tierMinimumUnits  | Define el límite inferior del intervalo de nivel para el que se definen los precios. Por ejemplo, si el intervalo es de 0 a 100, tierMinimumUnits sería 0.  |
| effectiveStartDate  | Fecha de inicio en la que entra en vigor el precio. |
| effectiveEndDate  | Fecha de finalización del precio efectivo. |
| unitPrice  | Precio por unidad en el momento de la facturación (no el precio combinado efectivo) como específico para un medidor y el nombre del pedido del producto.  Nota: El precio unitario no es el mismo que el precio efectivo en las descargas de detalles de uso, en el caso de servicios que tienen precios diferentes entre niveles.  En el caso de los servicios con precios de varios niveles, el precio efectivo es una tarifa mixta en todos los niveles y no muestra un precio unitario específico por nivel. El precio combinado o precio efectivo es el precio neto de la cantidad consumida que se extiende a lo largo de los distintos niveles (donde cada nivel tiene un precio unitario específico). |
| basePrice  | El precio de mercado en el momento en que el cliente firma o el precio de mercado en el momento en que se inicia el medidor de servicio, si es después del inicio de sesión.   |

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobación del acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- [Visualización y descarga de los precios de la organización](billing-ea-pricing.md)
