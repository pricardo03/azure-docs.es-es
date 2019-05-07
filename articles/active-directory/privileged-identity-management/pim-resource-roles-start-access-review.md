---
title: Creación de una revisión de acceso de roles de recursos de Azure en PIM - Azure Active Directory | Microsoft Docs
description: Obtenga información sobre cómo crear una revisión de acceso de roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 04/29/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 78e4de5bbc56f95c0e903b1dac4e8481373716f3
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143500"
---
# <a name="create-an-access-review-of-azure-resource-roles-in-pim"></a>Creación de una revisión de acceso de roles de recursos de Azure en PIM

Acceso a los roles de recursos de Azure con privilegios para los empleados cambia con el tiempo. Para reducir el riesgo asociado con las asignaciones de roles obsoletos, debe revisar con regularidad acceso. Puede usar Azure Active Directory (Azure AD) Privileged Identity Management (PIM) para crear las revisiones de acceso para los roles de recursos de Azure con privilegios. También puede configurar las revisiones de acceso periódicas que se producen automáticamente.

Este artículo describe cómo crear una o varias revisiones de acceso para los roles de recursos de Azure con privilegios.

## <a name="prerequisites"></a>Requisitos previos

- [Administrador de roles con privilegios](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)

## <a name="open-access-reviews"></a>Revisiones de acceso abierto

1. Inicie sesión en [portal Azure](https://portal.azure.com/) con un usuario que sea miembro del rol de administrador de roles con privilegios.

1. Abra **Azure AD Privileged Identity Management**.

1. En el menú izquierdo, haga clic en **recursos de Azure**.

1. Haga clic en el recurso que quiera administrar, por ejemplo, una suscripción o un grupo de administración.

1. En administrar, haga clic en **las revisiones de acceso**.

    ![Revisiones de acceso de recursos de Azure:](./media/pim-resource-roles-start-access-review/access-reviews.png)


[!INCLUDE [Privileged Identity Management access reviews](../../../includes/active-directory-privileged-identity-management-access-reviews.md)]


## <a name="start-the-access-review"></a>Inicio de la revisión de acceso

Una vez que haya especificado la configuración para una revisión de acceso, haga clic en **Iniciar**. La revisión de acceso aparecerá en la lista con un indicador de su estado.

![Lista de revisiones de acceso](./media/pim-resource-roles-start-access-review/access-reviews-list.png)

De forma predeterminada, Azure AD envía un correo electrónico a los revisores poco después de iniciar la revisión. Si decide no hacer que Azure AD envíe el correo electrónico, asegúrese de informar a los revisores de que hay una revisión de acceso esperando para que la lleven a cabo. Puede mostrarles las instrucciones sobre cómo [revisen el acceso a los roles de recursos de Azure](pim-resource-roles-perform-access-review.md).

## <a name="manage-the-access-review"></a>Administración de la revisión de acceso

Puede seguir el progreso a medida que los revisores las realizan en el **Introducción** página de la revisión de acceso. Derechos de acceso no se cambian en el directorio hasta que el [se completó la revisión](pim-resource-roles-complete-access-review.md).

![Progreso de revisiones de acceso](./media/pim-resource-roles-start-access-review/access-review-overview.png)

Si se trata de una revisión de un solo uso, a continuación, una vez que el período de revisión de acceso o el administrador detenga la revisión de acceso, siga los pasos de [revisión de acceso de roles de recursos de Azure](pim-resource-roles-complete-access-review.md) para ver los resultados y aplicarlos.  

Para administrar una serie de acceso de las revisiones, vaya a la revisión de acceso, y se encontrar próximas repeticiones en las revisiones programada y editar la fecha de finalización o agregar revisores en consecuencia.

Según las selecciones realizadas en **configuración de finalización**, voluntad de aplicar automáticamente se ejecutará después de la fecha de finalización de la revisión o al detener manualmente la revisión. El estado de la revisión cambiará de **completado** a través de los estados intermedios, como **Applying** y, finalmente, al estado **aplicado**. Debería ver a los usuarios denegados, si hay alguno, que se va a quitar de los roles en unos minutos.

## <a name="next-steps"></a>Pasos siguientes

- [Revisar el acceso a los roles de recursos de Azure](pim-resource-roles-perform-access-review.md)
- [Revisión de acceso de roles de recursos de Azure](pim-resource-roles-complete-access-review.md)
- [Creación de una revisión de acceso de roles de Azure AD](pim-how-to-start-security-review.md)
