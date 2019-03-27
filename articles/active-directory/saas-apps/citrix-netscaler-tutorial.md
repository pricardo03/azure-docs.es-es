---
title: 'Tutorial: Integración de Azure Active Directory con Citrix Netscaler | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Citrix Netscaler.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: af501bd0-8ff5-468f-9b06-21e607ae25de
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: ca5fec00d264bb50c75338d36ec793946f495bff
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58229263"
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-netscaler"></a>Tutorial: Integración de Azure Active Directory con Citrix Netscaler

En este tutorial, aprenderá a integrar Citrix Netscaler con Azure Active Directory (Azure AD).
La integración de Citrix Netscaler con Azure AD le proporciona las siguientes ventajas:

* En Azure AD puede controlar quién tiene acceso a Citrix Netscaler.
* Puede permitir que los usuarios inicien sesión automáticamente en Citrix Netscaler (Inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Citrix Netscaler, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en Citrix Netscaler

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Citrix Netscaler admite el inicio de sesión único iniciado por **SP**.

* Citrix Netscaler admite aprovisionamiento de usuarios **Just-In-Time**.

## <a name="adding-citrix-netscaler-from-the-gallery"></a>Adición de Citrix Netscaler desde la galería

Para configurar la integración de Citrix Netscaler en Azure AD, deberá agregar Citrix Netscaler desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Citrix Netscaler desde la galería, realice los siguientes pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Citrix Netscaler**, seleccione **Citrix Netscaler** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Citrix Netscaler en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Citrix Netscaler con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Citrix Netscaler.

Para configurar y probar el inicio de sesión único de Azure AD con Citrix Netscaler, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Citrix Netscaler](#configure-citrix-netscaler-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Citrix Netscaler](#create-citrix-netscaler-test-user)**: para tener un homólogo de Britta Simon en Citrix Netscaler que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Citrix Netscaler, realice los siguientes pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Citrix Netscaler**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Citrix Netscaler](common/sp-identifier-reply.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<<Your FQDN>>/CitrixAuthService/AuthService.asmx`
    
    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://<<Your FQDN>>`

    c. En el cuadro de texto **URL de respuesta (URL del Servicio de consumidor de aserciones)**, escriba una URL con el siguiente patrón: `https://<<Your FQDN>>/CitrixAuthService/AuthService.asmx`.
    
    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Citrix Netscaler](https://www.citrix.com/contact/technical-support.html) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

    > [!NOTE]
    > Para que funcione el inicio de sesión único, estas direcciones URL deben ser accesibles desde sitios públicos. Deberá habilitar el firewall u otras opciones de seguridad en Netscaler para permitir que Azure AD publique el token en la dirección URL de ACS configurada.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Set up Citrix Netscaler** (Configurar Citrix Netscaler), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-citrix-netscaler-single-sign-on"></a>Configuración del inicio de sesión único de Citrix Netscaler

1. En otra ventana del explorador web, inicie sesión en el inquilino de Citrix Netscaler como administrador.

2. Asegúrese de que la **versión de firmware de NetScaler = NS12.1: Build 48.13.nc**.

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure01.png)

3. En la página **Servidor virtual de VPN**, haga lo siguiente:

     ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure02.png)

     a. Establezca la opción **ICA Only** (Solo ICA) de la puerta de enlace en **true**.
    
    b. Establezca **Habilitar autenticación** en **true**.
    
    c. **DTLS** es opcional.
    
    d. Asegúrese de que **SSLv3** está **deshabilitado**.

4. Se crea un grupo **SSL Ciphers** (Cifrados de SSL) personalizado para lograr A+ en https://www.ssllabs.com como se indica a continuación:

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure03.png)

5. En la página **Configuración del servidor de autenticación SAML**, realice los pasos siguientes:

      ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure04.png)

     a. En el cuadro de texto **Name** (Nombre), escriba el nombre del servidor.

    b. En el cuadro de texto **Dirección URL de redireccionamiento**, pegue el valor de **Dirección URL de inicio de sesión** que copió de Azure Portal.

    c. En el cuadro de texto **Singe Logout URL** (Dirección URL de cierre de sesión único), pegue el valor de **Dirección URL de cierre de sesión** que copió de Azure Portal.

    d. En **IDP Certificate Name** (Nombre del certificado IDP), haga clic en el signo **"+"** para agregar el certificado que descargó de Azure Portal. Una vez descargado, seleccione el certificado de la lista desplegable.

    e. Los siguientes campos se deben configurar en esta página.

      ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure24.png)

    f. Establezca la opción **Requested Authentication Context** (Contexto de autenticación solicitado) en **Exact** (Exacto).

    g. Establezca **Signature Algorithm** (Algoritmo de firma) en **RSA-SHA256**.

    h. Establezca **Digest Method** en **SHA256**.

    i. Seleccione **Enforce Username** (Exigir nombre de usuario).

    j. Haga clic en **Aceptar**

6. Para configurar el **perfil de sesión**, realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure06.png)

     a. En el cuadro de texto **Name** (Nombre), escriba el nombre del perfil de la sesión.

    b. En la pestaña **Experiencia del cliente**, realice los cambios que se indican en la captura de pantalla siguiente.

    c. Continúe realizando los cambios en la **pestaña General** tal como se muestra a continuación y haga clic en **Aceptar**.

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure07.png)

    d. En la pestaña **Aplicaciones publicadas**, haga los cambios que se indican en la captura de pantalla siguiente y haga clic en **Aceptar**.

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure08.png)

    e. En la pestaña **Seguridad**, haga los cambios que se indican en la captura de pantalla siguiente y haga clic en **Aceptar**.

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure09.png)

