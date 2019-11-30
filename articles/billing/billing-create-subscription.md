---
title: Creación de una suscripción de Azure adicional | Microsoft Docs
description: Aprenda a agregar una nueva suscripción de Azure en Azure Portal.
services: billing
documentationcenter: ''
author: amberb
manager: amberb
editor: ''
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: banders
ms.openlocfilehash: 7ca7256cbe42949141bc216ee1cb5c89ceac935c
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74226366"
---
# <a name="create-an-additional-azure-subscription"></a>Creación de una suscripción de Azure adicional

Puede crear una suscripción adicional para su cuenta de facturación del [Contrato Enterprise (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), [Contrato de cliente de Microsoft](https://azure.microsoft.com/pricing/purchase-options/microsoft-customer-agreement/) o [Microsoft Partner Agreement](https://www.microsoft.com/licensing/news/introducing-microsoft-partner-agreement) en Azure Portal. Es posible que quiera una suscripción adicional para evitar alcanzar los límites de suscripción, crear entornos independientes para seguridad o aislar los datos por motivos de cumplimiento.

Si tiene una cuenta de facturación de Microsoft online Service Program (MOSP), puede crear suscripciones adicionales en el [portal de inicio de sesión de Azure](https://account.azure.com/signup?offer=ms-azr-0003p).

Para más información sobre las cuentas de facturación y la identificación del tipo de la cuenta de facturación, consulte [Visualización de las cuentas de facturación en Azure Portal](billing-view-all-accounts.md).

## <a name="permission-required-to-create-azure-subscriptions"></a>Permisos necesarios para crear suscripciones a Azure

Necesita los siguientes permisos para crear suscripciones:

|Cuenta de facturación  |Permiso  |
|---------|---------|
|Contrato Enterprise (EA) |  Rol de propietario de la cuenta en la inscripción del Contrato Enterprise. Para más información, consulte [Roles administrativos del Contrato Enterprise de Azure en Azure](billing-understand-ea-roles.md).    |
|Contrato de cliente de Microsoft (MCA) |  Rol de propietario o colaborador en la sección factura, perfil de facturación o cuenta de facturación. O rol de creador de suscripciones a Azure en la sección de factura.  Para más información, consulte [Tareas y roles de la facturación de suscripción](billing-understand-mca-roles.md#subscription-billing-roles-and-tasks).    |
|Microsoft Partner Agreement (MPA) |   Rol de administrador global y agente de administrador en la organización asociada. Para más información, consulte [Centro de partners: Asignar roles y permisos de usuarios](https://docs.microsoft.com/partner-center/permissions-overview)  |

## <a name="create-a-subscription-in-the-azure-portal"></a>Creación de una suscripción en Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Busque **Suscripciones**.

   ![Captura de pantalla que muestra la búsqueda de suscripciones en el portal](./media/billing-create-subscription/billing-search-subscription-portal.png)

1. Seleccione **Agregar**.

   ![Captura de pantalla que muestra el botón Agregar en la vista Suscripciones](./media/billing-create-subscription/subscription-add.png)

1. Si tiene acceso a varias cuentas de facturación, seleccione la cuenta de facturación para la que desea crear la suscripción.

1. Complete el formulario y haga clic en **Crear**. En las tablas siguientes se enumeran los campos del formulario para cada tipo de cuenta de facturación.

**Contrato Enterprise**

|Campo  |Definición  |
|---------|---------|
|NOMBRE     | El nombre para mostrar ayuda a identificar fácilmente la suscripción en Azure Portal.  |
|Oferta     | Seleccione Desarrollo/pruebas EA si va a usar esta suscripción para las cargas de trabajo de desarrollo o pruebas; si no, utilice Microsoft Azure Enterprise. La oferta de Desarrollo/pruebas debe estar habilitada para que la cuenta de inscripción cree suscripciones de desarrollo/pruebas del Contrato Enterprise.|

**Contrato de cliente de Microsoft**

|Campo  |Definición  |
|---------|---------|
|Perfil de facturación     | Los cargos correspondientes a su suscripción se facturarán en el perfil de facturación que haya seleccionado. Si tiene acceso a solo un perfil de facturación, se atenúa la selección.     |
|Sección de factura     | Los cargos de la suscripción aparecerán en esta sección de factura del perfil de facturación. Si tiene acceso a solo una sección de factura, se atenúa la selección.  |
|Plan     | Seleccione Plan de Microsoft Azure para Desarrollo/pruebas si va a usar esta suscripción para las cargas de trabajo de desarrollo o pruebas; si no, utilice Plan de Microsoft Azure. Si solo hay un plan para el perfil de facturación, la selección aparecerá atenuada.  |
|NOMBRE     | El nombre para mostrar ayuda a identificar fácilmente la suscripción en Azure Portal.  |

**Microsoft Partner Agreement**

|Campo  |Definición  |
|---------|---------|
|Cliente    | La suscripción se crea para el cliente que seleccione. Si solo tiene un cliente, la selección aparece atenuada.  |
|Vendedor    | El revendedor que proporciona servicios al cliente. Este campo es opcional, solo se aplica a los proveedores indirectos en el modelo de dos niveles de CSP. |
|NOMBRE     | El nombre para mostrar ayuda a identificar fácilmente la suscripción en Azure Portal.  |

## <a name="create-an-additional-azure-subscription-programmatically"></a>Creación de una suscripción a Azure adicional mediante programación

También puede crear suscripciones adicionales mediante programación. Para más información, consulte [Creación de suscripciones a Azure Enterprise mediante programación](../azure-resource-manager/programmatically-create-subscription.md).

## <a name="next-steps"></a>Pasos siguientes

- [Adición o cambio de los administradores de la suscripción de Azure](billing-add-change-azure-subscription-administrator.md)
- [Move resources to new resource group or subscription](../azure-resource-manager/resource-group-move-resources.md?toc=/azure/billing/TOC.json) (Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción)
- [Creación de grupos de administración para la organización y la administración de recursos](../governance/management-groups/create.md?toc=/azure/billing/TOC.json)
- [Cancelación de la suscripción de Azure](billing-how-to-cancel-azure-subscription.md)

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).
