---
title: 'Tutorial: Integración de Azure Active Directory con Gra-Pe | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Gra-Pe.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 073f8641-b64d-4754-b1a6-2b91c865b13d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeedes
ms.openlocfilehash: 5e642cba3354f4f8589e44d975c0cecf5692c5b5
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49433546"
---
# <a name="tutorial-azure-active-directory-integration-with-gra-pe"></a>Tutorial: Integración de Azure Active Directory con Gra-Pe

En este tutorial, aprenderá a integrar Gra-Pe con Azure Active Directory (Azure AD).

La integración de Gra-Pe con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Gra-Pe.
- Puede permitir que los usuarios inicien sesión automáticamente en Gra-Pe (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Gra-Pe, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Gra-Pe.

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Gra-Pe desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-gra-pe-from-the-gallery"></a>Incorporación de Gra-Pe desde la galería
Para configurar la integración de Gra-Pe en Azure AD, deberá agregar Gra-Pe desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Gra-Pe desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/grape-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/grape-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/grape-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **Gra-Pe**, seleccione **Gra-Pe** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/grape-tutorial/tutorial_grape_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Gra-Pe con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Gra-Pe para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Gra-Pe.

Para configurar y probar el inicio de sesión único de Azure AD con Gra-Pe, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Gra-Pe](#create-a-gra-pe-test-user)** : para tener un homólogo de Britta Simon en Gra-Pe que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en su aplicación Gra-Pe.

**Para configurar el inicio de sesión único de Azure AD con Gra-Pe, realice los pasos siguientes:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones **Gra-Pe**, seleccione **Inicio de sesión único**.

    ![imagen](./media/grape-tutorial/b1_b2_select_sso.png)

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![imagen](./media/grape-tutorial/b1_b2_saml_sso.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/grape-tutorial/b1-domains_and_urlsedit.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    En el cuadro de texto **URL de inicio de sesión**, escriba una URL como: `https://btm.tts.co.jp/portal/apl/SSOLogin.aspx`

    ![imagen](./media/grape-tutorial/tutorial_grape_url.png)

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** y guárdelo en su equipo.

    ![imagen](./media/grape-tutorial/tutorial_grape_certficate.png)

6. En la sección **Configurar Gra-Pe**, copie la dirección URL adecuada según sus necesidades.

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![imagen](./media/grape-tutorial/d1_samlsonfigure.png) 

7. Para configurar el inicio de sesión único en **Gra-Pe**, es preciso enviar el **certificado (Base64)** descargado y los valores de **URL de inicio de sesión, identificador de Azure AD y URL de cierre de sesión** copiados al [equipo de soporte técnico de Gra-Pe](https://www.toppantravel.com/inquiry/). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![imagen](./media/grape-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/grape-tutorial/d_adduser.png)

3. En las propiedades Usuario, siga estos pasos.

    ![imagen](./media/grape-tutorial/d_userproperties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-a-gra-pe-test-user"></a>Creación de un usuario de prueba de Gra-Pe

En esta sección, creará un usuario llamado Britta Simon en Gra-Pe. Trabaje con el [equipo de soporte técnico de Gra-Pe](https://www.toppantravel.com/inquiry/) para agregar los usuarios a la plataforma de Gra-Pe. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Gra-Pe.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![imagen](./media/grape-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **Gra-Pe**.

    ![imagen](./media/grape-tutorial/tutorial_grape_app.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/grape-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![imagen](./media/grape-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Gra-Pe en el panel de acceso, iniciará sesión automáticamente en la aplicación Gra-Pe.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


