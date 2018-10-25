---
title: 'Tutorial: integración de Azure Active Directory con G Suite | Microsoft Docs'
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
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: 6974350c6abbc6c5f5a8e10b22e91796e2564b08
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268066"
---
# <a name="tutorial-azure-active-directory-integration-with-g-suite"></a>Tutorial: integración de Azure Active Directory con G Suite

En este tutorial, aprenderá a integrar G Suite con Azure Active Directory (Azure AD).

La integración de G Suite con Azure AD proporciona las siguientes ventajas:

- En Azure AD se puede controlar quién tiene acceso a G Suite.
- Puede permitir que los usuarios inicien sesión automáticamente en G Suite (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con G Suite, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada de G Suite para el inicio de sesión único
- Una suscripción de Google Apps o Google Cloud Platform

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="frequently-asked-questions"></a>Preguntas frecuentes

1.  **P: ¿Es esta integración compatible con la integración del inicio de sesión único de Google Cloud Platform con Azure AD?**
    
    R: Sí. Google Cloud Platform y Google Apps comparten la misma plataforma de autenticación. Por ello, para la integración de GCP debe configurar el inicio de sesión único con Google Apps.


1. **P: ¿Son los Chromebooks y otros dispositivos Chrome compatibles con el inicio de sesión único de Azure AD?**
   
    R: Sí, los usuarios pueden iniciar sesión en sus dispositivos Chromebook con sus credenciales de Azure AD. Consulte este [artículo de soporte técnico de G Suite](https://support.google.com/chrome/a/answer/6060880) para información sobre por qué se les pueden pedir a los usuarios las credenciales dos veces.

1. **P: Si se habilita el inicio de sesión único, ¿podrán usar los usuarios sus credenciales de Azure AD para iniciar sesión en cualquier producto de Google, como Google Classroom, GMail, Google Drive, YouTube, etc.?**
   
    R: Sí, en función de la [ versión de G Suite](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) que decida habilitar o deshabilitar para su organización.

1. **P: ¿Puedo habilitar el inicio de sesión único solo para un subconjunto de usuarios de G Suite?**
   
    R: No; si activa el inicio de sesión único, es necesario de inmediato que todos los usuarios de G Suite se autentiquen con sus credenciales de Azure AD. Dado que G Suite no admite varios proveedores de identidades, el proveedor de identidades del entorno de G Suite puede ser Azure AD o Google, pero no ambos al mismo tiempo.

1. **P: Si un usuario inicia sesión mediante Windows, ¿se autentica automáticamente en G Suite sin que se le pida la contraseña?**
   
    R: Hay dos opciones para habilitar este escenario. En primer lugar, los usuarios podrían iniciar sesión en dispositivos Windows 10 a través de [Azure Active Directory Join](../device-management-introduction.md). Como alternativa, los usuarios podrían iniciar sesión en dispositivos Windows que están unidos a un dominio en un entorno Active Directory local que se ha habilitado para el inicio de sesión único en Azure AD a través de una implementación de los [Servicios de federación de Active Directory (AD FS)](../hybrid/plan-connect-user-signin.md) . Ambas opciones requieren los pasos del tutorial siguiente para permitir el inicio de sesión único entre Azure AD y G Suite.

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de G Suite desde la Galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-g-suite-from-the-gallery"></a>Incorporación de G Suite desde la Galería

Para configurar la integración de G Suite en Azure AD, deberá agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar G Suite desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/google-apps-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/google-apps-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/google-apps-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **G Suite**, seleccione **G Suite** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/google-apps-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con G Suite con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el homólogo de G Suite del usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre el usuario de Azure AD y el usuario asociado de G Suite.

Para establecer la relación de vínculo, en G Suite, asigne el valor de **nombre de usuario** de Azure AD como valor de **Username** (Nombre de usuario).

Para configurar y probar el inicio de sesión único de Azure AD con G Suite, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de G Suite](#create-a-g-suite-test-user)**: para tener un homólogo de Britta Simon en G Suite vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación G Suite.

**Para configurar el inicio de sesión único de Azure AD con G Suite, realice los pasos siguientes:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **G Suite**, seleccione **Inicio de sesión único**.

    ![imagen](./media/google-apps-tutorial/b1_b2_select_sso.png)

2. Haga clic en **Cambiar el modo de inicio de sesión único** en la parte superior de la pantalla para seleccionar el modo **SAML**.

      ![imagen](./media/google-apps-tutorial/b1_b2_saml_ssso.png)

3. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** a fin de habilitar el inicio de sesión único.

    ![imagen](./media/google-apps-tutorial/b1_b2_saml_sso.png)

4. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/google-apps-tutorial/b1-domains_and_urlsedit.png)

5. En la sección **Configuración básica de SAML**, siga estos pasos:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |

    ![imagen](./media/google-apps-tutorial/b1-domains_and_urls.png)
 
    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte de cliente de G Suite](https://www.google.com/contact/) para obtener estos valores.

6. La aplicación G Suite espera las instrucciones de aserción de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](./media/google-apps-tutorial/i3-attribute.png)

7. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:
    
    a. Haga clic en el botón **Editar** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](./media/google-apps-tutorial/i2-attribute.png)

    ![imagen](./media/google-apps-tutorial/i4-attribute.png)

    b. En la lista **Atributo de origen**, seleccione el valor del atributo.

    c. Haga clic en **Save**(Guardar).

8. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el certificado adecuado según sus requisitos y guárdelo en el equipo.

    ![imagen](./media/google-apps-tutorial/certificatebase64.png)

9. En la sección **Configurar G Suite**, copie la dirección URL adecuada según sus necesidades.

    Tenga en cuenta que en la dirección URL puede poner lo siguiente:

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![imagen](./media/google-apps-tutorial/d1_saml.png) 

10. Abra una nueva pestaña en el explorador e inicie sesión en la [consola de administración de G Suite](http://admin.google.com/) con la cuenta de administrador.

11. Haga clic en **Seguridad**. Si no ve el vínculo, puede estar oculto debajo del menú **Más controles** en la parte inferior de la pantalla.
   
    ![Haga clic en Seguridad.][10]

12. En la página **Seguridad**, haga clic en **Configurar inicio de sesión único (SSO)**.
   
    ![Haga clic en SSO.][11]

13. Realice los cambios de configuración siguientes:
   
    ![Configuración de SSO][12]
   
    a. Seleccione **Configurar SSO con un proveedor de identidades de terceros**.

    b. En el campo **Sign-in page URL** (Dirección URL de la página de inicio de sesión) de G Suite, pegue el valor de la **URL de servicio de inicio de sesión único** que copió de Azure Portal.

    c. En el campo **Sign-out page URL** (Dirección URL de cierre de sesión) de G Suite, pegue el valor de la **URL de cierre de sesión** que copió de Azure Portal. 

    d. En el campo **Change password URL** (Dirección URL de cambio de contraseña) de G Suite, pegue el valor de la **URL de cambio de contraseña** que copió de Azure Portal. 

    e. En G Suite, para el **certificado de verificación** y cargue el certificado que descargó de Azure Portal.

    f. Seleccione **Use a domain specific issuer** (Usar un emisor de dominio específico).

    g. Haga clic en **Guardar cambios**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, seleccione **Usuarios** y luego seleccione **Todos los usuarios**.

    ![imagen](./media/google-apps-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/google-apps-tutorial/d_adduser.png)

3. En las Propiedades de usuario, siga los pasos a continuación.

    ![imagen](./media/google-apps-tutorial/d_userproperties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-a-g-suite-test-user"></a>Creación de un usuario de prueba de G Suite

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en el software G Suite. G Suite admite el aprovisionamiento automático, que está habilitado de manera predeterminada. El usuario no tiene que hacer nada en esta sección. Si el usuario aún no existe en el software G Suite, se crea uno al intentar acceder.

>[!NOTE]
>Asegúrese de que el usuario ya existe en G Suite si el aprovisionamiento en Azure AD no se ha activado antes de probar el inicio de sesión único.

>[!NOTE] 
>Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de Google](https://www.google.com/contact/).



### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a G Suite.

1. En Azure Portal, seleccione **Aplicaciones empresariales** y **Todas las aplicaciones**.

    ![imagen](./media/google-apps-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **G Suite**.

    ![imagen](./media/google-apps-tutorial/d_all_proapplications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/google-apps-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![imagen](./media/google-apps-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de G Suite en el Panel de acceso, debería iniciar sesión automáticamente en la aplicación G Suite.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[10]: ./media/google-apps-tutorial/gapps-security.png
[11]: ./media/google-apps-tutorial/security-gapps.png
[12]: ./media/google-apps-tutorial/gapps-sso-config.png

