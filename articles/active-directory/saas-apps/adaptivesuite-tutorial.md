---
title: 'Tutorial: Integración de Azure Active Directory con Adaptive Insights | Microsoft Docs'
description: Obtenga más información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Adaptive Insights.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2019
ms.author: jeedes
ms.openlocfilehash: c217663c5752907e0b3d6372d4522f6aba982b3d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107404"
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-insights"></a>Tutorial: integración de Azure Active Directory con Adaptive Insights

En este tutorial obtendrá más información sobre cómo integrar Adaptive Insights con Azure Active Directory (Azure AD).
La integración de Adaptive Insights con Azure AD proporciona las siguientes ventajas:

* Puede controlar quién tiene acceso a Adaptive Insights en Azure AD.
* Puede permitir que los usuarios inicien sesión automáticamente en Adaptive Insights (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Adaptive Insights, se necesita lo siguiente:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción de Adaptive Insights con el inicio de sesión único habilitado

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Adaptive Insights admite el inicio de sesión único iniciado por **IDP**.

## <a name="adding-adaptive-insights-from-the-gallery"></a>Agregar Adaptive Insights desde la galería

Para configurar la integración de Adaptive Insights con Azure AD, deberá agregar Adaptive Insights desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Adaptive Insights desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. Para agregar la aplicación, escriba **Adaptive Insights** en el cuadro de búsqueda, seleccione **Adaptive Insights** en el panel de resultados y, luego, haga clic en el botón **Agregar**.

     ![Adaptive Insights en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Adaptive Insights con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Adaptive Insights.

Para configurar y probar el inicio de sesión único de Azure AD con Adaptive Insights, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Adaptive Insights](#configure-adaptive-insights-single-sign-on)** : para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Adaptive Insights](#create-adaptive-insights-test-user)** : para tener un homólogo de Britta Simon en Adaptive Insights vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Adaptive Insights, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Adaptive Insights**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la página **Configurar inicio de sesión único con SAML** realice los siguientes pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Adaptive Insights](common/idp-intiated.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    > [!NOTE]
    > Puede obtener el identificador (id. de entidad) y los valores de URL en la página **Configuración de SSO de SAML** de Adaptive Insights.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Configurar Adaptive Insights**, copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-adaptive-insights-single-sign-on"></a>Configuración del inicio de sesión único en Adaptive Insights

1. En otra ventana del explorador web, inicie sesión en el sitio de la empresa de Adaptive Insights como administrador.

2. Vaya a **Administración**.

    ![Administración](./media/adaptivesuite-tutorial/ic805644.png "Administración")

3. En la sección **Users and Roles** (Usuarios y roles), haga clic en **Manage SAML SSO Settings** (Administrar configuración de SSO de SAML).

    ![Manage SAML SSO Settings (Administrar configuración de SSO de SAML)](./media/adaptivesuite-tutorial/ic805645.png "Manage SAML SSO Settings (Administrar configuración de SSO de SAML)")

4. En la página **SAML SSO Settings** (Configuración de SSO de SAML), realice los pasos siguientes:

    ![SAML SSO Settings (Configuración de SSO de) SAML](./media/adaptivesuite-tutorial/ic805646.png "SAML SSO Settings (Configuración de SSO de) SAML")

    a. En el cuadro de texto **Identity provider name** (Nombre del proveedor de identidades), escriba el nombre de la configuración.

    b. Pegue el valor del **Azure Ad Identifier** (Identificador de Azure AD) que ha copiado de Azure Portal en el cuadro de texto **Identity Provider Entity ID** (Id. de entidad del proveedor de identidades).

    c. Pegue el valor de **Dirección URL de inicio de sesión** que copió de Azure Portal en el cuadro de texto **Identity provider SSO URL** (URL de SSO de proveedor de identidades).

    d. Pegue el valor de **URL de cierre de sesión** que copió de Azure Portal en el cuadro de texto **Custom logout URL** (Dirección URL de cierre de sesión personalizada).

    e. Para cargar el certificado descargado, haga clic en **Choose file**(Elegir archivo).

    f. Seleccione las opciones siguientes en cada caso:

     * En **SAML user id** (identificador de usuario de SAML), seleccione **User’s Adaptive Insights user name** (Nombre de usuario de Adaptive Insights del usuario).

     * En **SAML user id location** (Ubicación del id. de usuario de SAML), seleccione **User id in NameID of Subject** (identificador de usuario en NameID del tema).

     * En **SAML NameID format** (Formato de NameID de SAML), seleccione **Email address** (Dirección de correo electrónico).

     * En **Enable SAML** (Habilitar SAML), seleccione **Allow SAML SSO and direct Adaptive Insights login** (Permitir inicio de sesión único de SAML e inicio de sesión directo de Adaptive Insights).

    g. Copie la **URL de SSO de Adaptive Insights** y péguela en los cuadros de texto **identificador (id. de entidad)** y **URL de respuesta** en la sección **Dominio y direcciones URL de Adaptive Insights** de Azure Portal.

    h. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba brittasimon@yourcompanydomain.extension. Por ejemplo, BrittaSimon@contoso.com.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Adaptive Insights a Britta Simon para permitirle usar el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Adaptive Insights**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Adaptive Insights**.

    ![Vínculo a Adaptive Insights en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-adaptive-insights-test-user"></a>Creación de un usuario de prueba de Adaptive Insights

Para permitir que los usuarios de Azure AD inicien sesión en Adaptive Insights, deben aprovisionarse en Adaptive Insights. En el caso de Adaptive Insights, el aprovisionamiento es una tarea manual.

**Siga estos pasos para configurar el aprovisionamiento de usuario:**

1. Inicie sesión en el sitio de la compañía de **Adaptive Insights** como administrador.

2. Vaya a **Administración**.

   ![Administración](./media/adaptivesuite-tutorial/IC805644.png "Administración")

3. En la sección **Users and Roles** (Usuarios y roles), haga clic en **Add User** (Agregar usuario).

   ![Agregar usuario](./media/adaptivesuite-tutorial/IC805648.png "Agregar usuario")

4. En la sección **Nuevo usuario** , lleve a cabo estos pasos:

   ![Enviar](./media/adaptivesuite-tutorial/IC805649.png "Enviar")

   a. Escriba el **nombre**, el **inicio de sesión**, la **dirección de correo electrónico** y la **contraseña** o de un usuario válido de Azure Active Directory que quiera aprovisionar en los cuadros de texto relacionados.

   b. Seleccione un **Role**(rol).

   c. Haga clic en **Enviar**.

> [!NOTE]
> Para aprovisionar cuentas de usuario de AAD, puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Adaptive Insights ofrecida por Adaptive Insights.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Adaptive Insights en el panel de acceso, debería iniciar sesión automáticamente en la versión de Adaptive Insights para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)