---
title: Empezar a trabajar con la cuenta de facturación para un contrato de cliente de Microsoft en Azure | Microsoft Docs
description: Descripción de la cuenta de facturación para un contrato de cliente de Microsoft
services: billing
documentationcenter: ''
author: amberbhargava
manager: amberbhargava
editor: banders
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2019
ms.author: banders
ms.openlocfilehash: ea625a61ed600dbaa22fef85987e9570a6fb7dbc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60371467"
---
# <a name="get-started-with-your-billing-account-for-a-microsoft-customer-agreement"></a>Empezar a trabajar con la cuenta de facturación para un contrato de cliente de Microsoft

Se crea una cuenta de facturación por cada contrato que firme con Microsoft para usar Azure. Puede usar la cuenta de facturación para administrar la facturación y realizar el seguimiento de los costos. Puede tener acceso a varias cuentas de facturación. Por ejemplo, es posible que te hayas registrado en Azure para tus proyectos personales. También se puede tener acceso a Azure mediante el Contrato Enterprise de la organización o del contrato de cliente de Microsoft. Para cada uno de estos escenarios, se dispondría de una cuenta de facturación independiente.

Este artículo se aplica a una cuenta de facturación para un contrato de cliente de Microsoft. [Compruebe si tiene acceso a un contrato de cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement).

## <a name="understand-billing-account"></a>Descripción de la cuenta de facturación

La cuenta de facturación para el contrato de cliente de Microsoft contiene uno o más perfiles de facturación que le permiten administrar las facturas y los métodos de pago. Cada perfil de facturación contiene una o más secciones de factura que permiten organizar los costos en la factura del perfil de facturación.

El siguiente diagrama muestra la relación entre una cuenta de facturación, los perfiles de facturación y las secciones de factura.

![Diagrama que muestra la jerarquía de facturación para el contrato de cliente de Microsoft](./media/billing-mca-overview/mca-billing-hierarchy.png)

