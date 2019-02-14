---
title: 'Tutorial: Integración de Azure Active Directory con SharePoint local | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SharePoint local.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 85b8d4d0-3f6a-4913-b9d3-8cc327d8280d
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9491ecfe60e4e15fdf0d1e7b2c8ea05bc9eebf9
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56198791"
---
# <a name="tutorial-azure-active-directory-integration-with-sharepoint-on-premises"></a>Tutorial: Integración de Azure Active Directory con SharePoint local

En este tutorial se aprende a integrar SharePoint local con Azure Active Directory (Azure AD).
La integración de SharePoint local con Azure AD proporciona las siguientes ventajas:

* En Azure AD, puede controlar quién tiene acceso a SharePoint local.
* Puede permitir que los usuarios inicien sesión automáticamente en SharePoint local (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con SharePoint local, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción habilitada para inicio de sesión único de SharePoint local

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* SharePoint local admite el inicio de sesión único iniciado por **SP**.

## <a name="adding-sharepoint-on-premises-from-the-gallery"></a>Incorporación de SharePoint local desde la galería

Para configurar la integración de SharePoint local en Azure AD, tiene que agregar SharePoint local desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar SharePoint local desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **SharePoint local**, seleccione **SharePoint local** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

     ![SharePoint local en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, se configura y se prueba el inicio de sesión único de Azure AD con SharePoint local con un usuario de prueba llamada **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de SharePoint local.

Para configurar y probar el inicio de sesión único de Azure AD con SharePoint local, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de SharePoint local](#configure-sharepoint-on-premises-single-sign-on)**: para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de SharePoint local](#grant-access-to-sharepoint-on-premises-test-user)**: para tener un homólogo de Britta Simon en SharePoint local que esté vinculado a su representación en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con SharePoint local, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **SharePoint local**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de inicio de sesión único de Dominio y direcciones URL de SharePoint local](common/sp-identifier-reply.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<YourSharePointServerURL>/_trust/default.aspx`

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `urn:sharepoint:federation`

    c. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<YourSharePointServerURL>/_trust/default.aspx`

    > [!NOTE]
    > Estos valores no son reales. Actualícelos con la dirección URL de inicio de sesión, el identificador y la dirección URL de respuesta reales. Póngase en contacto con el [equipo de atención al cliente de SharePoint local](https://support.office.com/) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

    > [!Note]
    > Anote la ruta de acceso de archivo en la que descargó el archivo de certificado. Necesitará el archivo más adelante en el script de PowerShell para la configuración.

6. En la sección **Set up SharePoint on-premises** (Configurar SharePoint local), copie las direcciones URL que necesite. En **Dirección URL del servicio de inicio de sesión único**, use un valor del siguiente patrón: `https://login.microsoftonline.com/_my_directory_id_/wsfed` 

    > [!Note]
    > _my_directory_id_ es el identificador de inquilino de la suscripción de Azure AD.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    > [!NOTE]
    > La aplicación SharePoint local usa el token SAML 1.1, por lo que Azure AD espera la solicitud de WS Fed del servidor de SharePoint y, después de la autenticación, emite el token. SAML 1.1.

### <a name="configure-sharepoint-on-premises-single-sign-on"></a>Configuración de inicio de sesión único de SharePoint local

1. En otra ventana del explorador web, inicie sesión en el sitio de la empresa de SharePoint local como administrador.

2. **Configurar un nuevo proveedor de identidades de confianza en SharePoint Server 2016**

    Inicie sesión en el servidor de SharePoint Server 2016 y abra el Shell de administración de SharePoint 2016. Rellene los valores de $realm (valor de identificador de la sección de dominio y direcciones URL locales de SharePoint de Azure Portal), $wsfedurl (dirección URL del servicio de inicio de sesión único) y $filepath (ruta de acceso del archivo en la que ha descargado el archivo de certificado) en Azure Portal y ejecute los siguientes comandos para configurar un nuevo proveedor de identidades de confianza.

    > [!TIP]
    > Si no está familiarizado con PowerShell o quiere obtener más información sobre el funcionamiento de PowerShell, vea [SharePoint PowerShell](https://docs.microsoft.com/powershell/sharepoint/overview?view=sharepoint-ps). 

    ```
    $realm = "<Identifier value from the SharePoint on-premises Domain and URLs section in the Azure portal>"
    $wsfedurl="<SAML single sign-on service URL value which you have copied from the Azure portal>"
    $filepath="<Full path to SAML signing certificate file which you have downloaded from the Azure portal>"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname" -IncomingClaimTypeDisplayName "GivenName" -SameAsIncoming
    $map3 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" -IncomingClaimTypeDisplayName "SurName" -SameAsIncoming
    $map4 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress" -IncomingClaimTypeDisplayName "Email" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "SharePoint secured by Azure AD" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map,$map2,$map3,$map4 -SignInUrl $wsfedurl -IdentifierClaim "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"
    ```

    Luego siga estos pasos para habilitar al proveedor de identidades de confianza para la aplicación:

     a. En Administración central, vaya a **Administrar aplicación web** y seleccione la aplicación web que quiere proteger con Azure AD.

    b. En la cinta, haga clic en **Proveedores de autenticación** y elija la zona que quiere usar.

    c. Seleccione **Proveedor de identidad de confianza** y luego seleccione el proveedor de identidades que acaba de registrar, denominado *AzureAD*.

    d. En la opción de dirección URL de la página de inicio de sesión, seleccione **Página de inicio de sesión personalizada** y proporcione el valor "/_trust/".

    e. Haga clic en **OK**.

    ![Configuración del proveedor de autenticación](./media/sharepoint-on-premises-tutorial/fig10-configauthprovider.png)

    > [!NOTE]
    > Algunos de los usuarios externos no podrán usar esta integración de inicio de sesión único dado que su UPN habrá trastocado el valor de esta forma: `MYEMAIL_outlook.com#ext#@TENANT.onmicrosoft.com`. En breve permitiremos una configuración personalizada de la aplicación para administrar el UPN en función del tipo de usuario. Después de eso, todos los usuarios invitados podrán usar el inicio de sesión único sin problemas igual que los empleados de la organización.

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

### <a name="grant-access-to-sharepoint-on-premises-test-user"></a>Conceder acceso al usuario de prueba de SharePoint local

Los usuarios que inicien sesión en Azure AD y accedan a SharePoint deben tener acceso a la aplicación. Siga estos pasos para establecer los permisos para acceder a la aplicación web.

1. En Administración central, haga clic en **Administración de aplicaciones**.

2. En la página **Administración de aplicaciones**, en la sección **Aplicaciones web**, haga clic en **Administrar aplicaciones web**.

3. Haga clic en la aplicación web adecuada y luego en **Directiva de usuario**.

4. En Directiva para aplicación web, haga clic en **Agregar usuarios**.

    ![Búsqueda de un usuario por su notificación de nombre](./media/sharepoint-on-premises-tutorial/fig11-searchbynameclaim.png)

5. En el cuadro de diálogo **Agregar usuarios**, haga clic en la zona que corresponda de **Zonas** y luego en **Siguiente**.

6. En el cuadro de diálogo **Directiva para aplicación web**, en la sección **Elegir usuarios**, haga clic en el icono **Examinar**.

7. En el cuadro de texto **Buscar**, escriba el valor de **nombre principal de usuario (UPN)** para el que ha configurado la aplicación de SharePoint local en Azure AD y haga clic en **Buscar**. </br>Ejemplo: *brittasimon@contoso.com*.

8. En el encabezado AzureAD de la vista de lista, seleccione la propiedad de nombre y haga clic en **Agregar**; luego, haga clic en **Aceptar** para cerrar el cuadro de diálogo.

9. En Permisos, haga clic en **Control total**.

    ![Concesión de control total a un usuario de notificaciones](./media/sharepoint-on-premises-tutorial/fig12-grantfullcontrol.png)

10. Haga clic en **Finalizar** y, después, en **Aceptar**.

### <a name="configuring-one-trusted-identity-provider-for-multiple-web-applications"></a>Configuración de un proveedor de identidades de confianza para varias aplicaciones web

La configuración funciona para una sola aplicación web, pero se necesitará configuración adicional si va a usar el mismo proveedor de identidades de confianza con varias aplicaciones web. Por ejemplo, suponga que se hubiese ampliado una aplicación web para usar la dirección URL `https://portal.contoso.local` y ahora también quiere autenticar a los usuarios en `https://sales.contoso.local`. Para ello, se deberá actualizar el proveedor de identidades para respetar el parámetro WReply y actualizar el registro de aplicación en Azure AD para agregar una dirección URL de respuesta.

1. En Azure Portal, abra el directorio de Azure AD. Haga clic en **Registros de aplicaciones** y, luego, haga clic en **Ver todas las aplicaciones**. Haga clic en la aplicación que creó anteriormente (integración de SAML de SharePoint).

2. Haga clic en **Configuración**.

3. En la hoja de configuración, haga clic en **URL de respuesta**. 

4. Agregue la dirección URL de la aplicación web adicional con `/_trust/default.aspx` anexado a la dirección URL (por ejemplo, `https://sales.contoso.local/_trust/default.aspx`) y haga clic en **Guardar**.

5. En el servidor de SharePoint, abra la **consola de administración de SharePoint 2016** y ejecute los comandos siguientes, utilizando el nombre del emisor de tokens de identidad de confianza que usó anteriormente.

    ```
    $t = Get-SPTrustedIdentityTokenIssuer "AzureAD"
    $t.UseWReplyParameter=$true
    $t.Update()
    ```
6. En Administración Central, vaya a la aplicación web y habilite el proveedor de identidades de confianza existente. No olvide configurar también la dirección URL de la página de inicio de sesión como una página de inicio de sesión personalizada `/_trust/`.

7. En Administración Central, haga clic en la aplicación web y elija **Directiva de usuario**. Agregue un usuario con los permisos adecuados, como se demostró anteriormente en este artículo.

### <a name="fixing-people-picker"></a>Corrección del selector de personas

Los usuarios ahora pueden iniciar sesión en SharePoint 2016 mediante identidades de Azure AD, pero todavía hay oportunidades de mejora de la experiencia del usuario. Por ejemplo, la búsqueda de un usuario da lugar a varios resultados de búsqueda en el selector de personas. Hay un resultado de búsqueda por cada uno de los tres tipos de notificación creados en la asignación de notificación. Para elegir un usuario mediante el selector de personas, debe escribir su nombre de usuario exactamente y elegir el resultado de notificación **Nombre**.

![Resultados de búsqueda de notificaciones](./media/sharepoint-on-premises-tutorial/fig16-claimssearchresults.png)

No hay ninguna validación de los valores que se buscan, lo cual puede dar lugar a errores ortográficos o a que los usuarios elijan accidentalmente el tipo de notificación incorrecto para asignar, como la notificación **Apellido**. Esto puede evitar que los usuarios accedan correctamente a los recursos.

Para ayudar en este caso, hay una solución de código abierto denominada [AzureCP](https://yvand.github.io/AzureCP/) que proporciona un proveedor de notificaciones personalizado para SharePoint 2016. Usa Azure AD Graph para solucionar errores en lo que escriben los usuarios y realizar la validación. Más información en [AzureCP](https://yvand.github.io/AzureCP/).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, se habilita a Britta Simon para que use el inicio de sesión único de Azure, para lo cual se le concede acceso a SharePoint local.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, a continuación, seleccione **SharePoint local**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **SharePoint local**.

    ![Vínculo de SharePoint local en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-sharepoint-on-premises-test-user"></a>Creación de un usuario de prueba de SharePoint local

En esta sección, creará un usuario llamado Britta Simon en SharePoint local. Trabaje con el  [equipo de soporte técnico de SharePoint local](https://support.office.com/) para agregar a los usuarios a la plataforma de SharePoint local. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de SharePoint local en el panel de acceso, debería iniciar sesión automáticamente en la versión de SharePoint local para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
