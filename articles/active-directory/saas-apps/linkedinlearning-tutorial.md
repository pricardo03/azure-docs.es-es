---
title: 'Tutorial: Integración de Azure Active Directory con LinkedIn Learning | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y LinkedIn Learning.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/18/2018
ms.author: jeedes
ms.openlocfilehash: e8d829c4990aa798ce77ecc9caae51ace4c53023
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2018
ms.locfileid: "53808050"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a>Tutorial: Integración de Azure Active Directory con LinkedIn Learning

En este tutorial, aprenderá a integrar LinkedIn Learning con Azure Active Directory (Azure AD).
Integrar LinkedIn Learning con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a LinkedIn Learning.
* Puede permitir que los usuarios inicien sesión automáticamente en LinkedIn Learning (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con LinkedIn Learning, necesita lo siguiente:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en LinkedIn Learning

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* LinkedIn Learning admite el inicio de sesión único iniciado por **SP y IDP**
* LinkedIn Learning admite el aprovisionamiento de usuario **Just In Time**

## <a name="adding-linkedin-learning-from-the-gallery"></a>Agregar LinkedIn Learning desde la galería

Para configurar la integración de LinkedIn Learning en Azure AD, deberá agregar esta solución desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar LinkedIn Learning desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **LinkedIn Learning**, seleccione **LinkedIn Learning** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![LinkedIn Learning en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con LinkedIn Learning utilizando usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de LinkedIn Learning.

Para configurar y probar el inicio de sesión único de Azure AD con LinkedIn Learning, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de LinkedIn Learning](#configure-linkedin-learning-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de LinkedIn Learning](#create-linkedin-learning-test-user)**: para tener un homólogo de Britta Simon en LinkedIn Learning que esté vinculado a la representación de ella en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con LinkedIn Learning, realice los pasos siguientes:

1. En la página de integración de la aplicación **LinkedIn Learning** de [Azure Portal](https://portal.azure.com/), seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en el modo **Iniciado por IdP**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de LinkedIn Learning](common/idp-intiated.png)

     a. En el cuadro de texto **Identificador**, escriba el **Id.de entidad** que copió de LinkedIn Portal. 

    b. En el cuadro de texto **URL de respuesta**, escriba la **Assertion Consumer Service (ACS) Url** (URL de Servicio de consumidor de aserciones [ACS]) que copió de LinkedIn Portal.

    c. Si quiere configurar la aplicación en modo **Iniciado por SP**, haga clic en la opción **Establecer direcciones URL adicionales** de la sección **Configuración básica de SAML**, donde especificará su URL de inicio de sesión. Para crear la dirección URL de inicio de sesión, copie la **Assertion Consumer Service (ACS) Url** (URL de Servicio de consumidor de aserciones [ACS]) y reemplace /saml/ por /login/. Una vez realizada esta acción, la dirección URL de inicio de sesión debe tener el siguiente patrón:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Información de dominio y direcciones URL de inicio de sesión único de LinkedIn Learning](common/metadata-upload-additional-signon.png)

    > [!NOTE]
    > Estos valores no son reales. Los valores se actualizan con el identificador, la dirección URL de inicio de sesión y la dirección URL de respuesta reales, lo que se explica más adelante en la sección **Configuración del inicio de sesión único de LinkedIn Learning** del tutorial.

5. La aplicación LinkedIn Learning espera las aserciones de SAML en un formato específico, lo que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de pantalla le muestra un ejemplo de esto. El valor predeterminado de **Identificador de usuario único** es **user.userprincipalname**, pero LinkedIn Learning espera que este valor se asigne a la dirección de correo del usuario. Para ello, puede usar el atributo **user.mail** de la lista o usar el valor de atributo correspondiente en función de la configuración de su organización. 

    ![imagen](common/edit-attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, edite las notificaciones mediante el **icono Editar** o agregue notficaciones mediante **Agregar nueva notificación** para configurar el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:
    
    | NOMBRE | Atributo de origen |
    | ---------------| --------------- |
    | email  | user.mail  |
    | department  | user.department  |
    | firstname  | user.givenname  |
    | lastname  | user.surname  |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](./media/linkedinlearning-tutorial/tutorial_usermail.png)

    ![imagen](./media/linkedinlearning-tutorial/tutorial_usermailedit.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

### <a name="configure-linkedin-learning-single-sign-on"></a>Configuración del inicio de sesión único de LinkedIn Learning

1. En otra ventana del explorador web, inicie sesión como administrador en el inquilino LinkedIn Learning.

2. En **Account Center** (Centro de cuentas), haga clic en **Global Settings** (Configuración global) en **Settings** (Configuración). Seleccione también **Learning - Default** (Learning - Predeterminado) en la lista desplegable.

    ![Configurar inicio de sesión único](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

3. Haga clic en **OR Click Here to load and copy individual fields from the form** (O haga clic aquí para cargar y copiar campos individuales del formulario) y copie el **Id.de entidad** y la **Assertion Consumer Access (ACS) Url** (Url de Acceso de consumidor de aserciones [ACS]) en la sección **Configuración básica de SAML** de Azure Portal.

    ![Configurar inicio de sesión único](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

4. Vaya a la sección **LinkedIn Admin Settings** (Configuración de administrador de LinkedIn). Cargue el archivo XML que ha descargado de Azure Portal haciendo clic en la opción **Upload XML file** (Cargar archivo XML).

    ![Configurar inicio de sesión único](./media/linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

5. Haga clic en **On** (Activar) para habilitar SSO. El estado de SSO cambiará de **Not Connected** (No conectado) a **Connected** (Conectado).

    ![Configurar inicio de sesión único](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a LinkedIn Learning.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **LinkedIn Learning**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **LinkedIn Learning**.

    ![Vínculo a LinkedIn Learning en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-linkedin-learning-test-user"></a>Creación de un usuario de prueba de LinkedIn Learning

La aplicación LinkedIn Learning admite aprovisionamiento de usuarios Just-In-Time y, tras la autenticación, los usuarios se crean automáticamente en la aplicación. En la página de configuración del administrador del portal de LinkedIn Learning, invierta el conmutador **Automatically Assign licenses** (Asignar licencias automáticamente) a aprovisionamiento Just-In-Time activo, y este también asignará una licencia al usuario.

   ![Creación de un usuario de prueba de Azure AD](./media/linkedinlearning-tutorial/LinkedinUserprovswitch.png)

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de LinkedIn Learning en el panel de acceso, debería iniciar sesión automáticamente en la versión de LinkedIn Learning para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

