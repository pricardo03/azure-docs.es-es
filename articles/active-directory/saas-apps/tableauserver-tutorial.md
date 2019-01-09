---
title: 'Tutorial: Integración de Azure Active Directory con Tableau Server | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Tableau Server.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2018
ms.author: jeedes
ms.openlocfilehash: c727cddf41c269c214b541134cd9f688017ee687
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2018
ms.locfileid: "53789728"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Tutorial: Integración de Azure Active Directory con Tableau Server

En este tutorial, aprenderá a integrar Tableau Server con Azure Active Directory (Azure AD).

Integrar Tableau Server con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Tableau Server.
- Puede permitir que los usuarios inicien sesión automáticamente en Tableau Server (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Tableau Server, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Tableau Server

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Tableau Server desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-tableau-server-from-the-gallery"></a>Incorporación de Tableau Server desde la galería

Para configurar la integración de Tableau Server en Azure AD, será preciso que agregue Tableau Server desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Tableau Server desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Tableau Server**, seleccione **Tableau Server** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

    ![Tableau Server en la lista de resultados](./media/tableauserver-tutorial/tutorial-tableauserver-addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Tableau Server con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Tableau Server para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Tableau Server.

Para configurar y probar el inicio de sesión único de Azure AD con Tableau Server, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Tableau Server](#configure-tableau-server-single-sign-on)**: para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Creación de un usuario de prueba de Tableau Server](#create-tableau-server-test-user)**: para tener un homólogo de Britta Simon en Cisco Umbrella que esté vinculado a su representación en Azure AD.
5. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Tableau Server.

**Para configurar el inicio de sesión único de Azure AD con Tableau Server, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Tableau Server**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial-general-301.png)

3. La aplicación Tableau Server espera recibir una notificación personalizada **username** que se debe definir como sigue. Se usa como identificador de usuario en lugar de como notificación de identificador de usuario único. Puede administrar los valores de estos atributos en la sección **User Attributes & Claims** (Atributos y notificaciones del usuario) de la página de integración de aplicaciones. Haga clic en el botón **Edit** (Editar) para abrir el cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario).

    ![imagen](./media/tableauserver-tutorial/tutorial-tableauserver-attribute.png)

4. En la sección **Notificaciones del usuario** del cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario), configure el atributo token SAML como se muestra en la imagen anterior y realice los siguientes pasos:
    
    | Nombre del atributo | Valor de atributo | Espacio de nombres |
    | ---------------| --------------- | ----------- |   
    | nombre de usuario | user.userprincipalname | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](./media/tableauserver-tutorial/tutorial-tableauserver-add-attribute.png)

    ![imagen](./media/tableauserver-tutorial/tutorial-tableauserver-manage-attribute.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Escriba el valor de **Espacio de nombres**.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Save**(Guardar).

5. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

6. En la sección **Configuración básica de SAML**, siga estos pasos:

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://azure.<domain name>.link`.
    
    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://azure.<domain name>.link`

    c. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://azure.<domain name>.link/wg/saml/SSO/index.html`.

    ![imagen](./media/tableauserver-tutorial/tutorial-tableauserver-url.png)
     
    > [!NOTE] 
    > Los valores anteriores no son valores reales. Actualice los valores con la dirección URL y el identificador releas de la página de configuración de Tableau Server que se explica más adelante en el tutorial.

7. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic **Descargar** para descargar el **archivo XML de metadatos de federación** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/tableauserver-tutorial/tutorial-tableauserver-certificate.png)

### <a name="configure-tableau-server-single-sign-on"></a>Configuración del inicio de sesión único de Tableau Server 

1. Para configurar SSO para la aplicación, debe iniciar sesión en su inquilino de Tableau Server como administrador.

2. En la pestaña **CONFIGURACIÓN**, seleccione **User Identity & Access** (Identidad de usuario y acceso) y, a continuación, seleccione la pestaña del método de **Autenticación**.

    ![Configurar inicio de sesión único](./media/tableauserver-tutorial/tutorial-tableauserver-auth.png)

