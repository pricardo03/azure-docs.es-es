---
title: 'Tutorial: Integración de Azure Active Directory con ServiceNow | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y ServiceNow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 016f4fe5e0d1e8c84a13ca97d8f910c29d5984d8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67091129"
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Tutorial: Integración de Azure Active Directory con ServiceNow

En este tutorial, aprenderá a integrar ServiceNow con Azure Active Directory (Azure AD).
La integración de ServiceNow con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a ServiceNow.
* Puede permitir que los usuarios inicien sesión automáticamente en ServiceNow (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con ServiceNow, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción de ServiceNow con el inicio de sesión único habilitado
* Para ServiceNow, una instancia o inquilino de ServiceNow, versión Calgary o superior
* Para ServiceNow Express, una instancia de ServiceNow Express, versión Helsinki o superior
* El inquilino de ServiceNow debe tener habilitado el [complemento de inicio de sesión único en varios proveedores](https://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0). Esto puede hacerse [enviando una solicitud de servicio](https://hi.service-now.com).
* Para la configuración automática, habilite el complemento de varios proveedores para ServiceNow.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* ServiceNow admite el inicio de sesión único iniciado por **SP**

* ServiceNow admite el [**aprovisionamiento de usuario** automático](servicenow-provisioning-tutorial.md)

## <a name="adding-servicenow-from-the-gallery"></a>Adición de ServiceNow desde la galería

Para configurar la integración de ServiceNow en Azure AD, es preciso agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar ServiceNow desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **ServiceNow**, seleccione **ServiceNow** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![ServiceNow en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con ServiceNow con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de ServiceNow.

Para configurar y probar el inicio de sesión único de Azure AD con ServiceNow, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD para ServiceNow](#configure-azure-ad-single-sign-on-for-servicenow)** : para permitir a los usuarios usar esta característica.
2. **[Configuración del inicio de sesión único en ServiceNow](#configure-servicenow-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Configuración del inicio de sesión único de Azure AD para ServiceNow Express](#configure-azure-ad-single-sign-on-for-servicenow-express)** : para permitir a los usuarios usar esta característica.
4. **[Configuración del inicio de sesión único en ServiceNow Express](#configure-servicenow-express-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
5. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
6. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
7. **[Creación de un usuario de prueba en ServiceNow](#create-servicenow-test-user)** : para tener un homólogo de Britta Simon en ServiceNow que esté vinculado a la representación de ella en Azure AD.
8. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on-for-servicenow"></a>Configuración del inicio de sesión único de Azure AD para ServiceNow

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con ServiceNow, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **ServiceNow**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información acerca del inicio de sesión único de dominio y direcciones URL de ServiceNow](common/sp-identifier.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<instance-name>.service-now.com/navpage.do`

    b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Estos valores no son reales. Debe actualizar estos valores con la dirección URL de inicio de sesión y el identificador reales, como se explica más adelante en el tutorial. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

    a. Haga clic en el botón Copiar para copiar la **dirección URL de metadatos de federación de aplicación** y péguela en el Bloc de notas, se usará más adelante en el tutorial.

    b. Haga clic en **Descargar** para descargar el **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

6. En la sección **Set up ServiceNow** (Configurar ServiceNow), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-servicenow-single-sign-on"></a>Configuración del inicio de sesión único de ServiceNow

1. Inicie sesión en la aplicación ServiceNow como administrador.

2. Active el complemento **Integration - Multiple Provider Single Sign-On Installer**, para lo que debe seguir estos pasos:

    a. En el panel de navegación del lado izquierdo, busque la sección **System Definition** (Definición del sistema) con la barra de búsqueda y haga clic en **Plugins** (Complementos).

    ![Activar complemento](./media/servicenow-tutorial/tutorial_servicenow_03.png "activar complemento")

    b. Busque **Integration - Multiple Provider Single Sign-On Installer**.

     ![Activar complemento](./media/servicenow-tutorial/tutorial_servicenow_04.png "activar complemento")

    c. Seleccione el complemento. Haga clic con el botón derecho y seleccione **Activate/Upgrade** (Activar o actualizar).

     ![Activar complemento](./media/servicenow-tutorial/tutorial_activate.png "activar complemento")

    d. Haga clic en el botón **Activate**.

     ![Activar complemento](./media/servicenow-tutorial/tutorial_activate1.png "activar complemento")

3. En el panel de navegación del lado izquierdo, busque la sección de **SSO de varios proveedores** de la barra de búsqueda y haga clic en **Properties** (Propiedades).

    ![Configurar dirección URL de la aplicación](./media/servicenow-tutorial/tutorial_servicenow_06.png "Configurar dirección URL de la aplicación")

4. En el cuadro de diálogo **Multiple Provider SSO Properties** (Propiedades de SSO de varias proveedores), realice los pasos siguientes:

    ![Configurar dirección URL de la aplicación](./media/servicenow-tutorial/ic7694981.png "Configurar dirección URL de la aplicación")

    * En **Enable multiple provider SSO** (Habilitar SSO de varios proveedores), seleccione **Yes** (Sí).
  
    * En **Enable Auto Importing of users from all identity providers into the user table** (Habilitar la importación automática de usuarios de todos los proveedores de identidades en la tabla de usuarios), seleccione **Yes** (Sí).

    * En **Enable debug logging for the multiple provider SSO integration** (Habilitar registro de depuración para la integración de SSO de varios proveedores), seleccione **Yes** (Sí).

    * En el cuadro de texto **The field on the user table that...** (El campo en la tabla de usuario que...), escriba **user_name** (nombre_usuario).
  
    * Haga clic en **Save**(Guardar).

5. Hay dos maneras de configurar **ServiceNow**: automática y manual.

6. Para configurar **ServiceNow** automáticamente, siga estos pasos:

    * Vuelva a la página de inicio de sesión único de **ServiceNow** en Azure Portal.

    * ServiceNow tiene el servicio de configuración con un clic, es decir, Azure AD configura automáticamente ServiceNow para la autenticación de SAML. Para habilitarlo, vaya a la sección **ServiceNow Configuration** (Configuración de ServiceNow), haga clic en **Configure ServiceNow** (Configurar ServiceNow) para abrir la ventana Configurar inicio de sesión.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

    * Escriba el nombre de la instancia de ServiceNow, el nombre de usuario y la contraseña de administrador en el formulario **Configurar inicio de sesión** y haga clic en **Configurar ahora**. Tenga en cuenta que el nombre de usuario de administrador proporcionado debe tener asignado el rol **security_admin** en ServiceNow para que esto funcione. Si no es así, para configurar manualmente ServiceNow para usar Azure AD como proveedor de identidades SAML, haga clic en **Configurar manualmente el inicio de sesión único** y copie la **URL del servicio de inicio de sesión único de SAML, el identificador de entidad de SAML y la dirección URL de cierre de sesión** desde la sección Referencia rápida.

        ![Configurar dirección URL de la aplicación](./media/servicenow-tutorial/configure.png "Configurar dirección URL de la aplicación")

    * Inicie sesión en la aplicación ServiceNow como administrador.

    * En la configuración automática se configuran todos los parámetros necesarios de **ServiceNow** pero el **certificado X.509** no está habilitado de forma predeterminada. Tiene que asignarlo manualmente al proveedor de identidades en ServiceNow. Siga a continuación los pasos correspondientes:

    * En el panel de navegación del lado izquierdo, busque la sección de **Multi-Provider SSO** (Inicio de sesión único de varios proveedores) de la barra de búsqueda y haga clic en **Proveedores de identidades**.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_07.png "Configurar inicio de sesión único")

    * Haga clic en el proveedor de identidades generado automáticamente.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_08.png "Configurar inicio de sesión único")

    *  En la sección **Proveedor de identidades**, realice los siguientes pasos:

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/automatic_config.png "Configurar inicio de sesión único")

        * En el cuadro de texto **Name** (Nombre), escriba el nombre de la configuración (por ejemplo, **Inicio de sesión único federado de Microsoft Azure**).

        * Quite el valor rellenado **Identity Provider's SingleLogoutRequest** (Solicitud de cierre de sesión único del proveedor de identidades) del cuadro de texto.

        * Copie el valor de **ServiceNow Homepage**, péguelo en el cuadro de texto **Dirección URL de inicio de sesión** de la sección **ServiceNow Domain and URLs** (Dominio y direcciones URL de ServiceNow) de Azure Portal.

            > [!NOTE]
            > La página principal de la instancia de ServiceNow es una concatenación de su **URL de inquilino de ServiceNow** y **/navpage.do** (por ejemplo: `https://fabrikam.service-now.com/navpage.do`).

        * Copie el valor de **Entity ID / Issuer** (Id. de entidad / emisor), péguelo en el cuadro de texto **Identificador** de la sección **ServiceNow Domain and URLs** (Dominio y direcciones URL de ServiceNow) de Azure Portal.

        * Asegúrese de que el valor de **NameID Policy** (Directiva de id. de nombres) esté establecido en el valor `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`. 

    * Desplácese hacia abajo a la sección **Certificado X.509** y seleccione **Editar**.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_09.png "Configurar inicio de sesión único")

    * Seleccione el certificado y haga clic con el botón derecho en el icono de flecha para agregar el certificado.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_11.png "Configurar inicio de sesión único")

    * Haga clic en **Save**(Guardar).

    * Haga clic en **Prueba de conexión** en la esquina superior derecha de la página.

        ![Activar complemento](./media/servicenow-tutorial/tutorial_activate2.png "activar complemento")

    * Después de hacer clic en **Prueba de conexión**, aparecerá la ventana emergente donde deberá escribir las credenciales y se mostrará la página siguiente con los resultados. Se espera el error **SSO Logout Test Results** (Resultados de pruebas de cierre de sesión de SSO). Ignórelo y haga clic en el botón **Activar**.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/servicenowactivate.png "Configurar inicio de sesión único")
  
7. Para configurar **ServiceNow** manualmente, siga los pasos siguientes:

    * Inicie sesión en la aplicación ServiceNow como administrador.

    * En el panel de navegación de la izquierda, haga clic en **Identity Providers**(Proveedores de identidades).

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_07.png "Configurar inicio de sesión único")

    * En el cuadro de diálogo **Proveedores de identidades**, haga clic en **Nuevo**.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694977.png "Configurar inicio de sesión único")

    * En el cuadro de diálogo **Proveedores de identidades**, haga clic en **SAML**.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694978.png "Configurar inicio de sesión único")

    * En la ventana emergente **Import Identity Provider Metadata** (Importar metadatos del proveedor de identidades), realice los siguientes pasos:

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/idp.png "Configurar inicio de sesión único")

        * Escriba la **dirección URL de metadatos de federación de aplicación** que haya copiado de Azure Portal.

        * Haga clic en **Import**.

    * Se lee la dirección URL de metadatos de IdP y se rellena toda la información de campos.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694982.png "Configurar inicio de sesión único")

        * En el cuadro de texto **Name** (Nombre), escriba el nombre de la configuración (por ejemplo, **Inicio de sesión único federado de Microsoft Azure**).

        * Quite el valor rellenado **Identity Provider's SingleLogoutRequest** (Solicitud de cierre de sesión único del proveedor de identidades) del cuadro de texto.

        * Copie el valor de **ServiceNow Homepage**, péguelo en el cuadro de texto **Dirección URL de inicio de sesión** de la sección **ServiceNow Domain and URLs** (Dominio y direcciones URL de ServiceNow) de Azure Portal.

            > [!NOTE]
            > La página principal de la instancia de ServiceNow es una concatenación de su **URL de inquilino de ServiceNow** y **/navpage.do** (por ejemplo: `https://fabrikam.service-now.com/navpage.do`).

        * Copie el valor de **Entity ID / Issuer** (Id. de entidad / emisor), péguelo en el cuadro de texto **Identificador** de la sección **ServiceNow Domain and URLs** (Dominio y direcciones URL de ServiceNow) de Azure Portal.

        * Asegúrese de que el valor de **NameID Policy** (Directiva de id. de nombres) esté establecido en el valor `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`.

        * Haga clic en **Avanzadas**. En el cuadro de texto **Campo de usuario**, escriba **correo electrónico** o **user_name**, según qué campo se use para identificar de forma única a los usuarios en la implementación de ServiceNow.

            > [!NOTE]
            > Puede configurar Azure AD para que emita el identificador de usuario de Azure AD (nombre de usuario principal) o la dirección de correo electrónico como identificador único en el token SAML. Para ello, vaya a la sección **ServiceNow > Atributos > Inicio de sesión único** en Azure Portal y asigne el campo que desee al atributo **nameidentifier**. El valor almacenado para el atributo seleccionado en Azure AD (por ejemplo, nombre de usuario principal) debe coincidir con el valor almacenado en ServiceNow para el campo especificado (por ejemplo, user_name).

        * Haga clic en **Prueba de conexión** en la esquina superior derecha de la página.

        * Después de hacer clic en **Prueba de conexión**, aparecerá la ventana emergente donde deberá escribir las credenciales y se mostrará la página siguiente con los resultados. Se espera el error **SSO Logout Test Results** (Resultados de pruebas de cierre de sesión de SSO). Ignórelo y haga clic en el botón **Activar**.

          ![Configurar inicio de sesión único](./media/servicenow-tutorial/servicenowactivate.png "Configurar inicio de sesión único")

