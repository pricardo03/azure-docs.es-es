---
title: 'Tutorial: Integración de Azure Active Directory con Tableau Server | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Tableau Server.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/22/2019
ms.author: jeedes
ms.openlocfilehash: 539a06398675dc7851017ec5d428e0942e54ce1f
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2019
ms.locfileid: "59564772"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Tutorial: Integración de Azure Active Directory con Tableau Server

En este tutorial, aprenderá a integrar Tableau Server con Azure Active Directory (Azure AD).
Integrar Tableau Server con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Tableau Server.
* Puede permitir que los usuarios inicien sesión automáticamente en Tableau Server (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Tableau Server, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una suscripción habilitada para el inicio de sesión único en Tableau Server

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Tableau Server admite SSO iniciado por **SP**

## <a name="adding-tableau-server-from-the-gallery"></a>Incorporación de Tableau Server desde la galería

Para configurar la integración de Tableau Server en Azure AD, será preciso que agregue Tableau Server desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Tableau Server desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Tableau Server**, seleccione **Tableau Server** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

    ![Tableau Server en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Tableau Server con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Tableau Server.

Para configurar y probar el inicio de sesión único de Azure AD con Tableau Server, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Tableau Server](#configure-tableau-server-single-sign-on)**: para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Tableau Server](#create-tableau-server-test-user)**: para tener un homólogo de Britta Simon en Tableau Server vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Tableau Server, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Tableau Server**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información del dominio y las direcciones URL de inicio de sesión único de Tableau Server](common/sp-identifier-reply.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://azure.<domain name>.link`

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://azure.<domain name>.link`

    c. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://azure.<domain name>.link/wg/saml/SSO/index.html`

    > [!NOTE]
    > Los valores anteriores no son valores reales. Actualice los valores con la dirección URL y el identificador releas de la página de configuración de Tableau Server que se explica más adelante en el tutorial.

5. La aplicación Tableau Server espera recibir una notificación personalizada **username** que se debe definir como sigue. Se usa como identificador de usuario en lugar de como notificación de identificador de usuario único. Puede administrar los valores de estos atributos en la sección **User Attributes & Claims** (Atributos y notificaciones del usuario) de la página de integración de aplicaciones. Haga clic en el botón **Edit** (Editar) para abrir el cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario).

    ![imagen](common/edit-attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario), configure el atributo token SAML como se muestra en la imagen anterior y realice los siguientes pasos:

    | NOMBRE | Atributo de origen | Espacio de nombres |
    | ---------------| --------------- | ----------- |
    | nombre de usuario | user.userprincipalname | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | | |

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Escriba el valor de **Espacio de nombres**.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

8. En la sección **Set up Tableau Server** (Configurar Tableau Server), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-tableau-server-single-sign-on"></a>Configuración del inicio de sesión único de Tableau Server

1. Para configurar SSO para la aplicación, debe iniciar sesión en su inquilino de Tableau Server como administrador.

2. En la pestaña **CONFIGURACIÓN**, seleccione **User Identity & Access** (Identidad de usuario y acceso) y, a continuación, seleccione la pestaña del método de **Autenticación**.

    ![Configurar inicio de sesión único](./media/tableauserver-tutorial/tutorial-tableauserver-auth.png)

3. En la página **CONFIGURACIÓN**, realice los siguientes pasos:

    ![Configurar inicio de sesión único](./media/tableauserver-tutorial/tutorial-tableauserver-config.png)

     a. Como **Authentication Method** (Método de autenticación), seleccione SAML.

    b. Seleccione la casilla **Enable SAML Authentication for the server**(Habilitar autenticación SAML para el servidor).

    c. Dirección URL de retorno de Tableau Server: dirección URL a la que accederán los usuarios de Tableau Server; por ejemplo, <http://tableau_server>. No se recomienda usar `http://localhost`. No se admiten las direcciones URL que tienen una barra diagonal al final (por ejemplo, `http://tableau_server/`). Copie el valor de **Tableau Server return URL** (Dirección URL de retorno de Tableau Server) y péguela en el cuadro de texto **URL de inicio de sesión** de la sección **Configuración básica de SAML** de Azure Portal

    d. SAML entity ID (Id. de entidad SAML): el identificador de entidad identifica de forma exclusiva la instalación de Tableau Server en el IdP. Aquí, si quiere, puede especificar de nuevo la dirección URL de Tableau Server, pero no tiene que ser esa misma URL. Copie el valor de **SAML entity ID** (Id. de entidad SAML) y péguelo en el cuadro de texto **Identificador** de la sección **Configuración básica de SAML** de Azure Portal

    e. Haga clic en **Download XML Metadata File** (Descargar archivo de metadatos XML) y ábralo en la aplicación del editor de texto. Busque la URL del Servicio de consumidor de aserciones con Http Post e índice 0 y copie la URL. Ahora, péguelo en el cuadro de texto **URL de respuesta**  de la sección **Configuración básica de SAML** de Azure Portal

    f. Busque el archivo de metadatos de federación descargado desde Azure Portal y cárguelo en el **SAML Idp metadata file**(Archivo de metadatos del proveedor de identidades SAML).

    g. Escriba los nombres de los atributos que el proveedor de identidades usa para contener los nombres de usuario, los nombres para mostrar y las direcciones de correo electrónico.

    h. Haga clic en **Guardar**

    > [!NOTE]
    > El cliente tiene que cargar los certificados en la configuración de SSO de SAML de Tableau Server para que se omitan en el flujo de SSO. Si necesita ayuda para configurar SAML en Tableau Server, vea el artículo [Configuración de SAML](https://onlinehelp.tableau.com/v2018.2/server/en-us/saml_config_steps_tsm_ui.htm).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba `brittasimon@yourcompanydomain.extension`  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Tableau Server.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Tableau Server**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Tableau Server**.

    ![Vínculo a Tableau Online en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-tableau-server-test-user"></a>Crear usuario de prueba de Tableau Server

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en Tableau Server. Debe aprovisionar todos los usuarios en el servidor de Tableau.

Tenga en cuenta que el nombre de usuario debe coincidir con el valor que ha configurado en el atributo personalizado de **nombre de usuario** de Azure AD. Con la asignación correcta, la integración debería funcionar la configuración del inicio de sesión único de Azure AD.

> [!NOTE]
> Si necesita crear un usuario manualmente, póngase en contacto con el administrador de Tableau Server de su organización.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Tableau Server en el Panel de acceso, debería iniciar sesión automáticamente en la instancia de Tableau Server para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

