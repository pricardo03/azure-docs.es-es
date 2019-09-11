---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con F5 | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y F5.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 39382eab-05fe-4dc2-8792-62d742dfb4e1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/29/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea331bbabe238c351921a02a5012a9f8a087646f
ms.sourcegitcommit: 19a821fc95da830437873d9d8e6626ffc5e0e9d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70165838"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-f5"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con F5

En este tutorial, obtendrá información sobre cómo integrar F5 con Azure Active Directory (Azure AD). Al integrar F5 con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a F5.
* Permitir que los usuarios inicien sesión automáticamente en F5 con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en F5.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* F5 admite el inicio de sesión único iniciado por **SP e IDP**.
* El inicio de sesión único de F5 se puede configurar de tres maneras diferentes:

- [Configurar el inicio de sesión único de F5 para la aplicación Kerberos](#configure-f5-single-sign-on-for-kerberos-application)

- [Configuración del inicio de sesión único de F5 para la aplicación basada en encabezados](headerf5-tutorial.md)

- [Configuración del inicio de sesión único de F5 para la aplicación Kerberos avanzada](advance-kerbf5-tutorial.md)

## <a name="adding-f5-from-the-gallery"></a>Adición de F5 desde la galería

Para configurar la integración de F5 en Azure AD, será preciso agregar F5 desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **F5** en el cuadro de búsqueda.
1. Seleccione **F5** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-f5"></a>Configuración y prueba del inicio de sesión único de Azure AD para F5

Configure y pruebe el inicio de sesión único de Azure AD con F5 mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de F5.

Para configurar y probar el inicio de sesión único de Azure AD con F5, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de F5](#configure-f5-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de F5](#create-f5-test-user)** , para tener un homólogo de B.Simon en F5 que esté vinculado a su representación en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **F5**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<YourCustomFQDN>.f5.com/`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<YourCustomFQDN>.f5.com/`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<YourCustomFQDN>.f5.com/`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de F5](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Set up F5** (Configurar F5), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a permitir que B.Simon acceda a F5 mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **F5**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-f5-sso"></a>Configuración del inicio de sesión único de F5

- [Configuración del inicio de sesión único de F5 para la aplicación basada en encabezados](headerf5-tutorial.md)

- [Configuración del inicio de sesión único de F5 para la aplicación Kerberos avanzada](advance-kerbf5-tutorial.md)

### <a name="configure-f5-single-sign-on-for-kerberos-application"></a>Configuración del inicio de sesión único de F5 para la aplicación Kerberos

1. Abra una nueva ventana del explorador web e inicie sesión en el sitio de empresa de F5 (Kerberos) como administrador y haga lo siguiente:

1. Debe importar el certificado de metadatos en F5 (Kerberos), que se usará más adelante en el proceso de instalación. Vaya a **System > Certificate Management > Traffic Certificate Management > > SSL Certificate List** (Sistema > Administración de certificados > Lista de certificados SSL). Haga clic en **Import** (Importar) en la esquina derecha.

    ![Configuración de F5 (Kerberos)](./media/kerbf5-tutorial/configure01.png) 

1. Además, también se necesita un **certificado SSL** para el nombre de host (`Kerbapp.superdemo.live`), en este ejemplo, se ha usado un certificado comodín.

    ![Configuración de F5 (Kerberos)](./media/kerbf5-tutorial/configure02.png) 
 
1. Vaya a – **F5 BIG-IP Haga clic en Access > Guided Configuration > Federation > SAML Service Provider** (Acceso > Configuración guiada > Federación > Proveedor de servicios SAML).

    ![Configuración de F5 (Kerberos)](./media/kerbf5-tutorial/configure03.png) 

    ![Configuración de F5 (Kerberos)](./media/kerbf5-tutorial/configure04.png)

1. Especifique el **identificador de entidad** (el mismo que el que configuró en la configuración de la aplicación de Azure AD)

    ![Configuración de F5 (Kerberos)](./media/kerbf5-tutorial/configure05.png) 

1. Cree un nuevo servidor virtual y especifique la **dirección de destino**. Elija el **certificado comodín** (o **Cert** que cargó para la aplicación) que cargamos antes y la **clave privada asociada**.

    ![Configuración de F5 (Kerberos)](./media/kerbf5-tutorial/configure06.png)

1. Cargue los **metadatos** de configuración y especifique un nuevo **nombre para el conector IDP de SAML**. También deberá especificar el certificado de federación que se cargó anteriormente.

    ![Configuración de F5 (Kerberos)](./media/kerbf5-tutorial/configure07.png)  

1. **Cree un nuevo**  grupo de aplicaciones de back-end y especifique las **direcciones IP** de los servidores de aplicaciones de back-end.
 
    ![Configuración de F5 (Kerberos)](./media/kerbf5-tutorial/configure08.png)

1. En **Single Sign-on Settings** (Configuración de inicio de sesión único), elija **Kerberos** y seleccione **Advanced Settings** (Configuración avanzada). La solicitud debe crearse en `user@domain.suffix`.

1. En el **origen de nombre de usuario**, especifique `session.saml.last.attr.name. http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`. Consulte el Apéndice para obtener una lista completa de las variables y los valores.
Nombre de cuenta es la cuenta de delegación de F5 creada (consulte la documentación de F5).

    ![Configuración de F5 (Kerberos)](./media/kerbf5-tutorial/configure09.png)   

1. Póngase en contacto con el [equipo de soporte técnico al cliente de F5 (Kerberos) ](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45) para obtener detalles de documentación de **las propiedades de comprobación de puntos de conexión**.

    ![Configuración de F5 (Kerberos)](./media/kerbf5-tutorial/configure10.png) 

1. Póngase en contacto con el [equipo de soporte técnico al cliente de F5 (Kerberos) ](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45) para obtener detalles de documentación de **las propiedades de administración de sesión**.

    ![Configuración de F5 (Kerberos)](./media/kerbf5-tutorial/configure11.png) 
 
1. **Revise el resumen** y haga clic en **Implementar**.
 
    ![Configuración de F5 (Kerberos)](./media/kerbf5-tutorial/configure12.png)

    ![Configuración de F5 (Kerberos)](./media/kerbf5-tutorial/configure13.png)

### <a name="create-f5-test-user"></a>Creación de un usuario de prueba de F5

En esta sección, creará un usuario llamado B.Simon en F5. Trabaje con el  [equipo de soporte técnico del cliente de F5](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45) para agregar los usuarios a la plataforma de F5. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único. 

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de F5 en el panel de acceso, debería iniciar sesión automáticamente en la versión de F5 para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Prueba de F5 con Azure AD](https://aad.portal.azure.com/)

- [Configuración del inicio de sesión único de F5 para la aplicación basada en encabezados](headerf5-tutorial.md)

- [Configuración del inicio de sesión único de F5 para la aplicación Kerberos avanzada](advance-kerbf5-tutorial.md)

