---
title: Cómo administrar una identidad administrada asignada por el usuario mediante Azure Portal
description: Instrucciones paso a paso sobre cómo crear, enumerar y eliminar una identidad administrada asignada por el usuario.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 529daff80f8cfa0f9e4c9e64efd5648ae57e6e66
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43345027"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-the-azure-portal"></a>Crear, enumerar o eliminar una identidad administrada asignada por el usuario mediante Azure Portal

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Las identidades administradas de los recursos de Azure proporcionan a los servicios de Azure una identidad administrada en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo aprenderá a crear, enumerar y eliminar una identidad administrada asignada por el usuario mediante Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con las identidades administradas de los recursos de Azure, consulte la [sección de introducción](overview.md). **No olvide revisar la [diferencia entre una identidad administrada que haya asignado el sistema de otra que haya administrado el usuario](overview.md#how-does-it-work)**.
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para llevar a cabo las operaciones de administración de este artículo, su cuenta debe tener las siguientes asignaciones de roles:
    - Rol [Colaborador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) para crear, leer (enumerar), actualizar y eliminar una identidad administrada que haya asignado el usuario.
    - Rol [Operador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-operator) para leer (enumerar) las propiedades de una identidad administrada que haya asignado el usuario.

## <a name="create-a-user-assigned-managed-identity"></a>Crear una identidad administrada asignada por el usuario

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure para crear la identidad administrada asignada por el usuario.
2. En el cuadro de búsqueda, escriba *Identidades administradas* y en **Servicios**, haga clic en **Identidades administradas**.
3. Haga clic en **Agregar** y escriba valores en los campos siguientes del panel **Crear identidad administrada asignada por el usuario**:
   - **Nombre del recurso**: este es el nombre de la identidad administrada asignada por el usuario, por ejemplo, UAI1.
   - **Suscripción**: elija la suscripción donde crear la identidad administrada asignada por el usuario.
   - **Grupo de recursos**: cree un grupo de recursos que contenga la identidad asignada por el usuario o elija **Usar existente** para crear esta en un grupo de recursos existente.
   - **Ubicación**: elija una ubicación para implementar la identidad administrada asignada por el usuario, por ejemplo, **Oeste de Estados Unidos**.
4. Haga clic en **Create**(Crear).

![Crear una identidad administrada asignada por el usuario](./media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)

## <a name="list-user-assigned-managed-identities"></a>Enumerar identidades administradas asignadas por el usuario

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure para enumerar las identidades administradas asignadas por el usuario.
2. En el cuadro de búsqueda, escriba *Identidades administradas* y en Servicios, haga clic en **Identidades administradas**.
3. Se devuelve una lista de las identidades administradas asignadas por el usuario de la suscripción.  Para ver los detalles de una identidad administrada asignada por el usuario, haga clic en su nombre.

![Enumerar identidades administradas asignadas por el usuario](./media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-managed-identity"></a>Eliminar una identidad administrada asignada por el usuario

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure para eliminar una identidad administrada asignada por el usuario.
2. Seleccione la identidad administrada asignada por el usuario y haga clic en **Eliminar**.
3. En el cuadro de confirmación, elija **Sí**.

![Eliminar una identidad administradas asignada por el usuario](./media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)