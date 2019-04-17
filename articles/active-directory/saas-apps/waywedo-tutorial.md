---
title: 'Tutorial: integración de Azure Active Directory con Way We Do | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Way We Do.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 84fc4f36-ecd1-42c6-8a70-cb0f3dc15655
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 86f176ce43e4d7162c664115a6fd3ce9369fe79a
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59270071"
---
# <a name="tutorial-azure-active-directory-integration-with-way-we-do"></a>Tutorial: integración de Azure Active Directory con Way We Do

En este tutorial, aprenderá a integrar Way We Do con Azure Active Directory (Azure AD).
La integración de Way We Do con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Way We Do.
* Puede permitir que los usuarios inicien sesión automáticamente en Way We Do (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Way We Do, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una suscripción habilitada para el inicio de sesión único de Way We Do

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Way We Do admite el inicio de sesión único iniciado por **SP**.

* Way We Do admite el aprovisionamiento de usuarios **Just-In-Time**

## <a name="adding-way-we-do-from-the-gallery"></a>Incorporación de Way We Do desde la galería

Para configurar la integración de Way We Do en Azure AD, deberá agregar Way We Do desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Way We Do desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Way We Do**, seleccione **Way We Do** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Way We Do en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Way We Do con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Way We Do.

Para configurar y probar el inicio de sesión único de Azure AD con Way We Do, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Way We Do](#configure-way-we-do-single-sign-on)**: para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Way We Do](#create-way-we-do-test-user)**: para tener un homólogo de Britta Simon en Way We Do que esté vinculado a su representación en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Way We Do, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Way We Do**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Way We Do](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.waywedo.com/Authentication/ExternalSignIn`

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.waywedo.com`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Way We Do](mailto:support@waywedo.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (sin procesar)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificateraw.png)

6. En la sección **Set up Way We Do** (Configurar Way We Do), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-way-we-do-single-sign-on"></a>Configuración del inicio de sesión único de Way We Do

1. En otra ventana del explorador web, inicie sesión en Way We Do como Security Administrator (administrador de seguridad).

2. Haga clic en el **icono de persona** en la esquina superior derecha de cualquier página de Way We Do y, a continuación, haga clic en **Account** (Cuenta) en el menú desplegable.

    ![Cuenta de Way We Do](./media/waywedo-tutorial/tutorial_waywedo_account.png) 

3. Haga clic en el **icono de menú** para abrir el menú de navegación de inserción y haga clic en **Single Sign On** (Inicio de sesión único).

    ![Way We Do único](./media/waywedo-tutorial/tutorial_waywedo_single.png)

4. En la página **Single sign-on setup** (Configuración de inicio de sesión único), siga estos pasos:

    ![Way We Do guardar](./media/waywedo-tutorial/tutorial_waywedo_save.png)

     a. Haga clic en el botón de alternar de **Turn on single sign-on** (Activar inicio de sesión único) para cambiar a **Sí** y habilitar el inicio de sesión único.

    b. En el cuadro de texto **Single sign-on name**  (Nombre de inicio de sesión único), escriba su nombre.

    c. En el cuadro de texto **Identificador de entidad**, pegue el valor de **Identificador de Azure AD** que copió de Azure Portal.

    d. En el cuadro de texto **SAML SSO URL** (Dirección URL de inicio de sesión único de SAML), pegue el valor de **Dirección URL de inicio de sesión** que copió de Azure Portal.

    e. Para cargar el certificado, haga clic en el botón **select** (seleccionar) situado junto a **Certificate** (Certificado).

    f. **Configuración opcional** -
    
    * Enable Passwords (Habilitar las contraseñas): cuando esta opción está deshabilitada, la contraseña normal funciona para Way We Do de modo que los usuarios solo pueden usar el inicio de sesión único.

    * Enable Auto-provisioning (Habilitar aprovisionamiento automático): cuando esta opción está habilitada, la dirección de correo electrónico usada para el inicio de sesión se compara automáticamente con la lista de los usuarios de Way We Do. Si la dirección de correo electrónico no coincide con un usuario activo de Way We Do, agrega automáticamente una nueva cuenta de usuario para la persona que inicia sesión y se solicita la información que falta.

      > [!NOTE]
      > Los usuarios agregados mediante el inicio de sesión único se agregan como usuarios generales y no se les asigna ningún rol en el sistema. Un administrador puede modificar su rol de seguridad como editor o administrador, y también puede asignar uno o varios roles de organigrama. 

    g. Haga clic en **Guardar** para guardar la configuración.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba `brittasimon@yourcompanydomain.extension`. Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Way We Do.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **Way We Do**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Way We Do**.

    ![Vínculo a Way We Do en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-way-we-do-test-user"></a>Creación de un usuario de prueba de Way We Do

En esta sección, se crea un usuario llamado a Britta Simon en Way We Do. Way We Do admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario no existe en Way We Do, se crea después de la autenticación.

> [!Note]
> Si necesita crear manualmente un usuario, es preciso que se ponga contacto con el [equipo de soporte técnico de Way We Do](mailto:support@waywedo.com).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Way We Do en el panel de acceso y debería iniciar sesión automáticamente en la versión de Way We Do para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [ Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

