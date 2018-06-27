---
title: 'Tutorial: Creación de un inquilino de Azure Active Directory B2C | Microsoft Docs'
description: Aprenda a preparar el registro de sus aplicaciones mediante la creación de un inquilino de Azure Active Directory B2C desde Azure Portal.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 06/19/2018
ms.author: davidmu
ms.openlocfilehash: 04f3dbbe461bfe0f07b6930a92bdd8a721e55098
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296792"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Tutorial: Creación de un inquilino de Azure Active Directory B2C

Para que sus aplicaciones puedan interactuar con Azure Active Directory (Azure AD) B2C, deben estar registradas en un inquilino que administre.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Crear un inquilino de Azure AD B2C
> * Vincular el inquilino a la suscripción

En el siguiente tutorial aprenderá a registrar una aplicación.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

Inicie sesión en [Azure Portal](https://portal.azure.com/).

## <a name="create-an-azure-ad-b2c-tenant"></a>Creación de un inquilino de Azure AD B2C

1. Elija **Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. En el cuadro de búsqueda que está encima de la lista de recursos de Azure Marketplace, busque y **Active Directory B2C**, selecciónelo y, después, haga clic en **Crear**.
3. Elija **Create a new Azure AD B2C Tenant** (Crear un inquilino de Azure AD B2C), escriba un nombre de organización y un nombre de dominio inicial, que se utilice en el nombre del inquilino, seleccione el país y haga clic en **Crear**. Asegúrese de que el país del inquilino es el correcto, ya que no se puede cambiar posteriormente.

    ![Creación de un inquilino](./media/tutorial-create-tenant/create-tenant.png)

    En este ejemplo, el nombre del inquilino es contoso0522Tenant.onmicrosoft.com

Para empezar a administrar el nuevo inquilino, haga clic en la palabra **here** donde **Click here, to manage your new directory** (Haga clic aquí para administrar el directorio nuevo). Verá un mensaje de **solución de problemas** que indica que necesita vincular su suscripción al nuevo inquilino. 

## <a name="link-your-tenant-to-your-subscription"></a>Vinculación del inquilino a la suscripción

Debe vincular su inquilino de Azure AD B2C a su suscripción a Azure para habilitar todas las funcionalidades y pagar los cargos por uso. Si no vincula al inquilino a su suscripción, las aplicaciones no funcionarán correctamente.

1. Asegúrese de que usa el directorio que contiene la suscripción que desea asociar al nuevo inquilino, para lo que debe cambiar el directorio en la esquina superior derecha de Azure Portal.

    ![Cambio de directorios](./media/tutorial-create-tenant/switch-directories.png)

    Y, después, seleccionar el directorio que contiene la suscripción.

    ![Selección de directorio](./media/tutorial-create-tenant/select-directory.png)

2. Elija **Crear un recurso** en la esquina superior izquierda de Azure Portal.
3. En el cuadro de búsqueda que está encima de la lista de recursos de Azure Marketplace, busque y **Active Directory B2C**, selecciónelo y, después, haga clic en **Crear**.
4. Elija **Link an existing Azure AD B2C Tenant to my Azure subscription** (Vincular un inquilino de Azure AD B2C existente a mi suscripción de Azure), seleccione el inquilino que creó, seleccione su suscripción, escriba *myContosoTenantRG* como nombre de grupo de recursos, acepte la ubicación y haga clic en **Crear**.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo:

> [!div class="checklist"]
> * Creación de un inquilino de Azure AD B2C
> * Vinculación del inquilino a la suscripción

> [!div class="nextstepaction"]
> [Habilitación de una aplicación web para la autenticación con cuentas](active-directory-b2c-tutorials-web-app.md)