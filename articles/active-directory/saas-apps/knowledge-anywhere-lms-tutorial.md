---
title: 'Tutorial: Integración de Azure Active Directory con Knowledge Anywhere LMS | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Knowledge Anywhere LMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5cfa07b1-a792-4f0a-8c6f-1a13142193d9
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/14/2019
ms.author: jeedes
ms.openlocfilehash: c56c03637e5a8eb7716673974292482a9fb75212
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56877799"
---
# <a name="tutorial-azure-active-directory-integration-with-knowledge-anywhere-lms"></a>Tutorial: Integración de Azure Active Directory con Knowledge Anywhere LMS

En este tutorial, aprenderá a integrar Knowledge Anywhere LMS.con Azure Active Directory (Azure AD).
La integración de Knowledge Anywhere LMS con Azure AD ofrece las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Knowledge Anywhere LMS.
* Puede permitir que los usuarios inicien sesión automáticamente en Knowledge Anywhere LMS (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Knowledge Anywhere LMS, necesita lo siguiente:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para inicio de sesión único en Knowledge Anywhere LMS

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Knowledge Anywhere LMS admite el inicio de sesión único iniciado por **SP e IDP**.
* Knowledge Anywhere LMS admite el aprovisionamiento de usuarios **Just-In-Time**.

## <a name="adding-knowledge-anywhere-lms-from-the-gallery"></a>Adición de Knowledge Anywhere LMS desde la galería

Para configurar la integración de Knowledge Anywhere LMS en Azure AD, será preciso que lo agregue desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Knowledge Anywhere LMS desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Knowledge Anywhere LMS**, seleccione **Knowledge Anywhere LMS** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![Knowledge Anywhere LMS en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Knowledge Anywhere LMS con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Knowledge Anywhere LMS.

Para configurar y probar el inicio de sesión único de Azure AD con Knowledge Anywhere LMS, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único en Knowledge Anywhere LMS](#configure-knowledge-anywhere-lms-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Knowledge Anywhere LMS](#create-knowledge-anywhere-lms-test-user)**: para tener un homólogo de Britta Simon en Knowledge Anywhere LMS que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Knowledge Anywhere LMS, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Knowledge Anywhere LMS**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en el modo iniciado por **IDP** siga estos pasos:

    ![Información acerca del inicio de sesión único de dominio y direcciones URL de Knowledge Anywhere LMS](common/idp-intiated.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<CLIENTNAME>.knowledgeanywhere.com/`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<CLIENTNAME>.knowledgeanywhere.com/SSO/SAML/Response.aspx?<IDPNAME>`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL de respuesta y el identificador reales, que se explican más adelante en el tutorial.

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información acerca del inicio de sesión único de dominio y direcciones URL de Knowledge Anywhere LMS](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<CLIENTNAME>.knowledgeanywhere.com/`

    > [!NOTE]
    > El valor de la dirección URL de inicio de sesión no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico al cliente de Knowledge Anywhere LMS](https://knowany.zendesk.com/hc/en-us/articles/360000469034-SAML-2-0-Single-Sign-On-SSO-Set-Up-Guide) para obtener este valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

7. En la sección **Set up Knowledge Anywhere LMS** (Configurar Knowledge Anywhere LMS), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-knowledge-anywhere-lms-single-sign-on"></a>Configuración del inicio de sesión único en Knowledge Anywhere LMS

1. En otra ventana del explorador, abra el portal de administración de Knowledge Anywhere LMS.

2. Seleccione en la pestaña **Sitio**.

    ![Configuración de Knowledge Anywhere LMS](./media/knowledge-anywhere-lms-tutorial/configure1.png)

3. Seleccione en la pestaña **Configuración de SAML**.

    ![Configuración de Knowledge Anywhere LMS](./media/knowledge-anywhere-lms-tutorial/configure2.png)

4. Haga clic en **Agregar nuevo**.

    ![Configuración de Knowledge Anywhere LMS](./media/knowledge-anywhere-lms-tutorial/configure3.png)

5. En la página **Add/Update SAML Settings** (Agregar o actualizar la configuración de SAML), realice los pasos siguientes:

    ![Configuración de Knowledge Anywhere LMS](./media/knowledge-anywhere-lms-tutorial/configure4.png)

     a. Escriba el nombre de IDP según su organización. Por ejemplo, `Azure`.

    b. En el cuadro de texto **IDP Entity ID** (Identificador de entidad IDP), pegue el valor de **Identificador de Azure AD** que copió de Azure Portal.

    c. En el cuadro de texto **URL de IDP**, pegue la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    d. Abra en el Bloc de notas el archivo de certificado que descargó de Azure Portal, copie el contenido y, luego, péguelo en el cuadro de texto **Certificado**.

    e. En el cuadro de texto **Logout URL** (Dirección URL de cierre de sesión), pegue la **dirección URL de cierre de sesión** que ha copiado de Azure Portal.

    f. Seleccione **Sitio principal** en la lista desplegable para el **dominio**.

    g. Copie el valor **Id. de entidad de SP** y péguelo en el cuadro de texto **Identificador** de la sección **Configuración básica de SAML** en Azure Portal.

    h. Copie el valor **SP Response(ACS) URL** [URL de respuesta (ACS) de SP] y péguelo en el cuadro de texto **URL de respuesta** de la sección **Configuración básica de SAML** en Azure Portal.

    i. Haga clic en **Save**(Guardar).

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Knowledge Anywhere LMS.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **Knowledge Anywhere LMS**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Knowledge Anywhere LMS**.

    ![Vínculo a Knowledge Anywhere LMS en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-knowledge-anywhere-lms-test-user"></a>Creación de un usuario de prueba de Knowledge Anywhere LMS

En esta sección, se crea un usuario llamado a Britta Simon en Knowledge Anywhere LMS. Knowledge Anywhere LMS admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario deja de existir en Knowledge Anywhere LMS, se crea uno nuevo después de la autenticación.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Knowledge Anywhere LMS en el panel de acceso, debería iniciar sesión automáticamente en la versión de Knowledge Anywhere LMS para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)