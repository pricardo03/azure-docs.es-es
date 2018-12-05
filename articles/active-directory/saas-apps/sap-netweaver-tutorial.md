---
title: 'Tutorial: integración de Azure Active Directory con SAP NetWeaver | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SAP NetWeaver.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2018
ms.author: jeedes
ms.openlocfilehash: fac22508e679c1e1c93ec62a5b120ba9c7c52317
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "52162396"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-netweaver"></a>Tutorial: Integración de Azure Active Directory con SAP NetWeaver

En este tutorial, aprenderá a integrar SAP NetWeaver con Azure Active Directory (Azure AD).

La integración de SAP NetWeaver con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a SAP NetWeaver.
- Puede permitir que los usuarios inicien sesión automáticamente en SAP NetWeaver (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con SAP NetWeaver, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en SAP NetWeaver
- Se requiere SAP NetWeaver V7.20 como mínimo

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.
El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de SAP NetWeaver desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-sap-netweaver-from-the-gallery"></a>Adición de SAP NetWeaver desde la galería

Para configurar la integración de SAP NetWeaver en Azure AD, deberá agregar esta solución desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar SAP NetWeaver desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **SAP NetWeaver**, seleccione **SAP NetWeaver** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![SAP NetWeaver en la lista de resultados](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con SAP NetWeaver utilizando un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de SAP NetWeaver para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de SAP NetWeaver.

Para configurar y probar el inicio de sesión único de Azure AD con SAP NetWeaver, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de SAP NetWeaver](#creating-sapnetweaver-test-user)**: el objetivo es tener un homólogo de Britta Simon en SAP NetWeaver que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación SAP NetWeaver.

**Para configurar el inicio de sesión único de Azure AD con SAP NetWeaver, realice los pasos siguientes:**

1. Abra una ventana del explorador web e inicie sesión en el sitio de la compañía SAP NetWeaver como administrador.

2. Asegúrese de que los servicios **http** y **https** están activos y de que se asignan los puertos adecuados en el código de transacción **SMICM**.

3. Inicie sesión en el cliente empresarial del sistema de SAP (T01) donde se requiere el inicio de sesión único y active la administración de sesiones de seguridad HTTP.

     a. Vaya al código de transacción **SICF_SESSIONS**. Se muestran todos los parámetros de perfil correspondientes con sus valores actuales. Se ven como se muestra a continuación:
    ```
    login/create_sso2_ticket = 2
    login/accept_sso2_ticket = 1
    login/ticketcache_entries_max = 1000
    login/ticketcache_off = 0  login/ticket_only_by_https = 0 
    icf/set_HTTPonly_flag_on_cookies = 3
    icf/user_recheck = 0  http/security_session_timeout = 1800
    http/security_context_cache_size = 2500
    rdisp/plugin_auto_logout = 1800
    rdisp/autothtime = 60
    ```
    >[!NOTE]
    > Ajuste los parámetros anteriores según las necesidades de la organización. Los parámetros anteriores se proporcionan solo como indicación.

    b. De ser necesario, ajuste los parámetros en el perfil de instancia o predeterminado del sistema SAP y reinicie el sistema SAP.

    c. Haga doble clic en el cliente pertinente para habilitar la sesión de seguridad HTTP.

    ![Vínculo de descarga del certificado](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_profileparameter.png)

    d. Active los siguientes servicios SICF:
    ```
    /sap/public/bc/sec/saml2
    /sap/public/bc/sec/cdc_ext_service
    /sap/bc/webdynpro/sap/saml2
    /sap/bc/webdynpro/sap/sec_diag_tool (This is only to enable / disable trace)
    ```
4. Vaya al código de transacción **SAML2** en el cliente empresarial del sistema SAP [T01/122]. Se abrirá una interfaz de usuario en el explorador. En este ejemplo, se supone a 122 como el cliente empresarial SAP.

    ![Vínculo de descarga del certificado](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_sapbusinessclient.png)

5. Indique su nombre de usuario y contraseña para ingresar a la interfaz de usuario y haga clic en **Editar**.

    ![Vínculo de descarga del certificado](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_userpwd.png)

6. Reemplace el **nombre del proveedor** T01122 por **http://T01122** y haga clic en **Guardar**.

    > [!NOTE]
    > De forma predeterminada, el nombre de proveedor tiene el formato <sid><client>, pero Azure AD espera que el nombre tenga el formato <protocol>://<name>. Se recomienda que el nombre del proveedor se mantenga como https://<sid><client> para permitir la configuración de varios motores ABAP de SAP NetWeaver en Azure AD.

    ![Vínculo de descarga del certificado](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_providername.png)

7. **Generación de metadatos del proveedor de servicio**: una vez que hemos terminado con la configuración del **proveedor local** y de los **proveedores de confianza** en la interfaz de usuario de SAML 2.0, el siguiente paso sería generar el archivo de metadatos del proveedor de servicios (que contendrá todas las configuraciones, contextos de autenticación y otras configuraciones de SAP). Una vez que se genera este archivo, es necesario cargarlo en Azure AD.

    ![Vínculo de descarga del certificado](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_generatesp.png)

     a. Vaya a **la pestaña Proveedor local**.

    b. Haga clic en **Metadatos**.

    c. Guarde el **archivo de metadatos XML** generado en el equipo y cárguelo en la sección **Configuración básica de SAML** para rellenar de forma automática los valores **Identificador** y **URL de respuesta** en Azure Portal.

8. En la página de integración de la aplicación **SAP NetWeaver** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

9. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial_general_301.png)

10. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

11. En la sección **Configuración básica de SAML**, siga estos pasos:

     a. Haga clic en **Cargar archivo de metadatos** para cargar el **archivo de metadatos del proveedor de servicios** que obtuvo anteriormente.

    ![Carga del archivo de metadatos](common/editmetadataupload.png)

    b. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.

    ![Carga del archivo de metadatos](common/uploadmetadata.png)

    c. Una vez que se haya cargado correctamente el archivo de metadatos, los valores **Identificador** y **Dirección URL de respuesta** se rellenan automáticamente en el cuadro de texto de la sección **Configuración básica de SAML**, como se muestra a continuación:

    ![Información de dominio y direcciones URL de inicio de sesión único de SAP NetWeaver](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_url.png)

    d. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<your company instance of SAP NetWeaver>`.

12. La aplicación SAP NetWeaver espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![Sección de atributos](./media/sapnetweaver-tutorial/edit_attribute.png)

13. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:

     a. Haga clic en el icono **Editar** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.
    
    ![Sección de atributos](./media/sapnetweaver-tutorial/nameidattribute.png)

    b. En la pestaña **Administrar las notificaciones del usuario**, realice los siguientes pasos:

    ![Sección de atributos](./media/sapnetweaver-tutorial/nameidattribute1.png)

    * Seleccione **Transformación**.
  
    * Desde la lista **Transformación**, seleccione `ExtractMailPrefix()`.
  
    * Desde la lista**Parámetro 1**, seleccione `user.userprincipalname`.

    * Haga clic en **Save**(Guardar).

14. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic **Descargar** para descargar el **archivo XML de metadatos de federación** y, luego, guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_certificate.png)

15. En la sección **Set up SAP NetWeaver** (Configurar SAP NetWeaver), copie la dirección URL adecuada según sus necesidades.

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![Configuración de SAP NetWeaver](common/configuresection.png)

16. Inicie sesión en el sistema SAP y vaya al código de transacción SAML2. Se abre una nueva ventana del explorador con la pantalla de configuración de SAML.

17. Para configurar puntos de conexión para el proveedor de identidades de confianza (Azure AD), vaya a la pestaña **Proveedores de confianza**.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_samlconfig.png)

18. Presione **Agregar** y seleccione **Cargar archivo de metadatos** en el menú contextual.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_uploadmetadata.png)

19. Cargue el archivo de metadatos que ha descargado desde Azure Portal.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_metadatafile.png)

20. En la siguiente pantalla, escriba el nombre de alias. Por ejemplo, aadsts y presione **Siguiente** para continuar.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_aliasname.png)

21. Asegúrese de que su **algoritmo de síntesis** sea **SHA-256** y no requiera de ningún cambio y presione **Siguiente**.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_identityprovider.png)

22. En **Single Sign-On Endpoints** (Puntos de conexión de inicio de sesión único), utilice **HTTP POST** y haga clic en **Siguiente** para continuar.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_httpredirect.png)

23. En **Single Logout Endpoints** (Puntos de conexión de cierre de sesión único), seleccione **HTTPRedirect** y haga clic en **Siguiente** para continuar.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_httpredirect1.png)

24. En **Artifact Endpoints** (Puntos de conexión de artefacto), presione **Siguiente** para continuar.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_artifactendpoint.png)

25. En **Requisitos de autenticación**, haga clic en **Finalizar**.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_authentication.png)

26. Vaya a la pestaña **Proveedor de confianza** > **Federación de identidades** (desde la parte inferior de la pantalla). Haga clic en **Editar**.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_trustedprovider.png)

27. Haga clic en **Agregar** bajo la pestaña **Federación de identidades** (ventana de la parte inferior).

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_addidentityprovider.png)

28. En la ventana emergente, seleccione **Sin especificar** en **Formatos de NameID admitidos** y haga clic en Aceptar.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_nameid.png)

29. Tenga en cuenta que los valores **origen del id. de usuario** y **modo de asignación de id. de usuario** determinan el vínculo entre el usuario SAP y la notificación de Azure AD.  

    ####<a name="scenario-sap-user-to-azure-ad-user-mapping"></a>Escenario: usuario SAP para la asignación de usuario de Azure AD.

     a. Captura de pantalla de los detalles de NameID de SAP.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/nameiddetails.png)

    b. Captura de pantalla en la que se mencionan las notificaciones necesarias de Azure AD.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/claimsaad1.png)

    ####<a name="scenario-select-sap-user-id-based-on-configured-email-address-in-su01-in-this-case-email-id-should-be-configured-in-su01-for-each-user-who-requires-sso"></a>Escenario: seleccione el id. de usuario SAP según la dirección de correo electrónico configurada en SU01. En este caso, el id. de correo electrónico debe configurarse en su01 para cada usuario que requiera del inicio de sesión único.

     a.  Captura de pantalla de los detalles de NameID de SAP.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_nameiddetails1.png)

    b. Captura de pantalla en la que se mencionan las notificaciones necesarias de Azure AD.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/claimsaad2.png)

30. Haga clic en **Guardar** y, a continuación, haga clic en **Habilitar** para habilitar el proveedor de identidades.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/configuration1.png)

31. Haga clic en **Aceptar** cuando se le solicite.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/configuration2.png)

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_01.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_02.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.

### <a name="creating-sap-netweaver-test-user"></a>Creación de un usuario de prueba de SAP NetWeaver

En esta sección, creará un usuario llamado Britta Simon en SAP NetWeaver. Colabore con su equipo experto en SAP interno o con el asociado SAP de su organización para agregar usuarios en la plataforma de SAP NetWeaver.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a SAP NetWeaver.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **SAP NetWeaver**.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_app.png) 

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

1. Una vez que active el proveedor de identidades de Azure AD, intente acceder a la siguiente dirección URL para comprobar el inicio de sesión único (no se solicitará su nombre de usuario y contraseña)

    `https://<sapurl>/sap/bc/bsp/sap/it00/default.htm`

    (o) use la siguiente dirección URL

    `https://<sapurl>/sap/bc/bsp/sap/it00/default.htm`

    > [!NOTE]
    > Reemplace sapurl por el nombre de host real de SAP.

2. La dirección URL anterior debería dirigirlo a la pantalla que se menciona a continuación. Si puede acceder a la página a continuación, la configuración de SSO de Azure AD se realizó correctamente.

    ![Configurar inicio de sesión único](./media/sapnetweaver-tutorial/testingsso.png)

3. Si se le solicita el nombre de usuario y contraseña, diagnostique el problema al habilitar el seguimiento mediante la siguiente dirección URL

    `https://<sapurl>/sap/bc/webdynpro/sap/sec_diag_tool?sap-client=122&sap-language=EN#`

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
