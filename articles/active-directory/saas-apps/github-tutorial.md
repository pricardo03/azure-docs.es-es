---
title: 'Tutorial: Integración de Azure Active Directory con GitHub | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y GitHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 8761f5ca-c57c-4a7e-bf14-ac0421bd3b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25540d1f26fa6021ef05108f9743e77a6184f3b3
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65898566"
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Tutorial: Integración de Azure Active Directory con GitHub

En este tutorial, obtendrá información sobre cómo integrar GitHub con Azure Active Directory (Azure AD).
Integrar GitHub con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a GitHub.
* Puede permitir que los usuarios inicien sesión automáticamente en GitHub (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con GitHub, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una organización GitHub creada en [GitHub Enterprise Cloud](https://help.github.com/articles/github-s-products/#github-enterprise), que requiere el [plan de facturación de GitHub Enterprise](https://help.github.com/articles/github-s-billing-plans/#billing-plans-for-organizations).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* GitHub admite el inicio de sesión único iniciado por **SP**

* GitHub admite el [**aprovisionamiento de usuarios** automático](github-provisioning-tutorial.md)

## <a name="adding-github-from-the-gallery"></a>Adición de GitHub desde la galería

Para configurar la integración de GitHub en Azure AD, debe agregar GitHub desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar GitHub desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **GitHub**, seleccione **GitHub.com** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![GitHub en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con GitHub mediante un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de GitHub.

Para configurar y probar el SSO de Azure AD con GitHub, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de GitHub](#configure-github-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba en GitHub](#create-github-test-user)**: para tener un homólogo de Britta Simon en GitHub que esté vinculado a la representación de ella en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con GitHub, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **GitHub**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de GitHub](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://github.com/orgs/<entity-id>/sso`

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://github.com/orgs/<entity-id>`

    > [!NOTE]
    > Tenga en cuenta que estos no son valores reales. Tendrá que actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Aquí le recomendamos que utilice el valor de cadena único en el identificador. Vaya a la sección de administración de GitHub para recuperar estos valores.

5. La aplicación GitHub espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de pantalla muestra la lista de atributos predeterminados, donde **nameidentifier** se asigna con **user.userprincipalname**. La aplicación GitHub espera que **nameidentifier** se asigne con **user.mail**, por lo que debe editar la asignación de atributos haciendo clic en el icono **Editar** y cambiar dicha asignación.

    ![imagen](common/edit-attribute.png)

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

7. En la sección **Set up GitHub** (Configurar GitHub), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-github-single-sign-on"></a>Configuración de inicio de sesión único en GitHub

1. En otra ventana del navegador web, inicie sesión en el sitio de la organización de GitHub como administrador.

2. Vaya a **Configuración** y haga clic en **Seguridad**

    ![Configuración](./media/github-tutorial/tutorial_github_config_github_03.png)

3. Active la casilla **Habilitar autenticación SAML** para ver los campos de configuración del inicio de sesión único. Luego, use el valor de URL de inicio de sesión único para actualizar la URL de inicio de sesión único en la configuración de Azure AD.

    ![Configuración](./media/github-tutorial/tutorial_github_config_github_13.png)

4. Configure los campos siguientes:

    ![Configuración](./media/github-tutorial/tutorial_github_config_github_051.png)

     a. En el cuadro de texto **Sign-On URL** (Dirección URL de inicio de sesión), pegue el valor de la **URL de inicio de sesión** que ha copiado de Azure Portal.

    b. En el cuadro de texto **Emisor**, pegue el valor del **Identificador de Azure AD** que ha copiado de Azure Portal.

    c. Abra el certificado descargado de Azure Portal en el Bloc de notas y pegue el contenido en el cuadro de texto **Certificado público**.

    d. Haga clic en el icono **Editar** para editar el **Método de firma** y el **Método de resumen** desde **RSA-SHA1** y **SHA1** hasta **RSA-SHA256** y **SHA256** tal como se muestra a continuación.

    ![imagen](./media/github-tutorial/tutorial_github_sha.png)

5. Haga clic en **Test SAML configuration** (Probar configuración de SAML) para configura que no hay errores de validación durante el SSO.

    ![Configuración](./media/github-tutorial/tutorial_github_config_github_06.png)

6. Haga clic en **Guardar**

> [!NOTE]
> El inicio de sesión único en GitHub realiza la autenticación en una organización específica de GitHub, pero no reemplaza la autenticación propia de GitHub. Por tanto, si la sesión del usuario en github.com ha expirado, puede que se le pida que se autentique con la contraseña o el identificador de GitHub durante el proceso de inicio de sesión único.

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a GitHub.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **GitHub**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **GitHub**.

    ![El vínculo de GitHub en la lista Aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-github-test-user"></a>Creación de un usuario de prueba en GitHub

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en GitHub. GitHub admite el aprovisionamiento automático de usuarios, que está habilitado de forma predeterminada. [Aquí](github-provisioning-tutorial.md) puede encontrar más información sobre cómo configurar el aprovisionamiento automático de usuarios.

**Para crear un usuario manualmente, siga los pasos siguientes:**

1. Inicie sesión en el sitio de la empresa de GitHub como administrador.

2. Haga clic en **Contactos**.

    ![Personas](./media/github-tutorial/tutorial_github_config_github_08.png "Personas")

3. Haga clic en **Invitar a miembros**.

    ![Invitación de usuarios](./media/github-tutorial/tutorial_github_config_github_09.png "Invitación de usuarios")

4. En la página de diálogo **Invitar a miembros**, realice los siguientes pasos:

     a. En el cuadro de texto **Correo electrónico**, escriba la dirección de correo electrónico de la cuenta de Britta Simon.

    ![Invitar a personas](./media/github-tutorial/tutorial_github_config_github_10.png "Invitar a personas")

    b. Haga clic en **Enviar invitación**.

    ![Invitar a personas](./media/github-tutorial/tutorial_github_config_github_11.png "Invitar a personas")

    > [!NOTE]
    > El titular de la cuenta de Azure Active Directory recibirá un mensaje de correo y seguirá un vínculo para confirmar su cuenta antes de que se active.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de GitHub del panel de acceso, debería iniciar sesión automáticamente en la versión de GitHub para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
