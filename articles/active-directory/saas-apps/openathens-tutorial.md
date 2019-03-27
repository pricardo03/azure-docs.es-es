---
title: 'Tutorial: Integración de Azure Active Directory con OpenAthens | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y OpenAthens.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: dd4adfc7-e238-41d5-8b25-1811f08078b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/4/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3eca6fc3ab788ee7085c0df5f6c9770858af29ba
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57847355"
---
# <a name="tutorial-azure-active-directory-integration-with-openathens"></a>Tutorial: Integración de Azure Active Directory con OpenAthens

En este tutorial, obtendrá información sobre cómo integrar OpenAthens con Azure Active Directory (Azure AD).
La integración de OpenAthens con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a OpenAthens.
* Puede permitir que los usuarios inicien sesión automáticamente en OpenAthens (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con OpenAthens, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para inicio de sesión único en OpenAthens

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* OpenAthens admite SSO iniciado por **IDP**

* OpenAthens admite el aprovisionamiento de usuarios **Just-In-Time**.

## <a name="adding-openathens-from-the-gallery"></a>Adición de OpenAthens desde la galería

Para configurar la integración de OpenAthens en Azure AD, será preciso que agregue OpenAthens desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar OpenAthens desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **OpenAthens**, seleccione **OpenAthens** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![OpenAthens en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con OpenAthens con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de OpenAthens.

Para configurar y probar el inicio de sesión único de Azure AD con OpenAthens, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de OpenAthens](#configure-openathens-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de OpenAthens](#create-openathens-test-user)**: para tener un homólogo de Britta Simon en OpenAthens que esté vinculado a su representación en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con OpenAthens, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración aplicaciones de **OpenAthens** seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

5. En la sección **Configuración básica de SAML**, cargue el **archivo de metadatos del proveedor de servicios** (los pasos para ello se mencionan más adelante en este tutorial).

     a. Haga clic en **Cargar el archivo de metadatos**.

    ![openathens cargar metadatos](common/upload-metadata.png)

    b. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.

    ![Openathens explora metadatos de carga](common/browse-upload-metadata.png)

    c. Una vez que se haya cargado correctamente el archivo de metadatos, el valor de **Identificador** se rellena automáticamente en el cuadro de texto de la sección **Configuración básica de SAML**:

    ![Información de dominio y direcciones URL de inicio de sesión único de OpenAthens](common/idp-identifier.png)

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

### <a name="configure-openathens-single-sign-on"></a>Configuración del inicio de sesión único de OpenAthens

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la empresa de OpenAthens.

2. Seleccione **Connections** (Conexiones) de la lista de la pestaña **Management** (Administración). 

    ![Configurar inicio de sesión único](./media/openathens-tutorial/tutorial_openathens_application1.png)

3. Seleccione **SAML 1.1/2.0** y, a continuación, seleccione el botón **Configure** (Configurar).

    ![Configurar inicio de sesión único](./media/openathens-tutorial/tutorial_openathens_application2.png)
    
4. Para agregar la configuración, seleccione el botón **Browse** (Examinar) para cargar el archivo .xml de metadatos que descargó desde Azure Portal y, a continuación, seleccione **Add** (Agregar).

    ![Configurar inicio de sesión único](./media/openathens-tutorial/tutorial_openathens_application3.png)

5. Realice los pasos siguientes en la pestaña **Details** (Detalles).

    ![Configurar inicio de sesión único](./media/openathens-tutorial/tutorial_openathens_application4.png)

     a. En **Display name mapping** (Asignación de nombre para mostrar), seleccione **Use attribute** (Usar atributo).

    b. En el cuadro de texto **Display name attribute** (Atributo de nombre para mostrar), escriba el valor `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    c. En **Unique user mapping** (Asignación de usuario único), seleccione **Use attribute** (Usar atributo).

    d. En el cuadro de texto **Unique user attribute** (Atributo de usuario único), introduzca el valor `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    e. En **Status** (Estado), seleccione las tres casillas de verificación.

    f. En **Create local accounts** (Crear cuentas locales), seleccione **automatically** (automáticamente).

    g. Seleccione **Save changes** (Guardar los cambios).

    h. Desde la pestaña **Usuario de confianza**, copie la **Dirección URL de metadatos** y ábrala en el explorador para descargar el archivo **XML de metadatos de SP**. Cargue este archivo de metadatos de SP en la sección **Configuración básica de SAML** de Azure AD.

    ![Configurar inicio de sesión único](./media/openathens-tutorial/tutorial_openathens_application5.png)

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a OpenAthens.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **OpenAthens**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **OpenAthens**.

    ![Vínculo a OpenAthens en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-openathens-test-user"></a>Creación de un usuario de prueba de OpenAthens

En esta sección, se crea un usuario llamado a Britta Simon en OpenAthens. OpenAthens admite el **aprovisionamiento de usuarios Just-In-Time**, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario deja de existir en OpenAthens, se crea uno nuevo después de la autenticación.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de OpenAthens en el panel de acceso, debería iniciar sesión automáticamente en la versión de OpenAthens para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

