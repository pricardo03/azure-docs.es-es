---
title: 'Tutorial: Integración de Azure Active Directory con GetThere | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y GetThere.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0441087e-953f-4b51-9842-316da7b72392
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2018
ms.author: jeedes
ms.openlocfilehash: bcefa3966a6c854f02ce7b3a75306b3d1c888ecd
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49433107"
---
# <a name="tutorial-azure-active-directory-integration-with-getthere"></a>Tutorial: integración de Azure Active Directory con GetThere

En este tutorial, aprenderá a integrar GetThere con Azure Active Directory (Azure AD).

La integración de GetThere con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a GetThere.
- Puede permitir que los usuarios inicien sesión automáticamente en GetThere (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con GetThere, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en GetThere.

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de GetThere desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-getthere-from-the-gallery"></a>Adición de GetThere desde la galería
Para configurar la integración de GetThere en Azure AD, será preciso que agregue GetThere desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar GetThere desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/getthere-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/getthere-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/getthere-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **GetThere**, seleccione **GetThere** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/getthere-tutorial/tutorial_getthere_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con GetThere con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de GetThere para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario asociado de GetThere.

Para configurar y probar el inicio de sesión único de Azure AD con GetThere, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de GetThere](#create-a-getthere-test-user)**: para tener un homólogo de Britta Simon en GetThere que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación GetThere.

**Para configurar el inicio de sesión único de Azure AD con GetThere, siga estos pasos:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **GetThere**, seleccione **Inicio de sesión único**.

    ![imagen](./media/getthere-tutorial/B1_B2_Select_SSO.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML** para habilitar el inicio de sesión único.

    ![imagen](./media/getthere-tutorial/b1_b2_saml_sso.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/getthere-tutorial/b1-domains_and_urlsedit.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![imagen](./media/getthere-tutorial/tutorial_getthere_url.png)

    a. En el cuadro de texto **Identificador**, escriba cualquiera de las siguientes direcciones URL:
    | |
    |--|
    | `getthere.com` |
    | `http://idp.getthere.com` |

    b. En el cuadro de texto **Dirección URL de respuesta**, escriba cualquiera de las siguientes direcciones URL:
    | |
    |--|
    | `https://wx1.getthere.net/login/saml/post.act` |
    | `https://gtx2-gcte2.getthere.net/login/saml/post.act` |
    | `https://gtx2-gcte2.getthere.net/login/saml/ssoaasvalidate.act` |
    | `https://wx1.getthere.net/login/saml/ssoaavalidate.act` |
    
5. La aplicación GetThere espera el valor **Nombre de usuario** único en la notificación de nombre de usuario. El cliente puede asignar el valor correcto de la notificación del nombre de usuario. En el ejemplo siguiente, hemos asignado el **nombre de usuario** a **user.mail**, pero se puede cambiar la asignación según la configuración de la organización. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](./media/getthere-tutorial/i4-attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:
    
    | NOMBRE |  Atributo de origen |  Espacio de nombres |
    | ---------------| --------------- | --------------- |
    | Sitename | "Proporcione el valor según la organización" | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/sitename |
    | Nombre de usuario |  user.mail | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/username |
    
    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](./media/getthere-tutorial/i2-attribute.png)

    ![imagen](./media/getthere-tutorial/i3-attribute.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. En el cuadro de texto **Espacio de nombres**, escriba el espacio de nombres de atributo que se muestra para esa fila.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Save**(Guardar).

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** y guárdelo en su equipo.

    ![imagen](./media/getthere-tutorial/tutorial_getthere_certficate.png) 

8. En la sección **Configurar GetThere**, copie la dirección URL adecuada según sus necesidades.

    Tenga en cuenta que en la dirección URL puede poner lo siguiente:

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![imagen](./media/getthere-tutorial/d1_samlsonfigure.png) 

9. Para configurar el inicio de sesión único en **GetThere**, es preciso enviar el **certificado (Base64)** descargado y el valor de **Login URL, Azure Ad Identifier, Logout URL** (URL de cierre de sesión, identificador de Azure AD y URL de inicio de sesión) copiado a [GetThere Client support team](mailto:dataintegration@sabre.com) (Equipo de soporte técnico del cliente GetThere). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![imagen](./media/getthere-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/getthere-tutorial/d_adduser.png)

3. En las propiedades Usuario, siga estos pasos.

    ![imagen](./media/getthere-tutorial/d_userproperties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-a-getthere-test-user"></a>Creación de un usuario de prueba de GetThere

En esta sección, creará un usuario llamado Britta Simon en GetThere. Trabaje con el [equipo de soporte técnico del cliente de GetThere](mailto:dataintegration@sabre.com) para agregar los usuarios a la plataforma de GetThere. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a GetThere.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![imagen](./media/getthere-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **GetThere**.

    ![imagen](./media/getthere-tutorial/tutorial_getthere_app.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/getthere-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![imagen](./media/getthere-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de GetThere en el panel de acceso, debería iniciar sesión automáticamente en su aplicación GetThere.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
