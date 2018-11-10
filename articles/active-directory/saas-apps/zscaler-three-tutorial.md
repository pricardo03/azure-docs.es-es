---
title: 'Tutorial: Integración de Azure Active Directory con Zscaler Three | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Zscaler Three.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: jeedes
ms.openlocfilehash: b148967af0882993d8ab113bdf0fd3ad3835296f
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50092617"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-three"></a>Tutorial: Integración de Azure Active Directory con Zscaler Three

En este tutorial, aprenderá a integrar Zscaler Three con Azure Active Directory (Azure AD).

La integración de Zscaler Three con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Zscaler Three
- Puede permitir que los usuarios inicien sesión automáticamente en Zscaler Three (inicio de sesión único) con sus cuentas de Azure AD
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Zscaler Three, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Zscaler Three

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, aquí puede obtener una versión de evaluación de un mes: [Oferta de prueba](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.
El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Zscaler Three desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-zscaler-three-from-the-gallery"></a>Incorporación de Zscaler Three desde la galería

Para configurar la integración de Zscaler Three en Azure AD, deberá agregar Zscaler Three desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Zscaler Three desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Active Directory][1]

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![APLICACIONES][2]

3. Haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

4. En el panel de resultados, seleccione **Zscaler Three** y haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/zscaler-three-tutorial/tutorial_zscalerthree_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Zscaler Three con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Zscaler Three para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario asociado de Zscaler Three.

Para configurar y probar el inicio de sesión único de Azure AD con Zscaler Three, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Configuración de las opciones del proxy](#configuring-proxy-settings)**: para definir la configuración del proxy en Internet Explorer
3. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Creación de un usuario de prueba de Zscaler Three](#creating-a-zscaler-three-test-user)**: para tener un homólogo de Britta Simon en Zscaler Three vinculado a la representación del usuario en Azure AD.
5. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
6. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Zscaler Three.

**Para configurar el inicio de sesión único de Azure AD con Zscaler Three, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de aplicaciones **Zscaler Three**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](./media/zscaler-three-tutorial/tutorial_general_301.png)

3. Si necesita cambiar al modo **SAML** desde cualquier otro, haga clic en la opción **Cambiar el modo de inicio de sesión único** de la parte superior de la pantalla.

    ![Configurar inicio de sesión único](./media/zscaler-three-tutorial/tutorial_general_300.png)

4. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](./media/zscaler-three-tutorial/tutorial_general_302.png)

5. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Configurar inicio de sesión único](./media/zscaler-three-tutorial/tutorial_zscalerthree_url.png)

    En el cuadro de texto URL de inicio de sesión, escriba la dirección URL `https://login.zscalerthree.net/sfc_sso`

6. En la sección **Certificado de firma SAML**, haga clic **Descargar** para descargar el **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/zscaler-three-tutorial/tutorial_zscalerthree_certificate.png)

8. En la sección **Configurar Zscaler Three**, copie la **dirección URL de inicio de sesión**.

    ![Configurar inicio de sesión único](./media/zscaler-three-tutorial/tutorial_zscalerthree_configure.png)

9. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de Zscaler Three como administrador.

10. En el menú de la parte superior, haga clic en **Administration**(Administración).

    ![Administración](./media/zscaler-three-tutorial/ic800206.png "Administración")

9. En **Manage Administrators & Roles (Administrar administradores y roles)** haga clic en **Manage Users & Authentication (Administrar usuarios y autenticación)**.

    ![Administración de usuarios y autenticación](./media/zscaler-three-tutorial/ic800207.png "Administración de usuarios y autenticación")

10. En la sección **Choose Authentication Options for your Organization** (Elegir opciones de autenticación para su organización), lleve a cabo los pasos siguientes:

    ![Autenticación](./media/zscaler-three-tutorial/ic800208.png "Autenticación")

    a. Seleccione **Authenticate using SAML Single Sign-On**(Autenticarse mediante el inicio de sesión único SAML).

    b. Haga clic en **Configure SAML Single Sign-On Parameters**(Configurar parámetros de inicio de sesión único SAML).

11. En la página de diálogo **Configure SAML Single Sign-On Parameters** (Configurar parámetros de inicio de sesión único SAML), realice estos pasos y luego haga clic en **Done** (Listo)

    ![Inicio de sesión único](./media/zscaler-three-tutorial/ic800209.png "Inicio de sesión único")

    a. Pegue el valor de **Login URL** (Dirección URL de inicio de sesión), que ha copiado de Azure Portal, en el cuadro de texto **de la dirección URL del portal de SAML al que se dirige a los usuarios para la autenticación**.

    b. En el cuadro de texto **Attribute containing Login Name** (Atributo que contiene el nombre de inicio de sesión), escriba **NameID**.

    c. Para cargar el certificado descargado, haga clic en **pem de Zscaler**.

    d. Seleccione **Habilitar aprovisionamiento automático de SAML**.