3. En la página **CONFIGURACIÓN**, realice los siguientes pasos:

    ![Configurar inicio de sesión único](./media/tableauserver-tutorial/tutorial-tableauserver-config.png)

     a. Como **Authentication Method** (Método de autenticación), seleccione SAML.
    
    b. Seleccione la casilla **Enable SAML Authentication for the server**(Habilitar autenticación SAML para el servidor).

    c. Dirección URL de retorno de Tableau Server: dirección URL a la que accederán los usuarios de Tableau Server; por ejemplo, http://tableau_server. No se recomienda usar http://localhost. No se permiten las direcciones URL que tienen una barra diagonal al final; por ejemplo, http://tableau_server/). Copie el valor de **Tableau Server return URL** (URL de retorno de Tableau Server) y péguelo en el cuadro de texto **URL de inicio de sesión** de la sección **Tableau Server Domain and URLs** (Dominio y direcciones URL de Tableau Server) de Azure AD.

    d. SAML entity ID (Id. de entidad SAML): el identificador de entidad identifica de forma exclusiva la instalación de Tableau Server en el IdP. Aquí, si quiere, puede especificar de nuevo la dirección URL de Tableau Server, pero no tiene que ser esa misma URL. Copie el valor de **SAML entity ID** (Id. de entidad SAML) y péguelo en el cuadro de texto **Identificador** de la sección **Tableau Server Domain and URLs** (Dominio y direcciones URL de Tableau Server) de Azure AD.

    e. Haga clic en **Download XML Metadata File** (Descargar archivo de metadatos XML) y ábralo en la aplicación del editor de texto. Busque la URL del Servicio de consumidor de aserciones con Http Post e índice 0 y copie la URL. Ahora, péguelo en el cuadro de texto **URL de respuesta** de la sección **Tableau Server Domain and URLs** (Dominio y direcciones URL de Tableau Server) de Azure AD.

    f. Busque el archivo de metadatos de federación descargado desde Azure Portal y cárguelo en el **SAML Idp metadata file**(Archivo de metadatos del proveedor de identidades SAML).

    g. Escriba los nombres de los atributos que el proveedor de identidades usa para contener los nombres de usuario, los nombres para mostrar y las direcciones de correo electrónico.

    h. Haga clic en **Guardar**

    >[!NOTE] 
    >El cliente tiene que cargar los certificados en la configuración de SSO de SAML de Tableau Server para que se omitan en el flujo de SSO.
    >Si necesita ayuda para configurar SAML en Tableau Server, vea el artículo [Configuración de SAML](https://onlinehelp.tableau.com/v2018.2/server/en-us/saml_config_steps_tsm_ui.htm).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](common/create-aaduser-01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](common/create-aaduser-02.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
  
### <a name="create-tableau-server-test-user"></a>Crear usuario de prueba de Tableau Server

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en Tableau Server. Debe aprovisionar todos los usuarios en el servidor de Tableau. 

Tenga en cuenta que el nombre de usuario debe coincidir con el valor que ha configurado en el atributo personalizado de **nombre de usuario** de Azure AD. Con la asignación correcta, la integración debería funcionar. [Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on).

>[!NOTE]
>Si necesita crear un usuario manualmente, póngase en contacto con el administrador de Tableau Server de su organización.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Tableau Server.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Tableau Server**.

    ![Configurar inicio de sesión único](./media/tableauserver-tutorial/tutorial-tableauserver-app.png) 

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Tableau Server en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Tableau Server.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial-general-01.png
[2]: common/tutorial-general-02.png
[3]: common/tutorial-general-03.png
[4]: common/tutorial-general-04.png

[100]: common/tutorial-general-100.png

[201]: common/tutorial-general-201.png
[202]: common/tutorial-general-202.png
[203]: common/tutorial-general-203.png
