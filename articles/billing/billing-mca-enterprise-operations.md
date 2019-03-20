---
title: Completar las tareas de contrato Enterprise de contrato de cliente de Microsoft - Azure | Microsoft Docs
description: Obtenga información sobre cómo completar tareas de contrato Enterprise en la nueva cuenta de facturación.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/24/2018
ms.author: banders
ms.openlocfilehash: 9b76fa935263904957c87cd062c84d0607771369
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57341408"
---
# <a name="complete-enterprise-agreement-tasks-in-your-billing-account-for-a-microsoft-customer-agreement"></a>Completar tareas de contrato Enterprise en su cuenta de facturación para un contrato de cliente de Microsoft

Si su organización ha firmado un contrato de cliente de Microsoft para renovar su inscripción al contrato Enterprise, se crea una nueva cuenta de facturación para el acuerdo. La facturación en la nueva cuenta se organiza de forma diferente a su contrato Enterprise. En este artículo se describe cómo puede usar la nueva cuenta de facturación para realizar las tareas realizadas en su contrato Enterprise.

## <a name="how-billing-is-organized-in-the-new-account"></a>Cómo se organiza la facturación en la nueva cuenta

El diagrama siguiente describe cómo se organiza la facturación en la nueva cuenta de facturación.

![Imagen de ea mca post transición jerarquía](./media/billing-mca-setup-account/mca-post-transition-hierarchy.png)

