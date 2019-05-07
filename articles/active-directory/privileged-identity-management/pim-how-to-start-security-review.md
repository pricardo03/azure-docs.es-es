---
title: Creación de una revisión de acceso de roles de Azure AD en PIM - Azure Active Directory | Microsoft Docs
description: Obtenga información sobre cómo crear una revisión de acceso de roles de Azure AD en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/27/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0a0680ddf2c9e654455933bf09699ab81e8ab65d
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65141691"
---
# <a name="create-an-access-review-of-azure-ad-roles-in-pim"></a>Creación de una revisión de acceso de roles de Azure AD en PIM

Acceso con privilegios de los roles de Azure AD para los cambios de los empleados con el tiempo. Para reducir el riesgo asociado con las asignaciones de roles obsoletos, debe revisar con regularidad acceso. Puede usar Azure Active Directory (Azure AD) Privileged Identity Management (PIM) para crear las revisiones de acceso para los roles de Azure AD con privilegios. También puede configurar las revisiones de acceso periódicas que se producen automáticamente.

En este artículo se describe cómo crear una o varias de las revisiones de acceso para los roles de Azure AD con privilegios.

## <a name="prerequisites"></a>Requisitos previos

- [Administrador de roles con privilegios](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)

## <a name="open-access-reviews"></a>Revisiones de acceso abierto

1. Inicie sesión en [portal Azure](https://portal.azure.com/) con un usuario que sea miembro del rol de administrador de roles con privilegios.

1. Abra **Azure AD Privileged Identity Management**.

1. En el menú izquierdo, haga clic en **roles de Azure AD** y, a continuación, haga clic en **las revisiones de acceso**.

1. En administrar, haga clic en **las revisiones de acceso**.

    ![Revisiones de acceso de roles de Azure AD:](./media/pim-how-to-start-security-review/access-reviews.png)


[!INCLUDE [Privileged Identity Management access reviews](../../../includes/active-directory-privileged-identity-management-access-reviews.md)]


## <a name="start-the-access-review"></a>Inicio de la revisión de acceso

Una vez que haya especificado la configuración para una revisión de acceso, haga clic en **Iniciar**. La revisión de acceso aparecerá en la lista con un indicador de su estado.

![Lista de revisiones de acceso](./media/pim-how-to-start-security-review/access-reviews-list.png)

De forma predeterminada, Azure AD envía un correo electrónico a los revisores poco después de iniciar la revisión. Si decide no hacer que Azure AD envíe el correo electrónico, asegúrese de informar a los revisores de que hay una revisión de acceso esperando para que la lleven a cabo. Puede mostrarles las instrucciones sobre cómo [revisen el acceso a los roles de Azure AD](pim-how-to-perform-security-review.md).

## <a name="manage-the-access-review"></a>Administración de la revisión de acceso

Puede seguir el progreso a medida que los revisores las realizan en el **Introducción** página de la revisión de acceso. Derechos de acceso no se cambian en el directorio hasta que el [se completó la revisión](pim-how-to-complete-review.md).

![Progreso de revisiones de acceso](./media/pim-how-to-start-security-review/access-review-overview.png)

Si se trata de una revisión de un solo uso, a continuación, una vez que el período de revisión de acceso o el administrador detenga la revisión de acceso, siga los pasos de [revisión de acceso de roles de Azure AD](pim-how-to-complete-review.md) para ver los resultados y aplicarlos.  

Para administrar una serie de acceso de las revisiones, vaya a la revisión de acceso, y se encontrar próximas repeticiones en las revisiones programada y editar la fecha de finalización o agregar revisores en consecuencia.

Según las selecciones realizadas en **configuración de finalización**, voluntad de aplicar automáticamente se ejecutará después de la fecha de finalización de la revisión o al detener manualmente la revisión. El estado de la revisión cambiará de **completado** a través de los estados intermedios, como **Applying** y, finalmente, al estado **aplicado**. Debería ver a los usuarios denegados, si hay alguno, que se va a quitar de los roles en unos minutos.

## <a name="next-steps"></a>Pasos siguientes

- [Revisar el acceso a los roles de Azure AD](pim-how-to-perform-security-review.md)
- [Revisión de acceso de roles de Azure AD](pim-how-to-complete-review.md)
- [Creación de una revisión de acceso de roles de recursos de Azure](pim-resource-roles-start-access-review.md)
