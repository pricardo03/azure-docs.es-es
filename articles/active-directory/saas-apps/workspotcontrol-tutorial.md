---
title: 'Tutorial: integración de Azure Active Directory con Workspot Control | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Workspot Control.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3ea8e4e9-f61f-4f45-b635-b0e306eda3d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97f375c6f48d3dc497eb59e76f19fc64cf906b56
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57886514"
---
# <a name="tutorial-azure-active-directory-integration-with-workspot-control"></a>Tutorial: integración de Azure Active Directory con Workspot Control

En este tutorial, aprenderá a integrar Workspot Control con Azure Active Directory (Azure AD).

La integración de Workspot Control con Azure AD le proporciona las siguientes ventajas:

- En Azure AD puede controlar quién tiene acceso a Workspot Control.
- Puede permitir que los usuarios inicien sesión automáticamente en Workspot Control (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Workspot Control, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Workspot Control

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Workspot Control desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-workspot-control-from-the-gallery"></a>Incorporación de Workspot Control desde la galería
Para configurar la integración de Workspot Control en Azure AD, será preciso que agregue Workspot Control desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Workspot Control desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/workspotcontrol-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/workspotcontrol-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/workspotcontrol-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **Workspot Control**, seleccione **Workspot Control** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Workspot Control con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD necesita saber cuál es el usuario homólogo de Workspot Control para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Workspot Control.

Para configurar y probar el inicio de sesión único de Azure AD con Workspot Control, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Workspot Control](#create-a-workspot-control-test-user)**: para tener un homólogo de Britta Simon en Workspot Control que esté vinculado a su representación en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Workspot Control.

**Para configurar el inicio de sesión único de Azure AD con Workspot Control, realice los pasos siguientes:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Workspot Control**, seleccione **Inicio de sesión único**.

    ![imagen](./media/workspotcontrol-tutorial/B1_B2_Select_SSO.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML** para habilitar el inicio de sesión único.

    ![imagen](./media/workspotcontrol-tutorial/b1_b2_saml_sso.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/workspotcontrol-tutorial/b1-domains_and_urlsedit.png)

4. En la sección **Configuración de SAML básica**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por **IDP**:

    ![imagen](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_url.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<INSTANCENAME>-saml.workspot.com/saml/metadata`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<INSTANCENAME>-saml.workspot.com/saml/assertion`

    c. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

     ![imagen](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<INSTANCENAME>-saml.workspot.com/`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico al cliente de Workfront Control](mailto:support@workspot.com) para obtener estos valores. 

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** y guárdelo en su equipo.

    ![imagen](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_certficate.png) 

6. En la sección **Configurar Workspot Control**, copie la dirección URL adecuada según sus necesidades.

    Tenga en cuenta que en la dirección URL puede poner lo siguiente:

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![imagen](./media/workspotcontrol-tutorial/d1_samlsonfigure.png) 

7. En otra ventana del explorador web, inicie sesión en Workspot Control como administrador de seguridad.

8. En la barra de herramientas en la parte superior de la página, haga clic en  **Setup** (Configuración) y vaya a  **SAML**.

    ![imagen](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_setup.png)

9. En la página **Security Assertion Markup Language Configuration** (Configuración de Lenguaje de marcado de aserción de seguridad), realice los pasos siguientes:
 
    ![imagen](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_saml.png)

     a. En el cuadro de texto **Identificador de entidad**, pegue el valor de **Identificador de Azure AD** que copió de Azure Portal.   

    En el cuadro de texto **Dirección URL de inicio de sesión**, pegue el valor de la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    c. En el cuadro de texto **Dirección URL de cierre de sesión**, pegue el valor de la **dirección URL de cierre de sesión** que ha copiado de Azure Portal. 

    d. Haga clic en **Actualizar archivo** para cargar el certificado codificado en base 64 que descargó de Azure Portal en el certificado X.509.

    e. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![imagen](./media/workspotcontrol-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/workspotcontrol-tutorial/d_adduser.png)

3. En las propiedades Usuario, siga estos pasos.

    ![imagen](./media/workspotcontrol-tutorial/d_userproperties.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el **nombre de usuario** tipo de campo **brittasimon\@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-a-workspot-control-test-user"></a>Creación de un usuario de prueba de Workspot Control

Para permitir que los usuarios de Azure AD inicien sesión en Workspot Control, tienen que aprovisionarse en Workspot Control. En Workspot Control, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en Workspot Control como administrador de seguridad.

2. En la barra de herramientas de la parte superior de la página, haga clic en  **Users** (Usuarios) y vaya a  **Add user** (Agregar usuario).

    ![imagen](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_adduser.png)

3. En la página **Agregar un nuevo usuario**, realice los pasos siguientes:

    ![imagen](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_addnewuser.png)

     a. En el cuadro de texto **First Name** (Nombre), escriba el nombre de usuario, en este caso **Britta**.

    b. En el cuadro de texto **Last Name** (Apellidos), escriba los apellidos del usuario, en este caso **Simon**.

    c. En **correo electrónico** texto, escriba el correo electrónico del usuario, como **Brittasimon\@contoso.com**.

    d. Seleccione el rol de usuario apropiado de la lista desplegable **Rol**.

    e. Seleccione el grupo de roles de usuario apropiado de la lista desplegable **Grupo**.

    f. Haga clic en **Agregar usuario**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Workspot Control para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![imagen](./media/workspotcontrol-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **Workspot Control**.

    ![imagen](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_app.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/workspotcontrol-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![imagen](./media/workspotcontrol-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Workspot Control en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación Workspot Control.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
