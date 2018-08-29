---
title: 'Tutorial: Integración de Azure Active Directory con Salesforce Sandbox | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Salesforce Sandbox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2018
ms.author: jeedes
ms.openlocfilehash: 6feafba41cf65a752dd5bf0819b0b93bacff0aff
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "42144527"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Tutorial: Integración de Azure Active Directory con Salesforce Sandbox

En este tutorial, aprenderá a integrar Salesforce Sandbox con Azure Active Directory (Azure AD).

Los espacios aislados ofrecen la capacidad de crear varias copias de su organización en entornos independientes para una variedad de propósitos, como desarrollo, pruebas y aprendizaje, sin poner en peligro los datos y las aplicaciones de la organización de producción de Salesforce.
Para más información, consulte el [resumen sobre Sandbox](https://help.salesforce.com/articleView?id=create_test_instance.htm&language=en_us&type=5).

La integración de Salesforce Sandbox con Azure AD proporciona las siguientes ventajas:

- Puede controlar quién tiene acceso a Salesforce Sandbox desde Azure AD.
- Puede permitir que los usuarios inicien sesión automáticamente en Salesforce Sandbox (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Salesforce Sandbox, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Salesforce Sandbox

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Salesforce Sandbox desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Adición de Salesforce Sandbox desde la galería

Para configurar la integración de Salesforce Sandbox, deberá agregar Salesforce Sandbox desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Salesforce Sandbox desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Salesforce Sandbox**, seleccione **Salesforce Sandbox** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Salesforce Sandbox en la lista de resultados](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Salesforce Sandbox con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Salesforce Sandbox para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Salesforce Sandbox.

Para establecer la relación de vínculo, asigne el valor del **nombre de usuario** en Azure AD como el valor del **nombre de usuario** en Salesforce Sandbox.

Para configurar y probar el inicio de sesión único de Azure AD con Salesforce Sandbox, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Salesforce Sandbox](#create-a-salesforce-sandbox-test-user)**: el objetivo es tener un homólogo de Britta Simon en Salesforce Sandbox que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Salesforce Sandbox.

**Para configurar el inicio de sesión único de Azure AD con Salesforce Sandbox, realice los pasos siguientes:**

1. En la página de integración de la aplicación **Salesforce Sandbox** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. En la sección **Dominio y direcciones URL de Salesforce Sandbox**, realice los siguientes pasos si quiere configurar la aplicación en el modo iniciado por **IDP**:

   ![Información de dominio y direcciones URL de inicio de sesión único de Salesforce Sandbox](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url1.png)

   En el cuadro de texto **URL de respuesta** , escriba la **URL de respuesta** específica para su organización.

   > [!NOTE]
   > El valor se actualizará con la dirección URL de respuesta real que se explica más adelante en el tutorial.

4. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (sin procesar)** y, a continuación, guarde el archivo del certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

5. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/tutorial_general_400.png)

6. En la sección **Configuración de Salesforce Sandbox**, haga clic en **Configurar Salesforce Sandbox** para abrir la ventana **Configurar inicio de sesión**. Copie **SAML Entity ID and SAML Single Sign-On Service URL** (URL del servicio de inicio de sesión único de SAML e Identificador de entidad de SAML) de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_configure.png)

7. Abra una nueva pestaña en el explorador e inicie sesión en su cuenta de administrador de Salesforce Sandbox.

8. Haga clic en **Setup** (Configuración) en el **icono de configuración** de la esquina superior derecha de la página.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure1.png)

9. Desplácese hacia abajo hasta **SETTINGS** (CONFIGURACIÓN) en el panel de navegación y haga clic en **Identity** (Identidad) para expandir la sección relacionada. A continuación, haga clic en **Configuración de inicio de sesión único**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

10. En la página **Configuración de inicio de sesión único**, haga clic en el botón **Editar**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure3.png)

11. Seleccione **SAML habilitado** y haga clic en **Guardar**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

12. Para establecer la configuración de inicio de sesión único de SAML, haga clic en **Nuevo**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

