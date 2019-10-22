---
title: 'Tutorial: Integración del inicio de sesión único de Azure Active Directory con PureCloud by Genesys | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y PureCloud by Genesys.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e16a46db-5de2-4681-b7e0-94c670e3e54e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: dfaa5a4ebb8bc633dc49db08698aec31de9436d4
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72373146"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-purecloud-by-genesys"></a>Tutorial: Integración del inicio de sesión único de Azure Active Directory con PureCloud by Genesys

En este tutorial, aprenderá a integrar PureCloud by Genesys con Azure Active Directory (Azure AD). Al integrar PureCloud by Genesis con Azure AD, puede:

* Controlar en Azure AD quién tiene acceso a PureCloud by Genesys.
* Permitir que los usuarios inicien sesión automáticamente en PureCloud by Genesys con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Suscripción habilitada para el inicio de sesión único (SSO) en PureCloud by Genesys.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* PureCloud by Genesys admite el inicio de sesión único iniciado por **SP e IDP**.

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="adding-purecloud-by-genesys-from-the-gallery"></a>Incorporación de PureCloud by Genesys desde la galería

Para configurar la integración de PureCloud by Genesys en Azure AD, deberá agregar PureCloud by Genesys desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **PureCloud by Genesys** en el cuadro de búsqueda.
1. Seleccione **PureCloud by Genesys** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-purecloud-by-genesys"></a>Configuración y prueba del inicio de sesión único de Azure AD para PureCloud by Genesis

Configure y pruebe el inicio de sesión único de Azure AD con PureCloud by Genesys mediante un usuario de prueba llamado **B. Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de PureCloud by Genesys.

Para configurar y probar el inicio de sesión único de Azure AD con PureCloud by Genesys, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en PureCloud by Genesys](#configure-purecloud-by-genesys-sso)** , para configurar el inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de PureCloud by Genesys](#create-purecloud-by-genesys-test-user)** , para tener un homólogo de B.Simon en PureCloud by Genesys que esté vinculado a la representación de ella en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **PureCloud by Genesys**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL según su región:

    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.au/saml` |

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL según su región:

    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.com.au/saml`|

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **Dirección URL de inicio de sesión**, escriba una dirección URL según su región:
    
    | |
    |--|
    | `https://login.mypurecloud.com` |
    | `https://login.mypurecloud.de` |
    | `https://login.mypurecloud.jp` |
    | `https://login.mypurecloud.ie` |
    | `https://login.mypurecloud.com.au` |

1. La aplicación PureCloud by Genesys espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de atributos de token SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/default-attributes.png)

1. Además de lo anterior, la aplicación PureCloud by Genesys espera que se pasen algunos atributos más en la respuesta de SAML que se muestran a continuación. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | NOMBRE | Atributo de origen|
    | ---------------| --------------- |
    | Email | user.userprinicipalname |
    | OrganizationName | `Your organization name` |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Set up PureCloud by Genesys** (Configurar PureCloud by Genesys), copie las direcciones URL adecuadas según sus requisitos.

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

En esta sección, concederá acceso a B.Simon a PureCloud by Genesys para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **PureCloud by Genesys**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-purecloud-by-genesys-sso"></a>Configuración del inicio de sesión único en PureCloud by Genesys

1. En otra ventana del explorador web, inicie sesión en PureCloud by Genesys como administrador.

1. Haga clic en **Admin** (Administrador) en la parte superior y, en **Integrations** (Integraciones), vaya a **Single Sign-on** (Inicio de sesión único).

    ![Configurar inicio de sesión único](./media/purecloud-by-genesys-tutorial/configure01.png)

1. Cambie a la pestaña **ADFS/Azure AD(Premium)** y realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/purecloud-by-genesys-tutorial/configure02.png)

    a. Haga clic en **Browse** (Examinar) para cargar en **ADFS Certificate** (Certificado de ADFS) el certificado codificado en base-64 que ha descargado de Azure Portal.

    b. En el cuadro de texto **ADFS Issuer URI** (URI de emisor de ADFS), pegue el valor de **Identificador de Azure AD** que ha copiado de Azure Portal.

    c. En el cuadro de texto **Target URI** (URI de destino), pegue el valor de **URL de inicio de sesión** que ha copiado de Azure Portal.

    d. Para el valor de **Relying Party Identifier** (Identificador de usuario de confianza), debe ir a Azure Portal, a la página de integración de aplicaciones de **PureCloud by Genesys**, hacer clic en la pestaña **Propiedades** y copiar el valor de **Id. de aplicación**. Péguelo en el cuadro de texto **Relying Party Identifier** (Identificador de usuario de confianza). 

    ![Configurar inicio de sesión único](./media/purecloud-by-genesys-tutorial/configure06.png)

    e. Haga clic en **Guardar**

### <a name="create-purecloud-by-genesys-test-user"></a>Creación de un usuario de prueba de PureCloud by Genesys

Para permitir que los usuarios de Azure AD inicien sesión en PureCloud by Genesys, deben aprovisionarse en PureCloud by Genesys. En PureCloud by Genesys, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en PureCloud by Genesys como administrador.

1. Haga clic en **Admin** (Administrador) en la parte superior y vaya a **People** (Personas) en **People & Permissions** (Personas y permisos).

    ![Configurar inicio de sesión único](./media/purecloud-by-genesys-tutorial/configure03.png)

1. En la página People (Personas), haga clic en **Add Person** (Agregar persona).

    ![Configurar inicio de sesión único](./media/purecloud-by-genesys-tutorial/configure04.png)

1. En el elemento emergente **Add People to the Organization** (Agregar personas a la organización), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/purecloud-by-genesys-tutorial/configure05.png)

    a. En el cuadro de texto **Full name** (Nombre completo), escriba el nombre del usuario, por ejemplo, **B.Simon**.

    b. En el cuadro de texto **Email** (Correo electrónico), escriba el correo electrónico del usuario; por ejemplo, **b.simon\@contoso.com**.

    c. Haga clic en **Create**(Crear).

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de PureCloud by Genesys en el Panel de acceso, debería iniciar sesión automáticamente en la versión de PureCloud by Genesys para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Pruebe PureCloud by Genesis con Azure AD](https://aad.portal.azure.com/)