---
title: 'Tutorial: Integración de Azure Active Directory con HubSpot | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y HubSpot.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 57343ccd-53ea-4e62-9e54-dee2a9562ed5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: adcd0f094d584e770f1a3f4938ee677ba58a21a8
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59995700"
---
# <a name="tutorial-azure-active-directory-integration-with-hubspot"></a>Tutorial: Integración de Azure Active Directory con HubSpot

En este tutorial, aprenderá a integrar HubSpot con Azure Active Directory (Azure AD).
La integración de HubSpot con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a HubSpot.
* Puede permitir que los usuarios inicien sesión automáticamente en HubSpot (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con HubSpot, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una suscripción habilitada para el inicio de sesión único en HubSpot.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* HubSpot admite el inicio de sesión único iniciado por **SP e IDP**.

## <a name="adding-hubspot-from-the-gallery"></a>Incorporación de HubSpot desde la galería

Para configurar la integración de HubSpot en Azure AD, será preciso que agregue HubSpot desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar HubSpot desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **HubSpot**, seleccione **HubSpot** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![HubSpot en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con HubSpot con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de HubSpot.

Para configurar y probar el inicio de sesión único de Azure AD con HubSpot, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de HubSpot](#configure-hubspot-single-sign-on)**: para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de HubSpot](#create-hubspot-test-user)**: para tener un homólogo de Britta Simon en HubSpot que esté vinculado a la representación de ella en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con HubSpot, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **HubSpot**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de HubSpot](common/idp-intiated.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://api.hubspot.com/login-api/v1/saml/login?portalId=<CUSTOMER ID>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://api.hubspot.com/login-api/v1/saml/acs?portalId=<CUSTOMER ID>`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL de respuesta y el identificador reales, que se explican más adelante en el tutorial. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de HubSpot](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://app.hubspot.com/login`

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

7. En la sección **Set up HubSpot** (Configurar HubSpot), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-hubspot-single-sign-on"></a>Configuración del inicio de sesión único de HubSpot

1. Abra una nueva pestaña en el explorador e inicie sesión en su cuenta de administrador de HubSpot.

2. Haga clic en el **icono de configuración** de la esquina superior derecha de la página.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/config1.png)

3. Haga clic en **Account Defaults** (Valores predeterminados de cuenta).

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/config2.png)

4. Desplácese hacia abajo hasta la sección **Security** (Seguridad) y haga clic en **Set up** (Configurar).

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/config3.png)

5. En la sección **Set Up Single Sign-On** (Configurar inicio de sesión único), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/config4.png)

     a. Haga clic en el botón **copy** (copiar) para copiar el valor de **Audience URl(Service Provider Entity ID)** (URI de audiencia [Identificador de entidad de proveedor de servicios]) y pegarlo en el cuadro de texto **Identificador** de la sección **Configuración básica de SAML** de Azure Portal.

    b. Haga clic en el botón **copy** (copiar) para copiar el valor de **Sign on URl,ACS,Recipient, or Redirect** (Redireccionamiento, destinatario, ACS o URI de inicio de sesión) y pegarlo en el cuadro de texto **URL de respuesta** de la sección **Configuración básica de SAML** de Azure Portal.

    c. En el cuadro de texto **Identity Provider Identifier or Issuer URL** (URL del emisor o identificador de proveedor de identidades), pegue el valor de **Azure AD Identifier** (Identificador de Azure AD) que ha copiado de Azure Portal.

    d. En el cuadro de texto **Dirección URL del inicio de sesión único del proveedor de identidades**, pegue el valor de **Dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    e. Abra el archivo **Certificate(Base64)** descargado en el Bloc de notas. Copie el contenido del mismo en el Portapapeles y, después, péguelo en el cuadro de texto  **Certificado X.509**.

    f. Haga clic en **Comprobar**.

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

En esta sección, concederá acceso a Britta Simon a HubSpot para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **HubSpot**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **HubSpot**.

    ![Vínculo a HubSpot en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-hubspot-test-user"></a>Creación de un usuario de prueba de HubSpot

Para permitir que los usuarios de Azure AD inicien sesión en HubSpot, deben aprovisionarse en HubSpot. En el caso de HubSpot, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en el sitio de la compañía de **HubSpot** como administrador.

2. Haga clic en el **icono de configuración** de la esquina superior derecha de la página.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/config1.png)

3. Haga clic en **Users & Teams** (Usuarios y equipos).

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/user1.png)

4. Haga clic en **Crear usuario**.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/user2.png)

5. Escriba la dirección de correo electrónico del usuario como `brittasimon\@contoso.com` en el cuadro de texto **Add email addess(es)** (Agregar direcciones de correo electrónico) y haga clic en **Siguiente**.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/user3.png)

6. En la sección **Crear usuarios**, desplácese por cada pestaña y seleccione las opciones y los permisos adecuados para el usuario y haga clic en **Siguiente**.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/user4.png)

7. Haga clic en **Enviar** para enviar la invitación al usuario.

    ![Configurar inicio de sesión único](./media/hubspot-tutorial/user5.png)

    > [!NOTE]
    > El usuario se activará después de aceptar la invitación.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de HubSpot en el panel de acceso, debería iniciar sesión automáticamente en la instancia de HubSpot para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

