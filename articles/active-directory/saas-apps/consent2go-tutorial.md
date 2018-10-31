---
title: 'Tutorial: Integración de Azure Active Directory con Consent2Go | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Consent2Go.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ea93bc02-58ca-4468-84ff-359888fc6183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/09/2018
ms.author: jeedes
ms.openlocfilehash: 89fc0b69a3a7ca3f795b4ae0e79b11a4bcd9c9fb
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49433515"
---
# <a name="tutorial-azure-active-directory-integration-with-consent2go"></a>Tutorial: integración de Azure Active Directory con Consent2Go

En este tutorial, aprenderá a integrar Consent2Go con Azure Active Directory (Azure AD).

Integrar Consent2Go con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Consent2Go.
- Puede permitir que los usuarios inicien sesión automáticamente en Consent2Go (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Consent2Go, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Consent2Go.

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Consent2Go desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-consent2go-from-the-gallery"></a>Incorporación de Consent2Go desde la galería
Para configurar la integración de Consent2Go en Azure AD, deberá agregar Consent2Go desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Consent2Go desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/consent2go-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/consent2go-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/consent2go-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **Consent2Go**, seleccione **Consent2Go** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/consent2go-tutorial/tutorial_consent2go_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, va a configurar y probar el inicio de sesión único de Azure AD con Consent2Go con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Consent2Go para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Consent2Go.

Para configurar y probar el inicio de sesión único de Azure AD con Consent2Go, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Consent2Go](#create-a-consent2go-test-user)**: para tener un homólogo de Britta Simon en Consent2Go que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Consent2Go.

**Para configurar el inicio de sesión único de Azure AD con Consent2Go, siga estos pasos:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Consent2Go**, haga clic en **Inicio de sesión único**.

    ![imagen](./media/consent2go-tutorial/b1_b2_select_sso.png)

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![imagen](./media/consent2go-tutorial/b1_b2_saml_sso.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/consent2go-tutorial/b1-domains_and_urlsedit.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    En el cuadro de texto **URL de inicio de sesión**, escriba una URL como: `   https://www.mcbschools.com/Login`

    ![imagen](./media/consent2go-tutorial/tutorial_consent2go_url.png)

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el icono de copia para copiar la **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![imagen](./media/consent2go-tutorial/tutorial_consent2go_certificate.png) 

6. Para configurar el inicio de sesión único en **Consent2Go**, tiene que enviar la **dirección URL de metadatos de federación de la aplicación** generada al [equipo de soporte técnico de Consent2Go](mailto:support@consent2go.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![imagen](./media/consent2go-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/consent2go-tutorial/d_adduser.png)

3. En las propiedades Usuario, siga estos pasos.

    ![imagen](./media/consent2go-tutorial/d_userproperties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-a-consent2go-test-user"></a>Creación de un usuario de prueba de Consent2Go

En esta sección, creará un usuario llamado Britta Simon en Consent2Go. Trabaje con el [equipo de soporte técnico de Consent2Go](mailto:support@consent2go.com) para agregar los usuarios a la plataforma de Consent2Go. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Consent2Go.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![imagen](./media/consent2go-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **Consent2Go**.

    ![imagen](./media/consent2go-tutorial/tutorial_consent2go_app.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/consent2go-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![imagen](./media/consent2go-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Consent2Go en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación Consent2Go.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


