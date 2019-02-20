---
title: 'Tutorial: Integración de Azure Active Directory con Adaptive Insights | Microsoft Docs'
description: Obtenga más información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Adaptive Insights.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e334795a63ff298ff8f0695340f35fdff1ad03e0
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56166528"
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-insights"></a>Tutorial: integración de Azure Active Directory con Adaptive Insights

En este tutorial obtendrá más información sobre cómo integrar Adaptive Insights con Azure Active Directory (Azure AD).

La integración de Adaptive Insights con Azure AD proporciona las siguientes ventajas:

- Puede controlar quién tiene acceso a Adaptive Insights en Azure AD.
- Puede permitir que los usuarios inicien sesión automáticamente en Adaptive Insights (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Adaptive Insights, se necesita lo siguiente:

- Una suscripción de Azure AD
- Una suscripción que permita el inicio de sesión único en Adaptive Insights

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar Adaptive Insights desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-adaptive-insights-from-the-gallery"></a>Agregar Adaptive Insights desde la galería
Para configurar la integración de Adaptive Insights con Azure AD, deberá agregar Adaptive Insights desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Adaptive Insights desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/adaptivesuite-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/adaptivesuite-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/adaptivesuite-tutorial/a_new_app.png)

4. Para agregar la aplicación, escriba **Adaptive Insights** en el cuadro de búsqueda, seleccione **Adaptive Insights** en el panel de resultados y, luego, haga clic en el botón **Agregar**.

     ![imagen](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Adaptive Insights con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Adaptive Insights para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario asociado en Adaptive Insights.

Para configurar y probar el inicio de sesión único de Azure AD con Adaptive Insights, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Adaptive Insights](#create-an-adaptive-insights-test-user)**: para tener un homólogo de Britta Simon en Adaptive Insights vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Adaptive Insights.

**Para configurar el inicio de sesión único de Azure AD con Adaptive Insights, siga los pasos siguientes:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Adaptive Insights**, seleccione **Inicio de sesión único**.

    ![imagen](./media/adaptivesuite-tutorial/B1_B2_Select_SSO.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML** para habilitar el inicio de sesión único.

    ![imagen](./media/adaptivesuite-tutorial/b1_b2_saml_sso.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/adaptivesuite-tutorial/b1-domains_and_urlsedit.png)

4. En la sección **Configuración de SAML básica**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por **IDP**:

    ![imagen](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

     a. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`.

    >[!NOTE]
    > Puede obtener el identificador (id. de entidad) y los valores de URL en la página **Configuración de SSO de SAML** de Adaptive Insights.
 
5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** y guárdelo en su equipo.

    ![imagen](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_certficate.png) 

6. En la sección **Configurar Adaptive Insights**, copie la dirección URL adecuada según sus necesidades.

    Tenga en cuenta que en la dirección URL puede poner lo siguiente:

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![imagen](./media/adaptivesuite-tutorial/d1_samlsonfigure.png) 

7. En otra ventana del explorador web, inicie sesión en el sitio de Adaptive Insights de la compañía como administrador.

8. Vaya a **Administración**.

    ![Administración](./media/adaptivesuite-tutorial/IC805644.png "Administración")

9. En la sección **Users and Roles** (Usuarios y roles), haga clic en **Manage SAML SSO Settings** (Administrar configuración de SSO de SAML).

    ![Manage SAML SSO Settings (Administrar configuración de SSO de SAML)](./media/adaptivesuite-tutorial/IC805645.png "Manage SAML SSO Settings (Administrar configuración de SSO de SAML)")

10. En la página **SAML SSO Settings** (Configuración de SSO de SAML), realice los pasos siguientes:

    ![SAML SSO Settings (Configuración de SSO de) SAML](./media/adaptivesuite-tutorial/IC805646.png "SAML SSO Settings (Configuración de SSO de) SAML")

     a. En el cuadro de texto **Identity provider name** (Nombre del proveedor de identidades), escriba el nombre de la configuración.

    b. Pegue el valor del **Azure Ad Identifier** (Identificador de Azure AD) que ha copiado de Azure Portal en el cuadro de texto **Identity Provider Entity ID** (Id. de entidad del proveedor de identidades).

    c. Pegue el valor de **Dirección URL de inicio de sesión** que copió de Azure Portal en el cuadro de texto **Identity provider SSO URL** (URL de SSO de proveedor de identidades).

    d. Pegue el valor de **URL de cierre de sesión** que copió de Azure Portal en el cuadro de texto **Custom logout URL** (Dirección URL de cierre de sesión personalizada).

    e. Para cargar el certificado descargado, haga clic en **Choose file**(Elegir archivo).

    f. Seleccione las opciones siguientes en cada caso:

    * En **SAML user id** (identificador de usuario de SAML), seleccione **User’s Adaptive Insights user name** (Nombre de usuario de Adaptive Insights del usuario).

    * En **SAML user id location** (Ubicación del id. de usuario de SAML), seleccione **User id in NameID of Subject** (identificador de usuario en NameID del tema).

    * En **SAML NameID format** (Formato de NameID de SAML), seleccione **Email address** (Dirección de correo electrónico).

    * En **Enable SAML** (Habilitar SAML), seleccione **Allow SAML SSO and direct Adaptive Insights login** (Permitir inicio de sesión único de SAML e inicio de sesión directo de Adaptive Insights).

    g. Copie la **URL de SSO de Adaptive Insights** y péguela en los cuadros de texto **identificador (id. de entidad)** y **URL de respuesta** en la sección **Dominio y direcciones URL de Adaptive Insights** de Azure Portal.

    h. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![imagen](./media/adaptivesuite-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/adaptivesuite-tutorial/d_adduser.png)

3. En las propiedades Usuario, siga estos pasos.

    ![imagen](./media/adaptivesuite-tutorial/d_userproperties.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-an-adaptive-insights-test-user"></a>Creación de un usuario de prueba de Adaptive Insights

Para permitir que los usuarios de Azure AD inicien sesión en Adaptive Insights, deben aprovisionarse en este último. En el caso de Adaptive Insights, el aprovisionamiento es una tarea manual.

**Siga estos pasos para configurar el aprovisionamiento de usuario:** 

1. Inicie sesión en el sitio de la compañía de **Adaptive Insights** como administrador.
2. Vaya a **Administración**.

   ![Administración](./media/adaptivesuite-tutorial/IC805644.png "Administración")

3. En la sección **Users and Roles** (Usuarios y roles), haga clic en **Add User** (Agregar usuario).

   ![Agregar usuario](./media/adaptivesuite-tutorial/IC805648.png "Agregar usuario")
   
4. En la sección **Nuevo usuario** , lleve a cabo estos pasos:

   ![Enviar](./media/adaptivesuite-tutorial/IC805649.png "Enviar")

    a. Escriba el **nombre**, el **inicio de sesión**, la **dirección de correo electrónico** y la **contraseña** o de un usuario válido de Azure Active Directory que quiera aprovisionar en los cuadros de texto relacionados.

   b. Seleccione un **Role**(rol).

   c. Haga clic en **Enviar**.

>[!NOTE]
>Para aprovisionar cuentas de usuario de AAD, puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Adaptive Insights ofrecida por Adaptive Insights.
>

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Adaptive Insights a Britta Simon para permitirle usar el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![imagen](./media/adaptivesuite-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **Adaptive Insights**.

    ![imagen](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_app.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/adaptivesuite-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![imagen](./media/adaptivesuite-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Adaptive Insights del Panel de acceso, debería iniciar sesión automáticamente en su aplicación Adaptive Insights.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
