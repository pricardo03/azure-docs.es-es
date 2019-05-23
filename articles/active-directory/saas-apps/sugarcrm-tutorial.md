---
title: 'Tutorial: Integración de Azure Active Directory con Sugar CRM | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Sugar CRM.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3331b9fc-ebc0-4a3a-9f7b-bf20ee35d180
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/22/2019
ms.author: jeedes
ms.openlocfilehash: 2dea1dcd2f6ecef580d65a95d1227380901213eb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "65866439"
---
# <a name="tutorial-azure-active-directory-integration-with-sugar-crm"></a>Tutorial: Integración de Azure Active Directory con Sugar CRM

En este tutorial, aprenderá cómo integrar Sugar CRM con Azure Active Directory (Azure AD).
La integración de Sugar CRM con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Sugar CRM.
* Puede permitir que los usuarios inicien sesión automáticamente en Sugar CRM (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Sugar CRM, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Un suscripción habilitada para el inicio de sesión único en Sugar CRM

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Sugar CRM admite el SSO iniciado por **SP**

## <a name="adding-sugar-crm-from-the-gallery"></a>Agregación de Sugar CRM desde la galería

Para configurar la integración de Sugar CRM en Azure AD, deberá agregar Sugar CRM desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Sugar CRM desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Sugar CRM**, seleccione **Sugar CRM** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![Sugar CRM en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Sugar CRM con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Sugar CRM.

Para configurar y probar el inicio de sesión único de Azure AD con Sugar CRM, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Sugar CRM](#configure-sugar-crm-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Sugar CRM](#create-sugar-crm-test-user)**: para tener un homólogo de Britta Simon en Sugar CRM que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Sugar CRM, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Sugar CRM**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Sugar CRM](common/sp-signonurl.png)

    En el cuadro de texto **Dirección URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón:
    
    | |
    |--|
    | `https://<companyname>.sugarondemand.com`|
    | `https://<companyname>.trial.sugarcrm`|

    > [!NOTE]
    > Este valor no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de Sugar CRM](https://support.sugarcrm.com/) para obtener este valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up Sugar CRM** (Configurar Sugar CRM), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-sugar-crm-single-sign-on"></a>Configuración del inicio de sesión único en Sugar CRM

1. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de Sugar CRM como administrador.

1. Vaya a **Administración**.

    ![Administración](./media/sugarcrm-tutorial/ic795888.png "Administración")

1. En la sección **Administración**, haga clic en **Administración de contraseñas**.

    ![Administración](./media/sugarcrm-tutorial/ic795889.png "Administración")

1. Seleccione **Habilitar autenticación SAML**.

    ![Administración](./media/sugarcrm-tutorial/ic795890.png "Administración")

1. En la sección **Autenticación SAML** , realice los pasos siguientes:

    ![Autenticación SAML](./media/sugarcrm-tutorial/ic795891.png "Autenticación SAML")  

     a. En el cuadro de texto **Login URL** (Dirección URL de inicio de sesión), pegue el valor de la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.
  
    b. En el cuadro de texto **SLO URL** (Dirección URL de SLO), pegue el valor de **Dirección URL de cierre de sesión** que ha copiado de Azure Portal.
  
    c. Abra el certificado codificado en base 64 en el Bloc de notas, copie su contenido en el Portapapeles y luego pegue todo el certificado en el cuadro de texto **Certificado X.509** .
  
    d. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba `brittasimon@yourcompanydomain.extension`.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Sugar CRM.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **Sugar CRM**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Sugar CRM**.

    ![Vínculo Sugar CRM en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-sugar-crm-test-user"></a>Creación de un usuario de prueba en Sugar CRM

Para permitir que los usuarios de Azure AD inicien sesión en Sugar CRM, deben aprovisionarse en Sugar CRM. En el caso de SugarCRM, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en su sitio de compañía de **Sugar CRM** como administrador.

1. Vaya a **Administración**.

    ![Administración](./media/sugarcrm-tutorial/ic795888.png "Administración")

1. En la sección **Administración**, haga clic en **Administración de usuarios**.

    ![Administración](./media/sugarcrm-tutorial/ic795893.png "Administración")

1. Vaya a **Usuarios \> Crear nuevo usuario**.

    ![Creación de nuevos usuarios](./media/sugarcrm-tutorial/ic795894.png "Creación de nuevos usuarios")

1. En la pestaña **Perfil de usuario** , realice los siguientes pasos:

    ![Nuevo usuario](./media/sugarcrm-tutorial/ic795895.png "nuevo usuario")

    * Escriba el **nombre de usuario**, **apellidos** y la **dirección de correo electrónico** de un usuario de Azure Active Directory válido en los cuadros de texto relacionados.
  
1. Como **Estado**, seleccione **Activo**.

1. En la pestaña Contraseña, realice los pasos siguientes:

    ![Nuevo usuario](./media/sugarcrm-tutorial/ic795896.png "nuevo usuario")

     a. Escriba la contraseña en el cuadro de texto relacionado.

    b. Haga clic en **Save**(Guardar).

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de SugarCRM ofrecida por SugarCRM para aprovisionar cuentas de usuario de AAD.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Sugar CRM en el panel de acceso debería iniciar sesión automáticamente en la versión de Sugar CRM para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

