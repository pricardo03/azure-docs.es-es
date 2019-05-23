---
title: 'Tutorial: Integración de Azure Active Directory con Syncplicity | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Syncplicity.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 041f1e9706c7d815dad1a33104e7dd15b2cc3893
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65902028"
---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a>Tutorial: Integración de Azure Active Directory con Syncplicity

En este tutorial, aprenderá a integrar Syncplicity con Azure Active Directory (Azure AD).
La integración de Syncplicity con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Syncplicity.
* Puede permitir que los usuarios inicien sesión automáticamente en Syncplicity (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Syncplicity, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en Syncplicity

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Syncplicity admite inicio de sesión único iniciado por **SP**

## <a name="adding-syncplicity-from-the-gallery"></a>Incorporación de Syncplicity desde la galería

Para configurar la integración de Syncplicity en Azure AD, deberá agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Syncplicity desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Syncplicity**, seleccione **Syncplicity** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Syncplicity en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Syncplicity con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Syncplicity.

Para configurar y probar el inicio de sesión único de Azure AD con Syncplicity, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Syncplicity](#configure-syncplicity-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Syncplicity](#create-syncplicity-test-user)**: para tener un homólogo de Britta Simon en Syncplicity vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Syncplicity, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Syncplicity**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Syncplicity](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.syncplicity.com`

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.syncplicity.com/sp`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte de cliente de Syncplicity](https://www.syncplicity.com/contact-us) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up Syncplicity** (Configurar Syncplicity), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-syncplicity-single-sign-on"></a>Configuración del inicio de sesión único de Syncplicity

1. Inicie sesión en su inquilino de **Syncplicity** .

1. En el menú en la parte superior, haga clic en **admin** (Administración), seleccione **settings** (Configuración) y luego haga clic en **Custom domain and single sign-on** (Dominio personalizado e inicio de sesión único).

    ![Syncplicity](./media/syncplicity-tutorial/ic769545.png "Syncplicity")

1. En la página del cuadro de diálogo **Single Sign-On (SSO)** (Configuración de inicio de sesión único [SSO]), siga estos pasos:

    ![Inicio de sesión único \(SSO\)](./media/syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")

     a. En el cuadro de texto **Custom Domain** (Dominio personalizado), escriba el nombre de su dominio.
  
    b. Seleccione **Enabled** (Habilitado) como **Single Sign-On Status** (Estado de inicio de sesión único).

    c. En el cuadro de texto **Entity Id** (Identificador de entidad), pegue el valor de **Identificador de Azure AD** que copió de Azure Portal.

    d. En el cuadro de texto **Dirección URL de la página de inicio de sesión**, pegue el valor de la **Dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    e. En el cuadro de texto **Dirección URL de la página de cierre de sesión**, pegue el valor de la **Dirección URL de cierre de sesión** que ha copiado de Azure Portal.

    f. En **Identity Provider Certificate** (Certificado del proveedor de identidades), haga clic en **Elegir archivo** y cargue el certificado que ha descargado de Azure Portal.

    g. Haga clic en **GURDAR CAMBIOS**.

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Syncplicity.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **Syncplicity**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Syncplicity**.

    ![Vínculo a Syncplicity en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.
    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-syncplicity-test-user"></a>Creación de un usuario de prueba de Syncplicity

Para que los usuarios de AAD puedan iniciar sesión, deben aprovisionarse a Syncplicity. En esta sección se describe cómo crear cuentas de usuario de AAD en Syncplicity.

**Para aprovisionar cuentas de usuario en Syncplicity, realice los siguientes pasos:**

1. Inicie sesión en su inquilino de **Syncplicity** (por ejemplo, `https://company.Syncplicity.com`).

1. Haga clic en **admin** y seleccione **cuentas de usuario** y, a continuación, haga clic en **ADD A USER** (AGREGAR UN USUARIO).

    ![Administración de usuarios](./media/syncplicity-tutorial/ic769764.png "Administración de usuarios")

1. Escriba las **Email Addresses** (Direcciones de correo electrónico) de una cuenta de Azure AD que quiera aprovisionar, seleccione **User** (Usuario) como **Role** (Rol) y haga clic en **NEXT** (SIGUIENTE).

    ![Información de la cuenta](./media/syncplicity-tutorial/ic769765.png "Información de la cuenta")

    > [!NOTE]
    > El titular de la cuenta de AAD recibirá un mensaje de correo electrónico junto con un vínculo para confirmar y activar la cuenta.

1. Seleccione un grupo de la compañía de la que debe convertirse en miembro su nuevo usuario y haga clic en **NEXT**(SIGUIENTE).

    ![Pertenencia a grupos](./media/syncplicity-tutorial/ic769772.png "Pertenencia a grupos")

    > [!NOTE]
    > Si no se muestra ningún grupo, haga clic en **NEXT**(SIGUIENTE).

1. Seleccione las carpetas que desea colocar bajo el control de Syncplicity en el equipo del usuario y haga clic en **NEXT**(SIGUIENTE).

    ![Carpetas de Syncplicity](./media/syncplicity-tutorial/ic769773.png "Carpetas de Syncplicity")

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Syncplicity ofrecida por Syncplicity para aprovisionar cuentas de usuario de AAD.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Syncplicity en el panel de acceso y debería iniciar sesión automáticamente en la versión de Syncplicity para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