13. En la sección **Configuración del inicio de sesión único**, siga estos pasos:

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-saml-config1.png)

    a. Seleccione la casilla de verificación **SAML habilitado**.

    b. En el campo **Issuer** (Emisor), pegue el valor de **SAML Entity ID** (Identificador de entidad de SAML) que haya copiado de Azure Portal.

    c. Para cargar el **Certificado de proveedor de identidad**, haga clic en **Choose File** (Elegir archivo) para examinar y seleccionar el archivo de certificado que haya descargado de Azure Portal.

    d. En el cuadro de texto **Identity Provider Login URL** (Dirección URL de inicio de sesión del proveedor de identidades), pegue el valor de **Dirección URL del servicio de inicio de sesión único** que haya copiado de Azure Portal.

    e. En **SAML Identity Type** (tipo de identidad SAML), elija una de las siguientes opciones:

      * Seleccione **Assertion contains the User's Salesforce username** (La aserción contiene el nombre del usuario en Salesforce) si el nombre de usuario en Salesforce se pasa en la aserción SAML.

      * Seleccione **Assertion contains the Federation ID from the User object** (La aserción contiene el id. de federación del objeto de usuario) si el id. de federación del objeto de usuario se pasa en la aserción SAML.
  
    f. Como **Ubicación de identidad de SAML**, seleccione **Identidad es un elemento Atributo**.

    g. SFDC no admite el cierre de sesión SAML.  Como alternativa, pegue `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0` en el cuadro de texto **Custom Logout URL** (URL de cierre de sesión personalizada).

    h. Haga clic en **Save**(Guardar).

14. En la página **Configuración de inicio de sesión único**, haga clic en el botón **Descargar metadatos**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure4.png)

15. Abra los metadatos descargados en una ventana distinta del explorador y copie el valor de la **Ubicación** para pegarlo en el cuadro de texto **URL de respuesta** en la sección **Dominio y direcciones URL de Salesforce Sandbox** en el Azure Portal.  

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure5.png)

16. Si quiere configurar la aplicación en el modo iniciado por **SP**, estos son los requisitos previos:

    a. Debe tener un dominio comprobado.

    b. Se necesita configurar y habilitar su dominio en Salesforce Sandbox. Los pasos para hacerlo se explican más adelante en este tutorial.

    c. En la sección **Dominio y direcciones URL de Salesforce Sandbox** del Azure Portal, marque **Mostrar configuración avanzada de URL** y realice los pasos siguientes:
  
    ![Información de dominio y direcciones URL de inicio de sesión único de Salesforce Sandbox](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba el valor con el siguiente patrón: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    > [!NOTE]
    > Este valor se debe copiar desde el portal de Salesforce Sandbox, una vez que se ha habilitado el dominio.

17. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (sin procesar)** y, a continuación, guarde el archivo del certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

18. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/tutorial_general_400.png)

19. En la sección **Configuración de Salesforce Sandbox**, haga clic en **Configurar Salesforce Sandbox** para abrir la ventana **Configurar inicio de sesión**. Copie **SAML Entity ID and SAML Single Sign-On Service URL** (URL del servicio de inicio de sesión único de SAML e Identificador de entidad de SAML) de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_configure.png)

20. Abra una nueva pestaña en el explorador e inicie sesión en su cuenta de administrador de Salesforce Sandbox.

21. Haga clic en **Setup** (Configuración) en el **icono de configuración** de la esquina superior derecha de la página.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure1.png)

22. Desplácese hacia abajo hasta **SETTINGS** (CONFIGURACIÓN) en el panel de navegación y haga clic en **Identity** (Identidad) para expandir la sección relacionada. A continuación, haga clic en **Configuración de inicio de sesión único**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

23. En la página **Configuración de inicio de sesión único**, haga clic en el botón **Editar**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure3.png)

24. Seleccione **SAML habilitado** y haga clic en **Guardar**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

25. Para establecer la configuración de inicio de sesión único de SAML, haga clic en **Nuevo**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

