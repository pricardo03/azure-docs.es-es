---
title: 'Tutorial: Integración de Azure Active Directory con NetDocuments | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y NetDocuments.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/04/2019
ms.author: jeedes
ms.openlocfilehash: 929d5d7a8e2b45aeb4ef71e4599cfcf23be83088
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67096602"
---
# <a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Tutorial: Integración de Azure Active Directory con NetDocuments

En este tutorial, aprenderá a integrar NetDocuments con Azure Active Directory (Azure AD).
La integración de NetDocuments con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a NetDocuments.
* Puede permitir que los usuarios inicien sesión automáticamente en NetDocuments (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con NetDocuments, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en NetDocuments

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* NetDocuments admite el inicio de sesión único iniciado por **SP**.

## <a name="adding-netdocuments-from-the-gallery"></a>Adición de NetDocuments desde la galería

Para configurar la integración de NetDocuments en Azure AD, es preciso agregar dicha solución desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar NetDocuments desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **NetDocuments**, seleccione **NetDocuments** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![NetDocuments en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, puede configurar y probar el inicio de sesión único de Azure AD con NetDocuments con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de NetDocuments.

Para configurar y probar el inicio de sesión único de Azure AD con NetDocuments, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de NetDocuments](#configure-netdocuments-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de NetDocuments](#create-netdocuments-test-user)** : el objetivo es tener un homólogo de Britta Simon en NetDocuments que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con NetDocuments, realice los pasos siguientes:

1. En la página de integración de la aplicación **NetDocuments** de [Azure Portal](https://portal.azure.com/), seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de NetDocuments](common/sp-reply.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<Repository ID>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<Repository ID>`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de URL de respuesta y URL de inicio de sesión. El identificador del repositorio es un valor que comienza por **CA-** seguido por un código de ocho caracteres asociado al repositorio de NetDocuments. Para más información puede consultar el [documento de soporte técnico sobre identidad federada de NetDocuments](https://support.netdocuments.com/hc/en-us/articles/205220410-Federated-Identity-Login). También tiene la alternativa de ponerse en contacto con el [equipo de soporte técnico de NetDocuments](https://support.netdocuments.com/hc/) para obtener estos valores si tiene dificultades para configurar utilizando la información anterior. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Set up NetDocuments** (Configurar NetDocuments), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-netdocuments-single-sign-on"></a>Configuración del inicio de sesión único de NetDocuments

1. En otra ventana del explorador web, inicie sesión en como administrador en el sitio de la compañía de NetDocuments.

2. Vaya a **Administración**.

3. Haga clic en **Agregar y quitar usuarios y grupos**.
   
    ![Repositorio](./media/netdocuments-tutorial/ic795047.png "Repositorio")

4. Haga clic en **Configurar opciones de autenticación avanzadas**.
    
    ![Configurar opciones de autenticación avanzadas](./media/netdocuments-tutorial/ic795048.png "Configurar opciones de autenticación avanzadas")

5. En el cuadro de diálogo **Federated Identity** (Identidad federada), realice los pasos siguientes:
   
    ![Identidad federada](./media/netdocuments-tutorial/ic795049.png "Identidad federada")
   
    a. Como **Tipo de servidor de identidad federada**, seleccione **Servicios de federación de Active Directory**.
   
    b. Haga clic en el botón **Choose file** (Elegir archivo) para cargar el archivo de metadatos que descargó de Azure Portal.
   
    c. Haga clic en **OK**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon\@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a NetDocuments.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **NetDocuments**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **NetDocuments**.

    ![Vínculo a NetDocuments en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-netdocuments-test-user"></a>Creación de un usuario de prueba de NetDocuments

Para permitir que los usuarios de Azure AD inicien sesión en NetDocuments, deben aprovisionarse en dicha aplicación.  
En el caso de NetDocuments, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión como administrador en el sitio de la compañía **NetDocuments** .

2. En el menú de la parte superior, haga clic en **Administrador**.
   
    ![Administración](./media/netdocuments-tutorial/ic795051.png "Administración")

3. Haga clic en **Agregar y quitar usuarios y grupos**.
   
    ![Repositorio](./media/netdocuments-tutorial/ic795047.png "Repositorio")

4. En el cuadro de texto **Dirección de correo electrónico**, escriba la dirección de correo electrónico de la cuenta válida de Azure Active Directory que quiera aprovisionar y haga clic en **Agregar usuario**.
   
    ![Dirección de correo electrónico](./media/netdocuments-tutorial/ic795053.png "Dirección de correo electrónico")
   
    >[!NOTE]
    >El titular de la cuenta de Azure Active Directory recibirá un mensaje de correo electrónico con un vínculo para confirmar la cuenta antes de que se active. Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de NetDocuments ofrecida por NetDocuments para aprovisionar cuentas de usuario de Azure Active Directory.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de NetDocuments del Panel de acceso, debería iniciar sesión automáticamente en la versión de NetDocuments para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

