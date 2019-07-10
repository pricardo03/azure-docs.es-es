---
title: 'Tutorial: Integración de Azure Active Directory con Check Point CloudGuard Dome9 Arc | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Check Point CloudGuard Dome9 Arc.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 4c12875f-de71-40cb-b9ac-216a805334e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fdaaab8257d3a79130902e1ba0466f9cf15484f4
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147197"
---
# <a name="tutorial-integrate-check-point-cloudguard-dome9-arc-with-azure-active-directory"></a>Tutorial: Integración de Check Point CloudGuard Dome9 Arc en Azure Active Directory

En este tutorial, obtendrás información sobre cómo integrar Check Point CloudGuard Dome9 Arc en Azure Active Directory (Azure AD). Al integrar Check Point CloudGuard Dome9 Arc en Azure Active Directory, puedes:

* Controlar en Azure AD quién tiene acceso a Check Point CloudGuard Dome9 Arc.
* Permitir que los usuarios inicien sesión automáticamente en Check Point CloudGuard Dome9 Arc con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Suscripción habilitada para el inicio de sesión único (SSO) en Check Point CloudGuard Dome9 Arc.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba. Check Point CloudGuard Dome9 Arc admite el inicio de sesión único iniciado por **SP e IDP**.

## <a name="adding-check-point-cloudguard-dome9-arc-from-the-gallery"></a>Agregar Check Point CloudGuard Dome9 Arc desde la galería

Para configurar la integración de Check Point CloudGuard Dome9 Arc en Azure AD, debes agregar Check Point CloudGuard Dome9 Arc desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En el cuadro de búsqueda de la sección **Agregar desde la galería**, escribe **Check Point CloudGuard Dome9 Arc**.
1. Selecciona **Check Point CloudGuard Dome9 Arc** del panel de resultados y, luego, agrega la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

Configura y prueba el inicio de sesión único de Azure AD con Check Point CloudGuard Dome9 Arc usando una usuaria de prueba llamada **B. Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Check Point CloudGuard Dome9 Arc.

