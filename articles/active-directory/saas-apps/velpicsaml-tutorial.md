---
title: 'Tutorial: Integración de Azure Active Directory con Velpic SAML | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Velpic SAML.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 883620d6aa25f71a454c7add6d60e38b4974facf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67087663"
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a>Tutorial: Integración de Azure Active Directory con Velpic SAML

En este tutorial, aprenderá a integrar Velpic SAML con Azure Active Directory (Azure AD).
La integración de Velpic SAML con Azure AD le proporciona las siguientes ventajas:

* En Azure AD se puede controlar quién tiene acceso a Velpic SAML.
* Puede permitir que los usuarios inicien sesión automáticamente en Velpic SAML (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Velpic SAML, necesita lo siguiente:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una suscripción habilitada para el inicio de sesión único en Velpic SAML

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Velpic SAML admite el inicio de sesión único iniciado por **SP**.

## <a name="adding-velpic-saml-from-the-gallery"></a>Agregar Velpic SAML desde la galería

Para configurar la integración de Velpic SAML en Azure AD, es preciso agregar Velpic SAML desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Velpic SAML desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Velpic SAML**, seleccione **Velpic SAML** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Velpic SAML en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Velpic SAML utilizando un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Velpic SAML.

Para configurar y probar el inicio de sesión único de Azure AD con Velpic SAML, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Velpic SAML](#configure-velpic-saml-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Velpic SAML](#create-velpic-saml-test-user)** : para tener un homólogo de Britta Simon en Velpic SAML que esté vinculado a su representación en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Velpic SAML, realice los pasos siguientes:

1. En la página de integración de la aplicación [Velpic SAML](https://portal.azure.com/) de **Azure Portal**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Velpic SAML](common/sp-identifier.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<sub-domain>.velpicsaml.net`

    b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://auth.velpic.com/saml/v2/<entity-id>/login`

    > [!NOTE]
    > Tenga en cuenta que la URL de inicio de sesión se la proporcionará el equipo de Velpic SAML y el valor del identificador estará disponible cuando configure el complemento de SSO en Velpic SAML. Tiene que copiar ese valor de la página de la aplicación Velpic SAML y pegarlo aquí.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Set up Velpic SAML** (Configurar Velpic SAML), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-velpic-saml-single-sign-on"></a>Configuración del inicio de sesión único de Velpic SAML

1. En otra ventana del explorador web, inicie sesión como administrador en su sitio de la compañía de Velpic SAML.

2. Haga clic en la pestaña **Manage** (Administrar) y vaya a la sección **Integration** (Integración), donde tiene que hacer clic en el botón **Plugins** (Complementos) para crear el complemento Sign-In (Inicio de sesión).

    ![Complemento](./media/velpicsaml-tutorial/velpic_1.png)

3. Haga clic en el botón **"Add plugin"** (Agregar complemento).
    
    ![Complemento](./media/velpicsaml-tutorial/velpic_2.png)

4. Haga clic en el icono **SAML** de la página Add Plugin (Agregar complemento).
    
    ![Complemento](./media/velpicsaml-tutorial/velpic_3.png)

5. Escriba el nombre del nuevo complemento SAML y haga clic en el botón **"Add"** (Agregar).

    ![Complemento](./media/velpicsaml-tutorial/velpic_4.png)

6. Especifique los detalles de la manera siguiente:

    ![Complemento](./media/velpicsaml-tutorial/velpic_5.png)

    a. En el cuadro de texto **Name** (Nombre), escriba el nombre del complemento SAML.

    b. En el cuadro de texto **Issuer URL** (URL del emisor), pegue el **Id. de Azure AD** que copió de la ventana **Configurar inicio de sesión** de Azure Portal.

    c. En **Provider Metadata Config** (Config. de metadatos del proveedor), cargue el archivo XML de metadatos que descargó de Azure Portal.

    d. También puede habilitar aprovisionamiento Just-In-Time de SAML activando la casilla **"Auto create new users"** (Crear nuevos usuarios automáticamente). Si no existe ningún usuario de Velpic y no se habilita esta marca, el inicio desde Azure no se producirá. Si se habilita la marca, el usuario se aprovisionará automáticamente en Velpic en el momento de inicio de sesión. 

    e. Copie la **Single sign on URL** (URL de inicio de sesión) en el cuadro de texto y péguela en Azure Portal.
    
    f. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba `brittasimon@yourcompanydomain.extension`. Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Velpic SAML.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Velpic SAML**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Velpic SAML**.

    ![Vínculo de Velpic SAML en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-velpic-saml-test-user"></a>Creación de un usuario de prueba de Velpic SAML

Este paso no suele ser necesario porque la aplicación admite aprovisionamiento de usuarios Just-In-Time. Si el aprovisionamiento automático de usuarios no se habilita, puede llevarse a cabo la creación manual de usuarios tal y como se describe a continuación.

Inicie sesión como administrador en su sitio de la compañía de Velpic SAML y realice los pasos siguientes:
    
1. Haga clic en la pestaña Manage (Administrar), vaya a la sección Users (Usuarios) y haga clic en el botón New (Nuevo) para agregar usuarios.

    ![agregar usuario](./media/velpicsaml-tutorial/velpic_7.png)

2. En el cuadro de diálogo **"Create New User"** (Crear nuevo usuario), realice los pasos siguientes.

    ![user](./media/velpicsaml-tutorial/velpic_8.png)
    
    a. En el cuadro de texto **Nombre**, escriba Britta.

    b. En el cuadro de texto **Apellido**, escriba el apellido Simon.

    c. En el cuadro de texto **User Name** (Nombre de usuario), escriba el nombre del usuario de Britta Simon.

    d. En el cuadro de texto **Correo electrónico**, escriba la dirección de correo electrónico de la cuenta Brittasimon@contoso.com.

    e. El resto de la información es opcional, puede rellenarla si es necesario.
    
    f. Haga clic en **GUARDAR**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

1. Al hacer clic en el icono de Velpic SAML del panel de acceso, debería entrar en la página de inicio de sesión de la aplicación Velpic SAML. Debe ver el botón **"Log In With Azure AD"** (Iniciar sesión con Azure AD) en la página de inicio de sesión.

    ![Complemento](./media/velpicsaml-tutorial/velpic_6.png)

1. Haga clic en el botón **"Log In With Azure AD"** (Iniciar sesión con Azure AD) para iniciar sesión en Velpic con su cuenta de Azure AD.

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

