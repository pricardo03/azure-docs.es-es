---
title: 'Tutorial: Integración de Azure Active Directory con GitHub | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y GitHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8761f5ca-c57c-4a7e-bf14-ac0421bd3b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeedes
ms.openlocfilehash: dbd4634c575fd4f1886d3e7714ef9ddabbde0f8a
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341164"
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Tutorial: Integración de Azure Active Directory con GitHub

En este tutorial, obtendrá información sobre cómo integrar GitHub con Azure Active Directory (Azure AD).

Integrar GitHub con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a GitHub.
- Puede permitir que los usuarios inicien sesión automáticamente en GitHub mediante inicio de sesión único (SSO) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con GitHub, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en GitHub

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de GitHub desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-github-from-the-gallery"></a>Adición de GitHub desde la galería
Para configurar la integración de GitHub en Azure AD, debe agregar GitHub desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar GitHub desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/github-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/github-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/github-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **GitHub**, seleccione **GitHub** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/github-tutorial/tutorial_github_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con GitHub mediante un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo en GitHub de un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de GitHub.

Para configurar y probar el SSO de Azure AD con GitHub, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de GitHub](#create-a-github-test-user)**: para tener un homólogo de Britta Simon en GitHub que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación GitHub.

**Para configurar el inicio de sesión único de Azure AD con GitHub, realice los pasos siguientes:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **GitHub**, seleccione **Inicio de sesión único**.

    ![imagen](./media/github-tutorial/b1_b2_select_sso.png)

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** a fin de habilitar el inicio de sesión único.

    ![imagen](./media/github-tutorial/b1_b2_saml_sso.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/github-tutorial/b1-domains_and_urlsedit.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![imagen](./media/github-tutorial/tutorial_github_url.png) 

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://github.com/orgs/<entity-id>/sso`

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://github.com/orgs/<entity-id>`

    > [!NOTE]
    > Tenga en cuenta que estos no son valores reales. Tendrá que actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Aquí le recomendamos que utilice el valor de cadena único en el identificador. Vaya a la sección de administración de GitHub para recuperar estos valores.

5. La aplicación GitHub espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. Haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](./media/github-tutorial/i3-attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:
    
    a. Haga clic en el botón **Editar** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](./media/github-tutorial/i2-attribute.png)

    ![imagen](./media/github-tutorial/i4-attribute.png)

    b. En la lista **Atributo de origen**, seleccione el valor del atributo.

    c. Haga clic en **Save**(Guardar).
 
7. En la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **Certificado (Base64)** y guarde el archivo de certificado en el equipo.

    ![imagen](./media/github-tutorial/tutorial_github_certficate.png)

8. En la sección **Configurar GitHub**, copie la dirección URL adecuada según sus necesidades.

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![imagen](./media/github-tutorial/d1_samlsonfigure.png) 

9. En otra ventana del navegador web, inicie sesión en el sitio de la organización de GitHub como administrador.

10. Vaya a **Configuración** y haga clic en **Seguridad**

    ![Configuración](./media/github-tutorial/tutorial_github_config_github_03.png)

11. Active la casilla **Habilitar autenticación SAML** para ver los campos de configuración del inicio de sesión único. Luego, use el valor de URL de inicio de sesión único para actualizar la URL de inicio de sesión único en la configuración de Azure AD.

    ![Configuración](./media/github-tutorial/tutorial_github_config_github_13.png)

12. Configure los campos siguientes:

    ![Configuración](./media/github-tutorial/tutorial_github_config_github_051.png)

    a. En el cuadro de texto **Sign-On URL** (Dirección URL de inicio de sesión), pegue el valor de la **URL de inicio de sesión** que ha copiado de Azure Portal.

    b. En el cuadro de texto **Emisor**, pegue el valor del **Identificador de Azure AD** que ha copiado de Azure Portal.

    c. Abra el certificado descargado de Azure Portal en el Bloc de notas y pegue el contenido en el cuadro de texto **Certificado público**.

    d. Haga clic en el icono **Editar** para editar el **Método de firma** y el **Método de resumen** desde **RSA-SHA1** y **SHA1** hasta **RSA-SHA256** y **SHA256** tal como se muestra a continuación.

    ![imagen](./media/github-tutorial/tutorial_github_sha.png) 
    
13. Haga clic en **Test SAML configuration** (Probar configuración de SAML) para configura que no hay errores de validación durante el SSO.

    ![Configuración](./media/github-tutorial/tutorial_github_config_github_06.png)

14. Haga clic en **Guardar**

> [!NOTE]
> El inicio de sesión único en GitHub realiza la autenticación en una organización específica de GitHub, pero no reemplaza la autenticación propia de GitHub. Por tanto, si la sesión del usuario en GitHub.com ha expirado, puede que se le pida que se autentique con la contraseña o el identificador de GitHub durante el proceso de inicio de sesión único.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, seleccione **Usuarios** y, luego, seleccione **Todos los usuarios**.

    ![imagen](./media/github-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/github-tutorial/d_adduser.png)

3. En las Propiedades de usuario, siga los pasos a continuación.

    ![imagen](./media/github-tutorial/d_userproperties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-a-github-test-user"></a>Creación de un usuario de prueba de GitHub

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en GitHub. GitHub admite el aprovisionamiento automático de usuarios, que está habilitado de forma predeterminada. [Aquí](github-provisioning-tutorial.md) puede encontrar más información sobre cómo configurar el aprovisionamiento automático de usuarios.

**Para crear un usuario manualmente, siga los pasos siguientes:**

1. Inicie sesión en el sitio de la empresa de GitHub como administrador.

2. Haga clic en **Contactos**.

    ![Personas](./media/github-tutorial/tutorial_github_config_github_08.png "Personas")

3. Haga clic en **Invitar a miembros**.

    ![Invitación de usuarios](./media/github-tutorial/tutorial_github_config_github_09.png "Invitación de usuarios")

4. En la página de diálogo **Invitar a miembros**, realice los siguientes pasos:

    a. En el cuadro de texto **Correo electrónico**, escriba la dirección de correo electrónico de la cuenta de Britta Simon.

    ![Invitar a personas](./media/github-tutorial/tutorial_github_config_github_10.png "Invitar a personas")

    b. Haga clic en **Enviar invitación**.

    ![Invitar a personas](./media/github-tutorial/tutorial_github_config_github_11.png "Invitar a personas")

    > [!NOTE]
    > El titular de la cuenta de Azure Active Directory recibirá un mensaje de correo y seguirá un vínculo para confirmar su cuenta antes de que se active.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a GitHub.

1. En Azure Portal, seleccione **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![imagen](./media/github-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **GitHub**.

    ![imagen](./media/github-tutorial/tutorial_github_app.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/github-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el panel **Agregar asignación**.

    ![imagen](./media/github-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de GitHub del panel de acceso, debe iniciar sesión automáticamente en su aplicación GitHub.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