Para configurar y probar el inicio de sesión único de Azure AD con Check Point CloudGuard Dome9 Arc, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
2. **[Configuración de Check Point CloudGuard Dome9 Arc](#configure-check-point-cloudguard-dome9-arc)** para configurar el inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B. Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B. Simon para que use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba en Check Point CloudGuard Dome9 Arc](#create-check-point-cloudguard-dome9-arc-test-user)** para tener un homólogo de B. Simon en Check Point CloudGuard Dome9 Arc que esté vinculado a la representación de la usuaria en Azure AD.
6. **[Comprobación del inicio de sesión único](#test-sso)** , para verificar que la configuración funciona correctamente.

### <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Check Point CloudGuard Dome9 Arc**, busca la sección **Administrar** y selecciona **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, seleccione **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://secure.dome9.com/`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://secure.dome9.com/sso/saml/yourcompanyname`

    > [!NOTE]
    > Seleccionará el valor de nombre de la empresa en el portal de administración de dome9, un proceso que se explica más adelante en el tutorial.

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://secure.dome9.com/sso/saml/<yourcompanyname>`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de URL de respuesta y URL de inicio de sesión. Ponte en contacto con el [equipo de soporte técnico de Check Point CloudGuard Dome9 Arc](mailto:Dome9@checkpoint.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

6. La aplicación Check Point CloudGuard Dome9 Arc espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados. Haga clic en el icono  **Editar**  para abrir el cuadro de diálogo Atributos de usuario.

    ![imagen](common/edit-attribute.png)

7. Además de lo anterior, la aplicación Check Point CloudGuard Dome9 Arc espera que se usen algunos atributos más en la respuesta de SAML. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, realice los siguientes pasos para agregar el atributo Token SAML como se muestra en la tabla siguientes: 

    | NOMBRE |  Atributo de origen|
    | ---------------| --------------- |
    | memberof | user.assignedroles |

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

   ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar Check Point CloudGuard Dome9 Arc**, copia las direcciones URL adecuadas según tus necesidades.

   ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="configure-check-point-cloudguard-dome9-arc"></a>Configurar Check Point CloudGuard Dome9 Arc

1. En otra ventana del explorador web, inicia sesión como administrador en el sitio de la compañía de Check Point CloudGuard Dome9 Arc.

2. Haga clic en **Profile Settings** (Configuración de perfil) en la esquina superior derecha y, luego, en **Account Settings** (Configuración de la cuenta). 

    ![Configuración de Check Point CloudGuard Dome9 Arc](./media/dome9arc-tutorial/configure1.png)

3. Vaya a **SSO** y después haga clic en **ENABLE** (HABILITAR).

    ![Configuración de Check Point CloudGuard Dome9 Arc](./media/dome9arc-tutorial/configure2.png)

4. En la sección de configuración de SSO, realice los pasos siguientes:

    ![Configuración de Check Point CloudGuard Dome9 Arc](./media/dome9arc-tutorial/configure3.png)

    a. Escriba el nombre de la compañía en el cuadro de texto **Account ID** (Id. de cuenta). Este valor se usará en la dirección URL de respuesta mencionada en la sección **Configuración básica de SAML** de Azure Portal.

    b. En el cuadro de texto **Issuer** (Emisor), pega el valor de **Identificador de Azure AD** que copiaste de Azure Portal.

    c. En el cuadro de texto **Idp endpoint url** (URL de punto de conexión de Idp), pegue el valor de **Login URL** (Dirección URL de inicio de sesión) que ha copiado de Azure Portal.

    d. Abra el certificado descargado con codificación Base64 en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **X.509 certificate** (Certificado X.509).

    e. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, vas a crear una usuaria de prueba llamada B. Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, vas a permitir que B. Simon use el inicio de sesión único de Azure al conceder acceso a Check Point CloudGuard Dome9 Arc.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, selecciona **Check Point CloudGuard Dome9 Arc**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, selecciona **B.Simon** de la lista de usuarios y haz clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-check-point-cloudguard-dome9-arc-test-user"></a>Crear un usuario de prueba de Check Point CloudGuard Dome9 Arc

Para permitir que los usuarios de Azure AD inicien sesión en Check Point CloudGuard Dome9 Arc, tienen que aprovisionarse en la aplicación. Check Point CloudGuard Dome9 Arc admite el aprovisionamiento Just-In-Time, pero para que funcione correctamente, el usuario debe seleccionar un **rol** determinado y asignarlo al usuario.

   >[!Note]
   >Para la creación del **rol** y otros detalles, ponte en contacto con el [equipo de soporte técnico de Check Point CloudGuard Dome9 Arc](mailto:Dome9@checkpoint.com).

**Para aprovisionar una cuenta de usuario manualmente, realice estos pasos:**

1. Inicia sesión como administrador en el sitio de la compañía de Check Point CloudGuard Dome9 Arc.

2. Haga clic en **Users & Roles** (Usuarios y roles) y luego en **Users** (Usuarios).

    ![Agregar empleado](./media/dome9arc-tutorial/user1.png)

3. Haga clic en **ADD USER** (Agregar usuario).

    ![Agregar empleado](./media/dome9arc-tutorial/user2.png)

4. En la sección **Crear usuario** , lleve a cabo estos pasos:

    ![Agregar empleado](./media/dome9arc-tutorial/user3.png)

    a. En el cuadro de texto **Email** (Correo electrónico), escriba el correo electrónico del usuario, en el ejemplo B.Simon@contoso.com.

    b. En el cuadro de texto **Nombre**, escribe el nombre del usuario, en este caso, B.

    c. En el cuadro de texto **Apellidos**, escriba el apellido del usuario, en este caso Simon.

    d. Asegúrese de que **SSO User** (Usuario de SSO) esté establecido en **On** (Activado).

    e. Haga clic en **CREATE** (Crear).

### <a name="test-sso"></a>Prueba de SSO

Al seleccionar el icono de Check Point CloudGuard Dome9 Arc en el Panel de acceso, deberías iniciar sesión automáticamente en la instancia de Check Point CloudGuard Dome9 Arc para la que configuraste el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)