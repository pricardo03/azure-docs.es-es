---
title: Inicio rápido para crear inquilino y crearlos (Azure Active Directory) | Microsoft Docs
description: Instrucciones para buscar Azure Active Directory y crear un inquilino para su organización.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4bc3e3bb178efbbc8f4cee6d81417ce70bd7a662
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562071"
---
# <a name="quickstart-create-a-new-tenant-in-azure-active-directory"></a>Inicio rápido: Creación de un inquilino en Azure Active Directory
Puede hacer todas las tareas administrativas mediante el portal de Azure Active Directory (Azure AD), incluida la creación de un inquilino para su organización. 

En este tutorial, obtendrá información sobre cómo acceder a Azure Portal y a Azure Active Directory y sobre cómo crear un inquilino básico para su organización.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal
Inicie sesión en [Azure Portal](https://portal.azure.com/) en su organización con una cuenta de administrador global.

![Pantalla de Azure Portal, con la opción de Azure AD](media/active-directory-access-create-new-tenant/azure-ad-portal.png)

## <a name="create-a-new-tenant-for-your-organization"></a>Creación de un inquilino para la organización
Después de iniciar sesión en Azure Portal, puede crear un inquilino para su organización. El nuevo inquilino representa a su organización y le ayuda a administrar una instancia específica de Servicios en la nube de Microsoft para los usuarios internos y externos.

### <a name="to-create-a-new-tenant"></a>Para crear un inquilino
1. Seleccione **Crear un recurso**, **Identidad** y **Azure Active Directory**.

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

    ![Página de información general con el botón para eliminar directorio resaltado](media/active-directory-access-create-new-tenant/azure-ad-delete-new-tenant.png)

## <a name="next-steps"></a>Pasos siguientes
- Para cambiar o agregar nombres de dominio adicionales, vea [Incorporación de su nombre de dominio personalizado a Azure Active Directory](add-custom-domain.md).

- Para agregar usuarios, vea [Incorporación o eliminación de un nuevo usuario](add-users-azure-active-directory.md).

- Para agrear grupos y miembros, vea [Creación de un grupo básico y adición de miembros](active-directory-groups-create-azure-portal.md).

- Obtenga información sobre el [acceso basado en rol mediante Privileged Identity Management](../../role-based-access-control/pim-azure-resource.md) y el [acceso condicional](../../role-based-access-control/conditional-access-azure-management.md) para ayudar a administrar el acceso a aplicaciones y recursos de su organización.

- Obtenga información sobre Azure AD, incluida la [información de licencia básica, terminología y características asociadas](active-directory-whatis.md).
