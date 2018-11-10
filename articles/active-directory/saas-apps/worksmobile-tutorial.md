---
title: 'Tutorial: Integración de Azure Active Directory con LINE WORKS | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y LINE WORKS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 725f32fd-d0ad-49c7-b137-1cc246bf85d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: jeedes
ms.openlocfilehash: 0437bd46537d8b7024618706aba12660abca5512
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50414699"
---
# <a name="tutorial-azure-active-directory-integration-with-line-works"></a>Tutorial: Integración de Azure Active Directory con LINE WORKS

En este tutorial, obtendrá información sobre cómo integrar LINE WORKS con Azure Active Directory (Azure AD).

La integración de LINE WORKS con Azure AD proporciona las siguientes ventajas:

- En Azure AD se puede controlar quién tiene acceso a LINE WORKS.
- Puede permitir que los usuarios inicien sesión automáticamente en LINE WORKS (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con LINE WORKS, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en LINE WORKS

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de LINE WORKS desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-line-works-from-the-gallery"></a>Incorporación de LINE WORKS desde la galería
Para configurar la integración de LINE WORKS en Azure AD, es preciso agregar LINE WORKS desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar LINE WORKS desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/worksmobile-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/worksmobile-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/worksmobile-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **LINE WORKS** , seleccione **LINE WORKS**  en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/worksmobile-tutorial/tutorial_lineworks_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con LINE WORKS con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de LINE WORKS para un usuario de Azure AD. Es decir, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de LINE WORKS.

Para configurar y probar el inicio de sesión único de Azure AD con LINE WORKS, es necesario completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de LINE WORKS](#create-a-line-works-test-user)**: para tener un homólogo de Britta Simon en LINE WORKS que esté vinculado a su representación en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación LINE WORKS.

**Para configurar el inicio de sesión único de Azure AD con LINE WORKS, realice los pasos siguientes:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **LINE WORKS**, seleccione **Inicio de sesión único**.

    ![imagen](./media/worksmobile-tutorial/b1_b2_select_sso.png)

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![imagen](./media/worksmobile-tutorial/b1_b2_saml_sso.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/worksmobile-tutorial/b1-domains_and_urlsedit.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://auth.worksmobile.com/d/login/<domain>/?userId=<ID@domain>`

    b. En el cuadro de texto **Identificador**, escriba una dirección URL: `worksmobile.com`

    ![imagen](./media/worksmobile-tutorial/tutorial_lineworks_url.png)

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Raw)** y guárdelo en su equipo.

    ![imagen](./media/worksmobile-tutorial/tutorial_lineworks_certficate.png)

6. En la sección **Configurar LINE WORKS**, copie la dirección URL adecuada según sus necesidades.

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![imagen](./media/worksmobile-tutorial/d1_samlsonfigure.png) 

7. Para configurar el inicio de sesión único en el lado **LINE WORKS**, lea los [documentos de SSO de LINE WORKS](https://developers.worksmobile.com/jp/document/1001080101) y configure un entorno de LINE WORKS.

> [!NOTE]
> Deberá convertir el archivo de certificado descargado de .cert a .pem.


### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![imagen](./media/worksmobile-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/worksmobile-tutorial/d_adduser.png)

3. En las propiedades Usuario, siga estos pasos.

    ![imagen](./media/worksmobile-tutorial/d_userproperties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-a-line-works-test-user"></a>Creación de un usuario de prueba de LINE WORKS

En esta sección, creará un usuario llamado Britta Simon en LINE WORKS. Acceda a la [página de administración de LINE WORKS](https://admin.worksmobile.com) y agregue usuarios a la plataforma LINE WORKS.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a LINE WORKS.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![imagen](./media/worksmobile-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **LINE WORKS**.

    ![imagen](./media/worksmobile-tutorial/tutorial_lineworks_app.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/worksmobile-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![imagen](./media/worksmobile-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de LINE WORKS en el panel de acceso, debería iniciar sesión automáticamente en su aplicación LINE WORKS.
Para más información sobre el Panel de acceso, vea la [introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


