---
title: Crear una suscripción de Azure adicional para su cuenta de facturación | Microsoft Docs
description: Aprenda a agregar una nueva suscripción de Azure en Azure Portal.
services: billing
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
ms.date: 02/28/2019
ms.author: banders
ms.openlocfilehash: 105b8481486c088a05e3acb95081d3ee55b55f52
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57897119"
---
# <a name="create-an-additional-azure-subscription-for-microsoft-customer-agreement"></a>Crear una suscripción de Azure adicional para el contrato de cliente de Microsoft

Crear suscripciones adicionales para su cuenta de facturación para configurar los entornos independientes para desarrollo y pruebas de seguridad o para aislar los datos por motivos de cumplimiento.

En este artículo se aplica a una cuenta de facturación para un contrato de cliente de Microsoft. [Compruebe si tiene acceso a un contrato de cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement). Si desea crear suscripciones para otras cuentas de facturación, consulte [crear una suscripción adicional en el portal de Azure](billing-create-subscription.md).

Para crear una suscripción, debe ser un **propietario de la sección de factura**, **colaborador de la sección de factura**, o **creador de la suscripción de Azure**. Para obtener más información, consulte [tareas y roles de facturación de suscripción](billing-understand-mca-roles.md#subscription-billing-roles-and-tasks). Para proporcionar a otros usuarios permiso para crear suscripciones de Azure para su cuenta de facturación, consulte [asignar a otros usuarios permiso para crear suscripciones de Azure](#give-others-permission-to-create-azure-subscriptions).

## <a name="create-a-subscription-in-the-azure-portal"></a>Crear una suscripción en el portal de Azure

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque **Suscripciones**.

   ![Captura de pantalla que se muestra la búsqueda en el portal de suscripciones](./media/billing-mca-create-subscription/billing-search-cost-management-billing.png)

3. Seleccione **Agregar**.

4. Si tiene acceso a varias cuentas de facturación, seleccione la cuenta de facturación de su contrato de cliente de Microsoft.

   ![Captura de pantalla que muestra página Crear suscripción](./media/billing-mca-create-subscription/billing-mca-create-azure-subscription.png)

5. Seleccione un perfil de facturación. Los cargos de la suscripción se reflejarán en la factura del perfil de facturación y se pagará mediante sus métodos de pago. Si tiene acceso a un solo perfil de facturación, se atenúa la selección.

6. Seleccione una sección de la factura. Los cargos de la suscripción se reflejarán en esta sección de factura del perfil de facturación. Si tiene acceso a solo una sección de factura, se atenúa la selección.

7. Seleccione un plan para la suscripción. Seleccione **Plan de Microsoft Azure para DevTest**si va a usar esta suscripción para el desarrollo o pruebas else de cargas de trabajo utiliza **Plan de Microsoft Azure**. Si tiene acceso a un solo plan, se atenúa la selección.

8. Escriba el nombre de la suscripción. El nombre ayuda a identificar fácilmente la suscripción en el portal de Azure.

9. Seleccione **Crear**.

## <a name="give-others-permission-to-create-azure-subscriptions"></a>Asignar a otros usuarios permiso para crear suscripciones de Azure

Agregar usuarios como creadores de suscripción de Azure en una sección de factura que otorgarles permisos para crear suscripciones de Azure.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que se muestra la búsqueda en el portal de suscripciones](./media/billing-mca-create-subscription/billing-search-cost-management-billing.png)

3. Vaya a la sección de factura. Según el acceso, debe seleccionar una cuenta de facturación o el perfil de facturación. En la cuenta o perfil de facturación, seleccione **factura secciones** y, a continuación, una sección de factura.

4. Seleccione **administración de acceso (IAM)** desde el lado superior izquierdo.

5. En la parte superior de la página, seleccione **agregar**.

6. Seleccione **creador de la suscripción de Azure** para el rol.

   ![Captura de pantalla que muestra el proceso de asignar el rol de creador de suscripción de Azure a un usuario](./media/billing-mca-create-subscription/billing-mca-add-azure-subscription-creator.png)

7. Escriba la dirección de correo electrónico del usuario al que desea dar acceso.

8. Seleccione **Guardar**.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobar el acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si tiene alguna pregunta o necesita ayuda, [crear una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- [Asignar a otros usuarios permiso para crear recursos de Azure mediante roles integrados](../role-based-access-control/built-in-roles.md#built-in-role-descriptions)
- [Crear una máquina virtual windows](../virtual-machines/windows/quick-create-portal.md)
- [Crear una máquina virtual linux](../virtual-machines/linux/quick-create-portal.md)
- [Creación de grupos de administración para la organización y la administración de recursos](../governance/management-groups/create.md?toc=/azure/billing/TOC.json)
