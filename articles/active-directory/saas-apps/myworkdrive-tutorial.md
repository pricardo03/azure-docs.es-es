---
title: 'Tutorial: Integración de Azure Active Directory con MyWorkDrive | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y MyWorkDrive.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4d049778-3c7b-46c0-92a4-f2633a32334b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: jeedes
ms.openlocfilehash: 7310d300c68399c31d9580f070602aa3adbc75e3
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50094063"
---
# <a name="tutorial-azure-active-directory-integration-with-myworkdrive"></a>Tutorial: Integración de Azure Active Directory con MyWorkDrive

En este tutorial, obtendrá información sobre cómo integrar MyWorkDrive con Azure Active Directory (Azure AD).

La integración de MyWorkDrive con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a MyWorkDrive.
- Puede permitir que los usuarios inicien sesión automáticamente en MyWorkDrive (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con MyWorkDrive, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en MyWorkDrive

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de MyWorkDrive desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-myworkdrive-from-the-gallery"></a>Adición de MyWorkDrive desde la galería
Para configurar la integración de MyWorkDrive en Azure AD, deberá agregar MyWorkDrive desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar MyWorkDrive desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/myworkdrive-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/myworkdrive-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/myworkdrive-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **MyWorkDrive**, seleccione **MyWorkDrive** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/myworkdrive-tutorial/tutorial_myworkdrive_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con MyWorkDrive con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de MyWorkDrive para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de MyWorkDrive.

Para configurar y probar el inicio de sesión único de Azure AD con MyWorkDrive, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de MyWorkDrive](#create-a-myworkdrive-test-user)**: para tener un homólogo de Britta Simon en MyWorkDrive que esté vinculado a su representación en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación MyWorkDrive.

**Para configurar el inicio de sesión único de Azure AD con MyWorkDrive, siga estos pasos:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones **MyWorkDrive**, seleccione **Inicio de sesión único**.

    ![imagen](./media/myworkdrive-tutorial/B1_B2_Select_SSO.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML** para habilitar el inicio de sesión único.

    ![imagen](./media/myworkdrive-tutorial/b1_b2_saml_sso.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/myworkdrive-tutorial/b1-domains_and_urlsedit.png)

4. En la sección **Configuración de SAML básica**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por **IDP**:

    ![imagen](./media/myworkdrive-tutorial/tutorial_myworkdrive_url.png)

    En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<SERVER.DOMAIN.COM>/SAML/AssertionConsumerService.aspx`.

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![imagen](./media/myworkdrive-tutorial/tutorial_myworkdrive_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SERVER.DOMAIN.COM>/Account/Login-saml`. 

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de URL de respuesta y URL de inicio de sesión.  Escriba el nombre de host de su propia compañía, por ejemplo, MyWorkDrive Server.
    > 
    > URL de respuesta: `https://yourserver.yourdomain.com/SAML/AssertionConsumerService.aspx`
    > 
    > URL de inicio de sesión: `https://yourserver.yourdomain.com/Account/Login-saml`
    > 
    > Póngase en contacto con el equipo de soporte técnico de MyWorkDrive si no está seguro de cómo configurar su propio nombre de host y el certificado SSL para estos valores.

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el **icono** de copia para copiar la **Dirección URL de metadatos de federación de aplicación** y haga clic en **Descargar** para descargar el **certificado (Base64)** y guardarlo en el equipo.

    ![imagen](./media/myworkdrive-tutorial/tutorial_myworkdrive_certficate.png) 

7. En la sección **Configurar MyWorkDrive**, copie la dirección URL adecuada según sus necesidades.

    Tenga en cuenta que en la dirección URL puede poner lo siguiente:

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![imagen](./media/myworkdrive-tutorial/d1_samlsonfigure.png) 

8. Para configurar el inicio de sesión único en MyWorkDrive, descargue el **certificado (Base64), dirección URL de cierre de sesión, identificador de identidad SAML y dirección URL de servicio de inicio de sesión único de SAML**, y configúrelos manualmente en el servidor de MyWorkDrive, o copie y pegue la **dirección URL de metadatos de federación de aplicación** de Azure en la pantalla de configuración de Azure AD de SAML del Panel de administrador de MyWorkDrive Server. Para información adicional, póngase en contacto con el [equipo de soporte técnico de MyWorkDrive](mailto:support@myworkdrive.com).

    
### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![imagen](./media/myworkdrive-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/myworkdrive-tutorial/d_adduser.png)

3. En las propiedades Usuario, siga estos pasos.

    ![imagen](./media/myworkdrive-tutorial/d_userproperties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-a-myworkdrive-test-user"></a>Creación de un usuario de prueba de MyWorkDrive

En esta sección, creará un usuario llamado Britta Simon en MyWorkDrive. Trabaje con el  [equipo de soporte técnico de MyWorkDrive](mailto:support@myworkdrive.com) para agregar los usuarios a la plataforma de MyWorkDrive. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a MyWorkDrive in the Cloud.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![imagen](./media/myworkdrive-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **MyWorkDrive**.

    ![imagen](./media/myworkdrive-tutorial/tutorial_myworkdrive_app.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/myworkdrive-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![imagen](./media/myworkdrive-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de MyWorkDrive en el panel de acceso, debería iniciar sesión automáticamente en la aplicación MyWorkDrive.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
