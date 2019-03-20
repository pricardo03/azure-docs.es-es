---
title: 'Tutorial: Integración de Azure Active Directory con DocuSign | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y DocuSign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2fda9df8e7781a9e0c45fb1aead9f8167f89a833
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57850877"
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Tutorial: Integración de Azure Active Directory con DocuSign

En este tutorial, obtendrá información sobre cómo integrar DocuSign con Azure Active Directory (Azure AD).

La integración de DocuSign con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a DocuSign.
- Puede permitir que los usuarios inicien sesión automáticamente en DocuSign (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con DocuSign, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en DocuSign

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de DocuSign desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-docusign-from-the-gallery"></a>Adición de DocuSign desde la galería

Para configurar la integración de DocuSign en Azure AD, deberá agregar DocuSign desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar DocuSign desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **DocuSign**, seleccione **DocuSign** en el panel de resultados y, luego, haga clic en el botón **Agregar** para añadir la aplicación.

    ![DocuSign en la lista de resultados](./media/docusign-tutorial/tutorial_docusign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con DocuSign con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de DocuSign para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de DocuSign.

Para configurar y probar el inicio de sesión único de Azure AD con DocuSign, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de DocuSign](#creating-a-docusign-test-user)**: para tener un homólogo de Britta Simon en DocuSign que esté vinculado a la representación de ella en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación DocuSign.

**Para configurar el inicio de sesión único de Azure AD con DocuSign y, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **DocuSign**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial_general_301.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de DocuSign](./media/docusign-tutorial/tutorial_docusign_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/sp/<IDPID>`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL de inicio de sesión y el identificador reales, que se explican más adelante en la sección **View SAML 2.0 Endpoints** (Ver puntos de conexión de SAML 2.0) del tutorial.

5. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic **Descargar** para descargar el **certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/docusign-tutorial/tutorial_docusign_certificate.png) 

6. En la sección **Set up DocuSign** (Configurar DocuSign), copie la dirección URL adecuada según sus necesidades.

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![Configuración de DocuSign](common/configuresection.png)

7. En otra ventana del explorador web, inicie sesión en el **Portal de administración de DocuSign** como administrador.

8. En la parte superior derecha de la página, haga clic en el **logotipo** del perfil y luego haga clic en **Go to Admin** (Ir a administración).
  
    ![Configuración del inicio de sesión único][51]

9. En la página de soluciones de dominio, haga clic en **Dominios**

    ![Configuración del inicio de sesión único][50]

10. En la sección **Dominios**, haga clic en **CLAIM DOMAIN** (RECLAMAR DOMINIO).

    ![Configuración del inicio de sesión único][52]

11. En el cuadro de diálogo **Claim a domain** (Reclamar un dominio), en el cuadro de texto **Domain Name** (Nombre de dominio), escriba el dominio de la compañía y haga clic en **CLAIM** (RECLAMAR). Asegúrese de que comprueba el dominio y que su estado es activo.

    ![Configuración del inicio de sesión único][53]

12. En la página de soluciones de dominio, haga clic en **Proveedores de identidad**.
  
    ![Configuración del inicio de sesión único][54]

13. En la sección **proveedores de identidades**, haga clic en **AGREGAR PROVEEDOR DE IDENTIDADES**. 

    ![Configuración del inicio de sesión único][55]

14. En la página **Identity Provider Settings** (Configuración del proveedor de identidades), siga estos pasos:

    ![Configuración del inicio de sesión único][56]

     a. En el cuadro de texto **Name** (Nombre), escriba un nombre único para la configuración. No utilice espacios.

    b. En el cuadro de texto **Emisor de proveedor de identidades**, pegue el valor de **Identificador Azure AD** que ha copiado de Azure Portal.

    c. En el cuadro de texto **Identity Provider Login URL** (Dirección URL de inicio de sesión del proveedor de identidades), pegue el valor de **Dirección URL de inicio de sesión**  que ha copiado de Azure Portal.

    d. En el cuadro de texto **Identity Provider Logout URL** (Dirección URL de cierre de sesión del proveedor de identidades), pegue el valor de **URL de cierre de sesión** que ha copiado de Azure Portal.

    e. Seleccione **Sign AuthN Request** (Firmar solicitud de autenticación).

    f. En **Send AuthN request by** (Enviar solicitud de autorización por), seleccione **POST**.

    g. En **Send logout request by** (Enviar solicitud de cierre de sesión por), seleccione **GET**.

    h. En la sección **Custom Attribute Mapping** (Asignación de atributos personalizados), haga clic en **AGREGAR NUEVA ASIGNACIÓN**.

    ![Configuración del inicio de sesión único][62]

    i. Elija el campo que desea asignar con la reclamación de Azure AD. En este ejemplo, la notificación **emailaddress** está asociada con el valor de **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**. Este es el nombre de notificación predeterminado de Azure AD para la notificación de correo electrónico y luego haga clic en **GUARDAR**.

    ![Configuración del inicio de sesión único][57]

    > [!NOTE]
    > Utilice el **identificador de usuario** adecuado para asignar el usuario de Azure AD a la asignación de usuarios de DocuSign. Seleccione el campo apropiado y escriba el valor adecuado según la configuración de la organización.

    j. En la sección **Identity Provider Certificate** (Certificado del proveedor de identidades), haga clic en **AGREGAR CERTIFICADO**, cargue el certificado que ha descargado del Portal del portal de Azure AD y haga clic en **GUARDAR**.

    ![Configuración del inicio de sesión único][58]

    k. En la sección **Proveedores de identidades**, haga clic en **ACCIONES** y luego en **Puntos de conexión**.

    ![Configuración del inicio de sesión único][59]

    l. En el **Portal de administración de DocuSign**, en la sección **View SAML 2.0 Endpoints** (Ver puntos de conexión de SAML 2.0), siga estos pasos:

    ![Configuración del inicio de sesión único][60]

    * Copie la información del cuadro de texto **Service Provider Issuer URL** (Dirección URL del emisor del proveedor de servicios) y luego péguela en el cuadro de texto **Identificador** en la sección **DocuSign Domain and URLs** (Dominio y direcciones URL de DocuSign) de Azure Portal.

    * Copie la información del cuadro de texto **Service Provider Login URL** (Dirección URL de inicio de sesión del proveedor de servicios) y luego péguela en el cuadro de texto **Sign On URL** (Dirección URL de inicio de sesión) en la sección **DocuSign Domain and URLs** (Dominio y direcciones URL de DocuSign) de Azure Portal.

    * Haga clic en **Close**

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_02.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el **nombre de usuario** , escriba **brittasimon\@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.

### <a name="creating-a-docusign-test-user"></a>Creación de un usuario de prueba de DocuSign

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en DocuSign. DocuSign admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Al intentar acceder a DocuSign, se crea un nuevo usuario, en caso de que no exista.
>[!Note]
>Si necesita crear manualmente un usuario, es preciso que se ponga en contacto con el  [equipo de soporte técnico de DocuSign](https://support.docusign.com/).

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a DocuSign.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **DocuSign**.

    ![Configurar inicio de sesión único](./media/docusign-tutorial/tutorial_docusign_app.png)

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de DocuSign en el panel de acceso, debería iniciar sesión automáticamente en su aplicación DocuSign.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

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
[50]: ./media/docusign-tutorial/tutorial_docusign_18.png
[51]: ./media/docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/docusign-tutorial/tutorial_docusign_29.png
[62]: ./media/docusign-tutorial/tutorial_docusign_30.png
