---
title: 'Tutorial: Creación de un inquilino de Azure Active Directory B2C'
description: Aprenda a preparar el registro de sus aplicaciones mediante la creación de un inquilino de Azure Active Directory B2C desde Azure Portal.
services: B2C
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/07/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 57aed26fe2f2f03dca347836f21d1cad14a07642
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71063348"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Tutorial: Creación de un inquilino de Azure Active Directory B2C

Para que sus aplicaciones puedan interactuar con Azure Active Directory B2C (Azure AD B2C), deben estar registradas en un inquilino que administre.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Creación de un inquilino de Azure AD B2C
> * Vinculación del inquilino a la suscripción

En el siguiente tutorial aprenderá a registrar una aplicación.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-an-azure-ad-b2c-tenant"></a>Creación de un inquilino de Azure AD B2C

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2. Asegúrese de que está usando el directorio que contiene su suscripción. Seleccione el filtro **Directorio y suscripción** en el menú superior, seleccione el directorio que contiene la suscripción. Se trata de un directorio diferente al que contendrá al inquilino de Azure AD B2C.

    ![Filtro de directorio y suscripción con el inquilino de suscripción seleccionado](./media/tutorial-create-tenant/switch-directory-subscription.PNG)

3. Elija **Crear un recurso** en la esquina superior izquierda de Azure Portal.
4. Busque y seleccione **Active Directory B2C** y haga clic en **Crear**.
5. Elija **Crear un nuevo inquilino de Azure AD B2C** y escriba el nombre de la organización y el del dominio inicial. Seleccione el país o la región (no se puede cambiar más adelante) y haga clic en **Crear**.

    El nombre de dominio inicial se usa como parte del nombre de inquilino. En este ejemplo, el nombre del inquilino es *contoso0926Tenant.onmicrosoft.com*:

    ![Página de creación de inquilinos de B2C en Azure Portal](./media/tutorial-create-tenant/create-tenant.PNG)

6. En la página **Crear nuevo inquilino B2C o vincular al inquilino existente**, elija **Vincular un inquilino de Azure AD B2C existente a mi suscripción de Azure**.

    Seleccione el inquilino que ha creado y su suscripción.

    Como grupo de recursos, seleccione **Crear nuevo**. Escriba un nombre para el grupo de recursos que contendrá el inquilino, seleccione la ubicación y, después, haga clic en **Crear**.
1. Para empezar a usar el nuevo inquilino, asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello,seleccione el filtro **Directorio y suscripción** en el menú superior y elija el directorio que lo contiene.

    Si al principio no ve el nuevo inquilino de Azure B2C en la lista en la lista, actualice la ventana del explorador y seleccione el filtro **Directorio y suscripción** de nuevo en el menú superior.

    ![Filtro de directorio y suscripción con el inquilino de B2C seleccionado](./media/tutorial-create-tenant/switch-directories.PNG)

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo:

> [!div class="checklist"]
> * Creación de un inquilino de Azure AD B2C
> * Vinculación del inquilino a la suscripción

A continuación, aprenda a registrar una aplicación web en el nuevo inquilino.

> [!div class="nextstepaction"]
> [Registro de las aplicaciones >](tutorial-register-applications.md)
