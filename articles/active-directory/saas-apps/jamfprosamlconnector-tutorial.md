---
title: 'Tutorial: Integración de Azure Active Directory con Jamf Pro | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Jamf Pro.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 18b575b74c80499f2ddd6648bf051b5245077d2f
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58906147"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Tutorial: Integración de Azure Active Directory con Jamf Pro

En este tutorial, aprenderá a integrar Jamf Pro con Azure Active Directory (Azure AD).
La integración de Jamf Pro con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Jamf Pro.
* Puede permitir que los usuarios inicien sesión automáticamente en Jamf Pro (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Jamf Pro, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción habilitada para el inicio de sesión único de Jamf Pro

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Jamf Pro admite SSO iniciado por **SP e IDP**

## <a name="adding-jamf-pro-from-the-gallery"></a>Incorporar Jamf Pro desde la galería

Para configurar la integración de Jamf Pro en Azure AD, deberá agregar Jamf Pro desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Jamf Pro desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Jamf Pro**, seleccione **Jamf Pro** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Jamf Pro en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Jamf Pro con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Jamf Pro.

Para configurar y probar el inicio de sesión único de Azure AD con Jamf Pro, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Jamf Pro](#configure-jamf-pro-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Jamf Pro](#create-jamf-pro-test-user)**: para tener un homólogo de Britta Simon en Jamf Pro que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Jamf Pro, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Jamf Pro**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar en el modo iniciado por **IDP**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Jamf Pro](common/idp-intiated.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.jamfcloud.com/saml/SSO`

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.jamfcloud.com`

    ![Información de dominio y direcciones URL de inicio de sesión único de Jamf Pro](common/metadata-upload-additional-signon.png)

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Obtendrá el valor del identificador real de la sección **Inicio de sesión único** en el portal de Jamf Pro, que se explica más adelante en el tutorial. Puede extraer el valor real del **subdominio** del valor del identificador y usar esa información del **subdominio** en las direcciones URL de inicio de sesión y de respuesta. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el botón de copia para copiar **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

### <a name="configure-jamf-pro-single-sign-on"></a>Configuración del inicio de sesión único de Jamf Pro

1. Para automatizar la configuración de Jamf Pro, debe instalar la **extensión de explorador de inicio de sesión seguro de Mis aplicaciones**. Para ello, haga clic en **Install the extension** (Instalar la extensión).

    ![imagen](./media/jamfprosamlconnector-tutorial/install_extension.png)

2. Después de agregar la extensión al explorador, haga clic en **Configurar Jamf Pro** para ir a la aplicación Jamf Pro. Desde allí, proporcione las credenciales de administrador para iniciar sesión en Jamf Pro. La extensión de explorador configurará automáticamente la aplicación y automatizará los pasos 3 a 7.

    ![imagen](./media/jamfprosamlconnector-tutorial/d1_saml.png)

3. Si quiere configurar Jamf Pro manualmente, abra una nueva ventana del explorador web, inicie sesión en el sitio de empresa de Jamf Pro como administrador y realice los pasos siguientes:

4. Haga clic en el **icono de configuración** de la esquina superior derecha de la página.

    ![Configuración de Jamf Pro](./media/jamfprosamlconnector-tutorial/configure1.png)

5. Haga clic en **Inicio de sesión único de SAML**.

    ![Configuración de Jamf Pro](./media/jamfprosamlconnector-tutorial/configure2.png)

6. En la página **Single Sign-On** (Inicio de sesión único) realice los pasos siguientes:

    ![Inicio de sesión único de Jamf Pro](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

     a. Seleccione **Jamf Pro Server** para habilitar el acceso mediante el inicio de sesión único.

    b. Si selecciona **Allow bypass for all users** (Permitir omisión para todos los usuarios), no se remitirá a los usuarios a la página de inicio de sesión del proveedor de identidades para fines de autenticación, pero podrán iniciar sesión directamente en Jamf Pro. Cuando un usuario intenta tener acceso a Jamf Pro a través del proveedor de identidades, se produce la autorización y autenticación mediante SSO iniciadas por el proveedor de identidades.

    c. Seleccione la opción **NameID** para **USER MAPPING: SAML** (ASIGNACIÓN DE USUARIOS: SAML). De forma predeterminada, este valor se establece en **NameID**, pero se puede definir un atributo personalizado.

    d. Seleccione **Email** (Correo electrónico) para **USER MAPPING: JAMF PRO** (ASIGNACIÓN DE USUARIOS: JAMF PRO). Jamf Pro asigna los atributos SAML enviados por el proveedor de identidades de las siguientes maneras: por usuarios y por grupos. Cuando un usuario intenta acceder a Jamf Pro, de forma predeterminada, Jamf Pro obtiene información sobre el usuario del proveedor de identidades y la compara con las cuentas de usuario de Jamf Pro. Si la cuenta de usuario entrante no existe en Jamf Pro, se produce una asociación de nombres por grupo.

    e. Pegue el valor `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` en el cuadro de texto **GROUP ATTRIBUTE NAME** (NOMBRE DE ATRIBUTO DE GRUPO).

7. En la misma página, desplácese a **PROVEEDOR DE IDENTIDADES**, en la sección **Inicio de sesión único**, y siga estos pasos:

    ![Configuración de Jamf Pro](./media/jamfprosamlconnector-tutorial/configure3.png)

     a. Seleccione la opción **Otro** en el menú desplegable **PROVEEDOR DE IDENTIDADES**.

    b. En el cuadro de texto **OTRO PROVEEDOR**, escriba **Azure AD**.

    c. Seleccione la opción **URL de metadatos** en el menú desplegable **ORIGEN DE METADATOS DEL PROVEEDOR DE IDENTIDADES** y, en el siguiente cuadro de texto, pegue el valor de **Dirección URL de metadatos de federación de aplicación** que ha copiado en Azure Portal.

    d. Copie el valor del **id. de entidad** y péguelo en el cuadro de texto **Identificador (id. de entidad)**, que se encuentra en la sección **Dominio y direcciones URL de Jamf Pro** de Azure Portal.

    > [!NOTE]
    > Aquí el valor borroso es la parte del subdominio. Use este valor para completar las direcciones URL de inicio de sesión y respuesta en la sección **Dominio y direcciones URL de Jamf Pro** en Azure Portal.

    e. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon\@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Jamf Pro.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**,  **Jamf Pro**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Jamf Pro**.

    ![Vínculo a Jamf Pro en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-jamf-pro-test-user"></a>Creación de un usuario de prueba de Jamf Pro

Para permitir que los usuarios de Azure AD inicien sesión en Jamf Pro, tienen que aprovisionarse en Jamf Pro. En el caso de Jamf Pro, el aprovisionamiento es una tarea manual.

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

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Jamf Pro en el panel de acceso y debería iniciar sesión automáticamente en la versión de Jamf Pro para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
