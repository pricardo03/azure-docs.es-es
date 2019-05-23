---
title: 'Tutorial: Integración de Azure Active Directory con Workplace by Facebook | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Workplace by Facebook.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/12/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 27173c8beeecf2be43e80f59df8907952734c06b
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65905246"
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Tutorial: Integración de Azure Active Directory con Workplace by Facebook

En este tutorial obtendrá información sobre cómo integrar Workplace by Facebook con Azure Active Directory (Azure AD).
Integrar Workplace by Facebook con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Workplace by Facebook.
* Puede permitir que los usuarios inicien sesión automáticamente en Workplace by Facebook (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Workplace by Facebook, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para inicio de sesión único de Workplace by Facebook

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

> [!NOTE]
> Facebook tiene dos productos: Workplace Standard (gratis) and Workplace Premium (de pago). Todo inquilino de Workplace Premium puede configurar la integración de SCIM y SSO sin otras implicaciones de coste ni requerimiento de licencias. SSO y SCIM no están disponibles en las instancias de Workplace Standard.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Workplace by Facebook admite el inicio de sesión único iniciado por **SP**.
* Workplace by Facebook admite el **aprovisionamiento Just-In-Time**.
* Workplace by Facebook admite el **[aprovisionamiento automático de usuarios](workplacebyfacebook-provisioning-tutorial.md)**.

## <a name="adding-workplace-by-facebook-from-the-gallery"></a>Incorporación de Workplace by Facebook desde la galería

Para configurar la integración de Workplace by Facebook en Azure AD, deberá agregar Workplace by Facebook desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Workplace by Facebook desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Workplace by Facebook**, seleccione **Workplace by Facebook** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Workplace by Facebook en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección podrá configurar y probar el inicio de sesión único de Azure AD con Workplace by Facebook con un usuario de prueba llamado **Britta Simon**.
Para que funcione el inicio de sesión único, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Workplace by Facebook.

Para configurar y probar el inicio de sesión único de Azure AD con Workplace by Facebook, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Workplace by Facebook](#configure-workplace-by-facebook-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Workplace by Facebook](#create-workplace-by-facebook-test-user)**: para tener un homólogo de Britta Simon en Workplace by Facebook que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Workplace by Facebook, siga estos pasos:

1. En la página de integración de la aplicación **Workplace by Facebook** de [Azure Portal](https://portal.azure.com/), seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Workplace by Facebook](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<instancename>.facebook.com`

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://www.facebook.com/company/<instanceID>`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Consulte la página de autenticación del panel de la empresa Workplace para obtener los valores correctos de la comunidad Workplace.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up Workplace by Facebook** (Configurar Workplace by Facebook), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-workplace-by-facebook-single-sign-on"></a>Configuración del inicio de sesión único de Workplace by Facebook

1. En otra ventana del explorador web, inicie sesión en el sitio de empresa de Workplace by Facebook como administrador.
  
    > [!NOTE]
    > Como parte del proceso de autenticación SAML, es posible que Workplace use cadenas de consulta de hasta 2,5 kilobytes de tamaño para pasar parámetros a Azure AD.

2. En el **panel de administración**, vaya a la pestaña **Seguridad**.

    ![Panel de administración](./media/workplacebyfacebook-tutorial/tutorial-workplace-by-facebook-configure01.png)

3. En la pestaña **Autenticación**, seleccione **Inicio de sesión único (SSO)** y realice los pasos siguientes:

    ![Pestaña Autenticación](./media/workplacebyfacebook-tutorial/tutorial-workplace-by-facebook-configure02.png)

     a. En el cuadro de texto **Dirección URL de SAML**, pegue el valor de la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    b. En el cuadro de texto **SAML Issuer URI** (URI del emisor de SAML), pegue el valor del **identificador de Azure AD** que ha copiado de Azure Portal.

    c. En el cuadro de texto **Logout URL SAML** (Dirección URL de cierre de sesión de SAML), pegue el valor de **Dirección URL de cierre de sesión** que ha copiado de Azure Portal.

    d. Abra el **certificado codificado en Base 64** descargado de Azure Portal en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **Certificado SAML**.

    e. Copie el valor de **Audience URL** (URL de público) de su instancia y péguela en el cuadro de texto **Identifier (Entity ID)** [Identificador (Id. de entidad)] de la sección **Configuración de SAML básica** de Azure Portal.

    f. Copie el valor de **Recipient URL** (URL de destinatario) y péguelo en el cuadro de texto **URL de inicio de sesión** de la sección **Configuración básica de SAML** en Azure Portal.

    g. Desplácese hasta la parte inferior de la sección y haga clic en el botón **Probar SSO**. Como resultado aparece una ventana emergente en la que se muestra la página de inicio de sesión de Azure AD. Escriba las credenciales de la forma habitual para autenticarse.

    **Solución de problemas:** Asegúrese de que la dirección de correo electrónico que se devuelve desde Azure AD es la misma que la cuenta de Workplace con la que ha iniciado sesión.

    h. Una vez que la prueba se ha realizado correctamente, desplácese hasta la parte inferior de la página y haga clic en el botón **Guardar**.

    i. Ahora se presentará a todos los usuarios de Workplace la página de inicio de sesión de Azure AD para la autenticación.

4. **Redirigir el cierre de sesión de SAML (opcional)** -

    Puede optar por configurar una dirección URL de cierre de sesión de SAML que puede usarse para apuntar a la página de cierre de sesión de Azure AD. Si esta opción está habilitada y configurada, ya no se dirige al usuario a la página de cierre de sesión de Workplace, sino que se le redirige a la dirección URL agregada en la opción Redirigir el cierre de sesión de SAML.

### <a name="configuring-reauthentication-frequency"></a>Configuración de la frecuencia de reautenticación

Puede configurar Workplace para que solicite una comprobación SAML cada día, cada tres días, cada semana, cada dos semanas, cada mes o nunca.

> [!NOTE]
> El valor mínimo para la comprobación SAML en aplicaciones móviles se establece en una semana.

También puede forzar un restablecimiento de SAML para todos los usuarios con este botón: Require SAML authentication for all users now (Requerir autenticación de SAML para todos los usuarios ahora).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon\@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para usar el inicio de sesión único de Azure al concederle acceso a Workplace by Facebook.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **Workplace by Facebook**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **Workplace by Facebook**.

    ![Vínculo de Workplace by Facebook en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-workplace-by-facebook-test-user"></a>Creación de un usuario de prueba de Workplace by Facebook

En esta sección se crea un usuario llamado Britta Simon en Workplace by Facebook. Workplace by Facebook admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada.

El usuario no tiene que hacer nada en esta sección. Si no existe un usuario en Workplace by Facebook, se crea uno cuando intenta obtener acceso a Workplace by Facebook.

>[!Note]
>Si necesita crear un usuario de forma manual, póngase en contacto con el [equipo de soporte de cliente de Workplace by Facebook](https://workplace.fb.com/faq/)

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Workplace by Facebook en el panel de acceso, debería iniciar sesión automáticamente en la versión de Workplace by Facebook para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Configuración del aprovisionamiento de usuarios](workplacebyfacebook-provisioning-tutorial.md)
