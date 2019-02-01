---
title: Cambio del nombre o el logotipo de una aplicación empresarial en Azure Active Directory | Microsoft Docs
description: Cómo cambiar el nombre o el logotipo de una aplicación empresarial personalizada en Azure Active Directory
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/28/2017
ms.author: barbkess
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 2813ef3ebea4446d0d2dffa20aa79d1b0b5509cb
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55160724"
---
# <a name="change-the-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a>Cambio del nombre o el logotipo de una aplicación empresarial en Azure Active Directory
Cambiar el nombre o el logotipo de una aplicación empresarial personalizada en Azure Active Directory (Azure AD) es fácil. Debe tener los permisos adecuados para realizar estos cambios, y debe ser el creador de la aplicación personalizada.

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a>¿Cómo puedo cambiar el nombre o el logotipo de una aplicación empresarial?
1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta que tenga el rol de administrador global en el directorio.
2. Seleccione **Todos los servicios**, escriba **Azure Active Directory** en el cuadro de texto y, después, seleccione **Entrar**.
3. En el panel **Azure Active Directory - *nombreDelDirectorio*** (es decir, el panel de Azure AD del directorio que está administrando), seleccione **Aplicaciones empresariales**.

    ![Apertura de Enterprise apps (Aplicaciones empresariales)](./media/change-name-or-logo-portal/open-enterprise-apps.png)
4. En el panel **Aplicaciones empresariales**, seleccione **Todas las aplicaciones**. Verá una lista de las aplicaciones que puede administrar.
5. En el panel **Aplicaciones empresariales - Todas las aplicaciones**, seleccione una aplicación.
6. En el panel ***nombreDeLaAplicación*** (es decir, el panel con el nombre de la aplicación seleccionada en el título), seleccione **Propiedades**.

    ![Selección del comando Propiedades](./media/change-name-or-logo-portal/select-app.png)
7. En el panel ***nombreDeAplicación*** **- Propiedades**, busque el archivo que desea usar como nuevo logotipo o edite el nombre de la aplicación, o ambas opciones.

    ![Cambio del logotipo o de la aplicación o comando de propiedades de nombre](./media/change-name-or-logo-portal/change-logo.png)
8. Haga clic en el comando **Guardar** .

## <a name="next-steps"></a>Pasos siguientes
* [Ver todos mis grupos](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Asignar un usuario o grupo a una aplicación empresarial](assign-user-or-group-access-portal.md)
* [Eliminación de asignaciones de usuario o grupo de una aplicación empresarial](remove-user-or-group-access-portal.md)
* [Deshabilitar los inicios de sesión de los usuarios de una aplicación empresarial](disable-user-sign-in-portal.md)
