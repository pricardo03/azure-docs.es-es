---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Citrix NetScaler (autenticación basada en encabezados) | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Citrix NetScaler.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b0adc7bf-696d-44c9-a57a-f9e9471b8710
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/13/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9c442ca731ecb10f977c19b86cb32caded36659
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75470540"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-citrix-netscaler-header-based-authentication"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Citrix NetScaler (autenticación basada en encabezados)

En este tutorial, aprenderá a integrar Citrix NetScaler con Azure Active Directory (Azure AD). Al integrar Citrix NetScaler con Azure AD, puede hacer lo siguiente:

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

- [Configuración del inicio de sesión único de Citrix NetScaler para la autenticación basada en encabezados](#configure-citrix-netscaler-single-sign-on-for-header-based-authentication)

- [Configuración del inicio de sesión único de Citrix NetScaler para la autenticación basada en Kerberos](citrix-netscaler-tutorial.md)


## <a name="adding-citrix-netscaler-from-the-gallery"></a>Incorporación de Citrix NetScaler desde la galería

Para configurar la integración de Citrix NetScaler en Azure AD, deberá agregar Citrix NetScaler desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Citrix NetScaler** en el cuadro de búsqueda.
1. Seleccione **Citrix NetScaler** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-single-sign-on-for-citrix-netscaler"></a>Configuración y prueba del inicio de sesión único de Azure AD para Citrix NetScaler

Configure y pruebe el inicio de sesión único de Azure AD con Citrix NetScaler mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Citrix NetScaler.

Para configurar y probar el inicio de sesión único de Azure AD con Citrix NetScaler, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de Citrix NetScaler](#configure-citrix-netscaler-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Citrix NetScaler](#create-citrix-netscaler-test-user)** : para tener un homólogo de B.Simon en Citrix NetScaler que esté vinculado a la representación del usuario en Azure AD.
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

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Dirección URL de metadatos de federación de aplicación**, copie la dirección URL y guárdela en el Bloc de notas.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. La aplicación Citrix NetScaler espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados. Haga clic en el icono **Editar** y cambie la asignación de atributos.

    ![imagen](common/edit-attribute.png)

1. Además de lo anterior, la aplicación Citrix NetScaler espera que se devuelvan algunos atributos más en la respuesta de SAML. En la sección Notificaciones del usuario del cuadro de diálogo Atributos de usuario, haga lo siguiente para agregar el atributo Token SAML como se muestra en la tabla siguiente:

    | Nombre | Atributo de origen|
    | ---------------| --------------- |
    | mySecretID  | user.userprincipalname |
    
    1. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    1. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    1. Deje **Espacio de nombres** en blanco.

    1. Seleccione **Atributo** como origen.

    1. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    1. Haga clic en **Aceptar**.

    1. Haga clic en **Save**(Guardar).


1. En la sección **Set up Citrix NetScaler** (Configurar Citrix NetScaler), copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, va a conceder a B.Simon acceso a Citrix NetScaler mediante el inicio de sesión único de Azure.

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

- [Configuración del inicio de sesión único de Citrix NetScaler para la autenticación basada en encabezados](#configure-citrix-netscaler-single-sign-on-for-header-based-authentication)

- [Configuración del inicio de sesión único de Citrix NetScaler para la autenticación basada en Kerberos](citrix-netscaler-tutorial.md)

### <a name="publishing-web-server"></a>Publicación del servidor web 

1. Cree un **servidor virtual**.

    a. Vaya a **Administración del tráfico > Equilibrio de carga > Servicios**.
    
    b. Haga clic en **Agregar**.

    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/web01.png)

    c. Especifique los detalles del servidor web que ejecuta las aplicaciones a continuación:
    * **Nombre del servicio**
    * **IP de servidor/ Servidor existente**
    * **Protocolo**
    * **Puerto**

     ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/web01.png)

### <a name="configuring-load-balancer"></a>Configuración de Load Balancer

1. Para configurar Load Balancer, realice los pasos siguientes:

    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/load01.png)

    a. Vaya a **Administración del tráfico > Equilibrio de carga > Servidores virtuales**.

    b. Haga clic en **Agregar**.

    c. Especifique los detalles siguientes:

    * **Nombre**
    * **Protocolo**
    * **Dirección IP**
    * **Puerto**
    * Haga clic en **Aceptar**.

### <a name="bind-virtual-server"></a>Enlace del servidor virtual

Enlace Load Balancer con el servidor virtual creado anteriormente.

![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/bind01.png)

![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/bind02.png)

### <a name="bind-certificate"></a>Enlace del certificado

Dado que este servicio se va a publicar como SSL, enlace el certificado de servidor y, a continuación, pruebe la aplicación.

![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/bind03.png)

![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/bind04.png)

## <a name="citrix-adc-saml-profile"></a>Perfil de SAML de Citrix ADC

### <a name="create-authentication-policy"></a>Creación de la directiva de autenticación

1. Vaya a **Seguridad > AAA: Tráfico de aplicación > Directivas > Autenticación > Directivas de autenticación**.

2. Haga clic en **Agregar** y, a continuación, especifique los detalles.

    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/policy01.png)

    a. Nombre de la **Directiva de autenticación**.

    b. Expresión: **true**.

    c. Tipo de acción **SAML**.

    d. Acción = haga clic en **Agregar** (siga el asistente para crear el servidor SAML de autenticación).
    
    e. Haga clic en Crear en la **Directiva de autenticación**.

