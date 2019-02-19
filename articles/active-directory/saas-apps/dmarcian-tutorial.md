---
title: 'Tutorial: Integración de Azure Active Directory con dmarcian | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y dmarcian.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a04b9383-3a60-4d54-9412-123daaddff3b
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/30/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4768c38b2e76bfa8cdff3187c32c03fdaaf57bbf
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56207660"
---
# <a name="tutorial-azure-active-directory-integration-with-dmarcian"></a>Tutorial: Integración de Azure Active Directory con dmarcian

En este tutorial, obtendrá información sobre cómo integrar dmarcian con Azure Active Directory (Azure AD).
La integración de dmarcian con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a dmarcian.
* Puede permitir que los usuarios inicien sesión automáticamente en dmarcian (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con dmarcian, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en dmarcian

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* dmarcian admite el inicio de sesión único iniciado por **SP** e **IDP**

## <a name="adding-dmarcian-from-the-gallery"></a>Adición de dmarcian desde la galería

Para configurar la integración de dmarcian en Azure AD, es preciso agregar dmarcian desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar dmarcian desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **dmarcian**, seleccione **dmarcian** en el panel de resultados y, luego, haga clic en el botón **Agregar** para añadir la aplicación.

     ![dmarcian en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con dmarcian con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de dmarcian.

Para configurar y probar el inicio de sesión único de Azure AD con dmarcian, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único en dmarcian](#configure-dmarcian-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba en dmarcian](#create-dmarcian-test-user)**: para tener un homólogo de Britta Simon en dmarcian que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con dmarcian, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **dmarcian**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en el modo iniciado por **IDP** siga estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de dmarcian](common/idp-intiated.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente:
    | |
    | -- |
    | `https://us.dmarcian.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian-eu.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian-ap.com/sso/saml/<ACCOUNT_ID>/sp.xml` |

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón:
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian-eu.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian-ap.com/login/<ACCOUNT_ID>/handle/` |

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de dmarcian](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **Dirección URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón:
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>` |
    | `https://dmarcian-eu.com/login/<ACCOUNT_ID>` |
    | `https://dmarciam-ap.com/login/<ACCOUNT_ID>` |
     
    > [!NOTE] 
    > Estos valores no son reales. Los valores se actualizan con el identificador, la dirección URL de inicio de sesión y la dirección URL de respuesta reales, lo que se explica más adelante en el tutorial. 

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el botón de copia para copiar **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

### <a name="configure-dmarcian-single-sign-on"></a>Configuración del inicio de sesión único en dmarcian

1. En otra ventana del explorador web, inicie sesión en dmarcian como administrador de seguridad.

2. Haga clic en **Profile** (Perfil) en la esquina superior derecha y vaya a **Preferences** (Preferencias).

    ![Preferencias ](./media/dmarcian-tutorial/tutorial_dmarcian_pref.png)

3. Desplácese hacia abajo y haga clic en la sección **Single Sign-On** (Inicio de sesión único) y, después, haga clic en **Configure** (Configurar).

    ![El inicio de sesión único ](./media/dmarcian-tutorial/tutorial_dmarcian_sso.png)

4. En la página **SAML Single Sign-On** (Inicio de sesión único de SAML), establezca **Status** (Estado) en **Enabled** (Habilitado) y realice los pasos siguientes:

    ![Autenticación ](./media/dmarcian-tutorial/tutorial_dmarcian_auth.png)

    * En la sección **Add dmarcian to your Identity Provider** (Agregar dmarcian al proveedor de identidades), haga clic en **COPY** para copiar el valor de **Assertion Consumer Service URL** (Dirección URL del servicio de consumidor de aserciones) de la instancia y péguelo en el cuadro de texto **Dirección URL de respuesta** de la sección **Configuración básica de SAML** en Azure Portal.

    * En la sección **Add dmarcian to your Identity Provider** (Agregar dmarcian al proveedor de identidades), haga clic en **COPY** para copiar el valor de **Entity ID** (Identificador de entidad) de la instancia y péguelo en el cuadro de texto **Identificador** de la sección **Configuración básica de SAML** en Azure Portal.

    * En la sección **Set up Authentication** (Configurar la autenticación), en el cuadro de texto **Identity Provider Metadata** (Metadatos del proveedor de identidades), pegue el valor de **Dirección URL de metadatos de federación de aplicación** que copió de Azure Portal.

    * En la sección **Set up Authentication** (Configurar autenticación), en el cuadro de texto **Attribute Statements** (Instrucciones de atributo), pegue la dirección URL `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    * En la sección **Set up Login URL** (Configurar dirección URL de inicio de sesión), copie el valor de **Login URL** (Dirección URL de inicio de sesión) de la instancia y péguelo en el cuadro de texto **Dirección URL de inicio de sesión** de la sección **Configuración básica de SAML** en Azure Portal.

        > [!Note]
        > Puede modificar la **dirección URL de inicio de sesión** según su organización.

    * Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a dmarcian.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **dmarcian**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **dmarcian**.

    ![Vínculo a dmarcian en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-dmarcian-test-user"></a>Creación de un usuario de prueba en dmarcian

Para permitir que los usuarios de Azure AD inicien sesión en dmarcian, deben aprovisionarse en dmarcian. En dmarcian, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en dmarcian como administrador de seguridad.

2. Haga clic en **Profile** (Perfil) en la esquina superior derecha y vaya a **Manage Users** (Administrar usuarios).

    ![Usuario ](./media/dmarcian-tutorial/tutorial_dmarcian_user.png)

3. En el lado derecho de la sección **SSO Users** (Usuarios de SSO), haga clic en **Add New User** (Agregar nuevo usuario).

    ![Agregar usuario ](./media/dmarcian-tutorial/tutorial_dmarcian_addnewuser.png)

4. En la ventana emergente **Add New user** (Agregar nuevo usuario), realice los siguientes pasos:

    ![Nuevo usuario ](./media/dmarcian-tutorial/tutorial_dmarcian_save.png)

     a. En el cuadro de texto **New User Email** (Correo electrónico del nuevo usuario), escriba el correo electrónico del usuario con el siguiente formato **brittasimon@contoso.com**.

    b. Si desea conceder derechos de administrador al usuario, seleccione **Make User an Admin** (Hacer que el usuario sea un administrador).

    c. Haga clic en **Agregar usuario**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de dmarcian en el Panel de acceso, debería iniciar sesión automáticamente en la instancia de dmarcian para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

