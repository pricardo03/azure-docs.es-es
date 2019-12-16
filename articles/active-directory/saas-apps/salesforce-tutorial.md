---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Salesforce | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Salesforce.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/13/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d137a96a967e017c67da7dfabf25502de32ba7a
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74970050"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-salesforce"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Salesforce

En este tutorial, aprenderá a integrar Salesforce con Azure Active Directory (Azure AD). Al integrar Salesforce con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Salesforce.
* Permitir que los usuarios inicien sesión automáticamente en Salesforce con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Suscripción habilitada para el inicio de sesión único (SSO) en Salesforce.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Salesforce admite el inicio de sesión único iniciado por **SP**

* Salesforce admite el [aprovisionamiento y desaprovisionamiento **automático** de usuarios](salesforce-provisioning-tutorial.md) (recomendado).

* Salesforce admite el aprovisionamiento de usuarios **Just-In-Time**

* Ahora se puede configurar la aplicación móvil Salesforce con Azure AD para habilitar el inicio de sesión único. En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

## <a name="adding-salesforce-from-the-gallery"></a>Adición de Salesforce desde la galería

Para configurar la integración de Salesforce en Azure AD, deberá agregar Salesforce desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Salesforce** en el cuadro de búsqueda.
1. Seleccione **Salesforce** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-salesforce"></a>Configuración y prueba del inicio de sesión único de Azure AD para Salesforce

Configure y pruebe el inicio de sesión único de Azure AD con Salesforce mediante una usuaria de prueba llamada **B. Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Salesforce.