| Contrato Enterprise   | Contrato de cliente de Microsoft    |
|------------------------|--------------------------------------------------------|
| Inscripción            | Use un perfil de facturación para administrar la facturación para su organización, similar a la inscripción al contrato Enterprise. Los administradores de empresa se conviertan en propietarios del perfil de facturación. Para más información sobre los perfiles de facturación, consulte [perfiles facturación](billing-mca-overview.md#understand-billing-profiles).
| department            | Usar una sección de la factura para organizar los costos, similares a los departamentos de su inscripción al contrato Enterprise. Departamento se convierte en las secciones de la factura y administradores de departamento se conviertan en propietarios de las secciones de la factura correspondiente. Para más información acerca de las secciones de la factura, consulte [secciones de la factura de entender](billing-mca-overview.md#understand-invoice-sections). |
| Cuenta               | No se admiten las cuentas que se crearon en su contrato Enterprise en la nueva cuenta de facturación. Las suscripciones de la cuenta pertenecen a la sección de factura correspondientes para su departamento. Los propietarios de cuentas pueden crear y administrar suscripciones para sus secciones de la factura. |

## <a name="changes-for-enterprise-administrators-in-the-new-billing-account"></a>Cambios para los administradores de organización en la nueva cuenta de facturación

Los siguientes cambios se aplican a los administradores de empresa en un contrato Enterprise que se obtuvo renovar un contrato de cliente de Microsoft.

- Se crea un perfil de facturación para la inscripción. Usará el perfil de facturación para administrar la facturación para su organización, al igual que su inscripción al contrato Enterprise. Para obtener más información sobre los perfiles de facturación, [perfiles facturación](billing-mca-overview.md#understand-billing-profiles).
- Una sección de la factura se crea para cada departamento de su inscripción al contrato Enterprise. Usará las secciones de la factura para administrar tus departamentos. Puede crear nuevas secciones de la factura para configurar otros departamentos. Para más información acerca de las secciones de la factura, consulte [comprender la factura secciones](billing-mca-overview.md#understand-invoice-sections).
- Deberá usar el rol de creador de la suscripción de Azure en las secciones se factura a asignar a otros usuarios permiso para crear suscripción de Azure, como las cuentas que se crearon en la inscripción al contrato Enterprise.
- Deberá usar el [portal Azure](https://portal.azure.com) para administrar la facturación de su organización, en lugar del portal de EA de Azure.

Se proporcionan los siguientes roles en la nueva cuenta de facturación:

**El propietario del perfil de facturación** -se le asigna el rol de propietario de facturación perfil en el perfil de facturación que se creó cuando se ha firmado el contrato. El rol permite administrar la facturación de su organización. Puede ver los cargos y facturas, organizar los costos en la factura, administrar métodos de pago y controlar el acceso a la facturación de su organización.

**Propietario de la sección de factura** -se le asigna el rol de propietario de la sección de factura en todas las secciones de factura que se crean para los departamentos de su inscripción al contrato Enterprise. El rol le permite controlar quién puede crear suscripciones de Azure y adquirir otros productos.

### <a name="view-charges-and-credits-balance-for-your-organization"></a>Ver los cargos y créditos de saldo de su organización

Use el perfil de facturación para realizar el seguimiento de los cargos y el saldo de créditos de Azure para su organización similar a su inscripción al contrato Enterprise.

Para obtener información sobre cómo ver el saldo de crédito para un perfil de facturación, consulte [realizar un seguimiento del saldo de crédito de Azure para el perfil de facturación](billing-mca-check-azure-credits-balance.md).

Para obtener información sobre cómo ver los cargos para un perfil de facturación, consulte [comprender los cargos de facturación del su contrato cliente de Microsoft](billing-mca-understand-your-bill.md).

### <a name="view-charges-for-a-department"></a>Ver cargos para un departamento

Se crea una sección de factura para cada departamento que tiene, en su contrato Enterprise. Puede ver los cargos de una sección de la factura en el portal de Azure. Para obtener más información, consulte [ver las transacciones en las secciones de la factura](billing-mca-understand-your-bill.md#view-transactions-by-invoice-sections).

### <a name="view-charges-for-an-account"></a>Ver los cargos de una cuenta

No se admiten las cuentas que se crearon en su inscripción al contrato Enterprise en la nueva cuenta de facturación. Las suscripciones de la cuenta pertenecen a la sección de factura correspondientes para su departamento. Los propietarios de cuentas pueden crear y administrar suscripciones para sus secciones de la factura.

Para ver el costo agregado para las suscripciones que pertenecían a una cuenta, debe establecer un centro de costo para cada suscripción. A continuación, puede usar el archivo csv de uso y los cargos de Azure para filtrar las suscripciones mediante el centro de costo.

### <a name="download-usage-and-charges-csv-price-sheet-and-tax-documents"></a>Descargar csv de uso y los cargos, hoja de precios y documentos de impuestos

Se genera una factura mensual por cada perfil de facturación de su cuenta de facturación. Para cada factura, puede descargar los archivos csv de uso y los cargos de Azure, hoja de precios y documento de impuestos (si procede). También puede descargar los archivos de csv Azure de uso y los cargos para los cargos del mes actual.

Para obtener información sobre cómo descargar archivos de csv de uso y los cargos de Azure, consulte [descargar uso para el contrato de cliente de Microsoft](billing-download-azure-daily-usage.md#download-usage-for-your-microsoft-customer-agreement).

Para obtener información sobre cómo descargar la hoja de precios, consulte [descargar los precios de su contrato de cliente de Microsoft](billing-ea-pricing.md#view-and-download-pricing-for-your-microsoft-customer-agreement).

Para obtener información sobre cómo descargar los documentos de impuestos, consulte [ver los documentos de impuestos del contrato de cliente de Microsoft](billing-mca-download-tax-document.md#view-and-download-tax-documents).

### <a name="add-an-additional-enterprise-administrator"></a>Agregar un administrador de empresa adicionales

Los usuarios pueden acceder al perfil de facturación para permitirles ver y administrar la facturación para su organización. Puede usar el **Access Control (IAM)** página en el portal de Azure para dar acceso.  Para obtener más información acerca de los roles de perfil de facturación, vea [perfil roles y tareas de facturación](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

Para obtener información sobre cómo proporcionar, el acceso a su perfil de facturación, consulte [administrar roles de facturación en Azure portal](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).

### <a name="create-a-new-department"></a>Crear un nuevo departamento

Cree una sección de la factura para organizar los costos según sus necesidades, al igual que los departamentos de su inscripción al contrato Enterprise. Puede crear una nueva sección de factura en el portal de Azure. Para obtener más información, consulte [cree secciones en la factura para organizar los costos](billing-mca-section-invoice.md).

### <a name="create-a-new-account"></a>Creación de una nueva cuenta

Asigne el rol de creador de la suscripción de Azure en las secciones de factura que otorgarles permisos para crear suscripción de Azure, como las cuentas que se crean en la inscripción al contrato Enterprise de usuarios. Para obtener más información, consulte [asignar a otros usuarios permiso para crear suscripciones de Azure](billing-mca-create-subscription.md#give-others-permission-to-create-azure-subscriptions).

## <a name="changes-for-department-administrators-in-the-new-billing-account"></a>Cambios para los administradores de departamento en la nueva cuenta de facturación

Los siguientes cambios se aplican a los administradores de departamento en un contrato Enterprise que se obtuvo renovar un contrato de cliente de Microsoft.

- Una sección de la factura se crea para cada departamento de su inscripción al contrato Enterprise. Las secciones de la factura se usará para administrar sus departamentos. Para más información acerca de las secciones de la factura, consulte [comprender la factura secciones](billing-mca-overview.md#understand-invoice-sections).
- Usará el rol de creador de la suscripción de Azure en la sección de la factura para asignar a otros usuarios permiso para crear suscripción de Azure, como las cuentas que se crean en la inscripción al contrato Enterprise.
- Usará el portal de Azure para administrar la facturación para su organización, en lugar del portal de EA de Azure.

Se proporcionan los siguientes roles en la nueva cuenta de facturación:

**Propietario de la sección de factura** -se le asigna el rol de propietario de la sección de factura en la sección de factura que se crea para los departamentos que tenía en el contrato Enterprise. El rol le permite ver y cargos de seguimiento y controlar quién puede crear suscripciones de Azure y comprar otros productos de la sección de factura.

### <a name="view-charges-for-your-department"></a>Ver cargos para el departamento

Puede ver los cargos de la sección de factura que se crea para su departamento en el portal de Azure [administración de costos + facturación página](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/Overview).

### <a name="add-an-additional-department-administrator"></a>Agregar un administrador de departamento adicionales

Se crea una sección de factura para cada departamento que tiene, en su contrato Enterprise. Puede usar el **acceso (IAM)** página en Azure portal para asignar a otros usuarios acceso para ver y administrar la sección de factura. Para más información acerca de los roles de la sección de factura, consulte [sección roles y tareas de factura](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

Para obtener información sobre cómo proporcionar, el acceso a la sección de factura, consulte [administrar roles de facturación en Azure portal](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).

### <a name="create-a-new-account-in-your-department"></a>Crear una nueva cuenta de su departamento

Asignar el rol de creador de la suscripción de Azure en la sección de factura que se crea para su departamento de usuarios. Para obtener más información, consulte [asignar a otros usuarios permiso para crear suscripciones de Azure](billing-mca-create-subscription.md#give-others-permission-to-create-azure-subscriptions).

### <a name="view-charges-for-accounts-in-your-departments"></a>Ver cargos para las cuentas de tus departamentos

No se admiten las cuentas que se crearon en su inscripción al contrato Enterprise en la nueva cuenta de facturación. Las suscripciones de la cuenta pertenecen a la sección de factura correspondientes para su departamento. Los propietarios de cuentas pueden crear y administrar suscripciones para sus secciones de la factura.

Para ver el costo agregado para las suscripciones que pertenecían a una cuenta de su departamento, debe establecer un centro de costo para cada suscripción. A continuación, puede usar el archivo de uso y los cargos de Azure para filtrar las suscripciones mediante el centro de costo.

## <a name="changes-for-account-owners-in-the-new-billing-account"></a>Cambios de los propietarios de cuenta de la nueva cuenta de facturación

Los propietarios de cuentas en el contrato Enterprise obtención permiso para crear suscripciones de Azure en la nueva cuenta de facturación. Las suscripciones existentes de Azure pertenecen a la sección de factura que se crea para su departamento. Si su cuenta no pertenece a un departamento, las suscripciones pertenecen a una sección de factura con el nombre de sección de la factura de forma predeterminada.

Para crear suscripciones adicionales de Azure, tiene la siguiente función en la nueva cuenta de facturación.

**Creador de la suscripción de Azure** -se le asigna el rol de creador de la suscripción de azure en la sección de factura que se crea para su departamento en el contrato Enterprise. Si su cuenta no pertenece a un departamento, get rol creador de suscripción de Azure en una sección denominada sección predeterminada de la factura. La función le permite crear suscripciones de Azure para la sección de factura.

### <a name="create-an-azure-subscription"></a>Cree una suscripción a Azure

Puede crear suscripciones de Azure para la sección de factura en el portal de Azure. Para obtener más información, consulte [crear una suscripción de Azure adicional para el contrato de cliente de Microsoft](billing-mca-create-subscription.md)

### <a name="view-charges-for-your-account"></a>Ver los cargos de la cuenta

Para ver los cargos para las suscripciones que pertenecían a una cuenta, vaya a la [página suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) en Azure portal. La página suscripciones muestra los cargos para toda la suscripción.

### <a name="view-charges-for-a-subscription"></a>Ver cargos para una suscripción

Puede ver los cargos para una suscripción o en el [página suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) o el análisis de costos de Azure. Para obtener más información sobre análisis de costos de Azure, consulte [explore y analice los costos con análisis de costos](../cost-management/quick-acm-cost-analysis.md).

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si necesita ayuda, [póngase en contacto con soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.

## <a name="next-steps"></a>Pasos siguientes

- [Descripción de cuenta de facturación de un contrato de cliente de Microsoft](billing-mca-overview.md)
- [Comprender la factura](billing-understand-your-bill.md)
- [Descripción de la factura](billing-understand-your-invoice.md)
- [Obtener la propiedad de las suscripciones de Azure a otros usuarios de facturación](billing-mca-request-billing-ownership.md)
