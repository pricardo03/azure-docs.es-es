---
title: 'Tutorial: Integración de Azure Active Directory con Silverback | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Silverback.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 32cfc96f-2137-49ff-818b-67feadcd73b7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: jeedes
ms.openlocfilehash: e7cb3049f680f81026e09388066001413922600a
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46123869"
---
# <a name="tutorial-azure-active-directory-integration-with-silverback"></a>Tutorial: Integración de Azure Active Directory con Silverback

En este tutorial, obtendrá información sobre cómo integrar Silverback con Azure Active Directory (Azure AD).

La integración de Silverback con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Silverback.
- Puede permitir que los usuarios inicien sesión automáticamente en Silverback (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Silverback, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción activa de Silverback

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar Silverback desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-silverback-from-the-gallery"></a>Agregar Silverback desde la galería
Para configurar la integración de Silverback en Azure AD, será preciso que agregue Silverback desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Silverback desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Silverback**, seleccione **Silverback** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Silverback en la lista de resultados](./media/silverback-tutorial/tutorial_silverback_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Silverback con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Silverback para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Silverback.

Para configurar y probar el inicio de sesión único de Azure AD con Silverback, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Silverback](#create-a-silverback-test-user)**: para tener un homólogo de Britta Simon en FileCloud que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Silverback.

**Para configurar el inicio de sesión único de Azure AD con Silverback, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Silverback**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/silverback-tutorial/tutorial_silverback_samlbase.png)

3. En la sección **Silverback Domain and URLs** (Dominios y direcciones URL de Silverback), lleve a cabo los pasos siguientes:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Silverback](./media/silverback-tutorial/tutorial_silverback_url.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<YOURSILVERBACKURL>.com/ssp`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `<YOURSILVERBACKURL>.com`

    c. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<YOURSILVERBACKURL>.com/sts/authorize/login`.

    > [!NOTE] 
    > Estos valores no son reales. Actualícelos con la dirección URL de inicio de sesión, el identificador y la dirección URL de respuesta reales. Póngase en contacto con el [equipo de atención al cliente de Silverback](mailto:helpdesk@matrix42.com) para obtener estos valores. 

4. En la sección **Certificado de firma de SAML**, haga clic en el botón Copiar para copiar la **dirección URL de metadatos de federación de la aplicación** y péguela en el Bloc de notas.

    ![Vínculo de descarga del certificado](./media/silverback-tutorial/tutorial_silverback_certificate.png) 

5. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/silverback-tutorial/tutorial_general_400.png)

6.  Inicie sesión en su servidor de Silverback como administrador y realice los pasos siguientes:

    a.  Navigate to **Admin** > **Authentication Provider** (Administrador > Proveedor de autenticación).

    b. En la página **Authentication Provider Settings** (Configuración del proveedor de autenticación), realice los siguientes pasos:

    ![El administrador ](./media/silverback-tutorial/tutorial_silverback_admin.png)

    c.  Haga clic en **Import from URL** (Importar desde URL).
    
    d.  Pegue la dirección URL de metadatos copiados y haga clic en **OK** (Aceptar).
    
    e.  Confirme con **OK** (Aceptar) y, a continuación, los valores se rellenarán automáticamente.
    
    f.  Seleccione **Show on Login Page** (Mostrar en la página de inicio de sesión).
    
    g.  Habilite la opción **Dynamic User Creation** (Creación dinámica de usuario) si quiere agregar automáticamente usuarios autorizados de Azure AD (opcional).
    
    h.  Cree un **título** para el botón en el Portal de autoservicio.

    i.  Cargue un **icono** haciendo clic en **Choose File** (Elegir archivo).
    
    j.  Seleccione el **color** de fondo del botón.
    
    k.  Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/silverback-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/silverback-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/silverback-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/silverback-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-silverback-test-user"></a>Crear un usuario de prueba de Silverback

Para permitir que los usuarios de Azure AD inicien sesión en Silverback, tienen que aprovisionarse en Silverback. En el caso de Silverback, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en el servidor de Silverback como administrador.

2. Vaya a **Users** (Usuarios) y **agregue un nuevo dispositivo de usuario**.

3. En la página **Basic** (Básica), realice los pasos siguientes:

    ![Usuario ](./media/silverback-tutorial/tutorial_silverback_user.png)

    a. En el cuadro de texto **Username** (Nombre de usuario), escriba el nombre de un usuario, por ejemplo, **Britta Simon**.

    b. En el cuadro de texto **First Name** (Nombre), escriba el nombre de usuario, en este caso **Britta**.

    c. En el cuadro de texto **Last Name** (Apellidos), escriba el nombre de usuario, en este caso **Simon**.

    d. En el cuadro de texto **E-mail Address** (Dirección de correo electrónico), escriba el correo electrónico del usuario, en este caso, **Brittasimon@contoso.com**.

    e. En el cuadro de texto **Password** (Contraseña), escriba su contraseña.
    
    f. En el cuadro de texto **Confirm Password** (Confirmar contraseña), vuelva a escribir la contraseña y confírmela.

    g. Haga clic en **Save**(Guardar).

>[!NOTE]
>Si no quiere crear manualmente cada usuario, seleccione la casilla en **Dynamic User Creation** (Creación dinámica de usuarios) en **Admin** > **Authentication Provider** (Administrador > Proveedor de autenticación).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Silverback.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a Silverback, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **Silverback**.

    ![Vínculo a Silverback en la lista de aplicaciones](./media/silverback-tutorial/tutorial_silverback_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Silverback en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Silverback.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/silverback-tutorial/tutorial_general_01.png
[2]: ./media/silverback-tutorial/tutorial_general_02.png
[3]: ./media/silverback-tutorial/tutorial_general_03.png
[4]: ./media/silverback-tutorial/tutorial_general_04.png

[100]: ./media/silverback-tutorial/tutorial_general_100.png

[200]: ./media/silverback-tutorial/tutorial_general_200.png
[201]: ./media/silverback-tutorial/tutorial_general_201.png
[202]: ./media/silverback-tutorial/tutorial_general_202.png
[203]: ./media/silverback-tutorial/tutorial_general_203.png

