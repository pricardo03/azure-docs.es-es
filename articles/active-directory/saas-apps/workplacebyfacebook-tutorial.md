---
title: 'Tutorial: Integración de Azure Active Directory con Workplace by Facebook | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Workplace by Facebook.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/01/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4328c6b5b85050ae5caf30fd4d321e50428f10b9
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68825354"
---
# <a name="tutorial-integrate-workplace-by-facebook-with-azure-active-directory"></a>Tutorial: Integración de Workplace by Facebook con Azure Active Directory

En este tutorial, obtendrá información sobre cómo integrar Workplace by Facebook con Azure Active Directory (Azure AD). Cuando integra Workplace by Facebook con Azure Active AD, puede:

* Controlar en Azure AD quién tiene acceso a Workplace by Facebook.
* Permitir que los usuarios inicien sesión automáticamente en Workplace by Facebook con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para inicio de sesión único (SSO) de Workplace by Facebook.

> [!NOTE]
> Facebook tiene dos productos: Workplace Standard (gratis) and Workplace Premium (de pago). Todo inquilino de Workplace Premium puede configurar la integración de SCIM y SSO sin otras implicaciones de coste ni requerimiento de licencias. SSO y SCIM no están disponibles en las instancias de Workplace Standard.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Workplace by Facebook admite el inicio de sesión único iniciado por **SP**.
* Workplace by Facebook admite el **aprovisionamiento Just-In-Time**.
* Workplace by Facebook admite el **[aprovisionamiento automático de usuarios](workplacebyfacebook-provisioning-tutorial.md)** .
* Ahora se puede configurar la aplicación Workplace by Facebook con Azure AD para habilitar el inicio de sesión único. En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

## <a name="adding-workplace-by-facebook-from-the-gallery"></a>Incorporación de Workplace by Facebook desde la galería

Para configurar la integración de Workplace by Facebook en Azure AD, deberá agregar Workplace by Facebook desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Workplace by Facebook** en el cuadro de búsqueda.
1. Seleccione **Workplace by Facebook** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

Configure y pruebe el inicio de sesión único de Azure AD con Workplace by Facebook mediante un usuario de prueba llamado **B. Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Workplace by Facebook.

