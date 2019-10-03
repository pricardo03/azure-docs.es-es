---
title: Creación de una suscripción de Azure adicional para la cuenta de facturación
description: Aprenda a agregar una nueva suscripción de Azure en Azure Portal.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: e1fafafe5f67d1775ca80a1f7c2aff4dec9e0bc4
ms.sourcegitcommit: d4c9821b31f5a12ab4cc60036fde00e7d8dc4421
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71709589"
---
# <a name="create-an-additional-azure-subscription-for-microsoft-customer-agreement"></a>Creación de una suscripción de Azure adicional para el contrato de cliente de Microsoft

Cree suscripciones adicionales para la cuenta de facturación con el fin de configurar entornos independientes para el desarrollo y las pruebas, para seguridad o para aislar los datos por motivos de cumplimiento.

Este artículo se aplica a una cuenta de facturación para un contrato de cliente de Microsoft. [Compruebe si tiene acceso a un contrato de cliente de Microsoft](#check-access). Si desea crear suscripciones para otros tipos de cuentas de facturación, consulte [Creación de una suscripción adicional en Azure Portal](billing-create-subscription.md).

Para crear una suscripción, debe ser **propietario de la sección de factura**, **colaborador de la sección de factura** o **creador de la suscripción a Azure**. Para más información, consulte [Tareas y roles de la facturación de suscripción](billing-understand-mca-roles.md#subscription-billing-roles-and-tasks). Para proporcionar a otros usuarios permiso para crear suscripciones a Azure para la cuenta de facturación, consulte [Concesión de permisos para crear suscripciones a Azure](#give-others-permission).

## <a name="create-a-subscription"></a>una suscripción

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque **Suscripciones**.

   ![Captura de pantalla que muestra la búsqueda de suscripciones en el portal](./media/billing-mca-create-subscription/billing-search-subscriptions.png)

3. Seleccione **Agregar**.

4. Si tiene acceso a varias cuentas de facturación, seleccione la cuenta de facturación de su contrato de cliente de Microsoft.

   ![Captura de pantalla que muestra la página de creación de suscripción](./media/billing-mca-create-subscription/billing-mca-create-azure-subscription.png)

5. Seleccione un perfil de facturación. Los cargos correspondientes a su suscripción se facturarán en el perfil de facturación seleccionado. Si tiene acceso a solo un perfil de facturación, se atenúa la selección.

6. Seleccione una sección de factura. Los cargos correspondientes a su suscripción se facturarán según esta sección de la factura del perfil de facturación. Si tiene acceso a solo una sección de factura, se atenúa la selección.

7. Seleccione un plan para la suscripción. Seleccione **Plan de Microsoft Azure para desarrollo y pruebas** si va a usar esta suscripción para las cargas de trabajo de desarrollo o pruebas; si no, utilice **Plan de Microsoft Azure**. Si tiene acceso a un solo plan, se atenúa la selección.

8. Escriba el nombre de la suscripción. El nombre ayuda a identificar fácilmente la suscripción en Azure Portal.

9. Seleccione **Crear**.

## <a name="give-others-permission"></a>Concesión de permisos a otros usuarios

Agregue usuarios como creadores de suscripción a Azure en una sección de factura con el fin de darles permiso para crear suscripciones a Azure.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda de suscripciones en el portal](./media/billing-mca-create-subscription/billing-search-cost-management-billing.png)

3. Vaya a la sección de facturas. Dependiendo de su acceso, es posible que tenga que seleccionar una cuenta de facturación o un perfil de facturación. En la cuenta o perfil de facturación, seleccione **Secciones de factura** y, después, una sección de factura de la lista. Todas las suscripciones que los usuarios crearán se facturarán en esta sección de factura.
   
   ![Captura de pantalla que muestra la selección de las secciones de la factura](./media/billing-mca-create-subscription/mca-select-invoice-sections.png)        

4. Seleccione **Administración de acceso (IAM)** en el panel izquierdo.

5. En la parte superior de la página, seleccione **Agregar**.

6. Seleccione **Creador de la suscripción a Azure** para el rol.

7. Escriba la dirección de correo electrónico del usuario al que desea dar acceso.

8. Seleccione **Guardar**.

## <a name="check-access"></a>Comprobación del acceso
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- [Concesión a otros usuarios de permiso para crear recursos de Azure mediante roles integrados](../role-based-access-control/built-in-roles.md#built-in-role-descriptions)
- [Creación de una máquina virtual Windows](../virtual-machines/windows/quick-create-portal.md)
- [Creación de una máquina virtual Linux](../virtual-machines/linux/quick-create-portal.md)
- [Creación de grupos de administración para la organización y la administración de recursos](../governance/management-groups/create.md?toc=/azure/billing/TOC.json)
