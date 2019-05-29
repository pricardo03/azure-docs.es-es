---
title: 'Tutorial: Integración de Azure Active Directory con Salesforce Sandbox | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Salesforce Sandbox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2019
ms.author: jeedes
ms.openlocfilehash: b2940f3eee3112fe1c6d57cc92157c573ecad109
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65904269"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Tutorial: Integración de Azure Active Directory con Salesforce Sandbox

En este tutorial, aprenderá a integrar Salesforce Sandbox con Azure Active Directory (Azure AD).

Los espacios aislados ofrecen la capacidad de crear varias copias de su organización en entornos independientes para una variedad de propósitos, como desarrollo, pruebas y aprendizaje, sin poner en peligro los datos y las aplicaciones de la organización de producción de Salesforce.
Para más información, consulte el [resumen sobre Sandbox](https://help.salesforce.com/articleView?id=create_test_instance.htm&language=en_us&type=5).

La integración de Salesforce Sandbox con Azure AD proporciona las siguientes ventajas:

* Puede controlar quién tiene acceso a Salesforce Sandbox desde Azure AD.
* Puede habilitar a los usuarios para que inicien sesión automáticamente en Salesforce Sandbox (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Salesforce Sandbox, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una suscripción habilitada para el inicio de sesión único en Salesforce Sandbox

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Salesforce Sandbox admite el inicio de sesión único iniciado por **SP e IDP**
* Salesforce Sandbox admite el aprovisionamiento de usuarios **Just-In-Time**
* Salesforce Sandbox admite el aprovisionamiento [**Automatizado** de usuarios](salesforce-sandbox-provisioning-tutorial.md)

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Adición de Salesforce Sandbox desde la galería

Para configurar la integración de Salesforce Sandbox, deberá agregar Salesforce Sandbox desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Salesforce Sandbox desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Salesforce Sandbox**, seleccione **Salesforce Sandbox** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Salesforce Sandbox en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Salesforce Sandbox con un usuario de prueba llamado **Britta Simon**.

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Salesforce Sandbox para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Salesforce Sandbox.

Para configurar y probar el inicio de sesión único de Azure AD con Salesforce Sandbox, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único en Salesforce Sandbox](#configure-salesforce-sandbox-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Salesforce Sandbox](#create-salesforce-sandbox-test-user)** : el objetivo es tener un homólogo de Britta Simon en Salesforce Sandbox que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Salesforce Sandbox, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/) en la página de integración de la aplicación **Salesforce Sandbox**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si tiene el **archivo de metadatos del proveedor de servicios** y desea realizar la configuración en el modo iniciado por **IDP**, realice los pasos siguientes:

     a. Haga clic en **Cargar el archivo de metadatos**.

    ![Carga del archivo de metadatos](common/upload-metadata.png)

    b. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.

    ![Elección del archivo de metadatos](common/browse-upload-metadata.png)

    > [!NOTE]
    > Obtendrá el archivo de metadatos del proveedor de servicios en el portal de Salesforce Sandbox como se explica más adelante en el tutorial.

    c. Una vez se ha cargado correctamente el archivo de metadatos, el valor de **Dirección URL de respuesta** se rellenará automáticamente en el cuadro de texto **Dirección URL de respuesta**.

    ![imagen](common/both-replyurl.png)

    > [!Note]
    > Si el valor **URL de respuesta** no se rellena automáticamente, hágalo manualmente según sus necesidades.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar **XML de metadatos** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Configurar Salesforce Sandbox**, copie la dirección o direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-salesforce-sandbox-single-sign-on"></a>Configuración del inicio de sesión único en Salesforce Sandbox

1. Abra una nueva pestaña en el explorador e inicie sesión en su cuenta de administrador de Salesforce Sandbox.

2. Haga clic en **Setup** (Configuración) en el **icono de configuración** de la esquina superior derecha de la página.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure1.png)

3. Desplácese hacia abajo hasta **SETTINGS** (CONFIGURACIÓN) en el panel de navegación y haga clic en **Identity** (Identidad) para expandir la sección relacionada. A continuación, haga clic en **Configuración de inicio de sesión único**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

4. En la página **Configuración de inicio de sesión único**, haga clic en el botón **Editar**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure3.png)

5. Seleccione **SAML habilitado** y haga clic en **Guardar**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

6. Para establecer la configuración de inicio de sesión único de SAML, haga clic en **New from Metadata File** (Nuevo archivo de metadatos).

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

7. Haga clic en **Choose File** (Elegir archivo) para cargar el archivo XML de metadatos que ha descargado desde Azure Portal y haga clic en **Create** (Crear).

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/xmlchoose.png)

