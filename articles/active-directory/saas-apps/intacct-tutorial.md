---
title: 'Tutorial: Integración de Azure Active Directory con Intacct | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e Intacct.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 31bc5622c6c6c3dd00bc59b5d8f3aa349055d125
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59279251"
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a>Tutorial: Integración de Azure Active Directory con Intacct

En este tutorial, obtendrá información sobre cómo integrar Intacct con Azure Active Directory (Azure AD).
Integrar Intacct con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Intacct.
* Puede permitir que los usuarios inicien sesión automáticamente en Intacct (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Intacct, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en Intacct

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Intacct admite SSO iniciado por **IDP**

## <a name="adding-intacct-from-the-gallery"></a>Incorporación de Intacct desde la Galería

Para configurar la integración de Intacct en Azure AD, será preciso agregar Intacct desde la Galería a la lista de aplicaciones SaaS administradas.

**Para agregar Intacct desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Intacct**, seleccione **Intacct** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Intacct en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Intacct con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Intacct.

Para configurar y probar el inicio de sesión único de Azure AD con Intacct, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único en Intacct](#configure-intacct-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Intacct](#create-intacct-test-user)**: para tener un homólogo de Britta Simon en Intacct que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Intacct, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Intacct**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Intacct](common/idp-reply.png)

    En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón:
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE]
    > Este valor no es real. Actualícelo con la dirección URL de respuesta real. Póngase en contacto con el [equipo de soporte técnico para clientes de Intacct](https://us.intacct.com/support) para obtener el valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up Intacct** (Configurar Intacct), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-intacct-single-sign-on"></a>Configuración del inicio de sesión único en Intacct

1. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de Intacct como administrador.

1. Haga clic en la pestaña **Compañía** y luego haga clic en**Información de la compañía**.

    ![Compañía](./media/intacct-tutorial/ic790037.png "Compañía")

1. Haga clic en la pestaña **Seguridad** y luego haga clic en **Editar**.

    ![Seguridad](./media/intacct-tutorial/ic790038.png "Seguridad")

1. En la sección **Inicio de sesión único (SSO)** , siga estos pasos:

    ![Inicio de sesión único](./media/intacct-tutorial/ic790039.png "inicio de sesión único")

     a. Seleccione **Habilitar inicio de sesión único**.

    b. En **Tipo de proveedor de identidades**, seleccione **SAML 2.0**.

    c. En el cuadro de texto **Issuer URL** (URL del emisor), pegue el valor de **Identificador de Azure AD**  que ha copiado de Azure Portal.

    d. En el cuadro de texto **Dirección URL de inicio de sesión**, pegue el valor de la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    e. Abra el certificado codificado en **base-64** en el Bloc de notas, copie el contenido del mismo en el Portapapeles y luego péguelo en el cuadro de texto **Certificado**.

    f. Haga clic en **Save**(Guardar).

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

En esta sección, concederá acceso a Britta Simon a Intacct para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** e **Intacct**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Intacct**.

    ![Vínculo de Intacct en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-intacct-test-user"></a>Creación de un usuario de prueba en Intacct

Para configurar los usuarios de Azure AD para que puedan iniciar sesión en Intacct, tienen que aprovisionarse en Intacct. Para Intacct, el aprovisionamiento es una tarea manual.

**Para aprovisionar cuentas de usuario, siga estos pasos:**

1. Inicie sesión en su inquilino de **Intacct** .

1. Haga clic en la pestaña **Compañía** y luego haga clic en **Usuarios**.

    ![Usuarios](./media/intacct-tutorial/ic790041.png "Usuarios")

1. Haga clic en la pestaña **Agregar**.

    ![Agregar](./media/intacct-tutorial/ic790042.png "Agregar")

1. En la sección **Información del usuario** , lleve a cabo estos pasos:

    ![Información del usuario](./media/intacct-tutorial/ic790043.png "Información del usuario")

     a. Escriba el **Id. de usuario**, **Apellidos**, **Nombre**, **Dirección de correo electrónico**, **Puesto** y **Teléfono** de una cuenta de Azure AD que quiera aprovisionar en la sección **Información del usuario**.

    b. Seleccione **Privilegios administrativos** de una cuenta de Azure AD que quiera aprovisionar.

    c. Haga clic en **Save**(Guardar). El titular de la cuenta de Azure AD recibe un mensaje de correo y sigue un vínculo para confirmar su cuenta antes de que se active.

> [!NOTE]
> Para aprovisionar cuentas de usuario de Azure AD, puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Intacct ofrecida por Intacct.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Intacct en el Panel de acceso y debería iniciar sesión automáticamente en la aplicación Intacct para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

