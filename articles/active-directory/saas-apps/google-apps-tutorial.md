---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con G Suite | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y G Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/14/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d5ef5816759074073c57ef0f616ddea4a159956f
ms.sourcegitcommit: f255f869c1dc451fd71e0cab340af629a1b5fb6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2020
ms.locfileid: "77370351"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-g-suite"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con G Suite

En este tutorial, aprenderá a integrar G Suite con Azure Active Directory (Azure AD). Al integrar G Suite con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a G Suite.
* Permitir que los usuarios inicien sesión automáticamente en G Suite con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

- Una suscripción de Azure AD.
- Una suscripción habilitada para el inicio de sesión único (SSO) en G Suite.
- Una suscripción de Google Apps o Google Cloud Platform

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción. Este documento se creó con la nueva experiencia de inicio de sesión del usuario. Si aún usa la anterior, el programa de instalación parecerá diferente. Puede habilitar la nueva experiencia en la configuración de inicio de sesión único de la aplicación G Suite. Vaya a las **aplicaciones empresariales de Azure AD**, seleccione **G Suite**, seleccione **Inicio de sesión único** y, luego, haga clic en **Probar la nueva experiencia**.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

1. **P: ¿Es esta integración compatible con la integración del inicio de sesión único de Google Cloud Platform con Azure AD?**

    A. Sí. Google Cloud Platform y Google Apps comparten la misma plataforma de autenticación. Por ello, para la integración de GCP debe configurar el inicio de sesión único con Google Apps.

2. **P: ¿Son los Chromebooks y otros dispositivos Chrome compatibles con el inicio de sesión único de Azure AD?**
  
    A. Sí, los usuarios pueden iniciar sesión en sus dispositivos Chromebook con sus credenciales de Azure AD. Consulte este [artículo de soporte técnico de G Suite](https://support.google.com/chrome/a/answer/6060880) para información sobre por qué se les pueden pedir a los usuarios las credenciales dos veces.

3. **P: Si se habilita el inicio de sesión único, ¿podrán usar los usuarios sus credenciales de Azure AD para iniciar sesión en cualquier producto de Google, como Google Classroom, GMail, Google Drive, YouTube, etc.?**

    A. Sí, en función de la [ versión de G Suite](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) que decida habilitar o deshabilitar para su organización.

4. **P: ¿Puedo habilitar el inicio de sesión único solo para un subconjunto de usuarios de G Suite?**

    A. No; si activa el inicio de sesión único, es necesario de inmediato que todos los usuarios de G Suite se autentiquen con sus credenciales de Azure AD. Dado que G Suite no admite varios proveedores de identidades, el proveedor de identidades del entorno de G Suite puede ser Azure AD o Google, pero no ambos al mismo tiempo.

5. **P: Si un usuario inicia sesión mediante Windows, ¿se autentica automáticamente en G Suite sin que se le pida la contraseña?**

    A. Hay dos opciones para habilitar este escenario. En primer lugar, los usuarios podrían iniciar sesión en dispositivos Windows 10 a través de [Azure Active Directory Join](../device-management-introduction.md). Como alternativa, los usuarios podrían iniciar sesión en dispositivos Windows que están unidos a un dominio en un entorno Active Directory local que se ha habilitado para el inicio de sesión único en Azure AD a través de una implementación de los [Servicios de federación de Active Directory (AD FS)](../hybrid/plan-connect-user-signin.md) . Ambas opciones requieren los pasos del tutorial siguiente para permitir el inicio de sesión único entre Azure AD y G Suite.

6. **P: ¿Qué debo hacer cuando aparece un mensaje de error de correo electrónico no válido?**

    A. En esta configuración, se necesita el atributo de correo electrónico para que los usuarios puedan iniciar sesión. Este atributo no se puede establecer manualmente.

    El atributo de correo electrónico se rellena automáticamente para cualquier usuario con una licencia válida de Exchange. Si el usuario no está habilitado para correo electrónico, se recibirá este error ya que la aplicación debe obtener este atributo para proporcionar acceso.

    Puede ir a portal.office.com con una cuenta de administrador, hacer clic en Centro de administración < Facturación < Suscripciones, seleccionar su suscripción a Office 365 y, a continuación, hacer clic en Asignar a usuarios, seleccionar los usuarios para los que desee comprobar su suscripción y, en el panel derecho, hacer clic en Editar licencias.

    Una vez asignada la licencia de Office 365, puede tardar algunos minutos en aplicarse. Después de eso, el atributo user.mail se rellenará automáticamente y el problema debería resolverse.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* G Suite admite el inicio de sesión único iniciado por **SP**.

* G Suite admite el [**aprovisionamiento automático** de usuarios](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-provisioning-tutorial)
* Una vez configurado G Suite, puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad).

## <a name="adding-g-suite-from-the-gallery"></a>Incorporación de G Suite desde la Galería

Para configurar la integración de G Suite en Azure AD, deberá agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **G Suite** en el cuadro de búsqueda.
1. Seleccione **G Suite** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-g-suite"></a>Configuración y prueba del inicio de sesión único de Azure AD para G Suite

Configure y pruebe el inicio de sesión único de Azure AD con G Suite mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de G Suite.

