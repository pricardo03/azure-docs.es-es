---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con DocuSign | Microsoft Docs'
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
ms.date: 09/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe1f018612e889f49993895d88cdaf9ad732b393
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70306306"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-docusign"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con DocuSign

En este tutorial, obtendrá información sobre cómo integrar DocuSign con Azure Active Directory (Azure AD). Al integrar DocuSign con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a DocuSign.
* Permitir que los usuarios inicien sesión automáticamente en DocuSign con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en DocuSign.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* DocuSign admite el inicio de sesión único iniciado por **SP**

* DocuSign admite el aprovisionamiento de usuarios **Just-In-Time**.

* DocuSign admite el [aprovisionamiento automático de usuarios](https://docs.microsoft.com/azure/active-directory/saas-apps/docusign-provisioning-tutorial).

## <a name="adding-docusign-from-the-gallery"></a>Adición de DocuSign desde la galería

Para configurar la integración de DocuSign en Azure AD, deberá agregar DocuSign desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **DocuSign** en el cuadro de búsqueda.
1. Seleccione **DocuSign** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-single-sign-on-for-docusign"></a>Configuración y prueba del inicio de sesión único de Azure AD para DocuSign

Configure y pruebe el inicio de sesión único de Azure AD con DocuSign mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de DocuSign.

Para configurar y probar el inicio de sesión único de Azure AD con DocuSign, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en DocuSign](#configure-docusign-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de DocuSign](#create-docusign-test-user)** , para tener un homólogo de B.Simon en DocuSign que esté vinculado a la representación de ella en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **DocuSign**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, especifique los valores de los siguientes campos:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/sp/<IDPID>`

    b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL de inicio de sesión y el identificador reales, que se explican más adelante en la sección **View SAML 2.0 Endpoints** (Ver puntos de conexión de SAML 2.0) del tutorial.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar DocuSign**, copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a permitir que B.Simon acceda a DocuSign mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **DocuSign**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-docusign-sso"></a>Configuración del inicio de sesión único de DocuSign

1. Para automatizar la configuración en DocuSign, debe instalar la **extensión del explorador de inicio de sesión seguro de Aplicaciones**. Para ello, haga clic en **Instalar la extensión**.

    ![Extensión Mis aplicaciones](common/install-myappssecure-extension.png)

2. Después de agregar la extensión al explorador, haga clic en **Configurar DocuSign** para ir a la aplicación DocuSign. En ella, escriba las credenciales de administrador para iniciar sesión en DocuSign. La extensión de explorador configurará automáticamente la aplicación y automatizará los pasos 3 a 5.

    ![Configuración](common/setup-sso.png)

3. Si quiere configurar DocuSign manualmente, abra una nueva ventana del explorador web, inicie sesión en el sitio de empresa de DocuSign o administrador y haga lo siguiente:

4. En la parte superior derecha de la página, haga clic en el **logotipo** del perfil y luego haga clic en **Go to Admin** (Ir a administración).
  
    ![Configuración del inicio de sesión único][51]

5. En la página de soluciones de dominio, haga clic en **Dominios**

    ![Configuración del inicio de sesión único][50]

6. En la sección **Dominios**, haga clic en **CLAIM DOMAIN** (RECLAMAR DOMINIO).

    ![Configuración del inicio de sesión único][52]

7. En el cuadro de diálogo **Claim a domain** (Reclamar un dominio), en el cuadro de texto **Domain Name** (Nombre de dominio), escriba el dominio de la compañía y haga clic en **CLAIM** (RECLAMAR). Asegúrese de que comprueba el dominio y que su estado es activo.

    ![Configuración del inicio de sesión único][53]

8. En la página de soluciones de dominio, haga clic en **Proveedores de identidad**.
  
    ![Configuración del inicio de sesión único][54]

9. En la sección **proveedores de identidades**, haga clic en **AGREGAR PROVEEDOR DE IDENTIDADES**. 

    ![Configuración del inicio de sesión único][55]

10. En la página **Identity Provider Settings** (Configuración del proveedor de identidades), siga estos pasos:

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

### <a name="create-docusign-test-user"></a>Creación de un usuario de prueba en DocuSign

En esta sección, se crea un usuario llamado B.Simon en DocuSign. DocuSign admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario no existe en DocuSign, se crea otro después de la autenticación.

>[!Note]
>Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de DocuSign](https://support.docusign.com/).

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de DocuSign en el Panel de acceso debería iniciar sesión automáticamente en la versión de DocuSign para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Prueba de DocuSign con Azure AD](https://aad.portal.azure.com/)

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