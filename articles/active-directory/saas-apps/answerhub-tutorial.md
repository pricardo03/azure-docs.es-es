---
title: 'Tutorial: Integración de Azure Active Directory con AnswerHub | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y AnswerHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: df88eee14ea8f3d7d7f8365a77750da67904793d
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56188059"
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Tutorial: Integración de Azure Active Directory con AnswerHub

En este tutorial, obtendrá información sobre cómo integrar AnswerHub con Azure Active Directory (Azure AD).
La integración de AnswerHub con Azure AD proporciona las siguientes ventajas:

* En Azure AD puede controlar quién tiene acceso a AnswerHub.
* Puede permitir que los usuarios inicien sesión automáticamente en AnswerHub (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con AnswerHub, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción habilitada para el inicio de sesión único en AnswerHub

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* AnswerHub admite el inicio de sesión único iniciado por **SP**.

## <a name="adding-answerhub-from-the-gallery"></a>Incorporación de AnswerHub desde la galería

Para configurar la integración de AnswerHub en Azure AD, será preciso que agregue AnswerHub desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar AnswerHub desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **AnswerHub**, seleccione **AnswerHub** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![AnswerHub en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con AnswerHub con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de AnswerHub.

Para configurar y probar el inicio de sesión único de Azure AD con AnswerHub, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de AnswerHub](#configure-answerhub-single-sign-on)**: para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de AnswerHub](#create-answerhub-test-user)**: para tener un homólogo de Britta Simon en AnswerHub que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con AnswerHub, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **AnswerHub**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de AnswerHub](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<company>.answerhub.com`

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://<company>.answerhub.com`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte de cliente de AnswerHub](mailto:success@answerhub.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up AnswerHub** (Configurar AnswerHub), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-answerhub-single-sign-on"></a>Configuración del inicio de sesión único de AnswerHub

1. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de AnswerHub como administrador.

    > [!NOTE]
    > Si necesita ayuda para configurar AnswerHub, póngase en contacto con el [equipo de soporte técnico de AnswerHub](mailto:success@answerhub.com.).

2. Vaya a **Administración**.

3. Haga clic en la pestaña **Usuario y grupo** .

4. En el panel de navegación izquierdo, en la sección **Social Settings** (Configuración social), haga clic en **SAML Setup** (Configuración de SAML).

5. Haga clic en la pestaña **Configuración de IDP** .

6. En la pestaña **Configuración de IDP** , lleve a cabo estos pasos:

    ![Configuración de SAML](./media/answerhub-tutorial/ic785172.png "Configuración de SAML")  
  
     a. En el cuadro de texto **IdP Login URL** (Dirección URL de inicio de sesión de IdP), pegue la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.
  
    b. En el cuadro de texto **IdP Logout URL** (Dirección URL de cierre de sesión de IdP), pegue la **dirección URL de cierre de sesión** que ha copiado de Azure Portal.

    c. En el cuadro de texto **IDP Name Identifier Format** (Formato del identificador de nombre de IDP), escriba el valor de identificador de usuario igual que el que se seleccionó en Azure Portal en la sección **Atributos de usuario**.
  
    d. Haga clic en **Claves y certificados**.

7. En la pestaña **Claves y certificados**, realice los pasos siguientes:

    ![Claves y certificados](./media/answerhub-tutorial/ic785173.png "Claves y certificados")  

     a. Abra el certificado codificado en Base 64 que descargó de Azure Portal en el Bloc de notas, copie el contenido en el Portapapeles y, luego, péguelo en el cuadro de texto **IDP Public Key (x509 Format)** (Clave pública de IDP [formato x509]).
  
    b. Haga clic en **Save**(Guardar).

8. En la pestaña **IDP Config** (Configuración de IDP), haga clic en **Save** (Guardar).

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a AnswerHub.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **AnswerHub**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **AnswerHub**.

    ![El vínculo de AnswerHub en la lista Aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-answerhub-test-user"></a>Creación de un usuario de prueba de AnswerHub

Para permitir que los usuarios de Azure AD inicien sesión en AnswerHub, tienen que aprovisionarse en AnswerHub. En el caso de AnswerHub, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en el sitio de la compañía de **AnswerHub** como administrador.

2. Vaya a **Administración**.

3. Haga clic en la pestaña **Users & Groups** (Usuarios y grupos).

4. En el panel de navegación izquierdo, en la sección **Manage Users** (Administrar usuarios), haga clic en **Create or import users** (Crear o importar usuarios) y, a continuación, haga clic en **Users & Groups** (Usuarios y grupos).

   ![Usuarios y grupos](./media/answerhub-tutorial/ic785175.png "Usuarios y grupos")

5. Escriba la **dirección de correo electrónico**, el **nombre de usuario** y la **contraseña** de una cuenta de Azure Active Directory válida que desee aprovisionar en los cuadros de texto relacionados y haga clic en **Save** (Guardar).

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de AnswerHub ofrecida por AnswerHub para aprovisionar cuentas de usuario de AAD.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de AnswerHub en el panel de acceso, debería iniciar sesión automáticamente en la versión de AnswerHub para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

