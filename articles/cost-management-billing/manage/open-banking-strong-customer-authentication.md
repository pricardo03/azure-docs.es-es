---
title: Banca abierta (PSD2) y autenticación segura de clientes (SCA) para los clientes de Azure
description: En este artículo se explica por qué se necesita la autenticación multifactor en algunas compras de Azure y cómo realizar la autenticación.
author: bandersmsft
manager: jureid
tags: billing
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/10/2019
ms.author: banders
ms.openlocfilehash: aff5e1e707980c9a63988c22653fa5fba0fc236b
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "75986904"
---
# <a name="open-banking-psd2-and-strong-customer-authentication-sca-for-azure-customers"></a>Banca abierta (PSD2) y autenticación segura de clientes (SCA) para los clientes de Azure

A partir del 14 de septiembre de 2019, los bancos de 31 países del [Espacio Económico Europao](https://en.wikipedia.org/wiki/European_Economic_Area) están obligados a comprobar la identidad de todas las personas que realizan compras en línea antes de procesar el pago. Esta comprobación requiere la autenticación multifactor para ayudar a garantizar que las compras en línea son seguras y están protegidas. La fecha de este requisito de comprobación se retrasará en algunos países. Para más información, consulte las [P+F de Microsoft sobre PSD2](https://support.microsoft.com/en-us/help/4517854?preview).

## <a name="what-psd2-means-for-azure-customers"></a>Qué significa PSD2 para los clientes de Azure

Si paga por Azure con una tarjeta de crédito que ha emitido un banco del [espacio económico Europao](https://en.wikipedia.org/wiki/European_Economic_Area), podría estar obligado a realizar la autenticación multifactor para el método de pago de su cuenta. Puede que se le pida que lleve a cabo el desafío de autenticación multifactor al registrar o actualizar su cuenta de Azure, incluso si no realiza una compra en ese momento. También es posible que se le pida que proporcione autenticación multifactor al cambiar el método de pago de su cuenta de Azure, quitar el límite de gasto o realizar un pago inmediato desde Azure Portal, como la liquidación de saldos pendientes o la compra de créditos de Azure.

Si su banco rechaza los cargos mensuales de Azure, recibirá un correo electrónico de vencimiento de Azure con instrucciones para solucionarlo. Puede realizar el desafío de autenticación multifactor y liquidar los cargos pendientes en Azure Portal.

## <a name="complete-multi-factor-authentication-in-the-azure-portal"></a>Realización de la autenticación multifactor en Azure Portal

En las secciones siguientes se describe cómo realizar la autenticación multifactor en Azure Portal. Use la información que le sea aplicable.

### <a name="change-the-active-payment-method-of-your-azure-account"></a>Cambio del método de pago activo de la cuenta de Azure

Puede cambiar el método de pago activo de la cuenta de Azure mediante estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de la cuenta y vaya a **Administración de costos + facturación**.
2. En la página **Información general**, seleccione la suscripción correspondiente en la cuadrícula **Mis suscripciones**.
3. En "Facturación", seleccione **Métodos de pago**. Puede agregar una nueva tarjeta de crédito o establecer una tarjeta existente como método de pago activo de la suscripción. Si su banco exige la autenticación multifactor, se le pedirá que realice un desafío de autenticación durante el proceso.

Para más información, consulte [Agregar, actualizar o quitar una tarjeta de crédito para Azure](change-credit-card.md).

### <a name="settle-outstanding-charges-for-azure-services"></a>Liquidación de los cargos pendientes de los servicios de Azure

Si el banco rechaza los cargos, el estado de la cuenta de Azure cambiará a **Vencida** en Azure Portal. Puede comprobar el estado de su cuenta mediante estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador de la cuenta.
2. Busque en **Administración de costos + facturación**.
3. En la página **Información general** de **Administración de costos + facturación**, revise la columna de estado de la cuadrícula **Mis suscripciones**.
4. Si en la suscripción pone **Vencida**, haga clic en **Liquidar saldo**. Durante el proceso, se le pedirá que realice la autenticación multifactor.

### <a name="settle-outstanding-charges-for-marketplace-and-reservation-purchases"></a>Liquidación de cargos pendientes de compras en Marketplace y de reservas

Las compras en Marketplace y de reservas se facturan de forma independiente de los servicios de Azure. Si el banco rechaza los cargos de Marketplace o de reservas, el estado de la factura se mostrará como **Vencida** en Azure Portal. Puede comprobar el estado de las facturas de Marketplace y de reservas mediante estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador de la cuenta.
2. Busque en **Administración de costos + facturación**.
3. En "Facturación", seleccione **Facturas**.
4. Haga clic en la pestaña **Azure Marketplace and Reservations** (Azure Marketplace y reservas) de la derecha.
5. Seleccione la suscripción correspondiente.
6. En la cuadrícula de facturas, revise la columna Estado. Si la factura está **Pasada** o **Vencida**, haga clic en **Pagar ahora.** Durante el proceso, se le pedirá que realice la autenticación multifactor.

## <a name="next-steps"></a>Pasos siguientes
- Consulte [Resolución del saldo de la suscripción de Azure vencido](resolve-past-due-balance.md) si necesita pagar una factura de Azure.
