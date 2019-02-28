---
title: 'Tutorial: Integración de Azure Active Directory con Dome9 Arc | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Dome9 Arc.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4c12875f-de71-40cb-b9ac-216a805334e5
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/31/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: da98268d0a6ee015c848e9fe91cb3deecf28c4f4
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56875708"
---
# <a name="tutorial-azure-active-directory-integration-with-dome9-arc"></a>Tutorial: Integración de Azure Active Directory con Dome9 Arc

En este tutorial, obtendrá información sobre cómo integrar Dome9 Arc con Azure Active Directory (Azure AD).
La integración de Dome9 Arc con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Dome9 Arc.
* Puede permitir que los usuarios inicien sesión automáticamente en Dome9 Arc (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Dome9 Arc, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en Dome9 Arc

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Dome9 Arc admite el inicio de sesión único iniciado tanto por **SP** como por **IDP**

## <a name="adding-dome9-arc-from-the-gallery"></a>Adición de Dome9 Arc desde la galería

Para configurar la integración de Dome9 Arc en Azure AD, debe agregar Dome9 Arc desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Dome9 Arc desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Dome9 Arc**, seleccione **Dome9 Arc** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Dome9 Arc en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, va a configurar y probar el inicio de sesión único de Azure AD con Dome9 Arc con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Dome9 Arc.

Para configurar y probar el inicio de sesión único de Azure AD con Dome9 Arc, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único en Dome9 Arc](#configure-dome9-arc-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba en Dome9 Arc](#create-dome9-arc-test-user)**: para tener un homólogo de Britta Simon en Dome9 Arc que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Dome9 Arc, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Dome9 Arc**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en el modo iniciado por **IDP** siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Dome9 Arc](common/idp-intiated.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://secure.dome9.com/`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://secure.dome9.com/sso/saml/yourcompanyname`

    > [!NOTE]
    > Seleccionará el valor de nombre de la empresa en el portal de administración de dome9, un proceso que se explica más adelante en el tutorial.

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Dome9 Arc](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://secure.dome9.com/sso/saml/<yourcompanyname>`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Dome9 Arc](mailto:support@dome9.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

6. La aplicación Dome9 Arc espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

7. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, edite las notificaciones mediante el **icono Editar** o agregue notificaciones mediante **Agregar nueva notificación** para configurar el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos: 

    | NOMBRE |  Atributo de origen|
    | ---------------| --------------- |
    | memberof | user.assignedroles |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

8. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

9. En la sección **Set up Dome9 Arc** (Configurar Dome9 Arc), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-dome9-arc-single-sign-on"></a>Configuración del inicio de sesión único en Dome9 Arc

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía de Dome9 Arc.

2. Haga clic en **Profile Settings** (Configuración de perfil) en la esquina superior derecha y, luego, en **Account Settings** (Configuración de la cuenta). 

    ![Configuración de Dome9 Arc](./media/dome9arc-tutorial/configure1.png)

3. Vaya a **SSO** y después haga clic en **ENABLE** (HABILITAR).

    ![Configuración de Dome9 Arc](./media/dome9arc-tutorial/configure2.png)

4. En la sección de configuración de SSO, realice los pasos siguientes:

    ![Configuración de Dome9 Arc](./media/dome9arc-tutorial/configure3.png)

     a. Escriba el nombre de la compañía en el cuadro de texto **Account ID** (Id. de cuenta). Este valor se usará en la dirección URL de respuesta mencionada en la sección de la dirección URL de Azure Portal.

    b. En el cuadro de texto **Issuer** (Emisor), pegue el valor de **Identificador Azure AD**, que ha copiado de Azure Portal.

    c. En el cuadro de texto **Idp endpoint url** (URL de punto de conexión de Idp), pegue el valor de **Login URL** (Dirección URL de inicio de sesión) que ha copiado de Azure Portal.

    d. Abra el certificado descargado con codificación Base64 en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **X.509 certificate** (Certificado X.509).

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
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Dome9 Arc.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Dome9 Arc**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Dome9 Arc**.

    ![Vínculo a Dome9 Arc en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-dome9-arc-test-user"></a>Creación de un usuario de prueba en Dome9 Arc

Para permitir que los usuarios de Azure AD inicien sesión en Dome9 Arc, tienen que aprovisionarse en la aplicación. Dome9 Arc admite el aprovisionamiento Just-In-Time, pero para que funcione correctamente, el usuario debe seleccionar un **rol** determinado y asignarlo al usuario.

   >[!Note]
   >Para la creación del **rol** y otros detalles, póngase en contacto con el [equipo de soporte técnico de Dome9 Arc](https://dome9.com/about/contact-us/).

**Para aprovisionar una cuenta de usuario manualmente, realice estos pasos:**

1. Inicie sesión como administrador en el sitio de la compañía de Dome9 Arc.

2. Haga clic en **Users & Roles** (Usuarios y roles) y luego en **Users** (Usuarios).

    ![Agregar empleado](./media/dome9arc-tutorial/user1.png)

3. Haga clic en **ADD USER** (Agregar usuario).

    ![Agregar empleado](./media/dome9arc-tutorial/user2.png)

4. En la sección **Crear usuario** , lleve a cabo estos pasos:

    ![Agregar empleado](./media/dome9arc-tutorial/user3.png)

     a. En el cuadro de texto **Email** (Correo electrónico), escriba el correo electrónico del usuario, en el ejemplo Brittasimon@contoso.com.

    b. En el cuadro de texto **Nombre**, escriba el nombre del usuario, en este caso, Britta.

    c. En el cuadro de texto **Apellidos**, escriba el apellido del usuario, en este caso Simon.

    d. Asegúrese de que **SSO User** (Usuario de SSO) esté establecido en **On** (Activado).

    e. Haga clic en **CREATE** (Crear).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Dome9 Arc en el Panel de acceso, debería iniciar sesión automáticamente en la instancia de Dome9 Arc para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

