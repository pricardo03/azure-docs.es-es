---
title: 'Tutorial: Integración de Azure Active Directory con DocuSign | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y DocuSign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: 5c7d6116ed2925e57f094a67f27a11f9e2d61831
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2019
ms.locfileid: "68499256"
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Tutorial: Integración de Azure Active Directory con DocuSign

En este tutorial, obtendrá información sobre cómo integrar DocuSign con Azure Active Directory (Azure AD).
La integración de DocuSign con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a DocuSign.
* Puede permitir que los usuarios inicien sesión automáticamente en DocuSign (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con DocuSign, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una suscripción habilitada para el inicio de sesión único en DocuSign

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* DocuSign admite el inicio de sesión único iniciado por **SP**

* DocuSign admite el [aprovisionamiento automático de usuarios](https://docs.microsoft.com/azure/active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial).

## <a name="adding-docusign-from-the-gallery"></a>Adición de DocuSign desde la galería

Para configurar la integración de DocuSign en Azure AD, deberá agregar DocuSign desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar DocuSign desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **DocuSign**, seleccione **DocuSign** en el panel de resultados y, luego, haga clic en el botón **Agregar** para añadir la aplicación.

     ![DocuSign en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con DocuSign con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de DocuSign.

Para configurar y probar el inicio de sesión único de Azure AD con DocuSign, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de DocuSign](#configure-docusign-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de DocuSign](#create-docusign-test-user)** : para tener un homólogo de Britta Simon en DocuSign que esté vinculado a la representación de ella en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con DocuSign, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **DocuSign**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de DocuSign](common/sp-identifier.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/sp/<IDPID>`

    b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL de inicio de sesión y el identificador reales, que se explican más adelante en la sección **View SAML 2.0 Endpoints** (Ver puntos de conexión de SAML 2.0) del tutorial.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up DocuSign** (Configurar DocuSign), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-docusign-single-sign-on"></a>Configuración del inicio de sesión único en DocuSign

1. En otra ventana del explorador web, inicie sesión en el **Portal de administración de DocuSign** como administrador.

2. En la parte superior derecha de la página, haga clic en el **logotipo** del perfil y luego haga clic en **Go to Admin** (Ir a administración).
  
    ![Configuración del inicio de sesión único][51]

3. En la página de soluciones de dominio, haga clic en **Dominios**

    ![Configuración del inicio de sesión único][50]

4. En la sección **Dominios**, haga clic en **CLAIM DOMAIN** (RECLAMAR DOMINIO).

    ![Configuración del inicio de sesión único][52]

5. En el cuadro de diálogo **Claim a domain** (Reclamar un dominio), en el cuadro de texto **Domain Name** (Nombre de dominio), escriba el dominio de la compañía y haga clic en **CLAIM** (RECLAMAR). Asegúrese de que comprueba el dominio y que su estado es activo.

    ![Configuración del inicio de sesión único][53]

6. En la página de soluciones de dominio, haga clic en **Proveedores de identidad**.
  
    ![Configuración del inicio de sesión único][54]

7. En la sección **proveedores de identidades**, haga clic en **AGREGAR PROVEEDOR DE IDENTIDADES**. 

    ![Configuración del inicio de sesión único][55]

8. En la página **Identity Provider Settings** (Configuración del proveedor de identidades), siga estos pasos:

    ![Configuración del inicio de sesión único][56]

    a. En el cuadro de texto **Name** (Nombre), escriba un nombre único para la configuración. No utilice espacios.

    b. En el cuadro de texto **Emisor de proveedor de identidades**, pegue el valor de **Identificador Azure AD** que ha copiado de Azure Portal.

    c. En el cuadro de texto **Identity Provider Login URL** (Dirección URL de inicio de sesión del proveedor de identidades), pegue el valor de **Dirección URL de inicio de sesión**  que ha copiado de Azure Portal.

    d. En el cuadro de texto **Identity Provider Logout URL** (Dirección URL de cierre de sesión del proveedor de identidades), pegue el valor de **URL de cierre de sesión** que ha copiado de Azure Portal.

    e. Seleccione **Sign AuthN Request** (Firmar solicitud de autenticación).

    f. En **Send AuthN request by** (Enviar solicitud de autorización por), seleccione **POST**.

    g. En **Send logout request by** (Enviar solicitud de cierre de sesión por), seleccione **GET**.

    h. En la sección **Custom Attribute Mapping** (Asignación de atributos personalizados), haga clic en **AGREGAR NUEVA ASIGNACIÓN**.

    ![Configuración del inicio de sesión único][62]

    i. Elija el campo que desea asignar con la reclamación de Azure AD. En este ejemplo, la notificación **emailaddress** está asociada con el valor de **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** . Este es el nombre de notificación predeterminado de Azure AD para la notificación de correo electrónico y luego haga clic en **GUARDAR**.

    ![Configuración del inicio de sesión único][57]

    > [!NOTE]
    > Utilice el **identificador de usuario** adecuado para asignar el usuario de Azure AD a la asignación de usuarios de DocuSign. Seleccione el campo apropiado y escriba el valor adecuado según la configuración de la organización.

    j. En la sección **Identity Provider Certificate** (Certificado del proveedor de identidades), haga clic en **AGREGAR CERTIFICADO**, cargue el certificado que ha descargado del Portal del portal de Azure AD y haga clic en **GUARDAR**.

    ![Configuración del inicio de sesión único][58]

    k. En la sección **Proveedores de identidades**, haga clic en **ACCIONES** y luego en **Puntos de conexión**.

    ![Configuración del inicio de sesión único][59]

    l. En el **Portal de administración de DocuSign**, en la sección **View SAML 2.0 Endpoints** (Ver puntos de conexión de SAML 2.0), siga estos pasos:

    ![Configuración del inicio de sesión único][60]

    * Copie la información del cuadro de texto **Service Provider Issuer URL** (Dirección URL del emisor del proveedor de servicios) y luego péguela en el cuadro de texto **Identificador** de la sección **Configuración básica de SAML** de Azure Portal.

    * Copie la información del cuadro de texto **Service Provider Login URL** (Dirección URL de inicio de sesión del proveedor de servicios) y péguela en el cuadro de texto **URL de inicio de sesión**  de la sección **Configuración básica de SAML** de Azure Portal.

    * Haga clic en **Close**

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a DocuSign.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **DocuSign**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **DocuSign**.

    ![Vínculo a DocuSign en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-docusign-test-user"></a>Creación de un usuario de prueba en DocuSign

En esta sección, se crea un usuario llamado Britta Simon en DocuSign. DocuSign admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario no existe en DocuSign, se crea otro después de la autenticación.

>[!Note]
>Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de DocuSign](https://support.docusign.com/).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de DocuSign en el Panel de acceso debería iniciar sesión automáticamente en la versión de DocuSign para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[50]: ./media/docusign-tutorial/tutorial_docusign_18.png
[51]: ./media/docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/docusign-tutorial/tutorial_docusign_29.png
[62]: ./media/docusign-tutorial/tutorial_docusign_30.png
