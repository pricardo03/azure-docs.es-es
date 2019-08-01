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
ms.openlocfilehash: 711b9152f9f3fa1b3573e39d1950f18b628c268a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056321"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Tutorial: Creación de un inquilino de Azure Active Directory B2C

Para que sus aplicaciones puedan interactuar con Azure Active Directory (Azure AD) B2C, deben estar registradas en un inquilino que administre.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Creación de un inquilino de Azure AD B2C
> * Vinculación del inquilino a la suscripción

En el siguiente tutorial aprenderá a registrar una aplicación.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-an-azure-ad-b2c-tenant"></a>Creación de un inquilino de Azure AD B2C

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2. Asegúrese de que está usando el directorio que contiene su suscripción. Haga clic en el **filtro de directorio y suscripciones** en el menú superior y seleccione el directorio que contiene su suscripción. Se trata de un directorio diferente al que contendrá al inquilino de Azure AD B2C.

    ![Cambiar al directorio de suscripción](./media/tutorial-create-tenant/switch-directory-subscription.PNG)

3. Elija **Crear un recurso** en la esquina superior izquierda de Azure Portal.
4. Busque y seleccione **Active Directory B2C** y haga clic en **Crear**.
5. Elija **Crear un nuevo inquilino de Azure AD B2C** y escriba el nombre de la organización y el del dominio inicial. Seleccione el país o la región (no se puede cambiar más adelante) y haga clic en **Crear**.

    El nombre de dominio inicial se usa como parte del nombre de inquilino. En este ejemplo, el nombre del inquilino es *contoso0926Tenant.onmicrosoft.com*:

    ![Creación de un inquilino](./media/tutorial-create-tenant/create-tenant.PNG)

6. En la página **Crear nuevo inquilino B2C o vincular al inquilino existente**, elija **Vincular un inquilino de Azure AD B2C existente a mi suscripción de Azure**.

    Seleccione el inquilino que ha creado y su suscripción.

    Como grupo de recursos, seleccione **Crear nuevo**. Escriba un nombre para el grupo de recursos que contendrá el inquilino, seleccione la ubicación y, después, haga clic en **Crear**.
1. Para empezar a usar el nuevo inquilino, asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que lo contiene.

    Si al principio no ve el nuevo inquilino de Azure B2C en la lista en la lista, actualice la ventana del explorador y seleccione el **filtro de directorio y suscripción** de nuevo en el menú superior.

    ![Cambiar al directorio del inquilino](./media/tutorial-create-tenant/switch-directories.PNG)

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo:

> [!div class="checklist"]
> * Creación de un inquilino de Azure AD B2C
> * Vinculación del inquilino a la suscripción

A continuación, aprenda a registrar una aplicación web en el nuevo inquilino.

> [!div class="nextstepaction"]
> [Registro de las aplicaciones >](tutorial-register-applications.md)