Para configurar y probar el inicio de sesión único de Azure AD con Salesforce, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
2. **[Configuración del inicio de sesión único de Salesforce](#configure-salesforce-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en Salesforce](#create-salesforce-test-user)** , para tener un homólogo de B. Simon en Salesforce que esté vinculado a su representación en Azure AD.
3. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Salesforce, realice los pasos siguientes:

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Salesforce**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, seleccione **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, siga estos pasos:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba el valor con el siguiente patrón:

    Cuenta de empresa: `https://<subdomain>.my.salesforce.com`

    Cuenta de desarrollador: `https://<subdomain>-dev-ed.my.salesforce.com`

    b. En el cuadro de texto **Identificador**, escriba el valor con el siguiente patrón:

    Cuenta de empresa: `https://<subdomain>.my.salesforce.com`

    Cuenta de desarrollador: `https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE]
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de atención al cliente de Salesforce](https://help.salesforce.com/support) para obtener estos valores.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Set up Salesforce** (Configurar Salesforce), copie las direcciones URL adecuada según sus necesidades.

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
   
    > [!NOTE]
    > Los atributos de usuario de Salesforce distinguen mayúsculas de minúsculas en la validación de SAML.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a permitir que B. Simon acceda a Salesforce mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Salesforce**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-salesforce-sso"></a>Configuración del inicio de sesión único de Salesforce

1. Abra una nueva pestaña en el explorador e inicie sesión en su cuenta de administrador de Salesforce.

2. Haga clic en **Setup** (Configuración) en el **icono de configuración** de la esquina superior derecha de la página.

    ![Configurar inicio de sesión único](./media/salesforce-tutorial/configure1.png)

3. Desplácese hacia abajo hasta **SETTINGS** (CONFIGURACIÓN) en el panel de navegación y haga clic en **Identity** (Identidad) para expandir la sección relacionada. A continuación, haga clic en **Configuración de inicio de sesión único**.

    ![Configurar inicio de sesión único](./media/salesforce-tutorial/sf-admin-sso.png)

4. En la página **Configuración de inicio de sesión único**, haga clic en el botón **Editar**.

    ![Configurar inicio de sesión único](./media/salesforce-tutorial/sf-admin-sso-edit.png)

    > [!NOTE]
    > Si no puede habilitar la configuración de inicio de sesión único para su cuenta de Salesforce, puede que necesite ponerse en contacto con el [equipo de soporte técnico de Salesforce](https://help.salesforce.com/support).

5. Seleccione **SAML habilitado** y haga clic en **Guardar**.

      ![Configurar inicio de sesión único](./media/salesforce-tutorial/sf-enable-saml.png)

6. Para establecer la configuración de inicio de sesión único de SAML, haga clic en **New from Metadata File** (Nuevo archivo de metadatos).

    ![Configurar inicio de sesión único](./media/salesforce-tutorial/sf-admin-sso-new.png)

7. Haga clic en **Choose File** (Elegir archivo) para cargar el archivo XML de metadatos que ha descargado desde Azure Portal y haga clic en **Create** (Crear).

    ![Configurar inicio de sesión único](./media/salesforce-tutorial/xmlchoose.png)

8. En la página **SAML Single Sign-On Settings** (Configuración de inicio de sesión único de SAML), los campos se rellenan automáticamente. Haga clic en Save (Guardar).

    ![Configurar inicio de sesión único](./media/salesforce-tutorial/salesforcexml.png)

9. En el panel de navegación izquierdo de Salesforce, haga clic en **Company Settings** (Configuración de la empresa) para expandir la sección relacionada y haga clic en **My Domain** (Mi dominio).

    ![Configurar inicio de sesión único](./media/salesforce-tutorial/sf-my-domain.png)

10. Desplácese hacia abajo hasta la sección **Authentication Configuration** (Configuración de autenticación) y haga clic en el botón **Edit** (Editar).

    ![Configurar inicio de sesión único](./media/salesforce-tutorial/sf-edit-auth-config.png)

11. En la sección **Authentication Configuration** (Configuración de autenticación), seleccione **AzureSSO** como **Authentication Service** (Servicio de autenticación) de su configuración del inicio de sesión único de SAML y haga clic en **Save** (Guardar).

    ![Configurar inicio de sesión único](./media/salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Si se selecciona más de un servicio de autenticación, cuando los usuarios intentan realizar un inicio de sesión único para el entorno Salesforce, se les pedirá que seleccionen el servicio de autenticación con el que les gustaría iniciar sesión. Si no desea que esto ocurra, **deje sin activar todos los demás servicios de autenticación**.

### <a name="create-salesforce-test-user"></a>Creación de un usuario de prueba de Salesforce

En esta sección, creará una usuaria llamada B. Simon en Salesforce. Salesforce admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si el usuario no existe aún en Salesforce, se crea uno nuevo cuando se intenta acceder a esta aplicación. Salesforce también admite el aprovisionamiento automático de usuarios. [Aquí](salesforce-provisioning-tutorial.md) puede encontrar más detalles sobre cómo configurar el aprovisionamiento automático de usuarios.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Salesforce en el panel de acceso y debería iniciar sesión automáticamente en la versión de Salesforce para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="test-sso-for-salesforce-mobile"></a>Prueba del inicio de sesión único para Salesforce (móvil)

1. Abra la aplicación móvil Salesforce. En la página de inicio de sesión, haga clic en **Use Custom Domain** (Usar dominio personalizado).

    ![Aplicación móvil Salesforce](media/salesforce-tutorial/mobile-app1.png)

1. En el cuadro de texto **Custom Domain** (Dominio personalizado), escriba su nombre de dominio personalizado registrado y haga clic en **Continue** (Continuar).

    ![Aplicación móvil Salesforce](media/salesforce-tutorial/mobile-app2.png)

1. Escriba sus credenciales de Azure AD para iniciar sesión en la aplicación Salesforce y haga clic en **Next** (Siguiente).

    ![Aplicación móvil Salesforce](media/salesforce-tutorial/mobile-app3.png)

1. En la página **Allow Access** (Permitir acceso) como se muestra a continuación, haga clic en **Allow** (Permitir) para dar acceso a la aplicación Salesforce.

    ![Aplicación móvil Salesforce](media/salesforce-tutorial/mobile-app4.png)

1. Finalmente, después de iniciar sesión correctamente, aparecerá la página principal de la aplicación.

    ![Aplicación móvil Salesforce](media/salesforce-tutorial/mobile-app5.png) ![Salesforce mobile app](media/salesforce-tutorial/mobile-app6.png)

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Configuración del aprovisionamiento de usuarios](salesforce-provisioning-tutorial.md)

- [Pruebe Salesforce con Azure AD](https://aad.portal.azure.com)
