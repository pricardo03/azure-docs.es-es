---
title: 'Creación de secciones en la factura para organizar los costos: Azure | Microsoft Docs'
description: Aprenda a organizar los costos con secciones de factura.
services: ''
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.topic: conceptual
ms.workload: na
ms.date: 02/28/2019
ms.author: banders
ms.openlocfilehash: 21d6c1671c57341d785c002f360c05cc5c610657
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60371613"
---
# <a name="create-sections-on-your-invoice-to-organize-your-costs"></a>Creación de secciones en la factura para organizar los costos

Cree secciones en la factura para organizar los costos por departamento, entorno de desarrollo o en función de las necesidades de la organización. Después, conceda permisos a otros usuarios para crear suscripciones a Azure para la sección. Los cargos por uso o las compras de las suscripciones se reflejan en la sección de factura. Consulte los cargos totales para la sección en la factura, en Azure Portal o revíselos en el análisis de costos de Azure. Para más información, consulte [Visualización de las transacciones por secciones de factura](billing-mca-understand-your-bill.md#view-transactions-by-invoice-sections).

Este artículo se aplica a una cuenta de facturación para un contrato de cliente de Microsoft. [Compruebe si tiene acceso a un contrato de cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement).

## <a name="create-an-invoice-section-in-the-azure-portal"></a>Creación de una sección de factura en Azure Portal

Para crear una sección de factura, debe ser **propietario del perfil de facturación** o **colaborador de perfil de facturación**. Para más información, consulte [Administración de las secciones de factura para el perfil de facturación](billing-understand-mca-roles.md#manage-invoice-sections-for-billing-profile).

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-mca-section-invoice/billing-search-cost-management-billing.png)

3. Seleccione **Secciones de factura** en el panel izquierdo. Dependiendo del acceso, es posible que tenga que seleccionar un perfil de facturación o una cuenta de facturación y, después, seleccionar **Secciones de factura**.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-mca-section-invoice/billing-mca-list-invoice-sections.png)

4. En la parte superior de la página, seleccione **Agregar**.

5. Escriba el nombre de la sección de factura.

   ![Captura de pantalla que muestra la página de creación de secciones de factura](./media/billing-mca-section-invoice/mca-create-invoice-section.png)

6. Seleccione un perfil de facturación. Los cargos por uso o las compras de la sección de factura se reflejarán en la factura del perfil de facturación.

7. Seleccione **Crear**.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobación del acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Creación de una suscripción a Azure adicional para el contrato de cliente de Microsoft](billing-mca-create-subscription.md)
- [Asignación a otros usuarios de permiso para crear suscripciones a Azure](billing-mca-create-subscription.md#give-others-permission-to-create-azure-subscriptions)
- [Obtención de la propiedad de la facturación de las suscripciones de usuarios de otras cuentas de facturación](billing-mca-request-billing-ownership.md)

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si necesita ayuda, [póngase en contacto con soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.
