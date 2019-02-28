---
title: 'Tutorial: Integración de Azure Active Directory con Evernote | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Evernote.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d374fd78d2307c7cc7688a308e24c32c7f4b3ad1
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56875037"
---
# <a name="tutorial-azure-active-directory-integration-with-evernote"></a>Tutorial: Integración de Azure Active Directory con Evernote

En este tutorial, obtendrá información sobre cómo integrar Evernote con Azure Active Directory (Azure AD).
La integración de Evernote con Azure AD proporciona las siguientes ventajas:

* En Azure AD puede controlar quién tiene acceso a Evernote.
* Puede permitir que los usuarios inicien sesión automáticamente en Evernote (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Evernote, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en Evernote

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Evernote admite el inicio de sesión único iniciado tanto por **SP** como por **IDP**

## <a name="adding-evernote-from-the-gallery"></a>Incorporación de Evernote desde la galería

Para configurar la integración de Evernote en Azure AD, es preciso agregar Evernote desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Evernote desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Evernote**, seleccione **Evernote** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Evernote en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Evernote con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Evernote.

Para configurar y probar el inicio de sesión único de Azure AD con Evernote, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único en Evernote](#configure-evernote-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba en Evernote](#create-evernote-test-user)**: para tener un homólogo de Britta Simon en Evernote que esté vinculado a la representación de ella en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Evernote, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Evernote**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si quiere configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Evernote](common/idp-identifier.png)

    En el cuadro de texto **Identificador**, escriba una dirección URL: `https://www.evernote.com/saml2`

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![imagen](common/both-preintegrated-signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL: `https://www.evernote.com/Login.action`

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

7. En la sección **Set up Evernote** (Configurar Evernote), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-evernote-single-sign-on"></a>Configuración del inicio de sesión único en Evernote

1. En otra ventana del explorador web, inicie sesión en el sitio de la empresa de Evernote como administrador.

2. Vaya a **"Consola de administración"**

    ![Admin-Console](./media/evernote-tutorial/tutorial_evernote_adminconsole.png)

3. En la **"Consola de administración"**, vaya a **"Seguridad"** y seleccione **"Inicio de sesión único"**

    ![SSO-Setting](./media/evernote-tutorial/tutorial_evernote_sso.png)

4. Configure los valores siguientes:

    ![Certificate-Setting](./media/evernote-tutorial/tutorial_evernote_certx.png)
    
     a.  **Enable SSO** (Habilitar SSO): El SSO está habilitado de manera predeterminada (haga clic en **Disable Single Sign-on** [Deshabilitar el inicio de sesión único] para quitar el requisito de SSO).

    b. Pegue el valor del campo **Dirección URL de inicio de sesión** que copió de Azure Portal en el cuadro de texto **SAML HTTP Request URL** (Dirección URL de solicitud HTTP de SAML).

    c. Abra el certificado que se descargó de Azure AD en un bloc de notas y copie el contenido, incluido "BEGIN CERTIFICATE" y "END CERTIFICATE", y péguelo en el cuadro de texto **X.509 Certificate** (Certificado X.509). 

    d. Haga clic en **Guardar cambios**

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Evernote.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Evernote**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Evernote**.

    ![Vínculo de Evernote en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-evernote-test-user"></a>Creación de un usuario de prueba en Evernote

Para permitir que los usuarios de Azure AD inicien sesión en Evernote, deben aprovisionarse en Evernote.  
En el caso de Evernote, el aprovisionamiento es una tarea manual.

**Para aprovisionar cuentas de usuario, realice estos pasos:**

1. Inicie sesión en el sitio de la empresa de Evernote como administrador.

2. Haga clic en la **"Consola de administración"**.

    ![Admin-Console](./media/evernote-tutorial/tutorial_evernote_adminconsole.png)

3. En la **"Consola de administración"**, vaya a **"Agregar usuarios"**.

    ![Add-testUser](./media/evernote-tutorial/create_aaduser_0001.png)

4. **Agregue miembros del equipo** en el cuadro de texto **Correo electrónico**, escriba la dirección de correo electrónico de la cuenta de usuario y haga clic en **Invitar**.

    ![Add-testUser](./media/evernote-tutorial/create_aaduser_0002.png)
    
5. Una vez que se envía la invitación, el titular de la cuenta de Azure Active Directory recibirá un correo electrónico para aceptar la invitación.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Evernote en el Panel de acceso, debería iniciar sesión automáticamente en la instancia de Evernote para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

