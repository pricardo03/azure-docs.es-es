---
title: 'Tutorial: Integración de Azure Active Directory con Druva | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Druva.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdf359f38bd7032b4de48a17b91ba1c10b165c91
ms.sourcegitcommit: f9448a4d87226362a02b14d88290ad6b1aea9d82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66807781"
---
# <a name="tutorial-integrate-druva-with-azure-active-directory"></a>Tutorial: Integración de Druva con Azure Active Directory

En este tutorial, aprenderá a integrar Druva con Azure Active Directory (Azure AD). Al integrar Druva con Azure AD, se puede:

* Controlar en Azure AD quién tiene acceso a Druva.
* Permitir que los usuarios puedan iniciar sesión automáticamente en Druva con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede obtener la evaluación gratuita de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Druva.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba. Druva admite el inicio de sesión único iniciado tanto por **SP** como por **IDP**

## <a name="adding-druva-from-the-gallery"></a>Incorporación de Druva desde la galería

Para configurar la integración de Druva en Azure AD, tendrá que agregar Druva desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Druva** en el cuadro de búsqueda.
1. Seleccione **Druva** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

Configure y pruebe el inicio de sesión único de Azure AD con Druva utilizando un usuario de prueba llamado **Britta Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Druva.

Para configurar y probar el inicio de sesión único de Azure AD con Druva, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
2. **[Configuración del inicio de sesión único en Druva](#configure-druva-sso)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Druva](#create-druva-test-user)** : para tener un homólogo de Britta Simon en Druva que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Druva**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, seleccione **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, no es necesario realizar ningún paso porque la aplicación ya está integrada previamente con Azure.

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL: `https://login.druva.com/login`

1. La aplicación Druva espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

1. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, edite las notificaciones mediante el **icono Editar** o agregue notificaciones mediante **Agregar nueva notificación** para configurar el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:

    | NOMBRE | Atributo de origen|
    | ------------------- | -------------------- |
    | insync\_auth\_token |Escriba el valor del token generado |

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

   ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Set up Druva** (Configurar Druva), copie las direcciones URL adecuadas según sus necesidades.

   ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="configure-druva-sso"></a>Configurar el inicio de sesión único de Druva

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía en Druva.

1. Vaya a **Administrar \> Configuración**.

    ![Configuración](./media/druva-tutorial/ic795091.png "Configuración")

1. En el cuadro de diálogo Single Sign-On Settings (Configuración de inicio de sesión único), siga estos pasos:

    ![Configuración de inicio de sesión único](./media/druva-tutorial/ic795092.png "Configuración de inicio de sesión único")

    a. En el cuadro de texto **ID Provider Login URL** (Dirección URL de inicio de sesión del proveedor de identidades), pegue el valor de **Dirección URL de inicio de sesión**  que ha copiado de Azure Portal.

    b. En el cuadro de texto **ID Provider Logout URL** (Dirección URL de cierre de sesión del proveedor de identidades), pegue el valor de **Dirección URL de cierre de sesión**  que ha copiado de Azure Portal.

    c. Abra el certificado codificado en base 64 en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **Certificado de proveedor de Id.** .

    d. Para abrir la página **Configuración**, haga clic en **Guardar**.

1. En la página **Configuración**, haga clic en**Generar token de SSO**.

    ![Configuración](./media/druva-tutorial/ic795093.png "Configuración")

1. En el cuadro de diálogo **Token de autenticación de inicio de sesión único** , siga estos pasos:

    ![Token de SSO](./media/druva-tutorial/ic795094.png "Token de SSO")

    a. Haga clic en **Copiar**, pegue el valor copiado en el cuadro de texto **Valor** en la sección **Agregar atributo** de Azure Portal.

    b. Haga clic en **Cerrar**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado Britta Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `Britta Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `BrittaSimon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, se habilita a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Druva.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Druva**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En la lista de usuarios del cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-druva-test-user"></a>Creación de un usuario de prueba en Druva

Para que los usuarios de Azure AD puedan iniciar sesión en Druva, es preciso aprovisionarlos en Druva. En el caso de Druva, el aprovisionamiento es una tarea manual.

**Siga estos pasos para configurar el aprovisionamiento de usuario:**

1. Inicie sesión en el sitio de la compañía de **Druva** como administrador.

1. Vaya a **Administrar \> usuarios**.

    ![Administración de usuarios](./media/druva-tutorial/ic795097.png "Administración de usuarios")

1. Haga clic en **Create New**.

    ![Administración de usuarios](./media/druva-tutorial/ic795098.png "Administración de usuarios")

1. En el cuadro de diálogo Create New User (Crear nuevo usuario), realice los pasos siguientes:

    ![Creación de un nuevo usuario](./media/druva-tutorial/ic795099.png "Creación de un nuevo usuario")

    a. En el cuadro de texto **Email address** (Dirección de correo electrónico), escriba la dirección de correo electrónico de un usuario, por ejemplo, **brittasimon\@contoso.com**.

    b. En el cuadro de texto **Name** (Nombre), escriba el nombre de usuario, por ejemplo, **BrittaSimon**.

    c. Haga clic en **Create User**(Crear usuario).

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Druva que proporcione Druva para aprovisionar cuentas de usuario de Azure AD.

### <a name="test-sso"></a>Prueba de SSO

Al seleccionar el icono de Druva en el Panel de acceso, debería iniciar sesión automáticamente en la instancia de Druva para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