7. Realice las conexiones de ICA conectándose al puerto de confiabilidad de sesión **2598** tal como se muestra en la siguiente captura de pantalla.

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure10.png)

8. En la sección **SAML**, agregue los **servidores** tal como se muestra en la captura de pantalla siguiente.

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure11.png)

9. En la sección **SAML**, agregue las **directivas** tal como se muestra en la captura de pantalla siguiente.

     ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure12.png)

10. En la página **Global Settings** (Configuración global), vaya a la sección **Clientless Access** (Acceso sin cliente).

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure13.png)

11. En la pestaña **Configuration** (Configuración), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure14.png)

     a. Seleccione **Allow Domains** (Permitir dominios).

    b. En el cuadro de texto **Domain Name** (Nombre de dominio), seleccione el dominio.

    c. Haga clic en **OK**.

12. Configure los valores de **StoreFront** en la pestaña **Receiver for Web Sites** (Receptor de sitios web) tal como se muestra en la captura de pantalla siguiente:

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure15.png)

13. En la lista desplegable **Manage Authentication Methods - Corp** (Administrar métodos de autenticación: Corporativos) haga lo siguiente:

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure16.png)

     a. Seleccione **User name and password** (Nombre de usuario y contraseña).

    b. Seleccione **Pass-through from NetScaler Gateway** (Paso a través de puerta de enlace de NetScaler).

    c. Haga clic en **OK**.

14. En el cuadro **Configure Trusted Domains** (Configuración de dominios de confianza), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure17.png)

     a. Seleccione **Trusted domains only** (Solo dominios de confianza).

    b. Haga clic en **Add** (Agregar) para agregar su dominio en el cuadro de texto **Trusted domains** (Dominios de confianza).

    c. Seleccione el dominio predeterminado de la lista **correspondiente**.

    d. Seleccione **Show domains list in logon page** (Mostrar lista de dominios en la página de inicio de sesión).

    e. Haga clic en **OK**.

15. En la ventana emergente **Manage NetScaler Gateways** (Administrar puertas de enlace de NetScaler), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure18.png)

     a. Haga clic en **Add** (Agregar) para agregar las puertas de enlace de NetScaler al cuadro de texto **NetScaler Gateways** (Puertas de enlace de NetScaler).

    b. Haga clic en **Cerrar**.

16. En la pestaña **StoreFront General Settings** (Configuración general de StoreFront), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure19.png)

     a. En el cuadro de texto **Display name** (Nombre para mostrar) escriba el nombre de su puerta de enlace de NetScaler.

    b. En el cuadro de texto **NetScaler Gateway URL** (Dirección URL de puerta de enlace de NetScaler) escriba el nombre de la dirección URL de su puerta de enlace de NetScaler.

    c. Establezca **Usage or role** (Uso o rol) en **Authentication and HDX routing** (Autenticación y enrutamiento HDX).

    d. Haga clic en **OK**.

17. En la pestaña **StoreFront Secure Ticket Authority** (Autoridad del vale de protección de StoreFront), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure20.png)

     a. Haga clic en el botón **Add** (Agregar) para agregar la **dirección URL de la autoridad del vale de protección** en el cuadro de texto correspondiente.

    b. Seleccione **Enable session reliability** (Habilitar confiabilidad de sesión).

    c. Haga clic en **OK**.

18. En la pestaña **StoreFront Authentication Settings** (Configuración de autenticación de StoreFront), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure21.png)

     a. Seleccione la **versión**.

    b. Establezca **Logon Type** (Tipo de inicio de sesión) en **Domain** (Dominio).

    c. Escriba su **dirección URL de devolución de llamada**.

    d. Haga clic en **OK**.

19. En la pestaña **StoreFront Deploy Citrix Receiver** (Receptor de Citrix de implementación de StoreFront), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure22.png)

     a. Establezca **Deployment option** (Opción de implementación) en **Use Receiver for HTML5 if local Receiver is unavailable** (Usar receptor para HTML5 si el receptor local no está disponible).

    b. Haga clic en **OK**.

20. En la ventana emergente **Manage Beacons** (Administrar avisos), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/citrix-netscaler-tutorial/configure23.png)

     a. Establezca **Internal beacon** (Aviso interno) en **Use the service URL** (Usar dirección URL de servicio).

    b. Haga clic en **Add** (Agregar) para agregar la dirección URL en el cuadro de texto **External beacons** (Avisos externos).

    c. Haga clic en **OK**.

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

En esta sección, permitirá que Britta Simon use el inicio de sesión único de Azure, para lo cual le concederá acceso a Citrix Netscaler.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Citrix Netscaler**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Citrix Netscaler**.

    ![Vínculo de Citrix Netscaler en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-citrix-netscaler-test-user"></a>Creación de un usuario de prueba de Citrix Netscaler

En esta sección, creará un usuario llamado Britta Simon en Citrix Netscaler. Citrix Netscaler admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si el usuario no existe ya en Citrix Netscaler, se crea uno después de la autenticación.

>[!NOTE]
>Si necesita crear manualmente un usuario, es preciso que se ponga en contacto con el [equipo de soporte técnico de Citrix Netscaler](https://www.citrix.com/contact/technical-support.html).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Citrix Netscaler en el panel de acceso, debería iniciar sesión automáticamente en la versión de Citrix Netscaler para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

