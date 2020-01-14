---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Citrix NetScaler (autenticación basada en Kerberos) | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Citrix NetScaler.
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
ms.date: 12/13/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 75e825f55a890be49000e209859670caa2c1c875
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75431215"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-citrix-netscaler-kerberos-based-authentication"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Citrix NetScaler (autenticación basada en Kerberos)

En este tutorial, aprenderá a integrar Citrix NetScaler con Azure Active Directory (Azure AD). Al integrar Citrix NetScaler con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Citrix NetScaler.
* Permitir que los usuarios inicien sesión automáticamente en Citrix NetScaler con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Prerequisites

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Citrix NetScaler.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Citrix NetScaler admite el inicio de sesión único iniciado por **SP**.

* Citrix NetScaler admite aprovisionamiento de usuarios **Just-In-Time**.

- [Configuración del inicio de sesión único de Citrix NetScaler para la autenticación basada en Kerberos](#configure-citrix-netscaler-single-sign-on-for-kerberos-based-authentication)

- [Configuración del inicio de sesión único de Citrix NetScaler para la autenticación basada en encabezados](header-citrix-netscaler-tutorial.md)

## <a name="adding-citrix-netscaler-from-the-gallery"></a>Incorporación de Citrix NetScaler desde la galería

Para configurar la integración de Citrix NetScaler en Azure AD, deberá agregar Citrix NetScaler desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Citrix NetScaler** en el cuadro de búsqueda.
1. Seleccione **Citrix NetScaler** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-citrix-netscaler"></a>Configuración y prueba del inicio de sesión único de Azure AD para Citrix NetScaler

Configure y pruebe el inicio de sesión único de Azure AD con Citrix NetScaler mediante una usuaria de prueba llamada **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Citrix NetScaler.

Para configurar y probar el inicio de sesión único de Azure AD con Citrix NetScaler, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Citrix NetScaler](#configure-citrix-netscaler-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Citrix NetScaler](#create-citrix-netscaler-test-user)** , para tener un homólogo de B.Simon en Citrix NetScaler que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Citrix NetScaler**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<<Your FQDN>>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<<Your FQDN>>/CitrixAuthService/AuthService.asmx`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<<Your FQDN>>/CitrixAuthService/AuthService.asmx`

    > [!NOTE]
    > Estos valores no son reales. Actualícelos con la dirección URL de inicio de sesión, el identificador y la dirección URL de respuesta reales. Póngase en contacto con el [equipo de soporte técnico de Citrix NetScaler](https://www.citrix.com/contact/technical-support.html) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

    > [!NOTE]
    > Para que funcione el inicio de sesión único, estas direcciones URL deben ser accesibles desde sitios públicos. Deberá habilitar el firewall u otras opciones de seguridad en Netscaler para permitir que Azure AD publique el token en la dirección URL de ACS configurada.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Dirección URL de metadatos de federación de aplicación**, copie la dirección URL y guárdela en el Bloc de notas.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Set up Citrix NetScaler** (Configurar Citrix NetScaler), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a conceder a B.Simon acceso a Citrix NetScaler utilizando el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Citrix NetScaler**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-citrix-netscaler-sso"></a>Configuración del inicio de sesión único de Citrix NetScaler

- [Configuración del inicio de sesión único de Citrix NetScaler para la autenticación basada en Kerberos](#configure-citrix-netscaler-single-sign-on-for-kerberos-based-authentication)

- [Configuración del inicio de sesión único de Citrix NetScaler para la autenticación basada en encabezados](header-citrix-netscaler-tutorial.md)

### <a name="publishing-web-server"></a>Publicación del servidor web 

1. Cree un **servidor virtual**.

    a. Vaya a **Traffic Management (Administración del tráfico) > Load Balancing (Equilibrio de carga) > Services (Servicios)** .
    
    b. Haga clic en **Agregar**.

    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/web01.png)

    c. Especifique los detalles del servidor web que ejecuta las aplicaciones siguientes:
    * **Nombre del servicio**
    * **IP de servidor/ Servidor existente**
    * **Protocolo**
    * **Puerto**

     ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/web01.png)

### <a name="configuring-load-balancer"></a>Configuración de Load Balancer

1. Para configurar Load Balancer, realice los pasos siguientes:

    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/load01.png)

    a. Vaya a **Traffic Management (Administración del tráfico) > Load Balancing (Equilibrio de carga) > Virtual Servers (Servidores virtuales)** .

    b. Haga clic en **Agregar**.

    c. Especifique los detalles siguientes:

    * **Nombre**
    * **Protocolo**
    * **Dirección IP**
    * **Puerto**
    * Haga clic en **ok** (Aceptar).

