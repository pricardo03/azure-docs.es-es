---
title: 'Tutorial: Integración de Azure Active Directory con ThousandEyes | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y ThousandEyes.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 587d6f8ecdc447672c14ce400ebc64609e4d472d
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59273913"
---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>Tutorial: Integración de Azure Active Directory con ThousandEyes

En este tutorial, obtendrá información sobre cómo integrar ThousandEyes con Azure Active Directory (Azure AD).
Integrar ThousandEyes con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a ThousandEyes.
* Puede permitir que los usuarios inicien sesión automáticamente en ThousandEyes (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con ThousandEyes, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para inicio de sesión único en ThousandEyes

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* ThousandEyes admite el inicio de sesión único iniciado por **SP**

* ThousandEyes admite el [**aprovisionamiento** automático de usuarios](https://docs.microsoft.com/azure/active-directory/saas-apps/thousandeyes-provisioning-tutorial)

## <a name="adding-thousandeyes-from-the-gallery"></a>Agregar ThousandEyes desde la galería

Para configurar la integración de ThousandEyes en Azure AD, será preciso que agregue ThousandEyes desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar ThousandEyes desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **ThousandEyes**, seleccione **ThousandEyes** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![ThousandEyes en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, se configura y se prueba el inicio de sesión único de Azure AD con ThousandEyes con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de ThousandEyes.

Para configurar y probar el inicio de sesión único de Azure AD con ThousandEyes, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de ThousandEyes](#configure-thousandeyes-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de ThousandEyes](#create-thousandeyes-test-user)**: para tener un homólogo de Britta Simon en Trakstar vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con ThousandEyes, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **ThousandEyes**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de ThousandEyes](common/sp-signonurl.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL: `https://app.thousandeyes.com/login/sso`

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up ThousandEyes** (Configurar ThousandEyes), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-thousandeyes-single-sign-on"></a>Configuración del inicio de sesión único de ThousandEyes

1. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de **ThousandEyes** como administrador.

2. En el menú de la parte superior, haga clic en **Configuración**.

    ![Configuración](./media/thousandeyes-tutorial/ic790066.png "Configuración")

3. Haga clic en **Cuenta**

    ![Cuenta](./media/thousandeyes-tutorial/ic790067.png "Cuenta")

4. Haga clic en la pestaña **Security & Authentication** (Seguridad y autenticación).

    ![Seguridad y autenticación](./media/thousandeyes-tutorial/ic790068.png "Seguridad y autenticación")

5. En la sección **Configurar inicio de sesión único** siga los pasos siguientes:

    ![Configurar inicio de sesión único](./media/thousandeyes-tutorial/ic790069.png "Configurar inicio de sesión único")

     a. Seleccione **Enable Single Sign-On**(Habilitar inicio de sesión único).

    b. En el cuadro de texto **Login Page URL** (Dirección URL de la página de inicio de sesión), pegue la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    c. En el cuadro de texto **Logout Page URL** (Dirección URL de la página de cierre de sesión), pegue la **dirección URL de cierre de sesión** que ha copiado de Azure Portal.

    d. En el cuadro de texto **Emisor de proveedor de identidades**, pegue el **Identificador de Azure AD** que ha copiado de Azure Portal.

    e. En **Certificado de verificación**, haga clic en **Elegir archivo** y cargue el certificado que ha descargado de Azure Portal.

    f. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba brittasimon@yourcompanydomain.extension. Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a ThousandEyes para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **ThousandEyes**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **ThousandEyes**.

    ![Vínculo a ThousandEyes en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-thousandeyes-test-user"></a>Creación de un usuario de prueba de ThousandEyes

El objetivo de esta sección es crear un usuario llamado Britta Simon en ThousandEyes. ThousandEyes admite el aprovisionamiento automático de usuarios, que está habilitado de forma predeterminada. [Aquí](thousandeyes-provisioning-tutorial.md) puede encontrar más información sobre cómo configurar el aprovisionamiento automático de usuarios.

**Para crear un usuario manualmente, siga los pasos siguientes:**

1. Inicie sesión en su sitio de la compañía de ThousandEyes como administrador.

2. Haga clic en **Configuración**.

    ![Configuración](./media/thousandeyes-tutorial/IC790066.png "Configuración")

3. Haga clic en **Cuenta**.

    ![Cuenta](./media/thousandeyes-tutorial/IC790067.png "Cuenta")

4. Haga clic en la pestaña **Accounts & Users** (Cuentas y usuarios).

    ![Cuentas y usuarios](./media/thousandeyes-tutorial/IC790073.png "Cuentas y usuarios")

5. En la sección **Add Users & Accounts** (Agregar usuarios y cuentas), realice los siguientes pasos:

    ![Agregar cuentas de usuario](./media/thousandeyes-tutorial/IC790074.png "Agregar cuentas de usuario")

     a. En el cuadro de texto **Nombre**, escriba el nombre de un usuario, por ejemplo, **Britta Simon**.

    b. En el cuadro de texto **Correo electrónico**, escriba el correo electrónico del usuario, en el ejemplo brittasimon@contoso.com.

    b. Haga clic en **Agregar nuevo usuario a la cuenta**.

    > [!NOTE]
    > El titular de la cuenta de Azure Active Directory recibirá un mensaje de correo electrónico con un vínculo para confirmar la cuenta y activarla.

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de ThousandEyes ofrecida por ThousandEyes para aprovisionar cuentas de usuario de Azure Active Directory.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de ThousandEyes en el panel de acceso y debería iniciar sesión automáticamente en la versión de ThousandEyes para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Configurar aprovisionamiento de usuarios](https://docs.microsoft.com/azure/active-directory/saas-apps/thousandeyes-provisioning-tutorial)
