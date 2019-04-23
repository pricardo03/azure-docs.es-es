---
title: 'Tutorial: integración de Azure Active Directory con iPass SmartConnect | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e iPass SmartConnect.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: dee6d039-f9bb-49a2-a408-5ed40ef17d9f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: bf8c7e76aa6dd69cc505be237e47d8e10e7e8db5
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59264869"
---
# <a name="tutorial-azure-active-directory-integration-with-ipass-smartconnect"></a>Tutorial: integración de Azure Active Directory con iPass SmartConnect

En este tutorial, obtendrá información sobre cómo integrar iPass SmartConnect con Azure Active Directory (Azure AD).
La integración de iPass SmartConnect con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a iPass SmartConnect.
* Puede permitir que los usuarios inicien sesión automáticamente en iPass SmartConnect (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con iPass SmartConnect, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción que permita el inicio de sesión único en iPass SmartConnect

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* iPass SmartConnect admite SSO iniciado por **SP e IDP**
* iPass SmartConnect admite el aprovisionamiento de usuarios **Just-In-Time**

## <a name="adding-ipass-smartconnect-from-the-gallery"></a>Agregar iPass SmartConnect desde la galería

Para configurar la integración de iPass SmartConnect en Azure AD, será preciso que agregue iPass SmartConnect desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar iPass SmartConnect desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **iPass SmartConnect**, seleccione **iPass SmartConnect** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![iPass SmartConnect en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con iPass SmartConnect con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de iPass SmartConnect.

Para configurar y probar el inicio de sesión único de Azure AD con iPass SmartConnect, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de iPass SmartConnect](#configure-ipass-smartconnect-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de iPass SmartConnect](#create-ipass-smartconnect-test-user)**: para tener un homólogo de Britta Simon en iPass SmartConnect que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con iPass SmartConnect, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **iPass SmartConnect**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, el usuario no tiene que realizar ningún paso porque la aplicación ya se ha integrado previamente con Azure.

    ![Información de dominio y direcciones URL de inicio de sesión único de iPass SmartConnect](common/preintegrated.png)

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de iPass SmartConnect](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL: `https://om-activation.ipass.com/ClientActivation/ssolanding.go`

6. La aplicación iPass SmartConnect espera las aserciones de SAML en un formato concreto. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

7. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, edite las notificaciones mediante el **icono Editar** o agregue notificaciones mediante **Agregar nueva notificación** para configurar el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:

    | NOMBRE |  Atributo de origen|
    | ---------------| ----------|
    | firstName | user.givenname |
    | lastName | user.surname |
    | email | user.userprincipalname |
    | nombre de usuario | user.userprincipalname |
    | | |

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

8. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

9. En la sección **Set up iPass SmartConnect,** (Configurar iPass SmartConnect), copie las direcciones URL adecuadas en función de sus requisitos.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-ipass-smartconnect-single-sign-on"></a>Configuración del inicio de sesión único en iPass SmartConnect

Para configurar el inicio de sesión único en **iPass SmartConnect**, es preciso enviar el **XML de metadatos de federación** descargado y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de iPass SmartConnect](mailto:help@ipass.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a iPass SmartConnect para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** e **iPass SmartConnect**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **iPass SmartConnect**.

    ![El vínculo de iPass SmartConnect en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-ipass-smartconnect-test-user"></a>Creación de un usuario de prueba en iPass SmartConnect

En esta sección, creará la usuaria llamada Britta Simon en iPass SmartConnect. Colabore con el  [equipo de soporte técnico de iPass SmartConnect](mailto:help@ipass.com)  para agregar los usuarios o el dominio que deben incluirse en la lista blanca en la plataforma de iPass SmartConnect. Si el dominio lo agrega el equipo, los usuarios se aprovisionarán automáticamente en la plataforma de iPass SmartConnect. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

**Para probar la aplicación en el flujo iniciado por SP, realice los pasos siguientes:**

 a. Descargue el cliente iPass SmartConnect para Windows [aquí](https://om-activation.ipass.com/ClientActivation/ssolanding.go).

![El vínculo de iPass SmartConnect en la lista de aplicaciones](./media/ipasssmartconnect-tutorial/testing3.png)

b. Instale el cliente e inícielo.

c. Para continuar, haga clic en **Introducción**.

![El vínculo de iPass SmartConnect en la lista de aplicaciones](./media/ipasssmartconnect-tutorial/testing1.png) 

d. Escriba el nombre de usuario de Azure con el dominio. Haga clic en **Continuar**. Se le redirigirá a la página de inicio de sesión de Azure.

![El vínculo de iPass SmartConnect en la lista de aplicaciones](./media/ipasssmartconnect-tutorial/testing2.png) 

e. Tras la autenticación correcta, se iniciará la activación del cliente. Se activará el cliente.

**Para probar la aplicación en el flujo iniciado por IdP, realice los pasos siguientes:**

 a. Inicie sesión en [https://myapps.microsoft.com](https://myapps.microsoft.com).

b. Haga clic en la aplicación iPass SmartConnect.

c. Inicia la página de SSA, haga clic en **Download App for Windows** (Descargar la aplicación para Windows) para instalar el cliente de iPass SmartConnect.

![El vínculo de iPass SmartConnect en la lista de aplicaciones](./media/ipasssmartconnect-tutorial/testing4.png)

d. Después de la instalación, el cliente en el primer inicio iniciará la activación automáticamente después de aceptar los términos y condiciones.

e. Si no se inicia la activación, haga clic en el botón Activar en la página de SSA para iniciar la activación.

f. Se activará el cliente.

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)