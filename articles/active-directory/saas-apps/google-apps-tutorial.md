---
title: 'Tutorial: Integración de Azure Active Directory con G Suite | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y G Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/04/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d48977c60fb4a13e1fc0dbd294fa4e7708f1cd5d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58878975"
---
# <a name="tutorial-azure-active-directory-integration-with-g-suite"></a>Tutorial: Integración de Azure Active Directory con G Suite

En este tutorial, aprenderá a integrar G Suite con Azure Active Directory (Azure AD).
La integración de G Suite con Azure AD proporciona las siguientes ventajas:

* En Azure AD se puede controlar quién tiene acceso a G Suite.
* Puede permitir que los usuarios inicien sesión automáticamente en G Suite (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con G Suite, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada de G Suite para el inicio de sesión único
- Una suscripción de Google Apps o Google Cloud Platform

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción. Este documento se creó con la nueva experiencia de inicio de sesión del usuario. Si aún usa la anterior, el programa de instalación parecerá diferente. Puede habilitar la nueva experiencia en la configuración de inicio de sesión único de la aplicación G Suite. Vaya a las **aplicaciones empresariales de Azure AD**, seleccione **G Suite**, seleccione **Inicio de sesión único** y, luego, haga clic en **Probar la nueva experiencia**.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="frequently-asked-questions"></a>Preguntas frecuentes

1. **P. ¿Es esta integración compatible con la integración del inicio de sesión único de Google Cloud Platform con Azure AD?**

    R: Sí. Google Cloud Platform y Google Apps comparten la misma plataforma de autenticación. Por ello, para la integración de GCP debe configurar el inicio de sesión único con Google Apps.

2. **P. ¿Son los Chromebooks y otros dispositivos Chrome compatibles con el inicio de sesión único de Azure AD?**
  
    R: Sí, los usuarios pueden iniciar sesión en sus dispositivos Chromebook con sus credenciales de Azure AD. Consulte este [artículo de soporte técnico de G Suite](https://support.google.com/chrome/a/answer/6060880) para información sobre por qué se les pueden pedir a los usuarios las credenciales dos veces.

3. **P. Si se habilita el inicio de sesión único, ¿podrán usar los usuarios sus credenciales de Azure AD para iniciar sesión en cualquier producto de Google, como Google Classroom, GMail, Google Drive, YouTube, etc.?**

    R: Sí, en función de la [ versión de G Suite](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) que decida habilitar o deshabilitar para su organización.

4. **P. ¿Puedo habilitar el inicio de sesión único solo para un subconjunto de usuarios de G Suite?**

    R: No; si activa el inicio de sesión único, es necesario de inmediato que todos los usuarios de G Suite se autentiquen con sus credenciales de Azure AD. Dado que G Suite no admite varios proveedores de identidades, el proveedor de identidades del entorno de G Suite puede ser Azure AD o Google, pero no ambos al mismo tiempo.

5. **P. Si un usuario inicia sesión mediante Windows, ¿se autentica automáticamente en G Suite sin que se le pida la contraseña?**

    R: Hay dos opciones para habilitar este escenario. En primer lugar, los usuarios podrían iniciar sesión en dispositivos Windows 10 a través de [Azure Active Directory Join](../device-management-introduction.md). Como alternativa, los usuarios podrían iniciar sesión en dispositivos Windows que están unidos a un dominio en un entorno Active Directory local que se ha habilitado para el inicio de sesión único en Azure AD a través de una implementación de los [Servicios de federación de Active Directory (AD FS)](../hybrid/plan-connect-user-signin.md) . Ambas opciones requieren los pasos del tutorial siguiente para permitir el inicio de sesión único entre Azure AD y G Suite.

6. **P: ¿Qué debo hacer cuando aparece un mensaje de error de correo electrónico no válido?**

    R: En esta configuración, se necesita el atributo de correo electrónico para que los usuarios puedan iniciar sesión. Este atributo no se puede establecer manualmente.

    El atributo de correo electrónico se rellena automáticamente para cualquier usuario con una licencia válida de Exchange. Si el usuario no está habilitado para correo electrónico, se recibirá este error ya que la aplicación debe obtener este atributo para proporcionar acceso.

    Puede ir a portal.office.com con una cuenta de administrador, hacer clic en Centro de administración < Facturación < Suscripciones, seleccionar su suscripción a Office 365 y, a continuación, hacer clic en Asignar a usuarios, seleccionar los usuarios para los que desee comprobar su suscripción y, en el panel derecho, hacer clic en Editar licencias.

    Una vez asignada la licencia de Office 365, puede tardar algunos minutos en aplicarse. Después de eso, el atributo user.mail se rellenará automáticamente y el problema debería resolverse.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* G Suite admite el inicio de sesión único iniciado por **SP**.
* G Suite admite el **[aprovisionamiento automático de usuarios](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-provisioning-tutorial)**.

## <a name="adding-g-suite-from-the-gallery"></a>Incorporación de G Suite desde la Galería

Para configurar la integración de G Suite en Azure AD, deberá agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar G Suite desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **G Suite**, seleccione **G Suite** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![G Suite en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con G Suite con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de G Suite.

Para configurar y probar el inicio de sesión único de Azure AD con G Suite, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de G Suite](#configure-g-suite-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de G Suite](#create-g-suite-test-user)**: para tener un homólogo de Britta Simon en G Suite vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con G Suite, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **G Suite**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar **Gmail**, realice los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de G Suite](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón:

    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `https://google.com` |
    | `https://google.com/a/<yourdomain.com>` |

    > [!NOTE]
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte de cliente de G Suite](https://www.google.com/contact/) para obtener estos valores.

5. En la sección **Configuración básica de SAML**, si desea configurar **Google Cloud Platform**, realice los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de G Suite](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://console.cloud.google.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón:
    
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `https://google.com` |
    | `https://google.com/a/<yourdomain.com>` |
    
    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte de cliente de G Suite](https://www.google.com/contact/) para obtener estos valores.

6. La aplicación G Suite espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token SAML. La siguiente captura de pantalla le muestra un ejemplo de esto. El valor predeterminado de **Identificador de usuario único** es **user.userprincipalname**, pero G Suite espera que este valor se asigne a la dirección de correo electrónico del usuario. Para ello, puede usar el atributo **user.mail** de la lista o usar el valor de atributo correspondiente en función de la configuración de su organización.

    ![imagen](common/edit-attribute.png)

7. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, edite las notificaciones mediante el **icono Editar** o agregue notificaciones mediante **Agregar nueva notificación** para configurar el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:

    | NOMBRE | Atributo de origen |
    | ---------------| --------------- |
    | Identificador de usuario único | User.mail |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

8. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

9. En la sección **Configurar G Suite**, copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-g-suite-single-sign-on"></a>Configuración del inicio de sesión único de G Suite

1. Abra una nueva pestaña en el explorador e inicie sesión en la [consola de administración de G Suite](https://admin.google.com/) con la cuenta de administrador.

2. Haga clic en **Seguridad**. Si no ve el vínculo, puede estar oculto debajo del menú **Más controles** en la parte inferior de la pantalla.

    ![Haga clic en Seguridad.][10]

3. En la página **Seguridad**, haga clic en **Configurar inicio de sesión único (SSO)**.

    ![Haga clic en SSO.][11]

4. Realice los cambios de configuración siguientes:

    ![Configuración de SSO][12]

     a. Seleccione **Configurar SSO con un proveedor de identidades de terceros**.

    b. En el campo **Dirección URL de la página de inicio de sesión** de G Suite, pegue el valor de la  **URL de inicio de sesión**  que copió de Azure Portal.

    c. En el campo **Dirección URL de la página de cierre de sesión** de G Suite, pegue el valor de la  **URL de cierre de sesión**  que copió de Azure Portal.

    d. En el campo **Cambiar dirección URL de contraseña** de G Suite, pegue el valor de la  **URL de cambio de contraseña**  que copió de Azure Portal.

    e. En G Suite, para el **certificado de verificación** y cargue el certificado que descargó de Azure Portal.

    f. Seleccione **Use a domain specific issuer** (Usar un emisor de dominio específico).

    g. Haga clic en **Guardar cambios**.

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a G Suite.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **G Suite**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **G Suite**.

    ![Vínculo a G Suite en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-g-suite-test-user"></a>Creación de un usuario de prueba de G Suite

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en el software G Suite. G Suite admite el aprovisionamiento automático, que está habilitado de manera predeterminada. El usuario no tiene que hacer nada en esta sección. Si el usuario aún no existe en el software G Suite, se crea uno al intentar acceder.

> [!NOTE]
> Asegúrese de que el usuario ya existe en G Suite si el aprovisionamiento en Azure AD no se ha activado antes de probar el inicio de sesión único.

> [!NOTE]
> Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de Google](https://www.google.com/contact/).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de G Suite en el panel de acceso y debería iniciar sesión automáticamente en la versión de G Suite para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Configuración del aprovisionamiento de usuarios](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-provisioning-tutorial)

<!--Image references-->

[10]: ./media/google-apps-tutorial/gapps-security.png
[11]: ./media/google-apps-tutorial/security-gapps.png
[12]: ./media/google-apps-tutorial/gapps-sso-config.png