### <a name="create-authentication-saml-server"></a>Creación del servidor SAML de autenticación

1. Lleve a cabo los siguiente pasos:

    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/server01.png)

    a. Especifique el **Nombre**.

    b. Importar metadatos (especifique la dirección URL de metadatos de federación de la interfaz de usuario de SAML de Azure que ha copiado anteriormente).
    
    c. Especifique el **Nombre del emisor**.

    d. Haga clic en **Crear**.

### <a name="create-authentication-virtual-server"></a>Creación del servidor virtual de autenticación

1.  Vaya a **Seguridad > AAA: Tráfico de aplicación >> Servidores virtuales de autenticación**.

2.  Haga clic en **Agregar** y realice los siguientes pasos:

    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/server02.png)

    a.  Especifique un **nombre**.

    b.  Elija **No direccionable**.

    c.  Protocolo **SSL**.

    d.  Haga clic en **OK**.

    e.  Haga clic en **Continuar**.

### <a name="configure-the-authentication-virtual-server-to-use-azure-ad"></a>Configuración del servidor virtual de autenticación para usar Azure AD

Deberá modificar las 2 secciones del servidor virtual de autenticación.

1.  **Directivas de autenticación avanzadas**

    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/virtual01.png)

    a. Seleccione la **Directiva de autenticación** que creó anteriormente.

    b. Haga clic en **Enlazar**.

      ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/virtual02.png)

2. **Servidores virtuales basados en formularios**

    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/virtual03.png)

    a.  Deberá proporcionar un **nombre de dominio completo**, ya que lo exige la interfaz de usuario.

    b.  Elija el **equilibrador de carga del servidor virtual** que desea proteger con la Autenticación de Azure AD.

    c.  Haga clic en **Enlazar**.

    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/virtual04.png)

    >[!NOTE]
    >Asegúrese también de hacer clic en **Listo** en la página de configuración del servidor virtual de autenticación.

3. Compruebe los cambios. Vaya a la dirección URL de la aplicación. Debería ver la página de inicio de sesión del inquilino en lugar del acceso no autenticado anterior.

    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/virtual05.png)

## <a name="configure-citrix-netscaler-single-sign-on-for-header-based-authentication"></a>Configuración del inicio de sesión único de Citrix NetScaler para la autenticación basada en encabezados

### <a name="citrix-adc-configuration"></a>Configuración de Citrix ADC

### <a name="create-an-rewrite-action"></a>Creación de una acción de reescritura

1. Vaya a **AppExpert > Reescritura > Acciones de reescritura**.
 
    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/header01.png)

2.  Haga clic en **Agregar**.

    a.  Especifique el **Nombre**.

    b.  Tipo = **INSERT_HTTP_HEADER**.

    c.  Especifique el **Nombre del encabezado** (SecretID en este ejemplo).

    d.  Especifique la expresión **aaa.USER.ATTRIBUTE(`mySecretID`)** , donde **mySecretID** es la notificación de SAML de Azure AD enviada a Citrix ADC.

    e.  Haga clic en **Crear**.

    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/header02.png)
 
### <a name="create-a-rewrite-policy"></a>Cree una directiva de reescritura.

1.  Vaya a **AppExpert > Reescritura > Directivas de reescritura**.
 
    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/header03.png)

2.  Haga clic en **Agregar**.

    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/header04.png)

    a.  Especifique el **Nombre**.

    b.  Elija la **acción** creada anteriormente.

    c. Como expresión, especifique **true**.

    d.  Haga clic en **Crear**.

### <a name="bind-rewrite-policy-to-virtual-servers"></a>Enlace de la directiva de reescritura a los servidores virtuales

1. Para enlazar una directiva de reescritura a un servidor virtual específico mediante la GUI.

2. Vaya a **Administración del tráfico > Equilibrio de carga > Servidores virtuales**.

3. En la lista del panel de detalles de los servidores virtuales, seleccione el **servidor virtual** al que desea enlazar la directiva de reescritura y, a continuación, haga clic en **Abrir**.

4. En el cuadro de diálogo Configurar servidor virtual (Equilibrio de carga), seleccione la pestaña **Directivas**. Todas las directivas configuradas en NetScaler aparecen en la lista.
 
    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/header05.png)

    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/header06.png)

5.  Seleccione la **casilla de verificación** situada junto al nombre de la directiva que desea enlazar a este servidor virtual.

    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/header07.png)

    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/header08.png)
 
6.  Haga clic en **OK**. Aparece un mensaje en la barra de estado que indica que la directiva se ha configurado correctamente.

### <a name="modify-saml-server-to-extract-attributes-from-claim"></a>Modificación del servidor SAML para extraer atributos de la notificación

1.  Vaya a **Seguridad > AAA: Tráfico de aplicación > Directivas > Autenticación > Directivas avanzadas > Acciones > Servidores**.

2.  Seleccione el **servidor SAML de autenticación** adecuado para la aplicación.
 
    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/header09.png)

3. En la sección Atributos, escriba los atributos de SAML que desea extraer mediante " , ". En este caso, se especifica el atributo llamado **mySecretID**.
 
    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/header10.png)

4. Compruebe el acceso a las aplicaciones.

    ![Configuración de Citrix NetScaler](./media/header-citrix-netscaler-tutorial/header11.png)

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

- [Pruebe Citrix NetScaler con Azure AD](https://aad.portal.azure.com/).

- [Configuración del inicio de sesión único de Citrix NetScaler para la autenticación basada en Kerberos](citrix-netscaler-tutorial.md)
