---
title: Empezar a trabajar con su cuenta de facturación para un contrato de cliente de Microsoft - Azure | Microsoft Docs
description: Descripción de cuenta de facturación de un contrato de cliente de Microsoft
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
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60371467"
---
# <a name="get-started-with-your-billing-account-for-a-microsoft-customer-agreement"></a>Empezar a trabajar con su cuenta de facturación para un contrato de cliente de Microsoft

Se crea una cuenta de facturación para cada contrato que inicie sesión con Microsoft para usar Azure. Use su cuenta de facturación para administrar la facturación y realizar un seguimiento de los costos. Puede tener acceso a varias cuentas de facturación. Por ejemplo, puede registrarse para Azure para sus proyectos personales. También podría tener acceso a Azure a través del contrato Enterprise o el contrato de cliente de Microsoft de su organización. Para cada uno de estos escenarios, tendría una cuenta de facturación independiente.

En este artículo se aplica a una cuenta de facturación para un contrato de cliente de Microsoft. [Compruebe si tiene acceso a un contrato de cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement).

## <a name="understand-billing-account"></a>Comprender la cuenta de facturación

Su cuenta de facturación para el acuerdo de cliente de Microsoft contiene uno o varios perfiles de facturación que le permiten administrar sus facturas y los métodos de pago. Cada perfil de facturación contiene uno o más secciones de factura que le permiten organizar los costos en la factura del perfil de facturación.

El siguiente diagrama muestra la relación entre una cuenta de facturación, los perfiles de facturación y las secciones de factura.

![Diagrama que muestra la jerarquía de facturación para el contrato de cliente de Microsoft](./media/billing-mca-overview/mca-billing-hierarchy.png)