Para configurar y probar el inicio de sesión único de Azure AD con G Suite, complete los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en G Suite](#configure-g-suite-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de G Suite](#create-g-suite-test-user)** : para tener un homólogo de B.Simon en G Suite vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **G Suite**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar **Gmail**, realice los pasos siguientes:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón:

    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `https://google.com` |
    | `https://google.com/a/<yourdomain.com>` |

    c. En el cuadro de texto **URL de respuesta** , escriba una dirección URL con el siguiente patrón: 

    | |
    |--|
    | `https://google.com` |
    | `https://google.com/a/<yourdomain.com>` |


1. En la sección **Configuración básica de SAML**, si desea configurar **Google Cloud Platform**, realice los pasos siguientes:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://console.cloud.google.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón:
    
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `https://google.com` |
    | `https://google.com/a/<yourdomain.com>` |
    
    c. En el cuadro de texto **URL de respuesta** , escriba una dirección URL con el siguiente patrón: 
    
    | |
    |--|
    | `https://google.com` |
    | `https://google.com/a/<yourdomain.com>` |

    > [!NOTE]
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. G Suite no proporciona el valor de identificador de entidad/identificador en la configuración del inicio de sesión único, por lo que si desactiva la opción **domain specific issuer** (emisor de dominio específico), el valor del identificador será `google.com`. Si activa la opción **domain specific issuer** (emisor de dominio específico), la opción será `google.com/a/<yourdomainname.com>`. Para activar o desactivar la opción **domain specific issuer** (emisor de dominio específico) debe ir a la sección **Configuración del inicio de sesión único de G Suite** que se explica más adelante en el tutorial. Para más información, póngase en contacto con el [equipo de soporte de cliente de G Suite](https://www.google.com/contact/).

1. La aplicación G Suite espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token SAML. La siguiente captura de pantalla le muestra un ejemplo de esto. El valor predeterminado de **Identificador de usuario único** es **user.userprincipalname**, pero G Suite espera que este valor se asigne a la dirección de correo electrónico del usuario. Para ello, puede usar el atributo **user.mail** de la lista o usar el valor de atributo correspondiente en función de la configuración de su organización.

    ![imagen](common/default-attributes.png)


1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar G Suite**, copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, habilitará a B.Simon para que use el inicio de sesión único de Azure concediéndole acceso a G Suite.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **G Suite**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-g-suite-sso"></a>Configuración del inicio de sesión único de G Suite

1. Abra una nueva pestaña en el explorador e inicie sesión en la [consola de administración de G Suite](https://admin.google.com/) con la cuenta de administrador.

2. Haga clic en **Seguridad**. Si no ve el vínculo, puede estar oculto debajo del menú **Más controles** en la parte inferior de la pantalla.

    ![Haga clic en Seguridad.][10]

3. En la página **Seguridad**, haga clic en **Configurar inicio de sesión único (SSO)** .

    ![Haga clic en SSO.][11]

4. Realice los cambios de configuración siguientes:

    ![Configuración de SSO][12]

    a. Seleccione **Configurar SSO con un proveedor de identidades de terceros**.

    b. En el campo **Dirección URL de la página de inicio de sesión** de G Suite, pegue el valor de la **URL de inicio de sesión** que copió de Azure Portal.

    c. En el campo **Dirección URL de la página de cierre de sesión** de G Suite, pegue el valor de la **URL de cierre de sesión** que copió de Azure Portal.

    d. En el campo **Change password URL** (Dirección URL de cambio de contraseña) de G Suite, pegue el valor de la **URL de cambio de contraseña** que copió de Azure Portal.

    e. En G Suite, para el **certificado de verificación** y cargue el certificado que descargó de Azure Portal.

    f. Active o desactive la opción **Use a domain specific issuer** (Usar un emisor de dominio específico) según la nota mencionada en la sección **Configuración básica de SAML** anterior en Azure AD.

    g. Haga clic en **Guardar cambios**.

### <a name="create-g-suite-test-user"></a>Creación de un usuario de prueba de G Suite

El objetivo de esta sección es [crear un usuario en G Suite](https://support.google.com/a/answer/33310?hl=en) llamado B.Simon. Una vez creado manualmente el usuario en G Suite, este podrá iniciar sesión con sus credenciales de inicio de sesión de Office 365.

G Suite también admite el aprovisionamiento automático de usuarios. Para configurar el aprovisionamiento automático de usuarios, primero debe [configurar G Suite para el aprovisionamiento automático de usuarios](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-provisioning-tutorial).

> [!NOTE]
> Asegúrese de que el usuario ya existe en G Suite si el aprovisionamiento en Azure AD no se ha activado antes de probar el inicio de sesión único.

> [!NOTE]
> Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de Google](https://www.google.com/contact/).

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de G Suite en el panel de acceso y debería iniciar sesión automáticamente en la versión de G Suite para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Configuración del aprovisionamiento de usuarios](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-provisioning-tutorial)

- [Prueba de G Suite con Azure AD](https://aad.portal.azure.com/)

- [¿Qué es el control de sesiones en Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Protección de G Suite con controles y visibilidad avanzados](https://docs.microsoft.com/cloud-app-security/protect-gsuite)

<!--Image references-->

[10]: ./media/google-apps-tutorial/gapps-security.png
[11]: ./media/google-apps-tutorial/security-gapps.png
[12]: ./media/google-apps-tutorial/gapps-sso-config.png