---
title: 'Tutorial: Integración de Azure Active Directory con Appraisd | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Appraisd.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: db063306-4d0d-43ca-aae0-09f0426e7429
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2018
ms.author: jeedes
ms.openlocfilehash: dae9a59b89f03a50b1adaed55cd8f97c06906526
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49118340"
---
# <a name="tutorial-azure-active-directory-integration-with-appraisd"></a>Tutorial: Integración de Azure Active Directory con Appraisd

En este tutorial, aprenderá a integrar Appraisd con Azure Active Directory (Azure AD).

La integración de Appraisd con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Appraisd.
- Puede permitir que los usuarios inicien sesión automáticamente en Appraisd (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Appraisd, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único en Appraisd

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Appraisd desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-appraisd-from-the-gallery"></a>Adición de Appraisd desde la galería
Para configurar la integración de Appraisd en Azure AD, será preciso que agregue Appraisd desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Appraisd desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/appraisd-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/appraisd-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/appraisd-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **Appraisd**, seleccione **Appraisd** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/appraisd-tutorial/tutorial_appraisd_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Appraisd con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Appraisd para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Appraisd.

Para configurar y probar el inicio de sesión único de Azure AD con Appraisd, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Appraisd](#create-an-appraisd-test-user)**: para tener un homólogo de Britta Simon en Appraisd que esté vinculado a la representación de ella en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Appraisd.

**Para configurar el inicio de sesión único de Azure AD con Appraisd, realice los pasos siguientes:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Appraisd**, seleccione **Inicio de sesión único**.

    ![imagen](./media/appraisd-tutorial/B1_B2_Select_SSO.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML** para habilitar el inicio de sesión único.

    ![imagen](./media/appraisd-tutorial/b1_b2_saml_sso.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/appraisd-tutorial/b1-domains_and_urlsedit.png)

4. En la sección **Configuración de SAML básica**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por **IDP**:

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_url.png)

    a. Haga clic en **Establecer direcciones URL adicionales**. 

    b. En el cuadro de texto **Estado de la retransmisión**, escriba una dirección URL: `<TENANTCODE>`

    c. Si quiere configurar la aplicación en modo iniciado por **SP**, en el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL siguiendo este patrón: `https://app.appraisd.com/saml/<TENANTCODE>`

    > [!NOTE]
    > La URL de inicio de sesión y el valor del estado de retransmisión se obtienen en la página de configuración de SSO de Appraisd, que se explica más adelante en el tutorial.
    
5. La aplicación Appraisd espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. Haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](./media/appraisd-tutorial/i3-attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:
    
    a. Haga clic en el botón **Editar** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](./media/appraisd-tutorial/i2-attribute.png)

    ![imagen](./media/appraisd-tutorial/i4-attribute.png)

    b. En la lista **Atributo de origen**, seleccione el valor del atributo.

    c. Haga clic en **Save**(Guardar). 

7. En la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **Certificado (Base64)** y guarde el archivo de certificado en el equipo.

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_certficate.png) 

8. En la sección **Configurar Appraisd**, copie la dirección URL adecuada según sus necesidades.

    a. URL de inicio de sesión

    b. Identificador de Azure Ad

    c. URL de cierre de sesión

    ![imagen](./media/appraisd-tutorial/d1_samlsonfigure.png)

9. En otra ventana del explorador web, inicie sesión en Appraisd como administrador de seguridad.

10. En la parte superior derecha de la página, haga clic en el icono **Configuración** y, después, vaya a **Configuración**.

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_sett.png)

11. En el lado izquierdo del menú, haga clic en el **inicio de sesión único SAML**.

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_single.png)

12. En la página **Configuración de inicio de sesión único de SAML 2.0**, siga estos pasos:
    
    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_saml.png)

    a. Copie el valor **Default Relay State** (Estado retransmisión predeterminado) y péguelo en el cuadro de texto **Estado de la retransmisión** de **Basic SAML Configuration** (Configuración básica de SAML) en Azure Portal.

    b. Copie el valor **Service-initiated login URL** (URL de inicio de sesión iniciado por el servicio) y péguelo en el cuadro de texto **URL de inicio de sesión** de **Basic SAML Configuration** (Configuración básica de SAML) en Azure Portal.

13. En la página **Identificación de usuarios**, desplácese hacia abajo y realice los pasos siguientes:

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_identifying.png)

    a. En el cuadro de texto **Dirección URL del inicio de sesión único del proveedor de identidades**, pegue el valor de **URL de inicio de sesión** que ha copiado de Azure Portal y haga clic en **Guardar**.

    b. En el cuadro de texto **Identity Provider Issuer URL** (Dirección URL del emisor del proveedor de identidades), pegue el valor de **Identificador de Azure Ad** que ha copiado de Azure Portal y haga clic en **Guardar**.

    c. En el Bloc de notas, abra el certificado codificado en Base 64 que descargó de Azure Portal, copie el contenido y, luego, péguelo en el cuadro de texto **X.509 Certificate** (Certificado X.509) y haga clic en **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, seleccione **Usuarios** y, a continuación, seleccione **Todos los usuarios**.

    ![imagen](./media/appraisd-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/appraisd-tutorial/d_adduser.png)

3. En las propiedades de usuario, realice los pasos siguientes.

    ![imagen](./media/appraisd-tutorial/d_userproperties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-an-appraisd-test-user"></a>Creación de un usuario de prueba Appraisd

Para permitir que los usuarios de Azure AD inicien sesión en Appraisd, deben aprovisionarse en Appraisd. En Appraisd, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en Appraisd como administrador de seguridad.

2. En la parte superior derecha de la página, haga clic en el icono **Settings** (Configuración) y, después, vaya al **Centro de administración**.

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_admin.png)

3. En la barra de herramientas situada en la parte superior de la página, haga clic en **Personas** y, después, vaya a **Agregar un nuevo usuario**.

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_user.png)

4. En la página **Agregar nuevo usuario**, realice los pasos siguientes:

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_newuser.png)
    
    a. En el cuadro de texto **First Name** (Nombre), escriba el nombre del usuario, en este caso **Britta**.

    b. En el cuadro de texto **Last Name** (Apellidos), escriba los apellidos del usuario, en este caso **Simon**.

    c. En el cuadro de texto **E-mail** (Correo electrónico), escriba el correo electrónico del usuario; por ejemplo, **Brittasimon@contoso.com**.

    d. Haga clic en **Agregar usuario**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediendo acceso a Appraisd.

1. En Azure Portal, seleccione **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![imagen](./media/appraisd-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **Appraisd**.

    ![imagen](./media/appraisd-tutorial/tutorial_appraisd_app.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/appraisd-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![imagen](./media/appraisd-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios y, a continuación, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Appraisd en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación Appraisd.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
