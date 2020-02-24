---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con IamIP Patent Platform | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y IamIP Patent Platform.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8d5b4fc1-e8fd-4418-a369-189272fef80d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/10/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bfbdc2bb650b6226c23617b55b71f1fde319b62d
ms.sourcegitcommit: f255f869c1dc451fd71e0cab340af629a1b5fb6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2020
ms.locfileid: "77371440"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-iamip-patent-platform"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con IamIP Patent Platform

En este tutorial, aprenderá a integrar IamIP Patent Platform con Azure Active Directory (Azure AD). Al integrar IamIP Patent Platform con Azure AD, puede:

* Controlar en Azure AD quién tiene acceso a IamIP Patent Platform.
* Permitir que los usuarios inicien sesión automáticamente en IamIP Patent Platform con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Un suscripción habilitada para el inicio de sesión único (SSO) en IamIP Patent Platform.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* IamIP Patent Platform admite el inicio de sesión único iniciado por **SP e IDP**
* Una vez configurado IamIP Patent Platform, puede aplicar controles de sesión que protejan el filtrado externo e interno de la información confidencial de la organización en tiempo real. Los controles de sesión proceden del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).


## <a name="adding-iamip-patent-platform-from-the-gallery"></a>Incorporación de IamIP Patent Platform desde la galería

Para configurar la integración de IamIP Patent Platform en Azure AD, deberá agregar IamIP Patent Platform desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **IamIP Patent Platform** en el cuadro de búsqueda.
1. Seleccione **IamIP Patent Platform** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-single-sign-on-for-iamip-patent-platform"></a>Configuración y prueba del inicio de sesión único de Azure AD para IamIP Patent Platform

Configure y pruebe el inicio de sesión único de Azure AD con IamIP Patent Platform mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de IamIP Patent Platform.

Para configurar y probar el inicio de sesión único de Azure AD con IamIP Patent Platform, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    * **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    * **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en IamIP Patent Platform](#configure-iamip-patent-platform-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    * **[Creación de un usuario de prueba en IamIP Patent Platform](#create-iamip-patent-platform-test-user)** : para tener un homólogo de B.Simon en IamIP Patent Platform vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **IamIP Patent Platform**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si tiene el **archivo de metadatos del proveedor de servicios** y quiere realizar la configuración en el modo iniciado por **IdP**, siga este procedimiento:

    a. Haga clic en **Cargar el archivo de metadatos**.

    ![Carga del archivo de metadatos](common/upload-metadata.png)

    b. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.

    ![Elección del archivo de metadatos](common/browse-upload-metadata.png)

    c. Una vez que se haya cargado correctamente el archivo de metadatos, el valor de **Identificador** y **URL de respuesta** se rellena automáticamente en la sección Configuración básica de SAML.

    ![imagen](common/idp-intiated.png)

    > [!Note]
    > Si los valores **Identificador** y **URL de respuesta** no se rellenan automáticamente, hágalo manualmente según sus necesidades.

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://patents.iamip.com/login-user`

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (sin procesar)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificateraw.png)

1. En la sección **Set up IamIP Patent Platform** (Configurar IamIP Patent Platform), copie las direcciones URL que necesite.

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

En esta sección va a permitir que B.Simon acceda a IamIP Patent Platform mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **IamIP Patent Platform**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-iamip-patent-platform-sso"></a>Configuración del inicio de sesión único en IamIP Patent Platform

Para configurar el inicio de sesión único en **IamIP Patent Platform**, es preciso enviar el **certificado (sin procesar)** descargado y las direcciones URL correspondientes copiadas de Azure Portal al [equipo de soporte técnico de IamIP Patent Platform](mailto:info@iamip.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-iamip-patent-platform-test-user"></a>Creación de un usuario de prueba en IamIP Patent Platform

En esta sección va a crear un usuario llamado B.Simon en IamIP Patent Platform. Trabaje con el  [equipo de soporte técnico de IamIP Patent Platform](mailto:info@iamip.com) para agregar los usuarios a la plataforma de IamIP Patent Platform. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de IamIP Patent Platform del panel de acceso, debería iniciar sesión automáticamente en la aplicación IamIP Patent Platform para la que configurara el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Prueba de IamIP Patent Platform con Azure AD](https://aad.portal.azure.com/)

- [¿Qué es el control de sesiones en Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)