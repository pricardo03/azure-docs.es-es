---
title: 'Tutorial: Integración de Azure Active Directory con Percolate | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Percolate.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 355f9659-b378-44c9-aa88-236e9b529a53
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: d399e83dbd1324827cc395cbf7834672baffa791
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59275851"
---
# <a name="tutorial-azure-active-directory-integration-with-percolate"></a>Tutorial: Integración de Azure Active Directory con Percolate

En este tutorial, aprenderá a integrar Percolate con Azure Active Directory (Azure AD).
La integración de Percolate con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Percolate.
* Puede permitir que los usuarios inicien sesión automáticamente en Percolate (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Percolate, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único en Percolate

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Percolate admite SSO iniciado tanto por **SP** como por **IDP**

## <a name="adding-percolate-from-the-gallery"></a>Adición de Percolate desde la galería

Para configurar la integración de Percolate en Azure AD, es preciso que agregue Percolate desde la galería a su lista de aplicaciones SaaS administradas.

**Para agregar Percolate desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Percolate**, seleccione **Percolate** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![Percolate en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Percolate con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Percolate.

Para configurar y probar el inicio de sesión único de Azure AD con Percolate, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Percolate](#configure-percolate-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Percolate](#create-percolate-test-user)**: para tener un homólogo de Britta Simon en Percolate que esté vinculado a su representación en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Percolate, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Percolate**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en el modo iniciado por  **IDP**, no es necesario realizar ningún paso porque la aplicación ya está integrada previamente en Azure.

    ![Información de dominio y direcciones URL de inicio de sesión único de Percolate](common/preintegrated.png)

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Percolate](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL: `https://percolate.com/app/login`

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el botón de copia para copiar **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

7. En la sección **Configurar Percolate**, copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-percolate-single-sign-on"></a>Configuración del inicio de sesión único en Percolate

1. En otra ventana del explorador web, inicie sesión en Percolate como administrador.

2. A la izquierda de la página principal, haga clic en **Configuración**.
    
    ![Configurar inicio de sesión único](./media/percolate-tutorial/configure01.png)

3. En el lado izquierdo de la barra de menús, haga clic en **SSO** en **Organización**.

    ![Configurar inicio de sesión único](./media/percolate-tutorial/configure02.png)

    a. En el cuadro de texto **Dirección URL de inicio de sesión**, pegue el valor de la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    b. En el cuadro de texto **Entity ID** (Identificador de entidad), pegue el valor de **Identificador de Azure AD** que copió de Azure Portal.

    c. En el Bloc de notas, abra el certificado codificado en Base 64 que descargó de Azure Portal, copie el contenido y, a continuación, péguelo en el cuadro de texto  **x509 certificates**  (Certificados x509).

    d. En el cuadro de texto **Atributo de correo electrónico**, escriba **emailaddress**.

    e. El campo **Identity provider metadata URL** (URL de metadatos del proveedor de identidades) es opcional y si tiene el valor de **Dirección URL de metadatos de federación de aplicación** que ha copiado de Azure Portal, péguelo en el cuadro de texto**Identity provider metadata URL** (URL de metadatos del proveedor de identidades).

    f. Seleccione **No** en **Should AuthNRequests be signed?** (¿Debe firmarse AuthNRequests?).

    g. Seleccione **No** en **Enable SSO auto-Provisioning** (Habilitar aprovisionamiento automático de SSO).

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
  
    b. En el campo **Nombre de usuario**, escriba brittasimon@yourcompanydomain.extension. Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Percolate.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **Percolate**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Percolate**.

    ![Vínculo a Percolate en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-percolate-test-user"></a>Creación de un usuario de prueba en Percolate

Para permitir que los usuarios de Azure AD inicien sesión en Percolate, deben aprovisionarse en esta solución. En Percolate, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión como administrador en Percolate.

2. En el lado izquierdo de la barra de menús, haga clic en **Users** (Usuarios) en **Organization** (Organización) y vaya a **New users** (Nuevos usuarios).

    ![Configurar inicio de sesión único](./media/percolate-tutorial/configure03.png)

3. En la página **Create users** (Crear usuarios), siga estos pasos:

    ![Configurar inicio de sesión único](./media/percolate-tutorial/configure04.png)

    a. En el cuadro de texto  **E-mail**  (Correo electrónico), escriba el correo electrónico del usuario; por ejemplo, brittasimon@contoso.com.

    b. En el cuadro de texto  **Fullname**  (Nombre completo), escriba el nombre de un usuario, por ejemplo,  **Britta Simon**.

    c. Haga clic en **Create users** (Crear usuarios).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Percolate en el Panel de acceso, debería iniciar sesión automáticamente en la versión de Percolate para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