### <a name="bind-virtual-server"></a>Enlace del servidor virtual

Enlace Load Balancer con el servidor virtual creado anteriormente.

![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/bind01.png)

![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/bind02.png)

### <a name="bind-certificate"></a>Enlace del certificado

Dado que se va a publicar este servicio como SSL, enlace el certificado de servidor y entonces pruebe la aplicación.

![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/bind03.png)

![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/bind04.png)

## <a name="citrix-adc-saml-profile"></a>Perfil SAML de ADC de Citrix

### <a name="create-authentication-policy"></a>Creación de la directiva de autenticación

1. Vaya a **Security (Seguridad) > AAA – Application Traffic (Tráfico de la aplicación) > Policies (Directivas) > Authentication (Autenticación) > Authentication Policies (Directivas de autenticación)** .

2. Haga clic en **Add** (Agregar) y, a continuación, especifique los detalles.

    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/policy01.png)

    a. Name (Nombre) para el nombre de la **Directiva de autenticación**.

    b. Expression (Expresión): **true**.

    c. Action type (Tipo de acción) **SAML**.

    d. Action (Acción) = haga clic en **Add** (Agregar) (siga el Asistente para crear el servidor SAML de autenticación).
    
    e. Haga clic en Create (Crear) en la **Authentication Policy** (Directiva de autenticación).

### <a name="create-authentication-saml-server"></a>Creación del servidor SAML de autenticación

1. Lleve a cabo los siguiente pasos:

    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/server01.png)

    a. Especifique **Name** (Nombre).

    b. Importe los metadatos (especifique la dirección URL de metadatos de federación de la interfaz de usuario de SAML de Azure que ha copiado anteriormente).
    
    c. Especifique **Issuer Name** (Nombre del emisor).

    d. Haga clic en **create** (crear).

### <a name="create-authentication-virtual-server"></a>Creación del servidor virtual de autenticación

1.  Vaya a **Seguridad > AAA: Tráfico de aplicación >> Servidores virtuales de autenticación**.

2.  Haga clic en **Agregar** y realice los siguientes pasos:

    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/server02.png)

    a.  Especifique un **nombre**.

    b.  Elija **Non-Addressable** (No direccionable).

    c.  Protocolo **SSL**.

    d.  Haga clic en **OK**.

    e.  Haga clic en **Continuar**.

### <a name="configure-the-authentication-virtual-server-to-use-azure-ad"></a>Configuración del servidor virtual de autenticación para usar Azure AD

Deberá modificar las dos secciones del servidor virtual de autenticación.

1.  **Advanced Authentication Policies** (Directivas de autenticación avanzadas)

    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/virtual01.png)

    a. Seleccione la **directiva de autenticación** que creó anteriormente.

    b. Haga clic en **Bind** (Enlazar).

      ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/virtual02.png)

2. **Servidores virtuales basados en formularios**

    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/virtual03.png)

    a.  Deberá proporcionar un **FQDN** (Nombre de dominio completo), ya que lo exige la interfaz de usuario.

    b.  Elija el **Virtual Server Load Balancer** (Equilibrador de carga del servidor virtual) que desea proteger con la Autenticación de Azure AD.

    c.  Haga clic en **Bind** (Enlazar).

    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/virtual04.png)

    >[!NOTE]
    >Asegúrese también de hacer clic en **Done** (Listo) en la página Authentication Virtual Server Configuration (Configuración del servidor virtual de autenticación).

3. Compruebe los cambios. Vaya a la dirección URL de la aplicación. Debería ver la página de inicio de sesión del inquilino en lugar del acceso no autenticado anterior.

    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/virtual05.png)

## <a name="configure-citrix-netscaler-single-sign-on-for-kerberos-based-authentication"></a>Configuración del inicio de sesión único de Citrix NetScaler para la autenticación basada en Kerberos

### <a name="create-a-kerberos-delegation-account-for-citrix-adc"></a>Creación de una cuenta de delegación de Kerberos para Citrix ADC

1. Cree una cuenta de usuario (en este ejemplo, AppDelegation).

    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/kerberos01.png)

2. Configure un SPN de HOST en estas cuentas.

    * setspn -S HOST/AppDelegation.IDENTT.WORK identt\appdelegation
    
        En el ejemplo anterior

        a. Identt.work    (FQDN de dominio)

        b. Identt        (nombre NetBIOS del dominio)

        c. AppDelegation (nombre de la cuenta de usuario de delegación)

3. Configuración de la delegación para el servidor WebServer 
 
    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/kerberos02.png)

    >[!NOTE]
    >En el ejemplo anterior, el nombre del servidor web interno que ejecuta el sitio WIA es cweb2

