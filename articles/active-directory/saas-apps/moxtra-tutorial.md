---
title: 'Tutorial: Integración de Azure Active Directory con Moxtra | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Moxtra.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/01/2019
ms.author: jeedes
ms.openlocfilehash: 7f64597d8da183a24bcf87543a448442052e5f77
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2019
ms.locfileid: "68944260"
---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a>Tutorial: Integración de Azure Active Directory con Moxtra

En este tutorial, obtendrá información sobre cómo integrar Moxtra con Azure Active Directory (Azure AD).
Integrar Moxtra con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Moxtra.
* Puede permitir que los usuarios inicien sesión automáticamente en Moxtra (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Moxtra, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en Moxtra

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Moxtra admite el inicio de sesión único iniciado por **SP**

## <a name="adding-moxtra-from-the-gallery"></a>Incorporación de Moxtra desde la galería

Para configurar la integración de Moxtra en Azure AD, es preciso agregar Moxtra desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Moxtra desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Moxtra**, seleccione **Moxtra** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![Moxtra en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Moxtra con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Moxtra.

Para configurar y probar el inicio de sesión único de Azure AD con Moxtra, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Moxtra](#configure-moxtra-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Moxtra](#create-moxtra-test-user)** : para tener un homólogo de Britta Simon en Moxtra que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Moxtra, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Moxtra**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Moxtra](common/sp-signonurl.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://www.moxtra.com/service/#login`

5. La aplicación Moxtra espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados. Haga clic en el icono **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

6. Además de lo anterior, la aplicación Moxtra espera que se usen algunos atributos más en la respuesta de SAML. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, realice los siguientes pasos para agregar el atributo Token SAML como se muestra en la tabla siguientes: 

    | NOMBRE | Atributo de origen|
    | ------------------- | -------------------- |    
    | firstname | user.givenname |
    | lastname | user.surname |
    | idpid    | <Identificador de Azure AD>

    > [!Note]
    > El valor del atributo **idpid** no es real. Puede obtener el valor real de la sección **Set up Moxtra** (Configurar Moxtra) del paso 8. 

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

8. En la sección **Set up Moxtra** (Configurar Moxtra), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-moxtra-single-sign-on"></a>Configuración del inicio de sesión único de Moxtra

1. En otra ventana del explorador, inicie sesión en su sitio de la compañía de Moxtra como administrador.

2. En la barra de herramientas de la izquierda, haga clic en **Admin Console > SAML Single Sign-on** (Consola de administración > Inicio de sesión único de SAML) y, luego, en **New** (Nuevo).
   
    ![Configurar inicio de sesión único](./media/moxtra-tutorial/tutorial_moxtra_06.png) 

3. En la página **SAML** , realice los siguientes pasos:
   
    ![Configurar inicio de sesión único](./media/moxtra-tutorial/tutorial_moxtra_08.png)   
 
    a. En el cuadro de texto **Name** (Nombre), escriba el nombre de la configuración (por ejemplo, *SAML*). 
  
    b. En el cuadro de texto **IdP Entity ID** (Identificador de la entidad de IdP), pegue el valor de **Identificador de Azure AD** que copió de Azure Portal. 
 
    c. En el cuadro de texto **Dirección URL de inicio de sesión**, pegue el valor de la **dirección URL de inicio de sesión** que ha copiado de Azure Portal. 
 
    d. En el cuadro de texto **AuthnContextClassRef**, escriba **urn: oasis: nombres: tc: SAML:2.0:ac:classes:Password**. 
 
    e. En el cuadro de texto **NameID format** (Formato de id. de nombre), escriba **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**. 
 
    f. Abra el certificado que ha descargado desde Azure Portal en el Bloc de notas, copie el contenido y luego péguelo en el cuadro de texto **Certificate** (Certificado).    
 
    g. En el cuadro de texto del dominio de correo electrónico SAML, escriba su dominio de correo electrónico SAML.    
  
    >[!NOTE]
    >Para ver los pasos para comprobar el dominio, haga clic en la "**i**" a continuación.

    h. Haga clic en **Update**(Actualizar).

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

En esta sección, concederá acceso a Britta Simon a Moxtra para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, a continuación, seleccione **Moxtra**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Moxtra**.

    ![Vínculo a Moxtra en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-moxtra-test-user"></a>Creación de un usuario de prueba de Moxtra

El objetivo de esta sección es crear un usuario llamado Britta Simon en Moxtra.

**Para crear un usuario llamado Britta Simon en Moxtra, realice los pasos siguientes:**

1. Inicie sesión en su sitio de la compañía de Moxtra como administrador.

1. En la barra de herramientas de la izquierda, haga clic en **Consola de administración > Administración de usuarios** y, luego, en **Agregar usuario**.
   
    ![Configurar inicio de sesión único](./media/moxtra-tutorial/tutorial_moxtra_10.png) 

1. En el cuadro de diálogo **Agregar usuario** , realice los pasos siguientes:
  
    a. En el cuadro de texto **Nombre**, escriba **Britta**.
  
    b. En el cuadro de texto **Apellidos**, escriba **Simon**.
  
    c. En el cuadro de texto **Email** (Correo electrónico), escriba la dirección de correo electrónico de Britta en Azure Portal.
  
    d. En el cuadro de texto **Division** (División), escriba **Dev**.
  
    e. En el cuadro de texto **Department** (Departamento), escriba **IT**.
  
    f. Seleccione **Administrator** (Administrador).
  
    g. Haga clic en **Agregar**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Moxtra en el panel de acceso, debería iniciar sesión automáticamente en la versión de Moxtra para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

