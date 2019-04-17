---
title: 'Tutorial: Integración de Azure Active Directory con Proxyclick | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Proxyclick.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5c58a859-71c2-4542-ae92-e5f16a8e7f18
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: b797c7d49495aac90abb31a9214bf2928784a866
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59262404"
---
# <a name="tutorial-azure-active-directory-integration-with-proxyclick"></a>Tutorial: Integración de Azure Active Directory con Proxyclick

En este tutorial se aprende a integrar Proxyclick con Azure Active Directory (Azure AD).
La integración de Proxyclick con Azure AD proporciona las siguientes ventajas:

* En Azure AD, puede controlar quién tiene acceso a Proxyclick.
* Puede permitir que los usuarios inicien sesión automáticamente en Proxyclick (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Proxyclick, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción habilitada para el inicio de sesión único en Proxyclick

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Proxyclick admite el inicio de sesión único iniciado por **SP** e **IDP**.

## <a name="adding-proxyclick-from-the-gallery"></a>Incorporación de Proxyclick desde la galería

Para configurar la integración de Proxyclick en Azure AD, es preciso agregar Proxyclick desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Proxyclick desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Proxyclick**, seleccione **Proxyclick** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

     ![Proxyclick en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección se configura y se prueba el inicio de sesión único de Azure AD con Proxyclick con un usuario de prueba de nombre **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Proxyclick.

Para configurar y probar el inicio de sesión único de Azure AD con Proxyclick, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Proxyclick](#configure-proxyclick-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Proxyclick](#create-proxyclick-test-user)**, para tener un homólogo de Britta Simon en Proxyclick que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Proxyclick, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Proxyclick**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

    ![Información de inicio de sesión único de Dominio y direcciones URL de Proxyclick](common/idp-intiated.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://saml.proxyclick.com/init/<companyId>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://saml.proxyclick.com/consume/<companyId>`

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de inicio de sesión único de Dominio y direcciones URL de Proxyclick](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://saml.proxyclick.com/init/<companyId>`

    > [!NOTE]
    > Estos valores no son reales. Los valores se actualizan con el identificador, la dirección URL de inicio de sesión y la dirección URL de respuesta reales, lo que se explica más adelante en el tutorial.

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

7. En la sección **Set up Proxyclick** (Configurar Proxyclick), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-proxyclick-single-sign-on"></a>Configuración del inicio de sesión único de Proxyclick

1. En otra ventana del explorador web, inicie sesión en el sitio de la empresa de Proxyclick como administrador.

2. Seleccione **Cuenta y configuración**.

    ![Configuración de Proxyclick](./media/proxyclick-tutorial/configure1.png)

3. Desplácese hacia abajo hasta **INTEGRACIONES** y seleccione **SAML**.

    ![Configuración de Proxyclick](./media/proxyclick-tutorial/configure2.png)

4. En la sección **SAML**, realice los pasos siguientes:

    ![Configuración de Proxyclick](./media/proxyclick-tutorial/configure3.png)

     a. Copie el valor **SAML Consumer URL** (URL de consumidor SAML) y péguelo en el cuadro de texto **URL de respuesta** de la sección **Configuración básica de SAML** de Azure Portal.

    b. Copie el valor **SAML SSO Redirect URL** (URL de redireccionamiento de SSO de SAML) y péguelo en los cuadros de texto **URL de inicio de sesión** e **Identificador** de la sección **Configuración básica de SAML** de Azure Portal.

    c. Seleccione **SAML Request Method** (Método de solicitud SAML) como **Redirección HTTP**.

    d. En el cuadro de texto **Issuer** (Emisor), pegue el valor de **Identificador de Azure AD** que ha copiado de Azure Portal.

    e. En el cuadro de texto **SAML 2.0 Endpoint URL** (URL de punto de conexión de SAML 2.0), pegue el valor de **URL de inicio de sesión** copiado de Azure Portal.

    f. Abra el archivo de certificado descargado de Azure Portal en el Bloc de notas y luego péguelo en el cuadro de texto **Certificado**.

    g. Haga clic en **Guardar cambios**.

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

En esta sección se habilita a Britta Simon para que use el inicio de sesión único de Azure al concederle acceso a Proxyclick.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **Proxyclick**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Proxyclick**.

    ![Vínculo de Proxyclick en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-proxyclick-test-user"></a>Creación de un usuario de prueba de Proxyclick

Para permitir que los usuarios de Azure AD inicien sesión en Proxyclick, deben aprovisionarse en Proxyclick. En el caso de Proxyclick, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en el sitio de la empresa de Proxyclick como administrador.

1. Haga clic en **Compañeros** en la barra de navegación superior.

    ![Agregar empleado](./media/proxyclick-tutorial/user1.png)

1. Haga clic en **Agregar compañero**

    ![Agregar empleado](./media/proxyclick-tutorial/user2.png)

1. En la sección **Agregar un compañero**, realice estos pasos:

    ![Agregar empleado](./media/proxyclick-tutorial/user3.png)

     a. En el cuadro de texto **Correo electrónico**, escriba la dirección de correo electrónico de un usuario, por ejemplo, brittasimon@contoso.com.

    b. En el cuadro de texto **Nombre**, escriba el nombre del usuario, en este caso, Britta.

    c. En el cuadro de texto **Apellidos**, escriba los apellidos del usuario, en este caso, Simon.

    d. Haga clic en **Agregar usuario**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Proxyclick en el panel de acceso, debería iniciar sesión automáticamente en la versión de Proxyclick para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

