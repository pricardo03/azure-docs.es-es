---
title: Ocultación de una aplicación de la experiencia del usuario en Azure Active Directory | Microsoft Docs
description: Cómo ocultar una aplicación de la experiencia del usuario en los paneles de acceso de Azure Active Directory o los iniciadores de Office 365.
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
ms.date: 01/04/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: b60384663d79294531225612a767663e0d71723f
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35302965"
---
# <a name="hide-an-application-from-users-experience-in-azure-active-directory"></a>Ocultación de una aplicación de la experiencia del usuario en Azure Active Directory

Si tiene una aplicación que no quiere que aparezca en los paneles de acceso de los usuarios o en los iniciadores de Office 365, hay varias opciones para ocultar el icono de esta aplicación.  Las dos opciones siguientes están disponibles para ocultar las aplicaciones de los iniciadores de aplicaciones del usuario.

- Ocultar una aplicación de terceros de los paneles de acceso de los usuarios y los iniciadores de aplicaciones de Office 365
- Ocultar todas las aplicaciones de Office 365 de los paneles de acceso de los usuarios

Al ocultar la aplicación, los usuarios todavía tendrán permisos de acceso a ella pero no podrán verla en los iniciadores de aplicaciones. Debe tener los permisos adecuados para administrar la aplicación empresarial y debe ser administrador global en el directorio.


## <a name="hiding-an-application-from-users-end-user-experiences"></a>Ocultación de una aplicación de las experiencias del usuario final
Puede seguir estos pasos, en función de su situación, para ocultar las aplicaciones del panel de acceso.

### <a name="how-do-i-hide-a-third-party-app-from-users-access-panel-and-o365-app-launchers"></a>¿Cómo se puede ocultar una aplicación de terceros del panel de acceso del usuario y de los iniciadores de aplicaciones de Office 365?
Siga estos pasos para ocultar una aplicación del panel de acceso de un usuario y de los iniciadores de aplicaciones de Office 365.

1.  Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta que tenga el rol de administrador global en el directorio.
2.  Seleccione **Todos los servicios**, escriba **Azure Active Directory** en el cuadro de texto y, después, seleccione **Entrar**.
3.  En la pantalla **Azure Active Directory - *nombreDelDirectorio*** (es decir, la pantalla de Azure AD del directorio que está administrando), seleccione **Aplicaciones empresariales**.
![Aplicaciones empresariales](./media/hide-application-from-user-portal/app1.png)
4.  En la pantalla **Aplicaciones empresariales**, seleccione **Todas las aplicaciones**. Verá una lista de las aplicaciones que puede administrar.
5.  En la pantalla **Aplicaciones empresariales - Todas las aplicaciones**, seleccione una aplicación.</br>
![Aplicaciones empresariales](./media/hide-application-from-user-portal/app2.png)
6.  En la pantalla ***nombreDeLaAplicación*** (es decir, la pantalla con el nombre de la aplicación seleccionada en el título), seleccione Propiedades.
7.  En la pantalla ***nombreDeLaAplicación* - Propiedades**, seleccione **Sí** en **¿Es visible para los usuarios?**
![Aplicaciones empresariales](./media/hide-application-from-user-portal/app3.png)
8.  Haga clic en el comando **Guardar** .

### <a name="how-do-i-hide-office-365-applications-from-users-access-panel"></a>¿Cómo se pueden ocultar las aplicaciones de Office 365 del panel de acceso del usuario?

Siga estos pasos para ocultar todas las aplicaciones de Office 365 del panel de acceso. Estas aplicaciones seguirán estando visibles en el Portal de Office 365.

1.  Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta que tenga el rol de administrador global en el directorio.
2.  Seleccione **Todos los servicios**, escriba **Azure Active Directory** en el cuadro de texto y, después, seleccione **Entrar**.
3.  En la pantalla **Azure Active Directory - *nombreDelDirectorio*** (es decir, la pantalla de Azure AD del directorio que está administrando), seleccione **Configuración de usuario**.
4.  En la pantalla **Configuración de usuario**, en **Aplicaciones empresariales**, seleccione **Sí** en **Los usuarios solo pueden ver las aplicaciones de Office 365 en el Portal de Office 365**.

![Aplicaciones Enterprise](./media/hide-application-from-user-portal/apps4.png)

## <a name="next-steps"></a>Pasos siguientes
* [Ver todos mis grupos](../active-directory-groups-view-azure-portal.md)
* [Asignar un usuario o grupo a una aplicación empresarial](assign-user-or-group-access-portal.md)
* [Eliminación de asignaciones de usuario o grupo de una aplicación empresarial](remove-user-or-group-access-portal.md)
* [Cambio del nombre o el logotipo de una aplicación empresarial](change-name-or-logo-portal.md)

