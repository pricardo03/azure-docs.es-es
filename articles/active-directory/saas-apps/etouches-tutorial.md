---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con etouches | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y etouches.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 76cccaa8-859c-4c16-9d1d-8a6496fc7520
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e0774873370d6006e0a1108c4b01c60274d69f0d
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2019
ms.locfileid: "75561199"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-etouches"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con etouches

En este tutorial, obtendrá información sobre cómo integrar etouches con Azure Active Directory (Azure AD). Al integrar etouches con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a etouches.
* Permitir que los usuarios inicien sesión automáticamente en etouches con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Prerequisites

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en etouches.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* etouches admite el inicio de sesión único iniciado por **SP**





## <a name="adding-etouches-from-the-gallery"></a>Incorporación de etouches desde la galería

Para configurar la integración de etouches en Azure AD, deberá agregar etouches desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **etouches** en el cuadro de búsqueda.
1. Seleccione **etouches** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-single-sign-on-for-etouches"></a>Configuración y prueba del inicio de sesión único de Azure AD para etouches

Configure y pruebe el inicio de sesión único de Azure AD con etouches mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de etouches.

Para configurar y probar el inicio de sesión único de Azure AD con etouches, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de etouches](#configure-etouches-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de etouches](#create-etouches-test-user)** : para tener un homólogo de B.Simon en etouches que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **etouches**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, especifique los valores de los siguientes campos:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`

    b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://www.eiseverywhere.com/<instance name>`

    > [!NOTE] 
    > Estos valores no son reales. El valor se actualizará con la dirección URL de inicio de sesión y el identificador reales, que se explican más adelante en el tutorial.

1. La aplicación etouches espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/edit-attribute.png)

1. Además de lo anterior, la aplicación etouches espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | Nombre | Atributo de origen|
    | ------------------- | -------------------- |
    | Email | user.mail | 

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Configurar etouches**, copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, va a conceder a B.Simon acceso a etouches mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **etouches**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-etouches-sso"></a>Configuración del inicio de sesión único de etouches

1. Para que SSO se configure para su aplicación, siga estos pasos en la aplicación etouches: 

    ![Configuración de etouches](./media/etouches-tutorial/tutorial_etouches_06.png) 

    a. Inicie sesión en la aplicación **etouches** con derechos de administrador.
   
    b. Vaya a la configuración de **SAML**.

    c. En la sección **Configuración general**, abra el certificado descargado de Azure Portal en el Bloc de notas, copie el contenido y, luego, péguelo en el cuadro de texto de metadatos de IDP. 

    d. Haga clic en el botón **Save & Stay** (Guardar y permanecer).
  
    e. Haga clic en el botón **Update Metadata** (Actualizar metadatos) en la sección SAML Metadata (Metadatos de SAML). 

    f. Esta acción abre la página y realiza el inicio de sesión único. Cuando el inicio de sesión único esté funcionando, puede configurar el nombre de usuario.

    g. En el campo Username (Nombre de usuario) seleccione **emailaddress**, como se muestra en la siguiente imagen. 

    h. Copie el valor de **SP entity ID** (Identificador de entidad del proveedor de servicios) y péguelo en el cuadro de texto **Identificador** de la sección **Configuración básica de SAML**  de Azure Portal.

    i. Copie el valor de **SSO URL / ACS** (URL de SSO/ACS) y péguelo en el cuadro de texto **URL de inicio de sesión**  de la sección **Configuración básica de SAML**  de Azure Portal.

### <a name="create-etouches-test-user"></a>Creación de un usuario de prueba en etouches

En esta sección, se crea un usuario denominado Britta Simon en etouches. Trabaje con el [equipo de soporte técnico de etouches](https://www.etouches.com/event-software/support/customer-support/) para agregar los usuarios a la plataforma de etouches.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de etouches del Panel de acceso, debería iniciar sesión automáticamente en la instancia de etouches para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Pruebe etouches con Azure AD](https://aad.portal.azure.com/).

