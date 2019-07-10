---
title: 'Tutorial: integración con Azure Active Directory con Bright Pattern Omnichannel Contact Center | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Bright Pattern Omnichannel Contact Center.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: de1e0091-ca10-4e7e-8014-f4579d8eabf6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 205b1746bac30a015d4efe4bde573be44563e2f1
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67451716"
---
# <a name="tutorial-integrate-bright-pattern-omnichannel-contact-center-with-azure-active-directory"></a>Tutorial: integrar Bright Pattern Omnichannel Contact Center con Azure Active Directory

En este tutorial, obtendrá información sobre cómo integrar Bright Pattern Omnichannel Contact Center con Azure Active Directory (Azure AD). Al integrar Bright Pattern Omnichannel Contact Center con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Bright Pattern Omnichannel Contact Center.
* Permitir que los usuarios inicien sesión automáticamente Bright Pattern Omnichannel Contact Center mediante sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede obtener la evaluación gratuita de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).
* Suscripción habilitada para inicio de sesión único (SSO) en Bright Pattern Omnichannel Contact Center.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Bright Pattern Omnichannel Contact Center admite SSO iniciado por **SP e IDP**
* Bright Pattern Omnichannel Contact Center admite el aprovisionamiento de usuarios **Just-In-Time**

## <a name="adding-bright-pattern-omnichannel-contact-center-from-the-gallery"></a>Agregar Bright Pattern Omnichannel Contact Center desde la galería

Para configurar la integración de Bright Pattern Omnichannel Contact Center en Azure AD, deberá agregar Bright Pattern Omnichannel Contact Center desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Bright Pattern Omnichannel Contact Center** en el cuadro de búsqueda.
1. Seleccione **Bright Pattern Omnichannel Contact Center** del panel de resultados y luego agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

Configure y pruebe el SSO de Azure AD con Bright Pattern Omnichannel Contact Center mediante un usuario de prueba llamado **B.Simon**. Para que SSO funcione, tiene que establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Bright Pattern Omnichannel Contact Center.

Para configurar y probar el SSO de Azure AD con Bright Pattern Omnichannel Contact Center, complete los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
2. **[Configuración del SSO de Bright Pattern Omnichannel Contact](#configure-bright-pattern-omnichannel-contact-center-sso)** , para configurar las opciones de inicio de sesión único en el lado de la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B. Simon para que use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Bright Pattern Omnichannel Contact Center](#create-bright-pattern-omnichannel-contact-center-test-user)** , para tener un homólogo de B.Simon en Bright Pattern Omnichannel Contact Center que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Bright Pattern Omnichannel Contact Center**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, seleccione **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `<SUBDOMAIN>_sso`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.brightpattern.com/agentdesktop/sso/redirect`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.brightpattern.com/`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico al cliente de Bright Pattern Omnichannel Contact Center](mailto:support@brightpattern.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. La aplicación Bright Pattern Omnichannel Contact Center espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

1. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, edite las notificaciones mediante el **icono Editar** o agregue notificaciones mediante **Agregar nueva notificación** para configurar el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos: 

    | NOMBRE | Espacio de nombres  |
    | ---------------| --------------- |
    | firstName | user.givenname |
    | lastName | user.surname |
    | email | user.mail |

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

   ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar Bright Pattern Omnichannel Contact Center**, copie las direcciones URL adecuadas según sus necesidades.

   ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="configure-bright-pattern-omnichannel-contact-center-sso"></a>Configuración del SSO de Bright Pattern Omnichannel Contact Center

Para configurar el inicio de sesión único en **Bright Pattern Omnichannel Contact Center**, debe enviar el **certificado (Base64)** descargado y las direcciones URL adecuadas copiadas desde Azure Portal al [equipo de soporte técnico de Bright Pattern Omnichannel Contact Center](mailto:support@brightpattern.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

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

En esta sección, habilitará a B.Simon para usar el inicio de sesión único de Azure al concederle acceso a Bright Pattern Omnichannel Contact Center.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Bright Pattern Omnichannel Contact Center**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-bright-pattern-omnichannel-contact-center-test-user"></a>Creación de un usuario de prueba Bright Pattern Omnichannel Contact Center

En esta sección, se crea un usuario llamado B.Simon en Bright Pattern Omnichannel Contact Center. Bright Pattern Omnichannel Contact Center admite el aprovisionamiento de usuarios Just-in-time, que está habilitado de manera predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario no existe en Bright Pattern Omnichannel Contact Center, se crea uno nuevo después de la autenticación.

### <a name="test-sso"></a>Prueba de SSO

Cuando selecciona el icono de Bright Pattern Omnichannel Contact Center en el panel de acceso, su sesión debería iniciar automáticamente en la instancia de Bright Pattern Omnichannel Contact Center para la que configure el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)