---
title: 'Tutorial: Integración de Azure Active Directory con Lucidchart | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Lucidchart.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1068d364-11f3-43b5-bd6d-26f00ecd5baa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/25/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e12b5984eb2fe4e54ffec7d6b4a2a6da202c70e4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67097803"
---
# <a name="tutorial-azure-active-directory-integration-with-lucidchart"></a>Tutorial: Integración de Azure Active Directory con Lucidchart

En este tutorial, aprenderá a integrar Lucidchart con Azure Active Directory (Azure AD).
La integración de Lucidchart con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Lucidchart.
* Puede permitir que los usuarios inicien sesión automáticamente en Lucidchart (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Lucidchart, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para inicio de sesión único en Lucidchart

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Lucidchart admite SSO iniciado por **SP**
* Lucidchart admite aprovisionamiento de usuarios **Just-In-Time**

## <a name="adding-lucidchart-from-the-gallery"></a>Adición de Lucidchart desde la galería

Para configurar la integración de Lucidchart en Azure AD, es preciso agregar Lucidchart desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Lucidchart desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Lucidchart**, seleccione **Lucidchart** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![Lucidchart en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Lucidchart con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Lucidchart.

Para configurar y probar el inicio de sesión único de Azure AD con Lucidchart, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único en Lucidchart](#configure-lucidchart-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba en Lucidchart](#create-lucidchart-test-user)** : el objetivo es tener un homólogo de Britta Simon en Lucidchart que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Lucidchart, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Lucidchart**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Lucidchart](common/sp-signonurl.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una URL como: `https://chart2.office.lucidchart.com/saml/sso/azure`

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Set up Lucidchart** (Configurar Lucidchart), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-lucidchart-single-sign-on"></a>Configuración del inicio de sesión único en Lucidchart

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía de Lucidchart.

2. En el menú de la parte superior, haga clic en **Equipo**.

    ![Equipo](./media/lucidchart-tutorial/ic791190.png "Equipo")

3. Haga clic en **Applications \> (Aplicaciones) Manage SAML** (Administrar SAML).

    ![Administrar SAML](./media/lucidchart-tutorial/ic791191.png "Administrar SAML")

4. En la página de diálogo **Configuración de la autenticación SAML** , realice los pasos siguientes:

    a. Seleccione **Habilitar autenticación SAML** y, después, haga clic en **Opcional**.

    ![Configuración de la autenticación SAML](./media/lucidchart-tutorial/ic791192.png "Configuración de la autenticación SAML")

    b. En el cuadro de texto **Dominio**, escriba el dominio y, después, haga clic en **Cambiar certificado**.

    ![Cambiar certificado](./media/lucidchart-tutorial/ic791193.png "Cambiar certificado")

    c. Abra el archivo de metadatos descargado, copie el contenido y luego péguelo en el cuadro de texto **Cargar metadatos** .

    ![Cargar metadatos](./media/lucidchart-tutorial/ic791194.png "Cargar metadatos")

    d. Seleccione **Automatically Add new users to the team** (Agregar nuevo usuario al equipo automáticamente) y, después, haga clic en **Save changes** (Guardar los cambios).

    ![Guardar cambios](./media/lucidchart-tutorial/ic791195.png "Guardar cambios")

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Lucidchart.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **Lucidchart**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Lucidchart**.

    ![Vínculo a Lucidchart en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-lucidchart-test-user"></a>Creación de un usuario de prueba en Lucidchart

No hay ningún elemento de acción para que configure el aprovisionamiento de usuarios para Lucidchart.  Cuando un usuario asignado intenta iniciar sesión en Lucidchart desde el Panel de acceso, Lucidchart comprueba si el usuario existe.  

Si no hay cuentas de usuario disponibles, Lucidchart crea una automáticamente.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Lucidchart del Panel de acceso, debería iniciar sesión automáticamente en la versión de Lucidchart para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