26. Si va a agregar una segunda instancia, debe habilitar un dominio, como se mencionó anteriormente (caso iniciado por SP). En la sección Configuración del inicio de sesión único de SAML siga estos pasos:

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

    a. En el cuadro de texto **Name** (Nombre), escriba el nombre de la configuración (por ejemplo, *SPSSOWAAD_Test*).

    b. En el campo **Issuer** (Emisor), pegue el valor de **SAML Entity ID** (Identificador de entidad de SAML) que haya copiado de Azure Portal.

    c. En el cuadro de texto **Id. de entidad**, use el valor `https://test.salesforce.com` para la primera instancia y para la segunda instancia de la aplicación puede usar el valor de identificador específico del inquilino.

    d. Para cargar el **certificado de proveedor de identidades**, haga clic en **Choose File** (Elegir archivo) para navegar y seleccionar el archivo de certificado que haya descargado de Azure Portal.

    e. En **SAML Identity Type** (tipo de identidad SAML), elija una de las siguientes opciones:

      * Seleccione **Assertion contains the User's Salesforce username** (La aserción contiene el nombre del usuario en Salesforce) si el nombre de usuario en Salesforce se pasa en la aserción SAML.

      * Seleccione **Assertion contains the Federation ID from the User object** (La aserción contiene el id. de federación del objeto de usuario) si el id. de federación del objeto de usuario se pasa en la aserción SAML.

      * Seleccione **Assertion contains the Use ID from the User object** (La aserción contiene el id. de uso del objeto de usuario) si el id. de federación del objeto de usuario se pasa en la aserción SAML.

    f. Como **SAML Identity Location**(Ubicación de identidad de SAML), seleccione **Identity is in the NameIdentifier element of the Subject statement** (La identidad está en el elemento NameIdentifier de la instrucción de sujeto).

    g. Para **Service Provider Initiated Request Binding** (Enlace de solicitud iniciada por el proveedor de servicio), seleccione **HTTP POST** (Método HTTP POST).

    h. En el cuadro de texto **Identity Provider Login URL** (Dirección URL de inicio de sesión del proveedor de identidades), pegue el valor de **Dirección URL del servicio de inicio de sesión único** que haya copiado de Azure Portal.

    i. SFDC no admite el cierre de sesión SAML.  Como alternativa, pegue `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0` en el cuadro de texto **Custom Logout URL** (URL de cierre de sesión personalizada).

    j. Haga clic en **Save**(Guardar).

27. Para habilitar su dominio en Salesforce Sandbox, lleve a cabo los siguientes pasos:

    > [!NOTE]
    > Antes de habilitar el dominio debe crearlo en Salesforce Sandbox. Para más información, consulte [Defining Your Domain Name](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US) (Definición del nombre de dominio). Una vez creado el dominio, asegúrese de que está configurado correctamente.

    * En el panel de navegación izquierdo de Salesforce, haga clic en **Company Settings** (Configuración de la empresa) para expandir la sección relacionada y haga clic en **My Domain** (Mi dominio).

         ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-my-domain.png)

    * En la sección **Authentication Configuration** (Configuración de autenticación), haga clic en **Edit** (Editar).

        ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-edit-auth-config.png)

    * En la sección **Authentication Configuration** (Configuración de autenticación), seleccione como el valor de **Servicio de autenticación** el nombre de la Configuración de inicio de sesión único SAML que ha establecido durante la configuración de SSO en Salesforce Sandbox. Finalmente, haga clic en **Save** (Guardar).

        ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure2.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/salesforce-sandbox-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/salesforce-sandbox-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/salesforce-sandbox-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/salesforce-sandbox-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="create-a-salesforce-sandbox-test-user"></a>Creación de un usuario de prueba de Salesforce Sandbox

En esta sección, creará un usuario llamado a Britta Simon en Salesforce Sandbox. Salesforce Sandbox admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario ya no existe en Salesforce Sandbox, se crea uno nuevo cuando se intenta acceder a esta aplicación. El espacio aislado de Salesforce también admite el aprovisionamiento automático de usuarios. [Aquí](salesforce-sandbox-provisioning-tutorial.md) puede encontrar más detalles sobre cómo configurar el aprovisionamiento automático de usuarios.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Salesforce Sandbox.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a Salesforce Sandbox, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **Salesforce Sandbox**.

    ![Vínculo de Salesforce Sandbox en la lista de aplicaciones](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Salesforce Sandbox en el Panel de acceso, iniciará sesión automáticamente en la aplicación Salesforce Sandbox.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configuración del aprovisionamiento de usuarios](salesforce-sandbox-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/salesforce-sandbox-tutorial/tutorial_general_01.png
[2]: ./media/salesforce-sandbox-tutorial/tutorial_general_02.png
[3]: ./media/salesforce-sandbox-tutorial/tutorial_general_03.png
[4]: ./media/salesforce-sandbox-tutorial/tutorial_general_04.png

[100]: ./media/salesforce-sandbox-tutorial/tutorial_general_100.png

[200]: ./media/salesforce-sandbox-tutorial/tutorial_general_200.png
[201]: ./media/salesforce-sandbox-tutorial/tutorial_general_201.png
[202]: ./media/salesforce-sandbox-tutorial/tutorial_general_202.png
[203]: ./media/salesforce-sandbox-tutorial/tutorial_general_203.png