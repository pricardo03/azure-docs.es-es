---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con SignalFx | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SignalFx.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6d5ab4b0-29bc-4b20-8536-d64db7530f32
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea81f0046d7f73d845ed49325a3d621e6b7735e7
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75443276"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-signalfx"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con SignalFx

En este tutorial, aprenderá a integrar SignalFx con Azure Active Directory (Azure AD). Si integra SignalFx con Azure AD, podrá hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a SignalFx.
* Permitir que los usuarios puedan iniciar sesión automáticamente en SignalFx con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Prerequisites

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único en SignalFx.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* SignalFx admite el inicio de sesión único iniciado por **IDP**
* SignalFx admite el aprovisionamiento de usuarios **Just-In-Time**

## <a name="adding-signalfx-from-the-gallery"></a>Adición de SignalFx desde la galería

Para configurar la integración de SignalFx en Azure AD, deberá agregar SignalFx desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **SignalFx** en el cuadro de búsqueda.
1. Seleccione **SignalFx** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-signalfx"></a>Configuración y prueba del inicio de sesión único de Azure AD con SignalFx

Configure y pruebe el inicio de sesión único de Azure AD con SignalFx utilizando un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de SignalFx.

Para configurar y probar el inicio de sesión único de Azure AD con SignalFx, complete los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    * **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    * **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en SignalFx](#configure-signalfx-sso)** , para configurar el inicio de sesión único en la aplicación.
    * **[Creación de un usuario de prueba en SignalFx](#create-signalfx-test-user)** , para tener un homólogo de B.Simon en SignalFx vinculado a la representación de este usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **SignalFx**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la página **Configurar el inicio de sesión único con SAML**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL: `https://api.signalfx.com/v1/saml/metadata`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://api.signalfx.com/v1/saml/acs/<integration ID>`

    > [!NOTE]
    > El valor anterior no es real. El valor se actualizará con la dirección URL de respuesta real, que se explica más adelante en el tutorial.

1. La aplicación SignalFx espera las aserciones de SAML en un formato específico. Para ello, es necesario agregar asignaciones de atributos personalizados a la configuración de los atributos del token SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/default-attributes.png)

1. Además de lo anterior, la aplicación SignalFx espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | Nombre |  Atributo de origen|
    | ------------------- | -------------------- |
    | User.FirstName  | user.givenname |
    | User.email  | user.mail |
    | PersonImmutableID       | user.userprincipalname    |
    | User.LastName       | user.surname    |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar SignalFx**, copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, va a permitir que B.Simon acceda a SignalFx utilizando el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **SignalFx**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-signalfx-sso"></a>Configuración del inicio de sesión único en SignalFx

1. Inicie sesión en su sitio de la compañía de SignalFx como administrador.

1. En SignalFx, en la parte superior, haga clic en **Integrations** (Integraciones) para abrir la página correspondiente.

    ![Integración de SignalFx](./media/signalfx-tutorial/tutorial_signalfx_intg.png)

1. Haga clic en el icono **Azure Active Directory** en la sección **Login Services** (Servicios de inicio de sesión).

    ![SignalFx saml](./media/signalfx-tutorial/tutorial_signalfx_saml.png)

1. Haga clic en **NEW INTEGRATION** (Nueva integración) y en la pestaña **INSTALL** (Instalar) realice los pasos siguientes:

    ![SignalFx samlintgpage](./media/signalfx-tutorial/tutorial_signalfx_azure.png)

    a. En el cuadro de texto **Nombre**, escriba un nuevo nombre de integración, como **OurOrgName SAML SSO**.

    b. Copie el valor de **Id. de integración** y anexe la **URL de respuesta** en el lugar de `<integration ID>` en el cuadro de texto **URL de respuesta** de la sección **Configuración básica de SAML** de Azure Portal.

    c. Haga clic en **Carga de archivo** para cargar el **certificado codificado en Base 64** descargado desde Azure Portal en el cuadro de texto **Certificado**.

    d. En el cuadro de texto **URL del emisor**, pegue el valor de **Identificador de Azure AD**  que ha copiado de Azure Portal.

    e. En el cuadro de texto **URL de metadatos**, pegue la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    f. Haga clic en **Save**(Guardar).

### <a name="create-signalfx-test-user"></a>Creación de un usuario de prueba de SignalFx

El objetivo de esta sección es crear un usuario llamado Britta Simon en SignalFx. SignalFx admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Al intentar acceder a SignalFx, se crea un nuevo usuario, en caso de que no exista.

Cuando un usuario inicia sesión en SignalFx desde SSO de SAML por primera vez, [el equipo de soporte técnico de SignalFx](mailto:kmazzola@signalfx.com) envía un correo electrónico con un vínculo en el que debe hacer clic para autenticarse. Esto solo ocurrirá la primera vez que el usuario inicie sesión; los intentos de inicio de sesión posteriores no requerirán la validación del correo electrónico.

> [!Note]
> Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de SignalFx](mailto:kmazzola@signalfx.com).

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de SignalFx en el Panel de acceso, debería iniciar sesión automáticamente en la instancia de SignalFx para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Pruebe SignalFx con Azure AD](https://aad.portal.azure.com/)