### <a name="citrix-aaa-kcd--kerberos-delegation-accounts"></a>Citrix AAA KCD (cuentas de delegación de Kerberos)

1.  Vaya a **Citrix Gateway (Puerta de enlace de Citrix) > AAA KCD (Kerberos Constrained Delegation) (Delegación limitada de Kerberos)** .

2.  Haga clic en Add (Agregar) y especifique los detalles siguientes:

    a.  Especifique **Name** (Nombre).

    b.  **Realm** (Territorio)

    c.  **Service SPN** `http/<host/fqdn>@DOMAIN.COM` (SPN de servicio).
    
    >[!NOTE]
    >@DOMAIN.com es obligatorio y se especifica en mayúsculas.

    d.  Especifique **Delegated user account** (Cuenta de usuario delegada).

    e.  Compruebe la contraseña del usuario delegado y especifique **Password** (Contraseña).

    f.  Haga clic en **OK**.
 
    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/kerberos03.png)

### <a name="citrix-traffic-policy-and-traffic-profile"></a>Directiva de tráfico y perfil de tráfico de Citrix

1.  Vaya a **Security (Seguridad) > AAA - Application Traffic (Tráfico de la aplicación) > Policies (Directivas) > Traffic Policies (Directivas de tráfico), Profiles (Perfiles) y Form SSO Profiles (Perfiles SSO de formulario) > Traffic Policies (Directivas de tráfico)** .

2.  Seleccione **Traffic Profiles** (Perfiles de tráfico).

3.  Haga clic en **Agregar**.

4.  Configure el perfil de tráfico.

    a.  Especifique **Name** (Nombre).

    b.  Especifique **Single Sign-on** (Inicio de sesión único).

    c.  Especifique la **cuenta KCD** creada en el paso anterior desde el menú desplegable.

    d.  Haga clic en **OK**.

    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/kerberos04.png)
 
5.  Seleccione **Traffic Policy** (Directiva de tráfico).

6.  Haga clic en **Agregar**.

7.  Configure la directiva de tráfico.

    a.  Especifique **Name** (Nombre).

    b.  Elija el **perfil de tráfico** creado anteriormente en la lista desplegable.

    c.  Establezca la expresión en **true**.

    d.  Haga clic en **Aceptar**.

    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/kerberos05.png)

### <a name="citrix-bind-traffic-policy-to-virtual-servers"></a>Enlace de la directiva de tráfico de Citrix a servidores virtuales

Para enlazar una directiva de tráfico a un servidor virtual específico mediante la GUI.

* Vaya a **Traffic Management (Administración del tráfico) > Load Balancing (Equilibrio de carga) > Virtual Servers (Servidores virtuales)** .

* En la lista del panel de detalles de los servidores virtuales, seleccione el **servidor virtual** al que desea enlazar la directiva de reescritura y, a continuación, haga clic en **Open** (Abrir).

* En el cuadro de diálogo Configure Virtual Server (Configurar servidor virtual) (Load Balancing) [(Equilibrio de carga)], seleccione la pestaña **Policies** (Directivas). Todas las directivas configuradas en NetScaler aparecen en la lista.
 
    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/kerberos06.png)

    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/kerberos07.png)

1.  Active la **casilla** situada junto al nombre de la directiva que desea enlazar a este servidor virtual.
 
    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/kerberos08.png)

    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/kerberos09.png)

1. Solo está enlazada la directiva; haga clic en **Done** (Listo).
 
    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/kerberos10.png)

1. Pruebe con el sitio web integrado de Windows.

    ![Configuración de Citrix NetScaler](./media/citrix-netscaler-tutorial/kerberos11.png)    

### <a name="create-citrix-netscaler-test-user"></a>Creación de un usuario de prueba de Citrix NetScaler

En esta sección, se crea un usuario llamado B.Simon en Citrix NetScaler. Citrix NetScaler admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si el usuario no existe ya en Citrix NetScaler, se crea uno después de la autenticación.

> [!NOTE]
> Si necesita crear manualmente un usuario, es preciso que se ponga en contacto con el [equipo de soporte técnico de Citrix NetScaler](https://www.citrix.com/contact/technical-support.html).

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Citrix NetScaler en el Panel de acceso, debería iniciar sesión automáticamente en la instancia de Citrix NetScaler para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Pruebe Citrix NetScaler con Azure AD](https://aad.portal.azure.com/)

- [Configuración del inicio de sesión único de Citrix NetScaler para la autenticación basada en encabezados](header-citrix-netscaler-tutorial.md)
