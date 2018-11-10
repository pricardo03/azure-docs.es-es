---
title: 'Tutorial: Integración de Azure Active Directory con BorrowBox | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y BorrowBox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: dd8e4178-9a63-492a-bd48-782e94e404af
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2018
ms.author: jeedes
ms.openlocfilehash: a8ed2f04bf3004907cdd6e33bfb30260233fb101
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50157163"
---
# <a name="tutorial-azure-active-directory-integration-with-borrowbox"></a>Tutorial: Integración de Azure Active Directory con BorrowBox

En este tutorial, aprenderá a integrar BorrowBox con Azure Active Directory (Azure AD).

La integración de BorrowBox con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a BorrowBox.
- Puede permitir que los usuarios inicien sesión automáticamente en BorrowBox (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con BorrowBox, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único de BorrowBox

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de BorrowBox desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-borrowbox-from-the-gallery"></a>Adición de BorrowBox desde la galería
Para configurar la integración de BorrowBox en Azure AD, deberá agregar BorrowBox desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar BorrowBox desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./common/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./common/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./common/a_new_app.png)

4. En el cuadro de búsqueda, escriba **BorrowBox**, seleccione **BorrowBox** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/borrowbox-tutorial/tutorial_borrowbox_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con BorrowBox con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de BorrowBox para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de BorrowBox.

Para configurar y probar el inicio de sesión único de Azure AD con BorrowBox, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de BorrowBox](#create-a-borrowbox-test-user)**: para tener un homólogo de Britta Simon en BorrowBox que esté vinculado a su representación en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación BorrowBox.

**Para configurar el inicio de sesión único de Azure AD con BorrowBox, siga estos pasos:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **BorrowBox**, seleccione **Inicio de sesión único**.

    ![imagen](./common/B1_B2_Select_SSO.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML** para habilitar el inicio de sesión único.

    ![imagen](./common/b1_b2_saml_sso.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./common/b1-domains_and_urlsedit.png)

4. En la sección **Configuración básica de SAML**, el usuario no tiene que realizar ningún paso porque la aplicación ya se ha integrado previamente con Azure.

    ![imagen](./media/borrowbox-tutorial/tutorial_borrowbox_url.png)

    a. Haga clic en **Establecer direcciones URL adicionales**.

    b. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://fe.bolindadigital.com/wldcs_bol_fo/b2i/mainPage.html?b2bSite=<ID>`

    ![imagen](./media/borrowbox-tutorial/tutorial_borrowbox_url1.png)

    > [!NOTE]
    > El valor de la dirección URL de inicio de sesión no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de cliente de BorrowBox](mailto:borrowbox@bolinda.com) para obtener el valor.

5. La aplicación BorrowBox espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **User Attributes & Claims** (Atributos y notificaciones del usuario) de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario).

    ![imagen](./media/borrowbox-tutorial/i4-attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario), configure el atributo token SAML como se muestra en la imagen anterior y realice los siguientes pasos:
    
    a. Haga clic en el icono **Edit** (Editar) para abrir el cuadro de diálogo **Manage user claims** (Administrar notificaciones de usuario).

    ![imagen](./media/borrowbox-tutorial/i2-attribute.png)

    ![imagen](./media/borrowbox-tutorial/i3-attribute.png)

    b. En la lista **Source attribute** (Atributo de origen), seleccione **user.mail**.

    c. Haga clic en **Save**(Guardar). 

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el certificado adecuado según sus requisitos y guárdelo en el equipo.

    ![imagen](./media/borrowbox-tutorial/tutorial_borrowbox_certificate.png) 

8. Para configurar el inicio de sesión único en **BorrowBox**, es preciso enviar el certificado o los metadatos que ha descargado de Azure Portal al [equipo de soporte técnico de BorrowBox](mailto:borrowbox@bolinda.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![imagen](./common/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./common/d_adduser.png)

3. En las propiedades Usuario, siga estos pasos.

    ![imagen](./common/d_userproperties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-a-borrowbox-test-user"></a>Creación de un usuario de prueba de BorrowBox

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en BorrowBox. BorrowBox admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Al intentar acceder a BorrowBox, se creará un nuevo usuario, en caso de que no exista.
>[!Note]
>Si necesita crear un usuario manualmente, póngase en contacto con el  [equipo de soporte técnico de BorrowBox](mailto:borrowbox@bolinda.com).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a BorrowBox.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![imagen](./common/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **BorrowBox**.

    ![imagen](./media/borrowbox-tutorial/tutorial_borrowbox_app.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./common/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![imagen](./common/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de BorrowBox en el panel de acceso, debería iniciar sesión automáticamente en su aplicación BorrowBox.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
