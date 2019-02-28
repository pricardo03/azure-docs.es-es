---
title: 'Tutorial: Integración de Azure Active Directory con Appraisd | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Appraisd.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: db063306-4d0d-43ca-aae0-09f0426e7429
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1c92e76d1fd49121d09a56f986c39de52dbbde1c
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56877731"
---
# <a name="tutorial-azure-active-directory-integration-with-appraisd"></a>Tutorial: Integración de Azure Active Directory con Appraisd

En este tutorial, aprenderá a integrar Appraisd con Azure Active Directory (Azure AD).
La integración de Appraisd con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Appraisd.
* Puede permitir que los usuarios inicien sesión automáticamente en Appraisd (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Appraisd, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único de Appraisd

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Appraisd admite el inicio de sesión único iniciado por **SP e IDP**

## <a name="adding-appraisd-from-the-gallery"></a>Adición de Appraisd desde la galería

Para configurar la integración de Appraisd en Azure AD, será preciso que agregue Appraisd desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Appraisd desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Appraisd**, seleccione **Appraisd** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Appraisd en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Appraisd con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Appraisd.

Para configurar y probar el inicio de sesión único de Azure AD con Appraisd, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Appraisd](#configure-appraisd-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Appraisd](#create-appraisd-test-user)**: para tener un homólogo de Britta Simon en Appraisd que esté vinculado a la representación de ella en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Appraisd, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Appraisd**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Set-up Single Sign-On with SAML** (Configurar inicio de sesión único con SAML), haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en el modo iniciado por **IDP** siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Appraisd](common/both-preintegrated-advanced-urls.png)

     a. Haga clic en **Establecer direcciones URL adicionales**.

    b. En el cuadro de texto **Estado de la retransmisión**, escriba una dirección URL: `<TENANTCODE>`

    c. Si quiere configurar la aplicación en modo iniciado por **SP**, en el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL siguiendo este patrón: `https://app.appraisd.com/saml/<TENANTCODE>`

    > [!NOTE]
    > La URL de inicio de sesión y el valor del estado de retransmisión se obtienen en la página de configuración de SSO de Appraisd, que se explica más adelante en el tutorial.

5. La aplicación Appraisd espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, edite las notificaciones mediante el **icono Editar** o agregue notificaciones mediante **Agregar nueva notificación** para configurar el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:

    | NOMBRE |  Atributo de origen|
    | ---------------| --------------- |
    | nameidentifier | user.mail |
    | | |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

8. En la sección **Configurar Appraisd**, copie la dirección URL adecuada según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-appraisd-single-sign-on"></a>Configuración del inicio de sesión único de Appraisd

1. En otra ventana del explorador web, inicie sesión en Appraisd como administrador de seguridad.

2. En la parte superior derecha de la página, haga clic en el icono **Settings** (Configuración) y, después, vaya a  **Configuration** (Configuración).

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_sett.png)

3. En el lado izquierdo del menú, haga clic en el **inicio de sesión único SAML**.

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_single.png)

4. En la página **Configuración de inicio de sesión único de SAML 2.0**, siga estos pasos:

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_saml.png)

     a. Copie el valor de **Default Relay State** (Estado retransmisión predeterminado) y péguelo en el cuadro de texto  **Estado de la retransmisión** de  **Configuración básica de SAML** en Azure Portal.

    b. Copie el valor de **Service-initiated login URL** (URL de inicio de sesión iniciado por el servicio) y péguelo en el cuadro de texto  **URL de inicio de sesión** de  **Configuración básica de SAML** en Azure Portal.

5. En la página **Identificación de usuarios**, desplácese hacia abajo y realice los pasos siguientes:

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_identifying.png)

     a. En el cuadro de texto **Dirección URL del inicio de sesión único del proveedor de identidades**, pegue el valor de **URL de inicio de sesión** que ha copiado de Azure Portal y haga clic en **Guardar**.

    b. En el cuadro de texto **Identity Provider Issuer URL** (Dirección URL del emisor del proveedor de identidades), pegue el valor de **Identificador de Azure Ad** que ha copiado de Azure Portal y haga clic en **Guardar**.

    c. En el Bloc de notas, abra el certificado codificado en Base 64 que descargó de Azure Portal, copie el contenido y, luego, péguelo en el cuadro de texto  **X.509 Certificate**  (Certificado X.509) y haga clic en **Guardar**.

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediendo acceso a Appraisd.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, luego, **Appraisd**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Appraisd**.

    ![Enlace a Appraisd en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-appraisd-test-user"></a>Creación de un usuario de prueba de Appraisd

Para permitir que los usuarios de Azure AD inicien sesión en Appraisd, deben aprovisionarse en Appraisd. En Appraisd, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en Appraisd como administrador de seguridad.

2. En la parte superior derecha de la página, haga clic en el icono **Settings** (Configuración) y, después, vaya a  **Administration centre** (Centro de administración).

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_admin.png)

3. En la barra de herramientas de la parte superior de la página, haga clic en  **People** (Personas) y, después, vaya a  **Add a new user** (Agregar un nuevo usuario).

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_user.png)

4. En la página **Agregar nuevo usuario**, realice los pasos siguientes:

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_newuser.png)

     a. En el cuadro de texto **First Name** (Nombre), escriba el nombre del usuario, en este caso **Britta**.

    b. En el cuadro de texto **Last Name** (Apellidos), escriba los apellidos del usuario, en este caso **Simon**.

    c. En el cuadro de texto **E-mail** (Correo electrónico), escriba el correo electrónico del usuario; por ejemplo, **Brittasimon@contoso.com**.

    d. Haga clic en **Agregar usuario**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Appraisd en el panel de acceso y debería iniciar sesión automáticamente en la versión de Appraisd para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
