---
title: 'Tutorial: Integración de Azure Active Directory con AirWatch | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y AirWatch.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7ec8b575157dcf2fe8430f554798af62b966c78d
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406694"
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Tutorial: Integración de Azure Active Directory con AirWatch

En este tutorial, aprenderá a integrar AirWatch con Azure Active Directory (Azure AD).
La integración de AirWatch con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a AirWatch.
* Puede permitir que los usuarios inicien sesión automáticamente en AirWatch (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con AirWatch, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción habilitada para el inicio de sesión único en AirWatch

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* AirWatch admite el inicio de sesión único iniciado por **SP**.

## <a name="adding-airwatch-from-the-gallery"></a>Incorporación de AirWatch desde la Galería

Para configurar la integración de AirWatch en Azure AD, es preciso agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar AirWatch desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **AirWatch**, seleccione **AirWatch** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![AirWatch en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con AirWatch con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de AirWatch.

Para configurar y probar el inicio de sesión único de Azure AD con AirWatch, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de AirWatch](#configure-airwatch-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Creación de un usuario de prueba de AirWatch](#create-airwatch-test-user)**: para tener un homólogo de Britta Simon en AirWatch vinculado a la representación del usuario en Azure AD.
5. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con AirWatch, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **AirWatch**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de AirWatch](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`

    b. En el cuadro de texto **Identificador (Id. de entidad)**, escriba el valor como: `AirWatch`

    > [!NOTE]
    > Este valor no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de AirWatch](https://www.air-watch.com/company/contact-us/) para obtenerlo. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. La aplicación AirWatch espera las aserciones de SAML en un formato concreto. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, edite las notificaciones mediante el **icono Editar** o agregue notificaciones mediante **Agregar nueva notificación** para configurar el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:

    | NOMBRE |  Atributo de origen|
    |---------------|----------------|
    | UID | user.userprincipalname |
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

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

8. En la sección **Configurar AirWatch**, copie las direcciones URL adecuada según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-airwatch-single-sign-on"></a>Configuración del inicio de sesión único de AirWatch

1. En otra ventana del explorador web, inicie sesión en el sitio de AirWatch de la compañía como administrador.

2. En el panel de navegación izquierdo, haga clic en **Accounts** (Cuentas) y, luego, en **Administrators** (Administradores).

   ![Administradores](./media/airwatch-tutorial/ic791920.png "Administradores")

3. Expanda el menú **Settings** (Configuración) y, a continuación, haga clic en **Directory Services** (Servicios de directorio).

   ![Configuración](./media/airwatch-tutorial/ic791921.png "Configuración")

4. Haga clic en la pestaña **Usuario**; en el cuadro de texto **DN base**, escriba el nombre de dominio y haga clic en **Guardar**.

   ![Usuario](./media/airwatch-tutorial/ic791922.png "Usuario")

5. Haga clic en la pestaña **Server** (Servidor).

   ![Servidor](./media/airwatch-tutorial/ic791923.png "Servidor")

6. Lleve a cabo los siguiente pasos:

    ![Cargar](./media/airwatch-tutorial/ic791924.png "Cargar")   

     a. En **Directory Type** (Tipo de directorio), seleccione **None** (Ninguno).

    b. Seleccione **Use SAML For Authentication**(Usar SAML para autenticación).

    c. Para cargar el certificado descargado, haga clic en **Upload**(Cargar).

7. En la sección **Request** (Solicitud), siga estos pasos:

    ![Solicitud](./media/airwatch-tutorial/ic791925.png "Solicitud")  

     a. En **Request Binding Type** (Tipo de enlace de solicitud), seleccione **POST**.

    b. En Azure Portal, en la página de diálogo **Configure single sign-on at Airwatch** (Configurar inicio de sesión único en Airwatch), copie el valor de **Dirección URL de inicio de sesión** y péguelo en el cuadro de texto **Identity Provider Single Sign On URL** (Dirección URL de inicio de sesión del proveedor de identidades).

    c. En la lista **NameID Format** (Formato de NameID), seleccione **Email address** (Dirección de correo electrónico).

    d. Haga clic en **Save**(Guardar).

8. Haga clic de nuevo en la pestaña **User** (Usuario).

    ![Usuario](./media/airwatch-tutorial/ic791926.png "Usuario")

9. En la sección **Attribute** (Atributo), realice estos pasos:

    ![Atributo](./media/airwatch-tutorial/ic791927.png "Atributo")

     a. En el cuadro de texto **Identificador de objetos**, escriba `http://schemas.microsoft.com/identity/claims/objectidentifier`.

    b. En el cuadro de texto **Nombre de usuario**, escriba `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    c. En el cuadro de texto **Nombre para mostrar**, escriba `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    d. En el cuadro de texto **Nombre**, escriba `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    e. En el cuadro de texto **Apellidos**, escriba `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.

    f. En el cuadro de texto **Correo electrónico**, escriba `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    g. Haga clic en **Save**(Guardar).

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a AirWatch.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**,  **AirWatch**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **AirWatch**.

    ![Vínculo a AirWatch en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-airwatch-test-user"></a>Creación de un usuario de prueba de AirWatch

Para permitir que los usuarios de Azure AD inicien sesión en AirWatch, tienen que aprovisionarse en AirWatch. En el caso de AirWatch, el aprovisionamiento es una tarea manual.

**Siga estos pasos para configurar el aprovisionamiento de usuario:**

1. Inicie sesión en el sitio de la compañía de **AirWatch** como administrador.

2. En el panel de navegación izquierdo, haga clic en **Accounts** (Cuentas) y luego en **Users** (Usuarios).
  
   ![Usuarios](./media/airwatch-tutorial/ic791929.png "Usuarios")

3. En el menú **Users** (Usuarios), haga clic en **List View** (Vista de lista) y, a continuación, haga clic en **Add \> Add User** (Agregar > Agregar usuario).
  
   ![Agregar usuario](./media/airwatch-tutorial/ic791930.png "Agregar usuario")

4. En el cuadro de diálogo **Add / Edit User** (Agregar/Editar usuario), realice los siguientes pasos:

   ![Agregar usuario](./media/airwatch-tutorial/ic791931.png "Agregar usuario")

    a. Especifique **Username** (Nombre de usuario), **Password** (Contraseña), **Confirm Password** (Confirmar contraseña), **First Name** (Nombre), **Last Name** (Apellido), **Email Address** (Correo electrónico) de una cuenta de Azure Active Directory válida que quiera aprovisionar en los cuadros de texto relacionados.

   b. Haga clic en **Save**(Guardar).

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de AirWatch ofrecida por AirWatch para aprovisionar cuentas de usuario de AAD.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de AirWatch en el panel de acceso y debería iniciar sesión automáticamente en la versión de AirWatch para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
