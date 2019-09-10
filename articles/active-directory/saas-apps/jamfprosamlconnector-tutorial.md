---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Jamf Pro | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Jamf Pro.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/28/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 24f0d067351ab2e24b103624c8534ac8d6f2e757
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70305432"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-jamf-pro"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Jamf Pro

En este tutorial, aprenderá a integrar Jamf Pro con Azure Active Directory (Azure AD). Al integrar Jamf Pro con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Jamf Pro.
* Permitir que los usuarios puedan iniciar sesión automáticamente en Jamf Pro con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Suscripción habilitada para el inicio de sesión único (SSO) en Jamf Pro.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Jamf Pro admite SSO iniciado por **SP e IDP**

## <a name="adding-jamf-pro-from-the-gallery"></a>Incorporar Jamf Pro desde la galería

Para configurar la integración de Jamf Pro en Azure AD, deberá agregar Jamf Pro desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Jamf Pro** en el cuadro de búsqueda.
1. Seleccione **Jamf Pro** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-jamf-pro"></a>Configuración y prueba del inicio de sesión único de Azure AD para Jamf Pro

Configure y pruebe el inicio de sesión único de Azure AD con Jamf Pro mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Jamf Pro.

Para configurar y probar el inicio de sesión único de Azure AD con Jamf Pro, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Jamf Pro](#configure-jamf-pro-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Jamf Pro](#create-jamf-pro-test-user)** , para tener un homólogo de B.Simon en Jamf Pro que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Jamf Pro**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.jamfcloud.com/saml/SSO`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.jamfcloud.com`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Obtendrá el valor del identificador real de la sección **Inicio de sesión único** en el portal de Jamf Pro, que se explica más adelante en el tutorial. Puede extraer el valor real del **subdominio** del valor del identificador y usar esa información del **subdominio** en las direcciones URL de inicio de sesión y de respuesta. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el botón de copia para copiar la **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

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

En esta sección, va a permitir que B.Simon acceda a Jamf Pro mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Jamf Pro**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-jamf-pro-sso"></a>Configuración del inicio de sesión único de Jamf Pro

1. Para automatizar la configuración de Jamf Pro, debe instalar la **extensión de explorador de inicio de sesión seguro de Mis aplicaciones**. Para ello, haga clic en **Install the extension** (Instalar la extensión).

    ![Extensión Mis aplicaciones](common/install-myappssecure-extension.png)

2. Después de agregar la extensión al explorador, haga clic en **Configurar Jamf Pro** para ir a la aplicación Jamf Pro. Desde allí, proporcione las credenciales de administrador para iniciar sesión en Jamf Pro. La extensión de explorador configurará automáticamente la aplicación y automatizará los pasos 3 a 7.

    ![Configuración](common/setup-sso.png)

3. Si quiere configurar Jamf Pro manualmente, abra una nueva ventana del explorador web, inicie sesión en el sitio de empresa de Jamf Pro como administrador y realice los pasos siguientes:

4. Haga clic en el **icono de configuración** de la esquina superior derecha de la página.

    ![Configuración de Jamf Pro](./media/jamfprosamlconnector-tutorial/configure1.png)

5. Haga clic en **Inicio de sesión único de SAML**.

    ![Configuración de Jamf Pro](./media/jamfprosamlconnector-tutorial/configure2.png)

6. En la página **Single Sign-On** (Inicio de sesión único) realice los pasos siguientes:

    ![Configuración de Jamf Pro](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Active **Enable Single Sign-On Authentication** (Habilitar autenticación de inicio de sesión único).

    b. Seleccione la opción **Otro** en el menú desplegable **PROVEEDOR DE IDENTIDADES**.

    c. En el cuadro de texto **OTRO PROVEEDOR**, escriba **Azure AD**.

    d. Copie el valor de **ID. de entidad** y péguelo en el cuadro de texto **Identificador (id. de entidad)** de la sección **Configuración básica de SAML**  de Azure Portal.

    > [!NOTE]
    > Aquí `<SUBDOMAIN>`, necesita usar este valor para completar las direcciones URL de inicio de sesión y respuesta en la sección **Configuración básica de SAML** en Azure Portal.

    e. Seleccione la opción **URL de metadatos** en el menú desplegable **ORIGEN DE METADATOS DEL PROVEEDOR DE IDENTIDADES** y, en el siguiente cuadro de texto, pegue el valor de **Dirección URL de metadatos de federación de aplicación** que ha copiado en Azure Portal.

7. En la misma página, desplácese hacia abajo hasta la sección **Asignación de usuario** y realice los pasos siguientes: 

    ![Inicio de sesión único de Jamf Pro](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a. Seleccione la opción **NameID** para **IDENTITY PROVIDER USER MAPPING** (Asignación de usuario del proveedor de identidades). De forma predeterminada, este valor se establece en **NameID**, pero se puede definir un atributo personalizado.

    b. Seleccione **Email** (Correo electrónico) para **JAMF PRO USER MAPPING** (Asignación de usuario de Jamf Pro). Jamf Pro asigna los atributos SAML enviados por el proveedor de identidades de las siguientes maneras: por usuarios y por grupos. Cuando un usuario intenta acceder a Jamf Pro, de forma predeterminada, Jamf Pro obtiene información sobre el usuario del proveedor de identidades y la compara con las cuentas de usuario de Jamf Pro. Si la cuenta de usuario entrante no existe en Jamf Pro, se produce una asociación de nombres por grupo.

    c. Pegue el valor `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` en el cuadro de texto **IDENTITY PROVIDER GROUP ATTRIBUTE NAME** (Nombre de atributo del proveedor de identidades).

    d. Si selecciona **Allow users to bypass the Single Sign-On authentication** (Permitir a los usuarios omitir la autenticación de inicio de sesión único), no se redireccionará a los usuarios a la página de inicio de sesión del proveedor de identidades para fines de autenticación, pero podrán iniciar sesión directamente en Jamf Pro. Cuando un usuario intenta tener acceso a Jamf Pro a través del proveedor de identidades, se produce la autorización y autenticación mediante SSO iniciadas por el proveedor de identidades.

    e. Haga clic en **Save**(Guardar).

### <a name="create-jamf-pro-test-user"></a>Creación de un usuario de prueba de Jamf Pro

Para permitir que los usuarios de Azure AD inicien sesión en Jamf Pro, tienen que aprovisionarse en Jamf Pro. En el caso de Jamf Pro, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en su sitio de la empresa de Jamf Pro como administrador.

2. Haga clic en el **icono de configuración** de la esquina superior derecha de la página.

    ![Agregar empleado](./media/jamfprosamlconnector-tutorial/configure1.png)

3. Haga clic en **Grupos y cuentas de usuario de Jamf Pro**.

    ![Agregar empleado](./media/jamfprosamlconnector-tutorial/user1.png)

4. Haga clic en **Nuevo**.

    ![Agregar empleado](./media/jamfprosamlconnector-tutorial/user2.png)

5. Seleccione **Crear cuenta estándar**.

    ![Agregar empleado](./media/jamfprosamlconnector-tutorial/user3.png)

6. En el cuadro de diálogo **Nueva cuenta**, realice los pasos siguientes:

    ![Agregar empleado](./media/jamfprosamlconnector-tutorial/user4.png)

    a. En el cuadro de texto **NOMBRE DE USUARIO**, escriba el nombre completo de Britta Simon.

    b. Seleccione las opciones adecuadas según su organización para **NIVEL DE ACCESO**, **CONJUNTO DE PRIVILEGIOS** y **ESTADO DE ACCESO**.

    c. En el cuadro de texto **NOMBRE COMPLETO**, escriba el nombre completo de Britta Simon.

    d. En el cuadro de texto **DIRECCIÓN DE CORREO ELECTRÓNICO**, escriba la dirección de correo electrónico de la cuenta de Britta Simon.

    e. En el cuadro de texto **CONTRASEÑA**, escriba la contraseña del usuario.

    f. En el cuadro de texto **CONFIRMAR CONTRASEÑA**, escriba la contraseña del usuario.

    g. Haga clic en **Save**(Guardar).

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Jamf Pro en el panel de acceso y debería iniciar sesión automáticamente en la versión de Jamf Pro para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Prueba de Jamf Pro con Azure AD](https://aad.portal.azure.com/)