Roles en la cuenta de facturación tienen el mayor nivel de permisos. De forma predeterminada, solo los administradores globales en Azure Active Directory de su organización obtención acceso a la cuenta de facturación. Estos roles deben asignarse a los usuarios que necesitan para ver las facturas y realizar un seguimiento de los costos para toda la organización como Finanzas o los administradores de TI. Para obtener más información, consulte [cuenta roles y tareas de facturación](billing-understand-mca-roles.md#billing-account-roles-and-tasks).

## <a name="understand-billing-profiles"></a>Comprender los perfiles de facturación

Use un perfil de facturación para administrar los métodos de factura y pago. Se genera una factura mensual para otros productos comprados mediante el perfil de facturación y suscripciones de Azure. Utilice los métodos de pago para pagar la factura.

Automáticamente se crea un perfil de facturación para su cuenta de facturación. Puede crear nuevos perfiles de facturación para configurar facturas adicionales. Por ejemplo, podría facturas diferentes para cada departamento o proyecto en su organización.

También puede crear secciones de la factura para organizar los costos en la factura de un perfil de facturación. Los cargos de las suscripciones de Azure y los productos adquiridos para una sección de la factura se muestran en la sección. Factura de facturación del perfil incluye los gastos para todas las secciones de factura.

Roles en los perfiles de facturación tienen permisos para ver y administrar las facturas y los métodos de pago. Asignar los roles a los usuarios que pagan las facturas como miembros del equipo de contabilidad de su organización. Para obtener más información, consulte [perfil roles y tareas de facturación](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="monthly-invoice-generated-for-each-billing-profile"></a>Factura mensual generada para cada perfil de facturación

Se genera una factura mensual en la fecha de facturación para cada perfil de facturación. La factura contiene todos los cargos de mes anterior.

Puede ver la factura, descargue los documentos y cambiar la configuración para recibir futuras facturas por correo electrónico, en el portal de Azure. Para obtener más información, consulte [descargar facturas para un contrato de cliente de Microsoft](billing-download-azure-invoice-daily-usage-date.md#download-invoices-for-a-microsoft-customer-agreement).

### <a name="invoices-paid-through-payment-methods"></a>Pagado a través de métodos de pago de facturas

Cada perfil de facturación tiene sus propios métodos de pago que se usan para pagar sus facturas. Se admiten los siguientes métodos de pago:

| Type             | Definición  |
|------------------|-------------|
|Créditos de Azure    |  Los créditos se aplican automáticamente a la cantidad total de facturación en su factura para calcular la cantidad que debe pagar. Para obtener más información, consulte [realizar un seguimiento del saldo de crédito de Azure para el perfil de facturación](billing-mca-check-azure-credits-balance.md). |
|Cheque o transferencia bancaria | Puede pagar el importe de la factura a través de comprobación o cable transferir. Se proporcionan las instrucciones de pago en la factura |

### <a name="control-azure-marketplace-and-reservation-purchases-by-applying-policies"></a>Compras de Marketplace de Azure de control y reserva aplicando directivas

Aplicar directivas para controlar las compras realizadas con un perfil de facturación. Puede establecer directivas para deshabilitar la compra de reservas de Azure y productos de Marketplace. Cuando se aplican las directivas, las suscripciones creadas para las secciones de la factura en el perfil de facturación no se puede usar para comprar las reservas de Azure y productos de Marketplace.

### <a name="allow-users-to-create-azure-subscriptions-by-enabling-azure-plans"></a>Permitir a los usuarios crear suscripciones de Azure habilitando los planes de Azure

Los planes de Azure se habilitan automáticamente cuando se crea un perfil de facturación. Todas las secciones de la factura en el perfil de facturación obtención acceso a estos planes. Los usuarios con acceso a la sección de factura usar los planes para crear suscripciones de Azure. No pueden crear suscripciones de Azure, a menos que un plan de Azure está habilitado para el perfil de facturación. Se admiten los siguientes planes de Azure en cuentas de facturación para el contrato de cliente de Microsoft:

| Plan             | Definición  |
|------------------|-------------|
|Plan de Microsoft Azure   | Permitir a los usuarios crear suscripciones que se pueden ejecutar las cargas de trabajo. Para obtener más información, consulte [Plan de Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0017g/) |
|Plan de Microsoft Azure para desarrollo y pruebas | Permitir a los suscriptores de Visual Studio crear las suscripciones que están restringidas para el desarrollo o pruebas de cargas de trabajo. Estas suscripciones aporta ventajas como tarifas inferior y el acceso a imágenes de máquina virtual exclusivo en el portal de Azure. Para obtener más información, consulte [Plan de Microsoft Azure de DevTest](https://azure.microsoft.com/offers/ms-azr-0148g/)|

## <a name="understand-invoice-sections"></a>Comprender las secciones de la factura

Crear secciones de la factura para organizar los costos en la factura de un perfil de facturación. Por ejemplo, puede que necesite una sola factura para su organización, pero desea organizar los costos por departamento, equipo o proyecto. Para este escenario, tiene un único perfil de facturación que se cree una sección de factura para cada departamento, equipo o proyecto.

Cuando se crea una sección de la factura, puede permitir que otros usuarios permiso para crear suscripciones de Azure para la sección. A continuación, se reflejan los cargos de uso y las compras para las suscripciones en la sección correspondiente de la factura.

Roles en la sección de la factura tienen permisos para controlar quién crea suscripciones de Azure. Asignar los roles a los usuarios configuración el entorno de Azure para los equipos en nuestra organización, como clientes potenciales de ingeniería y arquitectos técnicos. Para obtener más información, consulte [sección roles y tareas de factura](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobar el acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si necesita ayuda, [póngase en contacto con soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para obtener información sobre su cuenta de facturación:

- [Comprender las funciones administrativas de contrato de cliente de Microsoft en Azure](billing-understand-mca-roles.md)
- [Crear una suscripción de Azure adicional para el contrato de cliente de Microsoft](billing-mca-create-subscription.md)
- [Crear secciones de la factura para organizar los costos](billing-mca-section-invoice.md)