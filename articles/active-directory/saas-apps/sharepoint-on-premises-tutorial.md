---
title: 'Tutorial: Integración de Azure Active Directory con SharePoint local | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SharePoint local.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 85b8d4d0-3f6a-4913-b9d3-8cc327d8280d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2018
ms.author: jeedes
ms.openlocfilehash: 9c63808af8d883badd379cf8bc3372a1d65d6624
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52968667"
---
# <a name="tutorial-azure-active-directory-integration-with-sharepoint-on-premises"></a>Tutorial: Integración de Azure Active Directory con SharePoint local

En este tutorial se aprende a integrar SharePoint local con Azure Active Directory (Azure AD).

La integración de SharePoint local con Azure AD proporciona las siguientes ventajas:

- En Azure AD, puede controlar quién tiene acceso a SharePoint local.
- Puede permitir que los usuarios inicien sesión automáticamente en SharePoint local (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con SharePoint local, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único de SharePoint local

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.
El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de SharePoint local desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-sharepoint-on-premises-from-the-gallery"></a>Incorporación de SharePoint local desde la galería

Para configurar la integración de SharePoint local en Azure AD, tiene que agregar SharePoint local desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar SharePoint local desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **SharePoint local**, seleccione **SharePoint local** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![SharePoint local en la lista de resultados](./media/sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, se configura y se prueba el inicio de sesión único de Azure AD con SharePoint local con una usuaria de prueba llamada "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de SharePoint local para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de SharePoint local.

Para configurar y probar el inicio de sesión único de Azure AD con SharePoint local, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de SharePoint local](#grant-access-to-sharePoint-on-premises-test-user)**: para tener un homólogo de Britta Simon en SharePoint local que esté vinculado a su representación en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, se habilita el inicio de sesión único de Azure AD en Azure Portal y se configura el inicio de sesión único en la aplicación SharePoint local.

**Para configurar el inicio de sesión único de Azure AD con SharePoint local, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **SharePoint local**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_samlbase.png)

3. En la sección **Dominio y direcciones URL de SharePoint local**, realice los pasos siguientes:

    ![Información de inicio de sesión único de Dominio y direcciones URL de SharePoint local](./media/sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_url1.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<YourSharePointServerURL>/_trust/default.aspx`.

    b. En el cuadro de texto **Identificador**, escriba la dirección URL: `urn:sharepoint:federation`

    c. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<YourSharePointServerURL>/_trust/default.aspx`.

4. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_certificate.png)

    > [!Note]
    > Anote la ruta de acceso del archivo en la que ha descargado el archivo de certificado, ya que la usará más tarde para la configuración en el script de PowerShell.

5. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/sharepoint-on-premises-tutorial/tutorial_general_400.png)

6. En la sección **Configuración de SharePoint local**, haga clic en **Configurar SharePoint local** para abrir la ventana **Configurar inicio de sesión**. Copie el valor de **Identificador de entidad de SAML** de la **sección Referencia rápida** En **Dirección URL del servicio de inicio de sesión único**, use un valor del siguiente patrón: `https://login.microsoftonline.com/_my_directory_id_/wsfed` 

    > [!Note]
    > _my_directory_id_ es el identificador de inquilino de la suscripción de Azure AD.

    ![Configuración de SharePoint local](./media/sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_configure.png)

    > [!NOTE]
    > La aplicación SharePoint local usa el token SAML 1.1, por lo que Azure AD espera la solicitud de WS Fed del servidor de SharePoint y, después de la autenticación, emite el token. SAML 1.1.

7. En otra ventana del explorador web, inicie sesión en el sitio de la empresa de SharePoint local como administrador.

8. **Configurar un nuevo proveedor de identidades de confianza en SharePoint Server 2016**

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
    > Algunos de los usuarios externos no podrán usar esta integración de inicio de sesión único dado que su UPN habrá trastocado el valor de esta forma: `MYEMAIL_outlook.com#ext#@TENANT.onmicrosoft.com`. En breve permitiremos a los clientes configurar la aplicación para administrar el UPM en función del tipo de usuario. Después de eso, todos los usuarios invitados podrán usar el inicio de sesión único sin problemas igual que los empleados de la organización.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/sharepoint-on-premises-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/sharepoint-on-premises-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/sharepoint-on-premises-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/sharepoint-on-premises-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

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

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a SharePoint local, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **SharePoint local**.

    ![Vínculo de SharePoint en la lista de aplicaciones](./media/sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_app.png)

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de SharePoint local en el panel de acceso, debe iniciar sesión automáticamente en la aplicación SharePoint local.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Uso de Azure AD para la autenticación de SharePoint Server](https://docs.microsoft.com/office365/enterprise/using-azure-ad-for-sharepoint-server-authentication)

<!--Image references-->

[1]: ./media/sharepoint-on-premises-tutorial/tutorial_general_01.png
[2]: ./media/sharepoint-on-premises-tutorial/tutorial_general_02.png
[3]: ./media/sharepoint-on-premises-tutorial/tutorial_general_03.png
[4]: ./media/sharepoint-on-premises-tutorial/tutorial_general_04.png

[100]: ./media/sharepoint-on-premises-tutorial/tutorial_general_100.png

[200]: ./media/sharepoint-on-premises-tutorial/tutorial_general_200.png
[201]: ./media/sharepoint-on-premises-tutorial/tutorial_general_201.png
[202]: ./media/sharepoint-on-premises-tutorial/tutorial_general_202.png
[203]: ./media/sharepoint-on-premises-tutorial/tutorial_general_203.png
