---
title: 'Tutorial: Integración de Azure Active Directory con Salesforce Sandbox | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Salesforce Sandbox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6794d7eaccb488bb345227161f0bca02f14bc518
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57852583"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Tutorial: Integración de Azure Active Directory con Salesforce Sandbox

En este tutorial, aprenderá a integrar Salesforce Sandbox con Azure Active Directory (Azure AD).

Los espacios aislados ofrecen la capacidad de crear varias copias de su organización en entornos independientes para una variedad de propósitos, como desarrollo, pruebas y aprendizaje, sin poner en peligro los datos y las aplicaciones de la organización de producción de Salesforce.
Para más información, consulte la  [información general sobre Sandbox](https://help.salesforce.com/articleView?id=create_test_instance.htm&language=en_us&type=5).

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

2. Haga clic en **Cambiar el modo de inicio de sesión único** en la parte superior de la pantalla para seleccionar el modo **SAML**.

      ![Vínculo Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/tutorial_general_300.png)

3. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Vínculo Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/tutorial_general_301.png)

4. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.
   
    ![Vínculo Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/tutorial_general_302.png)

5. En la sección **Configuración de SAML básica**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por **IDP**:

     a. Haga clic en **Cargar el archivo de metadatos**.

    ![Carga del archivo de metadatos](./media/salesforce-sandbox-tutorial/upload_metadata.png)

    b. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.

    ![Elección del archivo de metadatos](./media/salesforce-sandbox-tutorial/browse_upload_metadata.png)

    > [!NOTE]
    > Obtendrá el archivo de metadatos del proveedor de servicios en el portal de Salesforce Sandbox como se explica más adelante en el tutorial.

    c. Una vez se ha cargado correctamente el archivo de metadatos, el valor de **Dirección URL de respuesta** se rellenará automáticamente en el cuadro de texto **Dirección URL de respuesta**.

    ![Información de dominio y direcciones URL de inicio de sesión único de Salesforce Sandbox](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url1.png)

6. En la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar **XML de metadatos de federación** y, luego, guarde el archivo XML en el equipo.

    ![Vínculo de descarga del certificado](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

7. Abra una nueva pestaña en el explorador e inicie sesión en su cuenta de administrador de Salesforce Sandbox.

8. Haga clic en **Setup** (Configuración) en el **icono de configuración** de la esquina superior derecha de la página.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure1.png)

9. Desplácese hacia abajo hasta **SETTINGS** (CONFIGURACIÓN) en el panel de navegación y haga clic en **Identity** (Identidad) para expandir la sección relacionada. A continuación, haga clic en **Configuración de inicio de sesión único**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

10. En la página **Configuración de inicio de sesión único**, haga clic en el botón **Editar**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure3.png)

11. Seleccione **SAML habilitado** y haga clic en **Guardar**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

12. Para establecer la configuración de inicio de sesión único de SAML, haga clic en **New from Metadata File** (Nuevo archivo de metadatos).

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

13. Haga clic en **Choose File** (Elegir archivo) para cargar el archivo XML de metadatos que ha descargado desde Azure Portal y haga clic en **Create** (Crear).

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/xmlchoose.png)

14. En la página **SAML Single Sign-On Settings** (Configuración de inicio de sesión único de SAML), los campos se rellenan automáticamente. Haga clic en Save (Guardar).

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/salesforcexml.png)

15. En la página **Single Sign-On Settings** (Configuración del inicio de sesión único), haga clic en el botón **Download Metadata** (Descargar metadatos) para descargar el archivo de metadatos del proveedor de servicio. Use este archivo en la sección **Configuración básica de SAML** en Azure Portal para configurar las direcciones URL necesarias, como se explicó anteriormente.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure4.png)

16. Si quiere configurar la aplicación en el modo iniciado por **SP**, estos son los requisitos previos:

     a. Debe tener un dominio comprobado.

    b. Se necesita configurar y habilitar su dominio en Salesforce Sandbox. Los pasos para hacerlo se explican más adelante en este tutorial.

    c. En Azure Portal, en la sección **Configuración básica de SAML**, haga clic en **Establecer direcciones URL adicionales** y realice el paso siguiente:
  
    ![Información de dominio y direcciones URL de inicio de sesión único de Salesforce Sandbox](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba el valor con el siguiente patrón: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    > [!NOTE]
    > Este valor se debe copiar desde el portal de Salesforce Sandbox, una vez que se ha habilitado el dominio.

17. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos de federación** y luego guarde el archivo XML en el equipo.

    ![Vínculo de descarga del certificado](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

18. Abra una nueva pestaña en el explorador e inicie sesión en su cuenta de administrador de Salesforce Sandbox.

19. Haga clic en **Setup** (Configuración) en el **icono de configuración** de la esquina superior derecha de la página.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure1.png)

20. Desplácese hacia abajo hasta **SETTINGS** (CONFIGURACIÓN) en el panel de navegación y haga clic en **Identity** (Identidad) para expandir la sección relacionada. A continuación, haga clic en **Configuración de inicio de sesión único**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

21. En la página **Configuración de inicio de sesión único**, haga clic en el botón **Editar**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/configure3.png)

22. Seleccione **SAML habilitado** y haga clic en **Guardar**.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

23. Para establecer la configuración de inicio de sesión único de SAML, haga clic en **New from Metadata File** (Nuevo archivo de metadatos).

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

24. Haga clic en **Choose File** (Elegir archivo) para cargar el archivo XML de metadatos y haga clic en **Create** (Crear).

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/xmlchoose.png)

25. En la página **SAML Single Sign-On Settings** (Configuración de inicio de sesión único de SAML) los campos se rellenan automáticamente, escriba el nombre de la configuración (por ejemplo: *SPSSOWAAD_Test*) en el cuadro de texto **Name** (Nombre) y haga clic en Guardar.

    ![Configurar inicio de sesión único](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

26. Para habilitar su dominio en Salesforce Sandbox, lleve a cabo los siguientes pasos:

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

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](./media/salesforce-sandbox-tutorial/create_aaduser_01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](./media/salesforce-sandbox-tutorial/create_aaduser_02.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el **nombre de usuario** tipo de campo **brittasimon\@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.

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

4. Haga clic en el botón **Agregar usuario**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

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