12. En la página del cuadro de diálogo **Configurar autenticación de usuario** , realice los pasos siguientes:

    ![Administración](./media/zscaler-three-tutorial/ic800210.png "Administración")

    a. Haga clic en **Save**(Guardar).

    b. Haga clic en **Activar ahora**.

## <a name="configuring-proxy-settings"></a>Configuración de los valores de proxy

### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Para definir la configuración de proxy en Internet Explorer

1. Inicie **Internet Explorer**.

2. Seleccione **Opciones de Internet** en el menú **Herramientas** para abrir el diálogo **Opciones de Internet**.

     ![Opciones de Internet](./media/zscaler-three-tutorial/ic769492.png "Opciones de Internet")

3. Haga clic en la pestaña **Conexiones** .
  
     ![Conexiones](./media/zscaler-three-tutorial/ic769493.png "Conexiones")

4. Haga clic en **Configuración de LAN** para abrir el diálogo **Configuración de LAN**.

5. En la sección del servidor proxy, lleve a cabo estos pasos:

    ![Servidor proxy](./media/zscaler-three-tutorial/ic769494.png "Servidor proxy")

    a. Seleccione **Usar un servidor proxy para la LAN**.

    b. En el cuadro de texto Dirección, escriba **gateway.zscalerthree.net**.

    c. En el cuadro de texto Puerto, escriba **80**.

    d. Seleccione **No usar servidor proxy para direcciones locales**.

    e. Haga clic en **Aceptar** para cerrar el diálogo **Configuración de red de área local (LAN)**.

6. Haga clic en **Aceptar** para cerrar el diálogo **Opciones de Internet**.

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](./media/zscaler-three-tutorial/create_aaduser_01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](./media/zscaler-three-tutorial/create_aaduser_02.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="creating-a-zscaler-three-test-user"></a>Creación de un usuario de prueba de Zscaler Three

Para permitir que los usuarios de Azure AD inicien sesión en Zscaler Three, deben aprovisionarse a Zscaler Three. En el caso de Zscaler Three, el aprovisionamiento es una tarea manual.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Siga estos pasos para configurar el aprovisionamiento de usuario:

1. Inicie sesión en su inquilino de **Zscaler Three**.

2. Haga clic en **Administración**.

    ![Administración](./media/zscaler-three-tutorial/ic781035.png "Administración")

3. Haga clic en **User Management**(Administración de usuarios).

     ![Agregar](./media/zscaler-three-tutorial/ic781036.png "Agregar")

4. En la pestaña **Usuarios**, haga clic en **Agregar**.

    ![Agregar](./media/zscaler-three-tutorial/ic781037.png "Agregar")

5. En la sección Agregar usuario, lleve a cabo estos pasos:

    ![Agregar usuario](./media/zscaler-three-tutorial/ic781038.png "Agregar usuario")

    a. Escriba el **identificador de usuario**, el **nombre para mostrar del usuario**, la **contraseña**, el valor de **Confirmar contraseña** y, después, seleccione **Grupos** y el valor de **Departamento** de una cuenta de Azure AD válida que quiera aprovisionar.

    b. Haga clic en **Save**(Guardar).

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Zscaler Three ofrecida por Zscaler Three para aprovisionar cuentas de usuario de Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Zscaler Three.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Zscaler Three**.

    ![Configurar inicio de sesión único](./media/zscaler-three-tutorial/tutorial_zscalerthree_app.png)

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Zscaler Three del panel de acceso, debería iniciar sesión en la aplicación Zscaler Three automáticamente.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/zscaler-three-tutorial/tutorial_general_01.png
[2]: ./media/zscaler-three-tutorial/tutorial_general_02.png
[3]: ./media/zscaler-three-tutorial/tutorial_general_03.png
[4]: ./media/zscaler-three-tutorial/tutorial_general_04.png

[100]: ./media/zscaler-three-tutorial/tutorial_general_100.png

[200]: ./media/zscaler-three-tutorial/tutorial_general_200.png
[201]: ./media/zscaler-three-tutorial/tutorial_general_201.png
[202]: ./media/zscaler-three-tutorial/tutorial_general_202.png
[203]: ./media/zscaler-three-tutorial/tutorial_general_203.png