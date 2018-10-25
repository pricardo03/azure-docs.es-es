---
title: 'Tutorial: Integración de Azure Active Directory con Zendesk | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Zendesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: 37d20eabfc8fb883cda346dc8b55a17b8b959218
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268181"
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Tutorial: Integración de Azure Active Directory con Zendesk

En este tutorial, aprenderá a integrar Zendesk con Azure Active Directory (Azure AD).

La integración de Zendesk con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Zendesk.
- Puede permitir que los usuarios inicien sesión automáticamente en Zendesk (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Zendesk, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Zendesk

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.
El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Zendesk desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-zendesk-from-the-gallery"></a>Adición de Zendesk desde la galería

Para configurar la integración de Zendesk en Azure AD, es preciso agregar dicha solución desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Zendesk desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/zendesk-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/zendesk-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/zendesk-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **Zendesk**, seleccione **Zendesk** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/zendesk-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Zendesk con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Zendesk para un usuario de Azure AD. Es decir, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Zendesk.

Para establecer la relación de vínculo, en Zendesk, asigne el valor de **nombre de usuario** de Azure AD como valor de **nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Zendesk, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Crear un usuario de prueba de Zendesk](#create-a-zendesk-test-user)**: el objetivo es tener un homólogo de Britta Simon en Zendesk que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Zendesk.

**Para configurar el inicio de sesión único de Azure AD con Zendesk, realice los pasos siguientes:**

1. En la página de integración de la aplicación **Zendesk** de [Azure Portal](https://portal.azure.com/), seleccione **Inicio de sesión único**.

    ![imagen](./media/zendesk-tutorial/b1_b2_select_sso.png)

2. Haga clic en **Cambiar el modo de inicio de sesión único** en la parte superior de la pantalla para seleccionar el modo **SAML**.

      ![imagen](./media/zendesk-tutorial/b1_b2_saml_ssso.png)

3. En el cuadro de diálogo **Select a Single sign-on method** (Seleccione un método de inicio de sesión único), haga clic en **Seleccionar** para el modo **SAML** a fin de habilitar el inicio de sesión único.

    ![imagen](./media/zendesk-tutorial/b1_b2_saml_sso.png)

4. En la página **Set up Single Sign-On with SAML** (Configurar el inicio de sesión único con SAML), haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/zendesk-tutorial/b1-domains_and_urlsedit.png)

5. En la sección **Configuración básica de SAML**, siga estos pasos:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el patrón siguiente: `https://<subdomain>.zendesk.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `<subdomain>.zendesk.com`.

    ![imagen](./media/zendesk-tutorial/b1-domains_and_urls.png)

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico para clientes de Zendesk](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) para obtener estos valores.

6. La aplicación Zendesk espera las aserciones de SAML en un formato específico. No hay ningún atributo SAML obligatorio, pero opcionalmente puede agregar un atributo desde la sección **Atributos de usuario** en la página de integración de la aplicación. En la página **Set up Single Sign-On with SAML** (Configurar el inicio de sesión único con SAML), haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](./media/zendesk-tutorial/i4-attribute.png)

7. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, configure el atributo Token SAML como se muestra en la imagen anterior y realice los siguientes pasos:

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](./media/zendesk-tutorial/i2-attribute.png)

    ![imagen](./media/zendesk-tutorial/i3-attribute.png)
    
    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.
    
    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.
    
    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

    > [!NOTE]
    > Los atributos de extensión se usan para agregar atributos que, de forma predeterminada, no están en Azure AD. Haga clic en [User attributes that can be set in SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) (Atributos de usuario que se pueden usar en SAML) para obtener la lista de atributos que acepta **Zendesk**.

8. En la sección **Certificado de firma de SAML**, copie la **huella digital** y guárdela en el equipo.

    ![imagen](./media/zendesk-tutorial/C3_certificate.png)

    a. Seleccione la opción adecuada para la **Opción de firma**, si es necesario.

    b. Seleccione la opción adecuada para el **Algoritmo de firma**, si es necesario.

    c. Haga clic en **Guardar**

9. En la sección **Set up Zendesk** (Configurar Zendesk), haga clic en **Ver instrucciones detalladas** para abrir la ventana **Configurar inicio de sesión**. Copie las direcciones URL de la sección **Referencia rápida**.

    Tenga en cuenta que en la dirección URL puede poner lo siguiente:

    a. Dirección URL del servicio de inicio de sesión único de SAML

    b. El identificador de entidad

    c. Dirección URL de cierre de sesión

    ![imagen](./media/zendesk-tutorial/d1_saml.png) 

10. Hay dos maneras de configurar Zendesk: automática y manual.
  
11. Para automatizar la configuración de Zendesk, debe instalar la **extensión de explorador de inicio de sesión seguro de Mis aplicaciones**. Para ello, haga clic en **Install the extension** (Instalar la extensión).

    ![imagen](./media/zendesk-tutorial/install_extension.png)

12. Después de agregar la extensión al explorador, haga clic en **Setup Zendesk** (Configurar Zendesk) para ir a la aplicación Zendesk. Desde allí, proporcione las credenciales de administrador para iniciar sesión en Zendesk. La extensión de explorador configurará automáticamente la aplicación y automatizará el paso 13.

     ![imagen](./media/zendesk-tutorial/d2_saml.png) 

13. Si quiere configurar Zendesk manualmente, abra una nueva ventana del explorador web, inicie sesión en el sitio de empresa de Zendesk como administrador y realice los pasos siguientes:

    * Haga clic en **Administrador**.

    * En el panel de navegación izquierdo, haga clic en **Settings** (Configuración) y luego en **Security** (Seguridad).

    * En la pestaña **Security** (Seguridad), lleve a cabo los pasos siguientes:

      ![Seguridad](././media/zendesk-tutorial/ic773089.png "Seguridad")

      ![Inicio de sesión único](././media/zendesk-tutorial/ic773090.png "Inicio de sesión único")

      a. Haga clic en la pestaña **Admin & Agents** (Administración y agentes).

      b. Seleccione **Single sign-on (SSO) and SAML** (Inicio de sesión único (SSO) y SAML) y, luego, seleccione **SAML**.

      c. En el cuadro de texto **SAML SSO URL** (Dirección URL de inicio de sesión único de SAML), pegue el valor de **dirección URL del servicio de inicio de sesión único de SAML** que ha copiado de Azure Portal.

      d. En el cuadro de texto **Logout URL** (Dirección URL de cierre de sesión remoto), pegue el valor de **dirección URL de cierre de sesión** que copió de Azure Portal.

      e. En el cuadro de texto **Certificate Fingerprint** (Huella digital de certificado), pegue el valor de **Huella digital** del certificado que haya copiado de Azure Portal.

      f. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, seleccione **Usuarios** y, luego, seleccione **Todos los usuarios**.

    ![imagen](./media/zendesk-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/zendesk-tutorial/d_adduser.png)

3. En las Propiedades de usuario, siga los pasos que se indican a continuación.

    ![imagen](./media/zendesk-tutorial/d_userproperties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.

### <a name="create-a-zendesk-test-user"></a>Crear un usuario de prueba de Zendesk

El objetivo de esta sección es crear un usuario llamado Britta Simon en Zendesk. Zendesk admite el aprovisionamiento automático de usuarios, que está habilitado de forma predeterminada. [Aquí](Zendesk-provisioning-tutorial.md) puede encontrar más información sobre cómo configurar el aprovisionamiento automático de usuarios.

**Para crear un usuario manualmente, siga los pasos siguientes:**

> [!NOTE]
> Las cuentas de **usuario final** se aprovisionan automáticamente al iniciar sesión. Las cuentas de **agente** y **administrador** se deben aprovisionar manualmente en **Zendesk** antes de iniciar la sesión.

1. Inicie sesión en su inquilino de **Zendesk** .

2. Seleccione la pestaña **Customer List** (Lista de clientes).

3. Seleccione la pestaña **User** (Usuario) y haga clic en **Add** (Agregar).

    ![Agregar usuario](././media/zendesk-tutorial/ic773632.png "Agregar usuario")
4. Escriba el **nombre** y la **dirección de correo electrónico** de una cuenta de Azure AD existente que quiera aprovisionar y, luego, haga clic en **Guardar**.

    ![Nuevo usuario](././media/zendesk-tutorial/ic773633.png "Nuevo usuario")

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Zendesk ofrecida por Zendesk para aprovisionar cuentas de usuario de AAD.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Zendesk.

1. En Azure Portal, seleccione **Aplicaciones empresariales** y **Todas las aplicaciones**.

    ![imagen](./media/zendesk-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **Zendesk**.

    ![imagen](./media/zendesk-tutorial/d_all_zendeskapplications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/zendesk-tutorial/d_leftpaneusers.png)

4. Haga clic en el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el panel **Agregar asignación**.

    ![imagen](./media/zendesk-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Zendesk en el Panel de acceso, debería iniciar sesión automáticamente en dicha aplicación.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configuración del aprovisionamiento de usuarios](zendesk-provisioning-tutorial.md)