Para configurar y probar el inicio de sesión único de Azure AD con Workplace by Facebook, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
2. **[Configuración del inicio de sesión único de Workplace by Facebook](#configure-workplace-by-facebook-sso)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Workplace by Facebook](#create-workplace-by-facebook-test-user)** : para tener un homólogo de B. Simon en Workplace by Facebook que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Workplace by Facebook**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, seleccione **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, especifique los valores de los siguientes campos:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<instancename>.facebook.com`

    b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://www.facebook.com/company/<instanceID>`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Consulte la página de autenticación del panel de la empresa Workplace para obtener los valores correctos de la comunidad Workplace.

4. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up Workplace by Facebook** (Configurar Workplace by Facebook), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="configure-workplace-by-facebook-sso"></a>Configuración del inicio de sesión único de Workplace by Facebook

1. En otra ventana del explorador web, inicie sesión en el sitio de empresa de Workplace by Facebook como administrador.
  
    > [!NOTE]
    > Como parte del proceso de autenticación SAML, es posible que Workplace use cadenas de consulta de hasta 2,5 kilobytes de tamaño para pasar parámetros a Azure AD.

2. En el **panel de administración**, vaya a la pestaña **Seguridad**.

    ![Panel de administración](./media/workplacebyfacebook-tutorial/tutorial-workplace-by-facebook-configure01.png)

3. En la pestaña **Autenticación**, seleccione **Inicio de sesión único (SSO)** y realice los pasos siguientes:

    ![Pestaña Autenticación](./media/workplacebyfacebook-tutorial/tutorial-workplace-by-facebook-configure02.png)

    a. En el cuadro de texto **Dirección URL de SAML**, pegue el valor de la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    b. En el cuadro de texto **SAML Issuer URI** (URI del emisor de SAML), pegue el valor del **identificador de Azure AD** que ha copiado de Azure Portal.

    c. En el cuadro de texto **Logout URL SAML** (Dirección URL de cierre de sesión de SAML), pegue el valor de **Dirección URL de cierre de sesión** que ha copiado de Azure Portal.

    d. Abra el **certificado codificado en Base 64** descargado de Azure Portal en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **Certificado SAML**.

    e. Copie el valor de **Audience URL** (URL de público) de su instancia y péguela en el cuadro de texto **Identifier (Entity ID)** [Identificador (Id. de entidad)] de la sección **Configuración de SAML básica** de Azure Portal.

    f. Copie el valor de **Recipient URL** (URL de destinatario) y péguelo en el cuadro de texto **URL de inicio de sesión** de la sección **Configuración básica de SAML** en Azure Portal.

    g. Desplácese hasta la parte inferior de la sección y haga clic en el botón **Probar SSO**. Como resultado aparece una ventana emergente en la que se muestra la página de inicio de sesión de Azure AD. Escriba las credenciales de la forma habitual para autenticarse.

    **Solución de problemas:** Asegúrese de que la dirección de correo electrónico que se devuelve desde Azure AD es la misma que la cuenta de Workplace con la que ha iniciado sesión.

    h. Una vez que la prueba se ha realizado correctamente, desplácese hasta la parte inferior de la página y haga clic en el botón **Guardar**.

    i. Ahora se presentará a todos los usuarios de Workplace la página de inicio de sesión de Azure AD para la autenticación.

4. **Redirigir el cierre de sesión de SAML (opcional)**  -

    Puede optar por configurar una dirección URL de cierre de sesión de SAML que puede usarse para apuntar a la página de cierre de sesión de Azure AD. Si esta opción está habilitada y configurada, ya no se dirige al usuario a la página de cierre de sesión de Workplace, sino que se le redirige a la dirección URL agregada en la opción Redirigir el cierre de sesión de SAML.

### <a name="configuring-reauthentication-frequency"></a>Configuración de la frecuencia de reautenticación

Puede configurar Workplace para que solicite una comprobación SAML cada día, cada tres días, cada semana, cada dos semanas, cada mes o nunca.

> [!NOTE]
> El valor mínimo para la comprobación SAML en aplicaciones móviles se establece en una semana.

También puede forzar un restablecimiento de SAML para todos los usuarios con este botón: Require SAML authentication for all users now (Requerir autenticación de SAML para todos los usuarios ahora).

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

En esta sección, habilitará a B. Simon para usar el inicio de sesión único de Azure al concederle acceso a Workplace by Facebook.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Workplace by Facebook**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-workplace-by-facebook-test-user"></a>Creación de un usuario de prueba de Workplace by Facebook

En esta sección se crea un usuario llamado B. Simon en Workplace by Facebook. Workplace by Facebook admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada.

El usuario no tiene que hacer nada en esta sección. Si no existe un usuario en Workplace by Facebook, se crea uno cuando intenta obtener acceso a Workplace by Facebook.

>[!Note]
>Si necesita crear un usuario de forma manual, póngase en contacto con el [equipo de soporte de cliente de Workplace by Facebook](https://workplace.fb.com/faq/)

### <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Workplace by Facebook en el panel de acceso, debería iniciar sesión automáticamente en la versión de Workplace by Facebook para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="test-sso-for-workplace-by-facebook-mobile"></a>Prueba del inicio de sesión único para Workplace by Facebook (Mobile)

1. Abra la aplicación de Workplace by Facebook Mobile. En la página de inicio de sesión, haga clic en **LOG IN** (iniciar sesión).

    ![Inicio de sesión](./media/workplacebyfacebook-tutorial/test05.png)

2. Escriba el correo electrónico de su empresa y haga clic en **CONTINUE** (CONTINUAR).

    ![El correo electrónico](./media/workplacebyfacebook-tutorial/test02.png)

3. Haga clic en **JUST ONCE** (SOLO UNA VEZ).

    ![Una vez](./media/workplacebyfacebook-tutorial/test04.png)

4. Haga clic en **Permitir**.

    ![Permiso](./media/workplacebyfacebook-tutorial/test03.png)

5. Finalmente, después de iniciar sesión correctamente, aparecerá la página principal de la aplicación.    

    ![Página principal](./media/workplacebyfacebook-tutorial/test01.png)

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Configuración del aprovisionamiento de usuarios](workplacebyfacebook-provisioning-tutorial.md)

