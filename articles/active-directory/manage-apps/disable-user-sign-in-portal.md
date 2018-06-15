---
title: Deshabilitación de los inicios de sesión de usuario de una aplicación empresarial en Azure Active Directory | Microsoft Docs
description: Cómo deshabilitar una aplicación empresarial para que ningún usuario pueda iniciar sesión en ella en Azure Active Directory
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: barbkess
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 86ff34bb0780811663d437a9183d0b688b69073d
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35302994"
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Deshabilitación de los inicios de sesión de usuario de una aplicación empresarial en Azure Active Directory
Deshabilitar una aplicación empresarial para que ningún usuario pueda iniciar sesión en ella en Azure Active Directory (Azure AD) es fácil. Debe tener los permisos adecuados para administrar la aplicación de empresa y debe ser administrador global en el directorio.

## <a name="how-do-i-disable-user-sign-ins"></a>¿Cómo puedo deshabilitar los inicios de sesión de usuario?
1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta que tenga el rol de administrador global en el directorio.
2. Seleccione **Todos los servicios**, escriba **Azure Active Directory** en el cuadro de texto y, después, seleccione **Entrar**.
3. En el panel **Azure Active Directory** -  ***nombreDelDirectorio*** (es decir, el panel de Azure AD del directorio que está administrando), seleccione **Aplicaciones empresariales**.

    ![Apertura de Enterprise apps (Aplicaciones empresariales)](./media/disable-user-sign-in-portal/open-enterprise-apps.png)
4. En el panel **Aplicaciones empresariales**, seleccione **Todas las aplicaciones**. Verá una lista de las aplicaciones que puede administrar.
5. En el panel **Aplicaciones empresariales - Todas las aplicaciones**, seleccione una aplicación.
6. En el panel ***nombreDeLaAplicación*** (es decir, el panel con el nombre de la aplicación seleccionada en el título), seleccione **Propiedades**.

    ![Selección del comando Todas las aplicaciones](./media/disable-user-sign-in-portal/select-app.png)
7. En el panel ***nombreDeLaAplicación*** - **Propiedades**, en **¿Habilitado para que los usuarios inicien sesión?**, seleccione **No**.
8. Haga clic en el comando **Guardar** .

## <a name="next-steps"></a>Pasos siguientes
* [Ver todos mis grupos](../active-directory-groups-view-azure-portal.md)
* [Asignar un usuario o grupo a una aplicación empresarial](assign-user-or-group-access-portal.md)
* [Eliminación de asignaciones de usuario o grupo de una aplicación empresarial](remove-user-or-group-access-portal.md)
* [Cambio del nombre o el logotipo de una aplicación empresarial](change-name-or-logo-portal.md)
