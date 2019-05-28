---
title: 'Tutorial: Integración de Azure Active Directory con LinkedIn Elevate | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y LinkedIn Elevate.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 49e5b6f52b92889ccc7c46f091ea1b90d43b0307
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704793"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a>Tutorial: Integración de Azure Active Directory con LinkedIn Elevate

En este tutorial, aprenderá a integrar LinkedIn Elevate con Azure Active Directory (Azure AD).
Integrar LinkedIn Elevate con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a LinkedIn Elevate.
* Puede permitir que los usuarios inicien sesión automáticamente en LinkedIn Elevate (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con LinkedIn Elevate, necesita lo siguiente:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Suscripción habilitada para el inicio de sesión único en LinkedIn Elevate

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* LinkedIn Elevate admite el inicio de sesión único iniciado por **SP e IDP**

* LinkedIn Elevate admite el aprovisionamiento de usuarios **Just In Time**

* LinkedIn Elevate admite el aprovisionamiento [**automático** de usuarios](linkedinelevate-provisioning-tutorial.md)

## <a name="adding-linkedin-elevate-from-the-gallery"></a>Agregar LinkedIn Elevate desde la galería

Para configurar la integración de LinkedIn Elevate en Azure AD, deberá agregar LinkedIn Elevate desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar LinkedIn Elevate desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **LinkedIn Elevate**, seleccione **LinkedIn Elevate** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![LinkedIn Elevate en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con LinkedIn Elevate utilizando usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de LinkedIn Elevate.

Para configurar y probar el inicio de sesión único de Azure AD con LinkedIn Elevate, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de LinkedIn Elevate](#configure-linkedin-elevate-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de LinkedIn Elevate](#create-linkedin-elevate-test-user)** : para tener un homólogo de Britta Simon en LinkedIn Elevate que esté vinculado a la representación de ella en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con LinkedIn Elevate, realice los pasos siguientes:

1. En la página de integración de la aplicación **LinkedIn Elevate** de [Azure Portal](https://portal.azure.com/), seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de LinkedIn Elevate](common/idp-intiated.png)

     a. En el cuadro de texto **Identificador**, escriba el valor de **Id. de entidad**; copiará el valor de identificador de entidad del portal de Linkedin que se explica más adelante en este tutorial.

    b. En el cuadro de texto **URL de respuesta**, escriba el valor de **Assertion Consumer Access (ACS) Url** (Dirección URL de Assertion Consumer Access [ACS]); este valor lo copiará de Linkedin Portal que se explica más adelante en este tutorial.

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de LinkedIn Elevate](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>`

6. La aplicación LinkedIn Elevate espera las aserciones de SAML en un formato específico, lo que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token SAML. La siguiente captura de pantalla muestra la lista de atributos predeterminados, donde  **nameidentifier** se asigna con  **user.userprincipalname**. La aplicación LinkedIn Elevate espera que nameidentifier se asigne a  **user.mail**, por lo que debe editar la asignación de atributos haciendo clic en el icono Editar y cambiar dicha asignación.

    ![imagen](common/edit-attribute.png)

7. Además de lo anterior, la aplicación LinkedIn Elevate espera que se usen algunos atributos más en la respuesta de SAML. En la sección Notificaciones del usuario del cuadro de diálogo  **Atributos de usuario** , realice los siguientes pasos para agregar el atributo Token SAML como se muestra en la tabla siguiente:

    | NOMBRE | Atributo de origen|
    | -------| -------------|
    | department | user.department |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

8. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

9. En la sección **Configurar LinkedIn Elevate**, copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-linkedin-elevate-single-sign-on"></a>Configuración del inicio de sesión único de LinkedIn Elevate

1. En otra ventana del explorador web, inicie sesión como administrador en el inquilino LinkedIn Learning.

1. En **Account Center** (Centro de cuentas), haga clic en **Global Settings** (Configuración global) en **Settings** (Configuración). Además, seleccione **Elevate - Elevate AAD Test** en la lista desplegable.

    ![Configurar inicio de sesión único](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_01.png)

1. Haga clic en **OR Click Here to load and copy individual fields from the form** (O haga clic aquí para cargar y copiar campos individuales del formulario) y haga lo siguiente:

    ![Configurar inicio de sesión único](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_03.png)

     a. Copie el valor **Entity Id** (Id. de entidad) y péguelo en el cuadro de texto **Identificador** de la sección **Basic SAML Configuration** (Configuración básica de SAML) de Azure Portal.

    b. Copie el valor de **Assertion Consumer Access (ACS) Url** (Dirección URL de Assertion Consumer Access [ACS]) y péguelo en el cuadro de texto **URL de respuesta** de **Basic SAML Configuration** (Configuración básica de SAML) de Azure Portal.

1. Vaya a la sección **LinkedIn Admin Settings** (Configuración de administrador de LinkedIn). Cargue el archivo XML que ha descargado de Azure Portal haciendo clic en la opción Upload XML file (Cargar archivo XML).

    ![Configurar inicio de sesión único](./media/linkedinelevate-tutorial/tutorial_linkedin_metadata_03.png)

1. Haga clic en **On** (Activar) para habilitar SSO. El estado de SSO cambiará de **Not Connected** (No conectado) a **Connected** (Conectado)

    ![Configurar inicio de sesión único](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_05.png)

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

En esta sección, permitirá que Britta Simon use el inicio de sesión único de Azure; para ello, le concederá acceso a LinkedIn Elevate.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **LinkedIn Elevate**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **LinkedIn Elevate**.

    ![Vínculo a LinkedIn Elevate en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-linkedin-elevate-test-user"></a>Creación de un usuario de prueba de LinkedIn Elevate

La aplicación LinkedIn Elevate admite aprovisionamiento de usuarios Just-In-Time y, tras la autenticación, los usuarios se crearán automáticamente en la aplicación. En la página de configuración del administrador del portal de LinkedIn Elevate, invierta el conmutador **Automatically Assign licenses** (Asignar licencias automáticamente) a aprovisionamiento Just-In-Time activo, y este también asignará una licencia al usuario. LinkedIn Elevate también admite el aprovisionamiento automático de usuarios. [Aquí](linkedinelevate-provisioning-tutorial.md) puede encontrar más detalles sobre cómo configurar el aprovisionamiento automático de usuarios.

   ![Creación de un usuario de prueba de Azure AD](./media/linkedinelevate-tutorial/LinkedinUserprovswitch.png)

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de LinkedIn Elevate en el panel de acceso, debería iniciar sesión automáticamente en la versión de LinkedIn Elevate para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)