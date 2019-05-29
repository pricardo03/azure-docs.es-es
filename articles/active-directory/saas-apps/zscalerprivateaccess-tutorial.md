---
title: 'Tutorial: Integración de Azure Active Directory con Zscaler Private Access (ZPA) | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Zscaler Private Access (ZPA).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: 3213667e95c1e5cb68a849d6031db9629e5b273b
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65895989"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a>Tutorial: Integración de Azure Active Directory con Zscaler Private Access (ZPA)

En este tutorial, aprenderá a integrar Zscaler Private Access (ZPA) con Azure Active Directory (Azure AD).
La integración de Zscaler Private Access (ZPA) con Azure AD ofrece las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Zscaler Private Access (ZPA).
* Puede permitir que los usuarios inicien sesión automáticamente en Zscaler Private Access (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Zscaler Private Access (ZPA), necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una suscripción habilitada para el inicio de sesión único de Zscaler Private Access (ZPA)

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Zscaler Private Access (ZPA) admite el inicio de sesión único iniciado por **SP**

## <a name="adding-zscaler-private-access-zpa-from-the-gallery"></a>Adición de Zscaler Private Access (ZPA) desde la galería

Para configurar la integración de Zscaler Private Access (ZPA) con Azure AD, debe agregar Zscaler Private Access (ZPA) desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Zscaler Private Access (ZPA) desde la galería, realice los siguientes pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Zscaler Private Access (ZPA)** , seleccione **Zscaler Private Access (ZPA)** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![Zscaler Private Access (ZPA) en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección configurará y probará el inicio de sesión único de Azure AD con Zscaler Private Access (ZPA) con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado en Zscaler Private Access (ZPA).

Para configurar y probar el inicio de sesión único de Azure AD con Zscaler Private Access (ZPA), es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único en Zscaler Private Access (ZPA)](#configure-zscaler-private-access-zpa-single-sign-on)** : para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba en Zscaler Private Access (ZPA)](#create-zscaler-private-access-zpa-test-user)** : para tener un homólogo de Britta Simon en Zscaler Private Access (ZPA) vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único en Azure AD con Zscaler Private Access (ZPA), siga estos pasos:

1. En la página de integración de la aplicación **Zscaler Private Access (ZPA)** de [Azure Portal](https://portal.azure.com/), seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Zscaler Private Access (ZPA)](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`

    b. En el cuadro de texto **Identificador (Id. de entidad)** , escriba una dirección URL: `https://samlsp.private.zscaler.com/auth/metadata`

    > [!NOTE]
    > El valor de la **dirección URL de inicio de sesión** no es real. Actualice el valor con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico para clientes de Zscaler Private Access (ZPA)](https://help.zscaler.com/zpa-submit-ticket) para obtener este valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Set up Zscaler Private Access (ZPA)** (Configurar Zscaler Private Access [ZPA]), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-zscaler-private-access-zpa-single-sign-on"></a>Configuración del inicio de sesión único en Zscaler Private Access (ZPA)

1. En otra ventana del explorador web, inicie sesión en el sitio de la compañía Zscaler Private Access (ZPA) como administrador.

2. Vaya a **Administrator** (Administrador) y haga clic en **Idp Configuration** (Configuración de Idp).

    ![Configuración del inicio de sesión único en la aplicación](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

3. En la sección **Idp Configuration** (Configuración de IDP), haga clic en **Add New IDP Configuration** (Agregar nueva configuración de IDP).

    ![Configuración del inicio de sesión único en la aplicación](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

4. En la sección **New IDP Configuration** (Nueva configuración de IDP), realice los siguientes pasos:

    ![Configuración del inicio de sesión único en la aplicación](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

     a. Haga clic en **Select File** (Seleccionar archivo) y cargue su archivo de metadatos descargado.

    b. Haga clic en el botón **Guardar** .

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

En esta sección permitirá que Britta Simon use el inicio de sesión único de Azure concediéndole acceso a Zscaler Private Access (ZPA).

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Zscaler Private Access (ZPA)** .

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Zscaler Private Access (ZPA)** .

    ![Vínculo a Zscaler Private Access (ZPA) en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-zscaler-private-access-zpa-test-user"></a>Creación de un usuario de prueba en Zscaler Private Access (ZPA)

En esta sección, creará un usuario llamado Britta Simon en Zscaler Private Access (ZPA). Trabaje con el [equipo de soporte técnico de Zscaler Private Access (ZPA)](https://help.zscaler.com/zpa-submit-ticket) para agregar los usuarios a la plataforma de Zscaler Private Access (ZPA).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Zscaler Private Access (ZPA) en el panel de acceso, debería iniciar sesión automáticamente en la versión de Zscaler Private Access (ZPA) para la que configurara el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