### <a name="configure-azure-ad-single-sign-on-for-servicenow-express"></a>Configuración del inicio de sesión único de Azure AD para ServiceNow Express

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **ServiceNow**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información acerca del inicio de sesión único de dominio y direcciones URL de ServiceNow](common/sp-identifier.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<instance-name>.service-now.com/navpage.do`

    b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Estos valores no son reales. Debe actualizar estos valores con la dirección URL de inicio de sesión y el identificador reales, como se explica más adelante en el tutorial. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. ServiceNow tiene el servicio de configuración con un clic, es decir, Azure AD configura automáticamente ServiceNow para la autenticación de SAML. Para habilitar este servicio, vaya a la sección **Set up ServiceNow** (Configurar ServiceNow) y haga clic en **Ver instrucciones detalladas** para abrir la ventana Configurar inicio de sesión.

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

7. Escriba el nombre de la instancia de ServiceNow, el nombre de usuario y la contraseña de administrador en el formulario **Configurar inicio de sesión** y haga clic en **Configurar ahora**. Tenga en cuenta que el nombre de usuario de administrador proporcionado debe tener asignado el rol **security_admin** en ServiceNow para que esto funcione. Si no es así, para configurar manualmente ServiceNow para usar Azure AD como proveedor de identidades SAML, haga clic en **Configurar manualmente el inicio de sesión único** y copie la **URL del servicio de inicio de sesión único de SAML, el identificador de entidad de SAML y la dirección URL de cierre de sesión** desde la sección Referencia rápida.

    ![Configurar dirección URL de la aplicación](./media/servicenow-tutorial/configure.png "Configurar dirección URL de la aplicación")

### <a name="configure-servicenow-express-single-sign-on"></a>Configuración del inicio de sesión único de ServiceNow Express

1. Inicie sesión en la aplicación ServiceNow Express como administrador.

2. En el panel de navegación del lado izquierdo, haga clic en **Inicio de sesión único**.

    ![Configurar dirección URL de la aplicación](./media/servicenow-tutorial/ic7694980ex.png "Configurar dirección URL de la aplicación")

3. En el cuadro de diálogo **Inicio de sesión único**, haga clic en el icono de configuración de la parte superior derecha y establezca las siguientes propiedades:

    ![Configurar dirección URL de la aplicación](./media/servicenow-tutorial/ic7694981ex.png "Configurar dirección URL de la aplicación")

    a. Cambie **Enable multiple provider SSO** (Habilitar SSO de varios proveedores), hacia la derecha.

    b. Cambie **Enable debug logging got the multiple provider SSO integration** (Habilitar registro de depuración para integración de SSO de varios proveedores) hacia la derecha.

    c. En el cuadro de texto **The field on the user table that...** (El campo en la tabla de usuario que...), escriba **user_name** (nombre_usuario).

4. En el cuadro de diálogo **Single Sign-On** (Inicio de sesión único), haga clic en **Add New Certificate** (Agregar nuevo certificado).

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694973ex.png "Configurar inicio de sesión único")

5. En el cuadro de diálogo **Certificados X.509** , realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694975.png "Configurar inicio de sesión único")

    a. En el cuadro de texto **Name** (Nombre), escriba el nombre de la configuración (por ejemplo, **TestSAML2.0**).

    b. Seleccione **Active**(Activo).

    c. En **Format** (Formato), seleccione **PEM**.

    d. En **Type** (Tipo), seleccione **Trust Store Cert** (Confiar en certificados de almacén).

    e. Abra el certificado codificado en base 64 descargado de Azure Portal en el bloc de notas, copie su contenido en el portapapeles y péguelo en el cuadro de texto **PEM Certificate** (Certificado PEM).

    f. Haga clic en **Update** (Actualizar).

6. En el cuadro de diálogo **Single Sign-On** (Inicio de sesión único), haga clic en **Add New IdP** (Agregar nuevo IdP).

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694976ex.png "Configurar inicio de sesión único")

7. En el cuadro de diálogo **Add New Identity Provider** (Agregar nuevo proveedor de identidades), en **Configure Identity Provider** (Configurar proveedor de identidades), siga estos pasos:

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694982ex.png "Configurar inicio de sesión único")

    a. En el cuadro de texto **Name** (Nombre), escriba el nombre de la configuración (por ejemplo, **SAML 2.0**).

    b. En el campo **Identity provider URL** (Dirección URL del proveedor de identidades), pegue el valor del **identificador del proveedor de identidades** que ha copiado de Azure Portal.

    c. En el campo **Identity Provider's AuthnRequest** (Solicitud de autenticación del proveedor de identidades), pegue el valor de la **dirección URL de solicitud de autenticación** que ha copiado de Azure Portal.

    d. En el campo **Identity Provider's SingleLogoutRequest** (SingleLogoutRequest del proveedor de identidades), pegue el valor de la **Logout URL** (Dirección URL de cierre de sesión), que ha copiado de Azure Portal

    e. Como **Certificado de proveedor de identidades**, seleccione el certificado que ha creado en el paso anterior.

8. Haga clic en **Advanced Settings** (Configuración avanzada) y en **Additional Identity Provider Properties** (Propiedades adicionales del proveedor de identidades), siga estos pasos:

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694983ex.png "Configurar inicio de sesión único")

    a. En el cuadro de texto **Protocol Binding for the IDP's SingleLogoutRequest** (Vinculación de protocolo para la solicitud de cierre de sesión único del proveedor de identidades), escriba **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect**.

    b. En el cuadro de texto **NameID Policy** (Directiva NameID), escriba **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified**.

    c. En **AuthnContextClassRef Method** (Método AuthnContextClassRef), escriba `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.

    d. Anule la selección de **Create an AuthnContextClass**(Crear AuthnContextClass).

9. En **Additional Identity Provider Properties** (Propiedades adicionales del proveedor de identidades), siga estos pasos:

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694984ex.png "Configurar inicio de sesión único")

    a. En el cuadro de texto **ServiceNow Homepage** (Página de inicio de ServiceNow), escriba la dirección URL de la página de inicio de instancia de ServiceNow.

    > [!NOTE]
    > La página principal de la instancia de ServiceNow es una concatenación de su **URL de inquilino de ServiceNow** y **/navpage.do** (por ejemplo: `https://fabrikam.service-now.com/navpage.do`).

    b. En el cuadro de texto **Entity ID / Issuer** (Id. de entidad / emisor), escriba la dirección URL de su inquilino de ServiceNow.

    c. En el cuadro de texto **Audience URI** (Identificador URI de audiencia), escriba la dirección URL de su inquilino ServiceNow.

    d. En el cuadro de diálogo **Clock Skew** (Desplazamiento del reloj), escriba **60**.

    e. En el cuadro de texto **Campo de usuario**, escriba **correo electrónico** o **user_name**, según qué campo se use para identificar de forma única a los usuarios en la implementación de ServiceNow.

    > [!NOTE]
    > Puede configurar Azure AD para que emita el identificador de usuario de Azure AD (nombre de usuario principal) o la dirección de correo electrónico como identificador único en el token SAML. Para ello, vaya a la sección **ServiceNow > Atributos > Inicio de sesión único** en Azure Portal y asigne el campo que desee al atributo **nameidentifier**. El valor almacenado para el atributo seleccionado en Azure AD (por ejemplo, nombre de usuario principal) debe coincidir con el valor almacenado en ServiceNow para el campo especificado (por ejemplo, user_name).

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
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon\@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a ServiceNow.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **ServiceNow**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **ServiceNow**.

    ![Vínculo a ServiceNow en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-servicenow-test-user"></a>Creación de un usuario de prueba en ServiceNow

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en ServiceNow. ServiceNow admite el aprovisionamiento automático de usuarios, que está habilitado de forma predeterminada. [Aquí](servicenow-provisioning-tutorial.md) puede encontrar más información sobre cómo configurar el aprovisionamiento automático de usuarios.

> [!NOTE]
> Si necesita crear manualmente un usuario, es preciso que se ponga en contacto con el [equipo de soporte técnico al cliente de ServiceNow](https://www.servicenow.com/support/contact-support.html)

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de ServiceNow del panel de acceso, debería iniciar sesión automáticamente en la versión de ServiceNow para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Configuración del aprovisionamiento de usuarios](servicenow-provisioning-tutorial.md)
