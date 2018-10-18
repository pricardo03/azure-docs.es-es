---
title: 'Tutorial: Creación de un inquilino de Azure Active Directory B2C | Microsoft Docs'
description: Aprenda a preparar el registro de sus aplicaciones mediante la creación de un inquilino de Azure Active Directory B2C desde Azure Portal.
services: B2C
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/26/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7571e5f4d95320ab92fa3b69b0ea1f05ff9c771f
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47408409"
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
2. Asegúrese de que usa el directorio que contiene la suscripción. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que la contiene. Se trata de un directorio diferente al que contendrá al inquilino de Azure AD B2C.

    ![Cambiar al directorio de suscripción](./media/tutorial-create-tenant/switch-directory-subscription.png)

3. Elija **Crear un recurso** en la esquina superior izquierda de Azure Portal.
4. Busque y seleccione **Active Directory B2C** y haga clic en **Crear**.
5. Elija **Create a new Azure AD B2C Tenant** (Crear un inquilino de Azure AD B2C), escriba un nombre de organización y un nombre de dominio inicial, que se utilice en el nombre del inquilino, seleccione el país (no se puede cambiar después) y haga clic en **Crear**.

    ![Creación de un inquilino](./media/tutorial-create-tenant/create-tenant.png)

    En este ejemplo, el nombre del inquilino es contoso0926Tenant.onmicrosoft.com.

6. En la página **Create new B2C Tenant or Link to an exiting Tenant** (Crear inquilino de B2C o vincular a un inquilino existente), elija **Vincular un inquilino de Azure AD B2C existente a mi suscripción de Azure**. A continuación, seleccione el inquilino que ha creado, elija su suscripción, haga clic en **Crear nuevo** y escriba un nombre para el grupo de recursos que contendrá el inquilino. Después, seleccione la ubicación y haga clic en **Crear**.
7. Para empezar a usar el nuevo inquilino, asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que lo contiene.

    ![Cambiar al directorio del inquilino](./media/tutorial-create-tenant/switch-directories.png)

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo:

> [!div class="checklist"]
> * Creación de un inquilino de Azure AD B2C
> * Vinculación del inquilino a la suscripción

> [!div class="nextstepaction"]
> [Habilitación de una aplicación web para la autenticación con cuentas](active-directory-b2c-tutorials-web-app.md)