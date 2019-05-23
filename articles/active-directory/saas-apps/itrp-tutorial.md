---
title: 'Tutorial: Integración de Azure Active Directory con ITRP | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e ITRP.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e09716a3-4200-4853-9414-2390e6c10d98
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: 343439bbf47914404c4598369926f2f02cf5c9af
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65898125"
---
# <a name="tutorial-azure-active-directory-integration-with-itrp"></a>Tutorial: Integración de Azure Active Directory con ITRP

En este tutorial se aprende a integrar ITRP con Azure Active Directory (Azure AD).
La integración de ITRP con Azure AD le ofrece las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a ITRP.
* Puede permitir que los usuarios inicien sesión automáticamente en ITRP (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con ITRP, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una suscripción habilitada para el inicio de sesión único en ITRP

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* ITRP admite el inicio de sesión único iniciado por **SP**

## <a name="adding-itrp-from-the-gallery"></a>Incorporación de ITRP desde la galería

Para configurar la integración de ITRP en Azure AD, tiene que agregar ITRP desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar ITRP desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **ITRP**, seleccione **ITRP** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![ITRP en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección se configura y prueba el inicio de sesión único de Azure AD con ITRP con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de ITRP.

Para configurar y probar el inicio de sesión único de Azure AD con ITRP, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de ITRP](#configure-itrp-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de ITRP](#create-itrp-test-user)**: para tener un homólogo de Britta Simon en ITRP vinculado a la representación de usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con ITRP, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **ITRP**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de ITRP](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<tenant-name>.itrp.com`

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://<tenant-name>.itrp.com`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Contacte con el [equipo de soporte técnico de cliente de ITRP](https://www.itrp.com/support) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la sección **Certificado de firma de SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Certificado de firma de SAML**.

    ![Edición del certificado de firma de SAML](common/edit-certificate.png)

6. En la sección **Certificado de firma de SAML**, copie el valor de **Huella digital** en el equipo.

    ![Copia del valor de la huella digital](common/copy-thumbprint.png)

7. En la sección **Set up ITRP** (Configurar ITRP), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-itrp-single-sign-on"></a>Configuración del inicio de sesión único de ITRP

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía de ITRP.

1. En la barra de herramientas de la parte superior, haga clic en el icono de **Configuración**.

    ![ITRP](./media/itrp-tutorial/ic775570.png "ITRP")

1. En el panel de navegación izquierdo, haga clic en **Inicio de sesión único**.

    ![Inicio de sesión único](./media/itrp-tutorial/ic775571.png "Inicio de sesión único")

1. Siga estos pasos en la sección de configuración de Single Sign-On (Inicio de sesión único):

    ![Inicio de sesión único](./media/itrp-tutorial/ic775572.png "Inicio de sesión único")

    ![Inicio de sesión único](./media/itrp-tutorial/ic775573.png "Inicio de sesión único")

     a. Haga clic en **Habilitado**.

    b. En el cuadro de texto  **Remote Log Out URL** (URL de cierre de sesión remoto), pegue el valor de **Dirección URL de cierre de sesión** que copió de Azure Portal.

    c. En el cuadro de texto  **Dirección URL de inicio de sesión único de SAML**, pegue el valor de la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    En el cuadro de texto **Certificate Fingerprint** (Huella digital de certificado), pegue el valor de **Huella digital** del certificado que haya copiado de Azure Portal.

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
  
    b. En el campo **Nombre de usuario**, escriba `brittasimon@yourcompanydomain.extension`. Por ejemplo, BrittaSimon@contoso.com.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a ITRP.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **ITRP**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **ITRP**.

    ![Vínculo a ITRP en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-itrp-test-user"></a>Creación de un usuario de prueba de ITRP

Para permitir que los usuarios de Azure AD inicien sesión en ITRP, tienen que aprovisionarse en ITRP. En el caso de ITRP, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en su inquilino de **ITRP**.

1. En la barra de herramientas de la parte superior, haga clic en el icono de **Registros**.

    ![Administración](./media/itrp-tutorial/ic775575.png "Administración")

1. En el menú emergente, seleccione **Contactos**.

    ![Personas](./media/itrp-tutorial/ic775587.png "Personas")

1. Haga clic en **Agregar nueva persona** (“+”).

    ![Administración](./media/itrp-tutorial/ic775576.png "Administración")

1. En el cuadro de diálogo Add New Person (Agregar nueva persona), realice los pasos siguientes:

    ![Usuario](./media/itrp-tutorial/ic775577.png "Usuario")

     a. Escriba el **nombre** y el **correo electrónico** de la cuenta válida de AAD que desee aprovisionar.

    b. Haga clic en **Save**(Guardar).

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de ITRP que proporcione ITRP para aprovisionar cuentas de usuario de AAD.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de ITRP en el panel de acceso y debería iniciar sesión automáticamente en la versión de ITRP para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
