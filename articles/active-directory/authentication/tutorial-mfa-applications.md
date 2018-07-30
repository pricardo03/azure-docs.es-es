---
title: Habilitación de un entorno piloto de Azure Multi-Factor Authentication
description: En este tutorial, habilitará Azure AD Multi-Factor Authentication para un grupo piloto de usuarios
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 098973e2ece3477ec87b154c0304c4ca7e0246d1
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2018
ms.locfileid: "39163393"
---
# <a name="tutorial-complete-an-azure-multi-factor-authentication-pilot-roll-out"></a>Tutorial: Habilitación de un entorno piloto de Azure Multi-Factor Authentication

En este tutorial, realizará los pasos para configurar una directiva de acceso condicional que habilite Azure Multi-factor Authentication (Azure MFA) al iniciar sesión en Azure Portal. La directiva se implementa y se prueba en un grupo específico de usuarios piloto. La implementación de Azure MFA con acceso condicional proporciona una flexibilidad significativa para las organizaciones y los administradores en comparación con el método tradicional aplicado.

> [!div class="checklist"]
> * Habilitación de Azure Multi-Factor Authentication
> * Prueba de Azure Multi-Factor Authentication

## <a name="prerequisites"></a>Requisitos previos

* Un inquilino de Azure AD activo con al menos una licencia de prueba habilitada.
* Una cuenta con privilegios de administrador global.
* Un usuario de prueba sin privilegios de administrador con una contraseña que conozca para las pruebas; si necesita crear un usuario, consulte el artículo [Inicio rápido: Incorporación de nuevos usuarios a Azure Active Directory](../add-users-azure-active-directory.md).
* Un grupo piloto para las pruebas del que el usuario sin privilegios de administrador sea miembro; si necesita crear un grupo, consulte el artículo [Creación de un grupo e incorporación de miembros en Azure Active Directory](../active-directory-groups-create-azure-portal.md).

## <a name="enable-azure-multi-factor-authentication"></a>Habilitación de Azure Multi-Factor Authentication

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta de administrador global.
1. Vaya a **Azure Active Directory** , **Acceso condicional**
1. Seleccione **Nueva directiva**
1. Denomine a la directiva **Piloto de MFA**
1. En **Usuarios y grupos**, seleccione el botón de opción **Seleccionar usuarios y grupos**
    * Seleccione el grupo piloto creado como parte de la sección de requisitos previos de este artículo
    * Haga clic en **Listo**.
1. En **Aplicaciones en la nube**, seleccione el botón de opción **Seleccionar aplicaciones**
    * La aplicación en la nube para Azure Portal es **Portal de administración de Microsoft Azure**
    * Haga clic en **Seleccionar**
    * Haga clic en **Listo**.
1. Omita la sección **Condiciones**
1. En **Conceder**, asegúrese de que el botón de opción **Conceder acceso** está seleccionado
    * Active la casilla **Requerir autenticación multifactor**
    * Haga clic en **Seleccionar**
1. Omita la sección **Sesión**
1. Establezca la opción **Habilitar directiva** en **Activado**
1. Haga clic en **Crear**

## <a name="test-azure-multi-factor-authentication"></a>Prueba de Azure Multi-Factor Authentication

Para demostrar que la directiva de acceso condicional funciona, pruebe a iniciar sesión en un recurso que no deba requerir MFA y, a continuación, en Azure Portal que requiera MFA.

1. Abra una nueva ventana del explorador en modo de incógnito o InPrivate, y vaya a [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com).
   * Inicie sesión con el usuario de prueba que creó como parte de la sección de requisitos previos de este artículo y tenga en cuenta que no debe pedirle que complete MFA.
   * Cierre la ventana del explorador.
2. Abra una nueva ventana del explorador en modo de incógnito o InPrivate, y vaya a [https://portal.azure.com](https://portal.azure.com).
   * Inicie sesión con el usuario de prueba creado como parte de la sección de requisitos previos de este artículo y tenga en cuenta que ahora se le debería exigir que se registre y use Azure Multi-Factor Authentication.
   * Cierre la ventana del explorador.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si decide que ya no desea utilizar la funcionalidad que se ha configurado como parte de este tutorial, realice el siguiente cambio.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Vaya a **Azure Active Directory** , **Acceso condicional**.
1. Seleccione la directiva de acceso condicional que ha creado.
1. Hacer clic en **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha habilitado Azure Multi-Factor Authentication. Continúe con el siguiente tutorial para ver cómo se puede integrar Azure Identity Protection en las experiencias de Multi-Factor Authentication y autoservicio de restablecimiento de contraseñas.

> [!div class="nextstepaction"]
> [Evaluación del riesgo en el inicio de sesión](tutorial-risk-based-sspr-mfa.md)