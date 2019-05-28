---
title: 'Tutorial: Integración de Azure Active Directory con KnowledgeOwl | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y KnowledgeOwl.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2ae30996-864d-4872-90bc-f770e1ea159a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/10/2019
ms.author: jeedes
ms.openlocfilehash: b57beb18b8bf7d82694f89e131d79d15c6fcd6bc
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64728299"
---
# <a name="tutorial-azure-active-directory-integration-with-knowledgeowl"></a>Tutorial: Integración de Azure Active Directory con KnowledgeOwl

En este tutorial, obtendrá información sobre cómo integrar KnowledgeOwl con Azure Active Directory (Azure AD).
La integración de KnowledgeOwl con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a KnowledgeOwl.
* Puede permitir que los usuarios inicien sesión automáticamente en KnowledgeOwl (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con KnowledgeOwl, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Suscripción de KnowledgeOwl con el inicio de sesión único habilitado

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* KnowledgeOwl admite el inicio de sesión único iniciado por **SP e IDP**.
* KnowledgeOwl admite el aprovisionamiento de usuarios **Just-In-Time**.

## <a name="adding-knowledgeowl-from-the-gallery"></a>Agregar KnowledgeOwl desde la galería

Para configurar la integración de KnowledgeOwl en Azure AD, es preciso agregar KnowledgeOwl desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar KnowledgeOwl desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **KnowledgeOwl**, seleccione **KnowledgeOwl** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

    ![KnowledgeOwl en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con KnowledgeOwl con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de KnowledgeOwl.

Para configurar y probar el inicio de sesión único de Azure AD con KnowledgeOwl, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de KnowledgeOwl](#configure-knowledgeowl-single-sign-on)** : para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Crear un usuario de prueba de KnowledgeOwl](#create-knowledgeowl-test-user)** : para tener un homólogo de Britta Simon en KnowledgeOwl que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con KnowledgeOwl, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **KnowledgeOwl**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

    ![Información sobre el dominio y las direcciones URL de inicio de sesión único de KnowledgeOwl](common/idp-intiated.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente:

    ||
    |-|
    | `https://app.knowledgeowl.com/sp`|
    | `https://app.knowledgeowl.com/sp/id/<unique ID>`|

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón:

    ||
    |-|
    | `https://subdomain.knowledgeowl.com/help/saml-login`|
    | `https://subdomain.knowledgeowl.com/docs/saml-login`|
    | `https://subdomain.knowledgeowl.com/home/saml-login`|
    | `https://privatedomain.com/help/saml-login`|
    | `https://privatedomain.com/docs/saml-login`|
    | `https://privatedomain.com/home/saml-login`|

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información sobre el dominio y las direcciones URL de inicio de sesión único de KnowledgeOwl](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **Dirección URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón:

    ||
    |-|
    | `https://subdomain.knowledgeowl.com/help/saml-login`|
    | `https://subdomain.knowledgeowl.com/docs/saml-login`|
    | `https://subdomain.knowledgeowl.com/home/saml-login`|
    | `https://privatedomain.com/help/saml-login`|
    | `https://privatedomain.com/docs/saml-login`|
    | `https://privatedomain.com/home/saml-login`|

    > [!NOTE]
    > Estos valores no son reales. Debe actualizar este valor con la dirección URL de respuesta, el identificador y la dirección URL de inicio de sesión reales, tal como se explica más adelante en el tutorial.

6. La aplicación KnowledgeOwl espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados. Haga clic en el icono **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

7. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, edite las notificaciones mediante el **icono Editar** o agregue notificaciones mediante **Agregar nueva notificación** para configurar el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos: 

    | NOMBRE | Atributo de origen | Espacio de nombres |
    | ------------ | -------------------- | -----|
    | ssoid | user.mail | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. En la lista **Espacio de nombres**, escriba el valor del espacio de nombres que se muestra para esa fila.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

8. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (sin procesar)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificateraw.png)

9. En la sección **Set up KnowledgeOwl** (Configurar KnowledgeOwl), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-knowledgeowl-single-sign-on"></a>Configuración del inicio de sesión único de KnowledgeOwl

1. En otra ventana del explorador web, inicie sesión en el sitio de la empresa de KnowledgeOwl como administrador.

1. Haga clic en **Configuración** y, a continuación, seleccione **Seguridad**.

    ![Configuración de KnowledgeOwl](./media/knowledgeowl-tutorial/configure1.png)

1. Desplácese hasta la opción **SAML SSO Integration** (Integración de SSO de SAML) y siga los pasos a continuación:

    ![Configuración de KnowledgeOwl](./media/knowledgeowl-tutorial/configure2.png)

     a. Seleccione **Enable SAML SSO** (Habilitar SSO de SAML).

    b. Copie el valor **SP Entity ID** (Id. de identidad de SP) en **Identificador (Id. de entidad)** en la sección **Configuración básica de SAML** de Azure Portal.

    c. Copie el valor de **SP Login URL** (Dirección URL de inicio de sesión SP) y péguelo en el cuadro de texto **Sign-on URL and Reply URL** (Direcciones URL de inicio de sesión y respuesta) de la sección **Configuración básica de SAML** de Azure Portal.

    d. En el cuadro de texto **IdP entityID** (Id. de entidad de IdP), pegue el valor de **Identificador de Azure AD** que copió de Azure Portal.

    e. En el cuadro de texto **IdP Login URL** (Dirección URL de inicio de sesión de IdP), pegue el valor de la **URL de inicio de sesión** que ha copiado de Azure Portal.

    f. En el cuadro de texto **IdP Logout URL** (URL de cierre de sesión de IdP), pegue el valor de **URL de cierre de sesión** que ha copiado de Azure Portal.

    g. Cargue el certificado que descargó de Azure Portal haciendo clic en **Upload IdP Certificate** (Cargar certificado de IdP).

    h. Haga clic en **Map SAML Attributes** (Asignar atributos de SAML) para asignar atributos y realice los pasos siguientes:

    ![Configuración de KnowledgeOwl](./media/knowledgeowl-tutorial/configure3.png)

    * Escriba `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ssoid` en el cuadro de texto **Id. de SSO**.
    * Escriba `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` en el cuadro de texto **Nombre de usuario / correo electrónico**.
    * Escriba `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` en el cuadro de texto **Nombre**.
    * Escriba `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` en el cuadro de texto **Apellidos**.
    * Haga clic en **Guardar**

    i. Haga clic en **Guardar** en la parte inferior de la página.

    ![Configuración de KnowledgeOwl](./media/knowledgeowl-tutorial/configure4.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba `brittasimon@yourcompanydomain.extension`.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a KnowledgeOwl.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, a continuación, seleccione **KnowledgeOwl**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **KnowledgeOwl**.

    ![Vínculo a KnowledgeOwl en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-knowledgeowl-test-user"></a>Creación de un usuario de prueba de KnowledgeOwl

En esta sección, se crea un usuario llamado Britta Simon en KnowledgeOwl. KnowledgeOwl admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario deja de existir en KnowledgeOwl, se crea otro después de la autenticación.

> [!Note]
> Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de KnowledgeOwl](mailto:support@knowledgeowl.com).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de KnowledgeOwl en el panel de acceso, debería iniciar sesión automáticamente en la versión de KnowledgeOwl para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)