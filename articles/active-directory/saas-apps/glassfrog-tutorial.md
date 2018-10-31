---
title: 'Tutorial: Integración de Azure Active Directory con GlassFrog | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y GlassFrog.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7cf5dae6-32d6-418e-8ef2-b2041e686086
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2018
ms.author: jeedes
ms.openlocfilehash: 1fdd1e8acf446ba3e409e2ff0bf9ca18f581c299
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49433186"
---
# <a name="tutorial-azure-active-directory-integration-with-glassfrog"></a>Tutorial: integración de Azure Active Directory con GlassFrog

En este tutorial, obtendrá información sobre cómo integrar GlassFrog con Azure Active Directory (Azure AD).

La integración de GlassFrog con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a GlassFrog.
- Puede permitir que los usuarios inicien sesión automáticamente en GlassFrog (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con GlassFrog, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en GlassFrog.

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de GlassFrog desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-glassfrog-from-the-gallery"></a>Incorporación de GlassFrog desde la galería
Para configurar la integración de GlassFrog en Azure AD, será preciso que agregue GlassFrog desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar GlassFrog desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/glassfrog-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/glassfrog-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/glassfrog-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **GitHub**, seleccione **GlassFrog** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/glassfrog-tutorial/tutorial_glassfrog_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con GlassFrog con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de GlassFrog para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de GlassFrog.

Para configurar y probar el SSO de Azure AD con GlassFrog, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de GlassFrog](#create-a-glassfrog-test-user)**: para tener un homólogo de Britta Simon en GlassFrog que esté vinculado a su representación en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación GlassFrog.

**Para configurar el inicio de sesión único de Azure AD con GlassFrog, realice los pasos siguientes:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **GlassFrog**, seleccione **Inicio de sesión único**.

    ![imagen](./media/glassfrog-tutorial/b1_b2_select_sso.png)

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![imagen](./media/glassfrog-tutorial/b1_b2_saml_sso.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/glassfrog-tutorial/b1-domains_and_urlsedit.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://app.glassfrog.com/people/sso?org_id=<ORGANIZATIONID>`

    ![imagen](./media/glassfrog-tutorial/tutorial_glassfrog_url.png)

    > [!NOTE] 
    > El valor de la dirección URL de inicio de sesión no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de GlassFrog](https://support.glassfrog.com/support/solutions/9000107654) para obtener el valor.
 
5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el archivo **XML de metadatos de federación**  y guárdelo en su equipo.

    ![imagen](./media/glassfrog-tutorial/tutorial_glassfrog_certificate.png) 

6. Para configurar el inicio de sesión único en **GlassFrog**, debe enviar el archivo **XML de metadatos de federación** descargado al [equipo de soporte técnico de GlassFrog](mailto:support@alchemy.fr). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![imagen](./media/glassfrog-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/glassfrog-tutorial/d_adduser.png)

3. En las propiedades Usuario, siga estos pasos.

    ![imagen](./media/glassfrog-tutorial/d_userproperties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-a-glassfrog-test-user"></a>Creación de un usuario de prueba de GlassFrog

En esta sección, creará un usuario llamado Britta Simon en GlassFrog. Trabaje con el [equipo de soporte técnico de GlassFrog](https://support.glassfrog.com/support/solutions/9000107654) para agregar los usuarios a la plataforma de GlassFrog. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a GlassFrog.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![imagen](./media/glassfrog-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **GlassFrog**.

    ![imagen](./media/glassfrog-tutorial/tutorial_glassfrog_app.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/glassfrog-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![imagen](./media/glassfrog-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de GlassFrog en el panel de acceso, debería iniciar sesión automáticamente en la aplicación GlassFrog.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


