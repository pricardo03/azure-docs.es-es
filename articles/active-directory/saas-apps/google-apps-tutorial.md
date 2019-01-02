---
title: 'Tutorial: Integración de Azure Active Directory con G Suite | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y G Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2018
ms.author: jeedes
ms.openlocfilehash: bcea7848c7331ecd326f0ccb6ab9f543ce972205
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52834696"
---
# <a name="tutorial-azure-active-directory-integration-with-g-suite"></a>Tutorial: Integración de Azure Active Directory con G Suite

En este tutorial, aprenderá a integrar G Suite con Azure Active Directory (Azure AD).

La integración de G Suite con Azure AD proporciona las siguientes ventajas:

- En Azure AD se puede controlar quién tiene acceso a G Suite.
- Puede permitir que los usuarios inicien sesión automáticamente en G Suite (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con G Suite, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada de G Suite para el inicio de sesión único
- Una suscripción de Google Apps o Google Cloud Platform

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción. Este documento se creó con la nueva experiencia de inicio de sesión del usuario. Si aún usa la antigua, el programa de instalación parecerá diferente. Puede habilitar la nueva experiencia en la configuración de inicio de sesión único de la aplicación G Suite. Vaya a las **aplicaciones empresariales de Azure AD**, seleccione **G Suite**, seleccione **Inicio de sesión único** y, luego, haga clic en **Probar la nueva experiencia**.

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

6. **Error: Correo electrónico no válido**

    En esta configuración, se necesita el atributo de correo electrónico para que los usuarios puedan iniciar sesión. Este atributo no se puede establecer manualmente.

    El atributo de correo electrónico se rellena automáticamente para cualquier usuario con una licencia válida de Exchange. Si el usuario no está habilitado para correo electrónico, se recibirá este error ya que la aplicación debe obtener este atributo para proporcionar acceso.

    Puede ir a portal.office.com con una cuenta de administrador, hacer clic en Centro de administración < Facturación < Suscripciones, seleccionar su suscripción a Office 365 y, a continuación, hacer clic en Asignar a usuarios, seleccionar los usuarios para los que desee comprobar su suscripción y, en el panel derecho, hacer clic en Editar licencias.

    Una vez asignada la licencia de Office 365, puede tardar algunos minutos en aplicarse. Después de eso, el atributo user.mail se rellenará automáticamente y el problema debería resolverse.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de G Suite desde la Galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-g-suite-from-the-gallery"></a>Incorporación de G Suite desde la Galería

Para configurar la integración de G Suite en Azure AD, deberá agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar G Suite desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **G Suite**, seleccione **G Suite** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

    ![G Suite en la lista de resultados](./media/google-apps-tutorial/tutorial_gsuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con G Suite con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el homólogo de G Suite del usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre el usuario de Azure AD y el usuario asociado de G Suite.

Para configurar y probar el inicio de sesión único de Azure AD con G Suite, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de G Suite](#creating-a-g-suite-test-user)**: para tener un homólogo de Britta Simon en G Suite vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación G Suite.

**Para configurar el inicio de sesión único de Azure AD con G Suite, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **G Suite**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial_general_301.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de G Suite](./media/google-apps-tutorial/tutorial_gsuite_url.png)

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

5. La aplicación G Suite espera las instrucciones de aserción de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](./media/google-apps-tutorial/i3-attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:

     a. Haga clic en el botón **Editar** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](./media/google-apps-tutorial/i2-attribute.png)

    ![imagen](./media/google-apps-tutorial/i4-attribute.png)

    b. En la lista **Atributo de origen**, seleccione el valor del atributo.

    c. Haga clic en **Save**(Guardar).

5. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic **Descargar** para descargar el **certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/google-apps-tutorial/tutorial_gsuite_certificate.png) 

6. En la sección **Configurar G Suite**, copie la dirección URL adecuada según sus necesidades.

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![Configuración de G Suite](common/configuresection.png)

9. Abra una nueva pestaña en el explorador e inicie sesión en la [consola de administración de G Suite](https://admin.google.com/) con la cuenta de administrador.

10. Haga clic en **Seguridad**. Si no ve el vínculo, puede estar oculto debajo del menú **Más controles** en la parte inferior de la pantalla.

    ![Haga clic en Seguridad.][10]

11. En la página **Seguridad**, haga clic en **Configurar inicio de sesión único (SSO)**.

    ![Haga clic en SSO.][11]

12. Realice los cambios de configuración siguientes:

    ![Configuración de SSO][12]

     a. Seleccione **Configurar SSO con un proveedor de identidades de terceros**.

    b. En el campo **Dirección URL de la página de inicio de sesión** de G Suite, pegue el valor de la  **URL de inicio de sesión**  que copió de Azure Portal.

    c. En el campo **Dirección URL de la página de cierre de sesión** de G Suite, pegue el valor de la  **URL de cierre de sesión**  que copió de Azure Portal.

    d. En el campo **Cambiar dirección URL de contraseña** de G Suite, pegue el valor de la  **URL de cambio de contraseña**  que copió de Azure Portal.

    e. En G Suite, para el **certificado de verificación** y cargue el certificado que descargó de Azure Portal.

    f. Seleccione **Use a domain specific issuer** (Usar un emisor de dominio específico).

    g. Haga clic en **Guardar cambios**.

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_02.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.

### <a name="creating-a-g-suite-test-user"></a>Creación de un usuario de prueba de G Suite

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en el software G Suite. G Suite admite el aprovisionamiento automático, que está habilitado de manera predeterminada. El usuario no tiene que hacer nada en esta sección. Si el usuario aún no existe en el software G Suite, se crea uno al intentar acceder.

> [!NOTE]
> Asegúrese de que el usuario ya existe en G Suite si el aprovisionamiento en Azure AD no se ha activado antes de probar el inicio de sesión único.

> [!NOTE]
> Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de Google](https://www.google.com/contact/).

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a G Suite.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **G Suite**.

    ![Configurar inicio de sesión único](./media/google-apps-tutorial/tutorial_gsuite_app.png)

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de G Suite en el Panel de acceso, debería iniciar sesión automáticamente en la aplicación G Suite.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
<!--Image references-->

[10]: ./media/google-apps-tutorial/gapps-security.png
[11]: ./media/google-apps-tutorial/security-gapps.png
[12]: ./media/google-apps-tutorial/gapps-sso-config.png