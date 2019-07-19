---
title: Correo electrónico de saldo vencido de Azure | Microsoft Docs
description: Describe cómo realizar el pago si la suscripción de Azure tiene un saldo pendiente
services: ''
documentationcenter: ''
author: genlin
manager: jureid
editor: ''
tags: billing
ms.assetid: d0b88c92-fb9d-4d12-931b-c26104ad63e9
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: banders
ms.openlocfilehash: f58969b167b6c0f0a66d46731ad76c1f6e9acc41
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491450"
---
# <a name="resolve-past-due-balance-for-your-azure-subscription"></a>Saldo de la suscripción de Azure vencido

Si no se recibe su pago o no puede procesarse, es posible que reciba un correo electrónico o vea una alerta en el Centro de cuentas de Azure o en Azure Portal.
Si es el [administrador de cuenta](billing-subscription-transfer.md#whoisaa), puede liquidar los cargos pendientes en [Azure Portal](https://portal.azure.com). Si se encuentra en un modo de factura de pago, envíe el pago a la ubicación indicada en la parte inferior de la factura.

> [!IMPORTANT]
> * Si tiene varias suscripciones que utilizan la misma tarjeta de crédito y todas están vencidas, deberá pagar todo el saldo pendiente a la vez.
> * El instrumento de pago que se utilice para liquidar los cargos pendientes se convertirá en el nuevo método de pago activo para todas las suscripciones que estaban usando el método de pago que produjo los errores.

## <a name="resolve-past-due-balance-in-the-azure-portal"></a>Resolución del saldo pendiente pasado en Azure Portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de la cuenta.
1. Busque **Administración de costos + facturación**.
1. En la página Información general, verá una lista de las suscripciones. Si el estado de la suscripción está vencido, haga clic en el enlace **Liquidar saldo**.
    ![Captura de pantalla que muestra el enlace Liquidar saldo](./media/billing-azure-subscription-past-due-balance/settle-balance-entry-point.png)
1. El saldo total pendiente refleja los cargos pendientes de todos los servicios de Microsoft que utilizaban el método de pago que produjo los errores.
1. Seleccione un método de pago para pagar el saldo. Este método de pago se convertirá en el método de pago activo para todas las suscripciones que actualmente usan el método de pago con errores.
    ![Captura de pantalla que muestra el enlace Seleccionar método de pago](./media/billing-azure-subscription-past-due-balance/settle-balance-screen.png)
1. Si el método de pago seleccionado también tiene cargos pendientes de servicios de Microsoft, esto se reflejará en el saldo pendiente total. Deberá pagar esos cargos pendientes también.
1. Haga clic en **Pagar**.

## <a name="not-getting-billing-email-notifications"></a>¿No se reciben notificaciones de correo electrónico de facturación?

Si es el administrador de la cuenta, [compruebe qué dirección de correo electrónico se utiliza para las notificaciones](billing-how-to-change-azure-account-profile.md). Se recomienda que use una dirección de correo electrónico que consulte periódicamente. Si el correo electrónico es el adecuado, compruebe la carpeta de correo basura.

## <a name="if-i-forget-to-pay-what-happens"></a>¿Qué ocurre si se olvida de pagar?

Se cancelará el servicio y los recursos ya no estarán disponibles. Los datos de Azure se eliminarán a los 90 días de terminarse el servicio. Para más información, consulte el [Microsoft Trust Center: How we manage your data](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409) (Centro de confianza de Microsoft: Administración de los datos).

Si tiene la certeza de que se ha procesado el pago pero la suscripción sigue deshabilitada, póngase en contacto con el [Soporte técnico de Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, cree una [solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).
