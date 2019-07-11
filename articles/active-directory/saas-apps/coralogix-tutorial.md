---
title: 'Tutorial: Integración de Azure Active Directory con Coralogix | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Coralogix.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ba79bfc1-992e-4924-b76a-8eb0dfb97724
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/2/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 721e0c40ec2e02dabee0681e01fea4182b906183
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67104643"
---
# <a name="tutorial-azure-active-directory-integration-with-coralogix"></a>Tutorial: Integración de Azure Active Directory con Coralogix

En este tutorial, aprenderá a integrar Coralogix con Azure Active Directory (Azure AD).
La integración de Coralogix con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Coralogix.
* Puede permitir que los usuarios inicien sesión automáticamente en Coralogix (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Coralogix, necesita los siguientes elementos:

- Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una versión de prueba de un mes aquí](https://azure.microsoft.com/pricing/free-trial/).
- Una suscripción habilitada para el inicio de sesión único en Coralogix 

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Coralogix admite el inicio de sesión único iniciado por SP.

## <a name="add-coralogix-from-the-gallery"></a>Adición de Coralogix desde la galería

Para configurar la integración de Coralogix en Azure AD, primero debe agregar Coralogix desde la galería a la lista de aplicaciones de SaaS administradas.

Para agregar Coralogix desde la galería, siga los pasos que se indican a continuación:

1. En el panel izquierdo de [Azure Portal](https://portal.azure.com), seleccione el icono **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, seleccione el botón **Nueva aplicación** en la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Coralogix**. En el panel de resultados, seleccione **Coralogix** y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Coralogix en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Coralogix con un usuario de prueba llamado Britta Simon.
Para que el inicio de sesión único funcione, es preciso establecer un vínculo entre un usuario de Azure AD y el usuario relacionado de Coralogix.

Para configurar y probar el inicio de sesión único de Azure AD con Coralogix, primero debe completar los siguientes bloques de creación:

1. [Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on), para permitir que los usuarios utilicen esta característica.
2. [Configuración del inicio de sesión único de Coralogix](#configure-coralogix-single-sign-on): para configurar el inicio de sesión único en la aplicación.
3. [Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user), para probar el inicio de sesión único de Azure AD con Britta Simon.
4. [Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user), para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. [Creación de un usuario de prueba de Coralogix](#create-a-coralogix-test-user): para tener un homólogo de Britta Simon en Coralogix que esté vinculado a la representación del usuario en Azure AD.
6. [Prueba del inicio de sesión único](#test-single-sign-on) para comprobar que la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Coralogix, siga los pasos que se indican a continuación:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Coralogix**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione **SAML** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Set-up Single Sign-On with SAML** (Configurar inicio de sesión único con SAML), seleccione el icono **Edit** (Editar) para abrir el cuadro de diálogo **Basic SAML Configuration** (Configuración básica de SAML).

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En el cuadro de diálogo **Configuración básica de SAML**, siga los pasos que se indican a continuación:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Coralogix](common/sp-identifier.png)

    a. En el cuadro **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.coralogix.com`.

    b. En el cuadro de texto **Identificador (Id. de entidad)** , escriba una dirección URL, como:
    
    `https://api.coralogix.com/saml/metadata.xml`

    o

    `https://aws-client-prod.coralogix.com/saml/metadata.xml` 

    > [!NOTE]
    > El valor de la dirección URL de inicio de sesión no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de Coralogix](mailto:info@coralogix.com) para obtener este valor. También puede hacer referencia a los patrones de la sección **Configuración básica de SAML** de Azure Portal.

5. La aplicación Coralogix espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, seleccione el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, edite las notificaciones con el icono **Editar**. Tal como se muestra en la imagen anterior, también puede agregar las notificaciones con **Agregar nueva notificación** para configurar el atributo de token SAML. A continuación, siga estos pasos:
    
    a. Seleccione el **icono Editar** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](./media/coralogix-tutorial/tutorial_usermail.png) ![imagen](./media/coralogix-tutorial/tutorial_usermailedit.png)

    b. En la lista **Elija el formato del identificador de nombre**, seleccione **Dirección de correo electrónico**.

    c. En la lista **Source attribute** (Atributo de origen), seleccione **user.mail**.

    d. Seleccione **Guardar**.

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, seleccione **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos. A continuación, guárdelo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

8. En la sección **Set up Coralogix** (Configurar Coralogix), copie las direcciones URL adecuadas.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-coralogix-single-sign-on"></a>Configuración del inicio de sesión único de Coralogix

Para configurar el inicio de sesión único en **Coralogix**, envíe el archivo **XML de metadatos de federación** descargado y las direcciones URL copiadas de Azure Portal al [equipo de soporte técnico de Coralogix](mailto:info@coralogix.com). Ellos se aseguran de que la conexión de inicio de sesión único de SAML está establecida correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. En la parte superior de la pantalla, seleccione **Nuevo usuario**.

    ![Botón Nuevo usuario](common/new-user.png)

3. En el cuadro de diálogo **Usuario**, siga los pasos que se indican a continuación.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba "brittasimon@yourcompanydomain.extension". Por ejemplo, en este caso, podría escribir "brittasimon@contoso.com".

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Seleccione **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Coralogix para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, después, **Coralogix**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Coralogix**.

    ![Vínculo a Coralogix en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Seleccione el botón **Agregar usuario**. Después, seleccione **Usuarios y grupos**, en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon**. A continuación, haga clic en el botón **Seleccionar** situado en la parte inferior de la pantalla.

6. Si espera algún valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione el rol adecuado para el usuario en la lista. A continuación, haga clic en el botón **Seleccionar** situado en la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-a-coralogix-test-user"></a>Creación de un usuario de prueba de Coralogix

En esta sección, creará un usuario llamado Britta Simon en Coralogix. Colabore con el  [equipo de soporte técnico de Coralogix](mailto:info@coralogix.com) para agregar los usuarios en la plataforma de Coralogix. Los usuarios deben crearse y activarse antes de usar el inicio de sesión único.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el portal MyApps.

Al seleccionar el icono de Coralogix en el portal MyApps, debería iniciarse automáticamente la sesión en Coralogix. Para obtener más información sobre el portal MyApps, consulte [¿Qué es el portal MyApps?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