8. En la página **SAML Single Sign-On Settings** (Configuración de inicio de sesión único de SAML), los campos se rellenan automáticamente. Haga clic en Save (Guardar).

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/salesforcexml.png)

9. En la página **Single Sign-On Settings** (Configuración del inicio de sesión único), haga clic en el botón **Download Metadata** (Descargar metadatos) para descargar el archivo de metadatos del proveedor de servicio. Use este archivo en la sección **Configuración básica de SAML** en Azure Portal para configurar las direcciones URL necesarias, como se explicó anteriormente.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure4.png)

10. Si quiere configurar la aplicación en el modo iniciado por **SP**, estos son los requisitos previos:

     a. Debe tener un dominio comprobado.

    b. Se necesita configurar y habilitar su dominio en Salesforce Sandbox. Los pasos para hacerlo se explican más adelante en este tutorial.

    c. En Azure Portal, en la sección **Configuración básica de SAML**, haga clic en **Establecer direcciones URL adicionales** y realice el paso siguiente:
  
    ![Información de dominio y direcciones URL de inicio de sesión único de Salesforce Sandbox](common/both-signonurl.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba el valor con el siguiente patrón: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    > [!NOTE]
    > Este valor se debe copiar desde el portal de Salesforce Sandbox, una vez que se ha habilitado el dominio.

11. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos de federación** y luego guarde el archivo XML en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

12. Abra una nueva pestaña en el explorador e inicie sesión en su cuenta de administrador de Salesforce Sandbox.

13. Haga clic en **Setup** (Configuración) en el **icono de configuración** de la esquina superior derecha de la página.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure1.png)

14. Desplácese hacia abajo hasta **SETTINGS** (CONFIGURACIÓN) en el panel de navegación y haga clic en **Identity** (Identidad) para expandir la sección relacionada. A continuación, haga clic en **Configuración de inicio de sesión único**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

15. En la página **Configuración de inicio de sesión único**, haga clic en el botón **Editar**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure3.png)

16. Seleccione **SAML habilitado** y haga clic en **Guardar**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

17. Para establecer la configuración de inicio de sesión único de SAML, haga clic en **New from Metadata File** (Nuevo archivo de metadatos).

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

18. Haga clic en **Choose File** (Elegir archivo) para cargar el archivo XML de metadatos y haga clic en **Create** (Crear).

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/xmlchoose.png)

19. En la página **SAML Single Sign-On Settings** (Configuración de inicio de sesión único de SAML) los campos se rellenan automáticamente, escriba el nombre de la configuración (por ejemplo: *SPSSOWAAD_Test*) en el cuadro de texto **Name** (Nombre) y haga clic en Guardar.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

20. Para habilitar su dominio en Salesforce Sandbox, lleve a cabo los siguientes pasos:

    > [!NOTE]
    > Antes de habilitar el dominio debe crearlo en Salesforce Sandbox. Para más información, consulte [Defining Your Domain Name](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US) (Definición del nombre de dominio). Una vez creado el dominio, asegúrese de que está configurado correctamente.

21. En el panel de navegación izquierdo de Salesforce, haga clic en **Company Settings** (Configuración de la empresa) para expandir la sección relacionada y haga clic en **My Domain** (Mi dominio).

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-my-domain.png)

22. En la sección **Authentication Configuration** (Configuración de autenticación), haga clic en **Edit** (Editar).

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-edit-auth-config.png)

23. En la sección **Authentication Configuration** (Configuración de autenticación), seleccione como el valor de **Servicio de autenticación** el nombre de la Configuración de inicio de sesión único SAML que ha establecido durante la configuración de SSO en Salesforce Sandbox. Finalmente, haga clic en **Save** (Guardar).

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure2.png)

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Salesforce Sandbox.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **Salesforce Sandbox**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Salesforce Sandbox**.

    ![Vínculo de Salesforce Sandbox en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-salesforce-sandbox-test-user"></a>Creación de un usuario de prueba de Salesforce Sandbox

En esta sección, creará un usuario llamado a Britta Simon en Salesforce Sandbox. Salesforce Sandbox admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario ya no existe en Salesforce Sandbox, se crea uno nuevo cuando se intenta acceder a esta aplicación. El espacio aislado de Salesforce también admite el aprovisionamiento automático de usuarios. [Aquí](salesforce-sandbox-provisioning-tutorial.md) puede encontrar más detalles sobre cómo configurar el aprovisionamiento automático de usuarios.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Salesforce Sandbox en el panel de acceso, debería iniciar sesión automáticamente en la instancia de Salesforce Sandbox para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

* [Configuración del aprovisionamiento de usuarios](salesforce-sandbox-provisioning-tutorial.md)
