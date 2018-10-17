---
title: Guía de inicio rápido para acceder a Azure Active Directory y crear un inquilino | Microsoft Docs
description: Guía de inicio rápido con pasos para buscar Azure Active Directory y crear un inquilino para su organización.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: lizross
custom: it-pro
ms.openlocfilehash: 8ef68c8afcf61a1a11c341a679443071aece9812
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363011"
---
# <a name="quickstart-access-azure-active-directory-to-create-a-new-tenant"></a>Guía de inicio rápido: Acceso a Azure Active Directory para crear un inquilino
Puede hacer todas las tareas administrativas mediante el portal de Azure Active Directory (Azure AD), incluida la creación de un inquilino para su organización. 

En este tutorial, obtendrá información sobre cómo acceder a Azure Portal y a Azure Active Directory y sobre cómo crear un inquilino básico para su organización.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos
Antes de comenzar, deberá:

- Asegurarse de que su organización tiene una licencia de Azure AD válida.

- Asegurarse de que es un administrador global.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal
Inicie sesión en [Azure Portal](https://portal.azure.com/) en su organización con una cuenta de administrador global.

![Pantalla de Azure Portal](media/active-directory-access-create-new-tenant/azure-ad-portal.png)

## <a name="create-a-new-tenant-for-your-organization"></a>Creación de un inquilino para la organización
Después de iniciar sesión en Azure Portal, puede crear un inquilino para su organización. El nuevo inquilino representa a su organización y le ayuda a administrar una instancia específica de Servicios en la nube de Microsoft para los usuarios internos y externos.

### <a name="to-create-a-new-tenant"></a>Para crear un inquilino
1. Seleccione **Azure Active Directory**, **Crear recursos**, **Identidad** y **Azure Active Directory**.

    Aparece la página **Crear directorio**.

    ![Página Crear de Azure Active Directory](media/active-directory-access-create-new-tenant/azure-ad-create-new-tenant.png)

2.  En la página **Crear directorio**, escriba la información siguiente:
    
    - Escriba _Contoso_ en el cuadro **Nombre de la organización**.

    - Escriba _Contoso_ en el cuadro **Nombre de dominio inicial**.

    - Deje la opción _Estados Unidos_ en el cuadro **País o región**.

3. Seleccione **Crear**.

El nuevo inquilino se crea con el dominio contoso.onmicrosoft.com.

## <a name="clean-up-resources"></a>Limpieza de recursos
Si no va a seguir usando esta aplicación, puede eliminar el inquilino mediante los siguientes pasos:

- Seleccione **Azure Active Directory** y, después, en la página **Contoso - Información general**, seleccione **Eliminar directorio**.

    El inquilino y su información asociada se eliminarán.

    ![Página Crear directorio](media/active-directory-access-create-new-tenant/azure-ad-delete-new-tenant.png)

## <a name="next-steps"></a>Pasos siguientes
- Para cambiar o agregar nombres de dominio adicionales, vea [Incorporación de su nombre de dominio personalizado a Azure Active Directory](add-custom-domain.md).

- Para agregar usuarios, vea [Incorporación o eliminación de un nuevo usuario](add-users-azure-active-directory.md).

- Para agrear grupos y miembros, vea [Creación de un grupo básico y adición de miembros](active-directory-groups-create-azure-portal.md).

- Obtenga información sobre el [acceso basado en rol mediante Privileged Identity Management](../../role-based-access-control/pim-azure-resource.md) y el [acceso condicional](../../role-based-access-control/conditional-access-azure-management.md) para ayudar a administrar el acceso a aplicaciones y recursos de su organización.