Los roles en la cuenta de facturación tienen el nivel más alto de permisos. De forma predeterminada, solo los administradores globales de Azure Active Directory de la organización tienen acceso a la cuenta de facturación. Estos roles deben asignarse a los usuarios que necesitan ver las facturas y realizar el seguimiento de los costos de toda la organización, como los administradores de finanzas o de TI. Para más información, consulte [Tareas y roles de la cuenta de facturación](billing-understand-mca-roles.md#billing-account-roles-and-tasks).

## <a name="understand-billing-profiles"></a>Descripción de los perfiles de facturación

Utilice un perfil de facturación para administrar la factura y los métodos de pago. Se genera una factura mensual para las suscripciones a Azure y otros productos comprados mediante el perfil de facturación. Utilice los métodos de pago para pagar la factura.

Se crea automáticamente un perfil de facturación para la cuenta de facturación. Puede crear nuevos perfiles de facturación para configurar facturas adicionales. Por ejemplo, podría querer facturas diferentes para cada departamento o proyecto de la empresa.

También puede crear secciones de factura para organizar los costos en una factura del perfil de facturación. Los cargos por suscripciones a Azure y los productos comprados para una sección de factura aparecen en la sección. La factura del perfil de facturación incluye cargos para todas las secciones de factura.

Los roles en los perfiles de facturación tienen permisos para ver y administrar las facturas y los métodos de pago. Asigne esos roles a los usuarios que pagan facturas, como miembros del equipo de contabilidad de la organización. Para más información, consulte [Tareas y roles del perfil de facturación](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="monthly-invoice-generated-for-each-billing-profile"></a>Factura mensual generada para cada perfil de facturación

Se genera una factura mensual en la fecha de factura por cada perfil de facturación. La factura contiene todos los cargos de mes anterior.

En Azure Portal puede ver la factura, descargar documentos y cambiar la configuración para recibir las facturas futuras por correo electrónico. Para más información, consulte [Descarga de facturas para un contrato de cliente de Microsoft](billing-download-azure-invoice-daily-usage-date.md#download-invoices-for-a-microsoft-customer-agreement).

### <a name="invoices-paid-through-payment-methods"></a>Factura pagada mediante métodos de pago

Cada perfil de facturación dispone de sus propios métodos de pago, que se usan para pagar las facturas. Se admiten los siguientes métodos de pago:

| Type             | Definición  |
|------------------|-------------|
|Créditos de Azure    |  Los créditos se aplican automáticamente a la cantidad total facturada en su factura para calcular la cantidad que tiene que pagar. Para más información, consulte [Seguimiento del saldo del crédito de Azure para el perfil de facturación](billing-mca-check-azure-credits-balance.md). |
|Cheque o transferencia bancaria | Puede pagar el importe de la factura por cheque o transferencia bancaria. Las instrucciones de pago se incluyen en la factura. |

### <a name="control-azure-marketplace-and-reservation-purchases-by-applying-policies"></a>Control de las compras de reservas y de Azure Marketplace mediante la aplicación de directivas

Aplique directivas para controlar las compras realizadas mediante un perfil de facturación. Puede establecer directivas para deshabilitar la compra de reservas de Azure y de productos de Marketplace. Cuando se aplican las directivas, no se pueden usar las suscripciones creadas para las secciones de factura en el perfil de facturación para comprar reservas de Azure ni productos de Marketplace.

### <a name="allow-users-to-create-azure-subscriptions-by-enabling-azure-plans"></a>Permiso a los usuarios para crear suscripciones de Azure mediante la habilitación de planes de Azure

Los planes Azure se activan automáticamente al crear un perfil de facturación. Todas las secciones de factura del perfil de facturación tienen acceso a estos planes. Los usuarios con acceso a la sección de factura utilizan los planes para crear suscripciones a Azure. No pueden crear suscripciones a Azure a menos que un plan de Azure esté habilitado para el perfil de facturación. Se admiten los siguientes planes de Azure en las cuentas de facturación para el contrato de cliente de Microsoft:

| Plan             | Definición  |
|------------------|-------------|
|Plan de Microsoft Azure   | Permita a los usuarios crear suscripciones que puedan ejecutar cualquier carga de trabajo. Para más información, consulte el [plan de Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0017g/). |
|Plan de Microsoft Azure para desarrollo y pruebas | Permita a los suscriptores de Visual Studio crear suscripciones restringidas para desarrollar o probar cargas de trabajo. Estas suscripciones aportan ventajas como tarifas más bajas y acceso a imágenes exclusivas de máquinas virtuales en Azure Portal. Para más información, consulte el [plan de Microsoft Azure Plan para desarrollo y pruebas](https://azure.microsoft.com/offers/ms-azr-0148g/).|

## <a name="understand-invoice-sections"></a>Descripción de las secciones de la factura

Cree secciones de factura para organizar los costos en la factura de un perfil de facturación. Por ejemplo, puede que necesite una única factura para la organización, pero desea organizar los costos por departamento, equipo o proyecto. Para este escenario, cuenta con un único perfil de facturación en el que se crea una sección de factura para cada departamento, equipo o proyecto.

Cuando se crea la sección de factura, puede dar permiso a otros usuarios para crear suscripciones a Azure para la sección. Los cargos por uso y las compras de las suscripciones se reflejan entonces en la sección apropiada de la factura.

Los roles en la sección de factura tienen permisos para controlar quién crea suscripciones a Azure. Asigne estos roles a los usuarios que configuren el entorno de Azure para los equipos de nuestra organización, como los jefes de ingeniería y los arquitectos técnicos. Para más información, consulte [Tareas y roles de la sección de factura](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobación del acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si necesita ayuda, [póngase en contacto con soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para obtener información sobre la cuenta de facturación:

- [Descripción de los roles administrativos del contrato de cliente de Microsoft en Azure](billing-understand-mca-roles.md)
- [Creación de una suscripción a Azure adicional para el contrato de cliente de Microsoft](billing-mca-create-subscription.md)
- [Creación de secciones en la factura para organizar los costos](billing-mca-section-invoice.md)