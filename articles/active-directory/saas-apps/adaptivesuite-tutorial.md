---
title: 'Tutorial: Integración de Azure Active Directory con Adaptive Insights | Microsoft Docs'
description: Obtenga más información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Adaptive Insights.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2018
ms.author: jeedes
ms.openlocfilehash: 307c3cf258a74d1ddfb409f0d5b22d9e1fd6bf4b
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36213828"
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-insights"></a>Tutorial: Integración de Azure Active Directory con Adaptive Insights

En este tutorial obtendrá más información sobre cómo integrar Adaptive Insights con Azure Active Directory (Azure AD).

La integración de Adaptive Insights con Azure AD proporciona las siguientes ventajas:

- Puede controlar quién tiene acceso a Adaptive Insights en Azure AD.
- Puede permitir que los usuarios inicien sesión automáticamente en Adaptive Insights (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>requisitos previos

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

    ![Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

4. Para agregar la aplicación, escriba **Adaptive Insights** en el cuadro de búsqueda, seleccione **Adaptive Insights** en el panel de resultados y, luego, haga clic en el botón **Agregar**.

    ![Creación de un usuario de prueba de Azure AD](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Adaptive Insights con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Adaptive Insights para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario asociado en Adaptive Insights.

Para configurar y probar el inicio de sesión único de Azure AD con Adaptive Insights, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Adaptive Insights](#creating-an-adaptive-insights-test-user)**: para tener un homólogo de Britta Simon en Adaptive Insights vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Adaptive Insights.

**Para configurar el inicio de sesión único de Azure AD con Adaptive Insights, siga los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Adaptive Insights**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_samlbase.png)

3. En la sección **Dominio y direcciones URL de Adaptive Insights**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    a. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`.

    >[!NOTE]
    > Puede obtener el identificador (id. de entidad) y los valores de URL en la página **Configuración de SSO de SAML** de Adaptive Insights.

4. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_certificate.png)

5. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/adaptivesuite-tutorial/tutorial_general_400.png)

6. En la sección **Configuración de Adaptive Insights**, haga clic en **Configurar Adaptive Insights** para abrir la ventana **Configurar inicio de sesión**. Copie los valores de **SAML Entity ID y SAML Single Sign-On Service URL** (Identificador de entidad de SAML y Dirección URL del servicio de inicio de sesión único de SAML) de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_configure.png) 

7. En otra ventana del explorador web, inicie sesión en el sitio de Adaptive Insights de la compañía como administrador.

8. Vaya a **Administración**.

    ![Administración](./media/adaptivesuite-tutorial/IC805644.png "Administración")

9. En la sección **Users and Roles** (Usuarios y roles), haga clic en **Manage SAML SSO Settings** (Administrar configuración de SSO de SAML).

    ![Manage SAML SSO Settings (Administrar configuración de SSO de SAML)](./media/adaptivesuite-tutorial/IC805645.png "Manage SAML SSO Settings (Administrar configuración de SSO de SAML)")

10. En la página **SAML SSO Settings** (Configuración de SSO de SAML), realice los pasos siguientes:

    ![SAML SSO Settings (Configuración de SSO de) SAML](./media/adaptivesuite-tutorial/IC805646.png "SAML SSO Settings (Configuración de SSO de) SAML")

    a. En el cuadro de texto **Identity provider name** (Nombre del proveedor de identidades), escriba el nombre de la configuración.

    b. Pegue el valor del **SAML Entity ID** (Identificador de entidad de SAML) que ha copiado de Azure Portal en el cuadro de texto **Identity Provider Entity ID** (Id. de entidad del proveedor de identidades).

    c. Pegue el valor de **SAML Single Sign-On Service URL** (Dirección URL del servicio de inicio de sesión único de SAML) que copió de Azure Portal en el cuadro de texto **Identity provider SSO URL** (Dirección URL de SSO del proveedor de identidades).

    d. Pegue el valor de **SAML Single Sign-On Service URL** (Dirección URL del servicio de inicio de sesión único de SAML) que copió de Azure Portal en el cuadro de texto **Custom logout URL** (Dirección URL de cierre de sesión personalizada).

    e. Para cargar el certificado descargado, haga clic en **Choose file**(Elegir archivo).

    f. Seleccione las opciones siguientes en cada caso:

    * En **SAML user id** (identificador de usuario de SAML), seleccione **User’s Adaptive Insights user name** (Nombre de usuario de Adaptive Insights del usuario).

    * En **SAML user id location** (Ubicación del id. de usuario de SAML), seleccione **User id in NameID of Subject** (identificador de usuario en NameID del tema).

    * En **SAML NameID format** (Formato de NameID de SAML), seleccione **Email address** (Dirección de correo electrónico).

    * En **Enable SAML** (Habilitar SAML), seleccione **Allow SAML SSO and direct Adaptive Insights login** (Permitir inicio de sesión único de SAML e inicio de sesión directo de Adaptive Insights).

    g. Copie la **URL de SSO de Adaptive Insights** y péguela en los cuadros de texto **identificador (id. de entidad)** y **URL de respuesta** en la sección **Dominio y direcciones URL de Adaptive Insights** de Azure Portal.

    h. Haga clic en **Save**(Guardar).

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/adaptivesuite-tutorial/create_aaduser_01.png) 

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.

    ![Creación de un usuario de prueba de Azure AD](./media/adaptivesuite-tutorial/create_aaduser_02.png) 

3. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.

    ![Creación de un usuario de prueba de Azure AD](./media/adaptivesuite-tutorial/create_aaduser_03.png) 

4. En la página de diálogo **Usuario**, realice los siguientes pasos:

    ![Creación de un usuario de prueba de Azure AD](./media/adaptivesuite-tutorial/create_aaduser_04.png)

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="creating-an-adaptive-insights-test-user"></a>Creación de un usuario de prueba de Adaptive Insights

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

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Adaptive Insights a Britta Simon para permitirle usar el inicio de sesión único de Azure.

![Asignar usuario][200]

**Para asignar a Britta Simon a Adaptive Insights, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Adaptive Insights**.

    ![Configurar inicio de sesión único](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_app.png)

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

El objetivo de esta sección es probar la configuración del inicio de sesión único de Microsoft Azure AD mediante el panel de acceso.

Al hacer clic en el icono de Adaptive Insights del Panel de acceso, debería iniciar sesión automáticamente en su aplicación Adaptive Insights.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/adaptivesuite-tutorial/tutorial_general_01.png
[2]: ./media/adaptivesuite-tutorial/tutorial_general_02.png
[3]: ./media/adaptivesuite-tutorial/tutorial_general_03.png
[4]: ./media/adaptivesuite-tutorial/tutorial_general_04.png

[100]: ./media/adaptivesuite-tutorial/tutorial_general_100.png

[200]: ./media/adaptivesuite-tutorial/tutorial_general_200.png
[201]: ./media/adaptivesuite-tutorial/tutorial_general_201.png
[202]: ./media/adaptivesuite-tutorial/tutorial_general_202.png
[203]: ./media/adaptivesuite-tutorial/tutorial_general_203.png
