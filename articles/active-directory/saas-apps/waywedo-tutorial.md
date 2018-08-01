---
title: 'Tutorial: Integración de Azure Active Directory con Way We Do | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Way We Do.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 84fc4f36-ecd1-42c6-8a70-cb0f3dc15655
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2018
ms.author: jeedes
ms.openlocfilehash: bc415ec7c577e221a1ab5af585dff5b4fc9ab7dc
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "39259683"
---
# <a name="tutorial-azure-active-directory-integration-with-way-we-do"></a>Tutorial: Integración de Azure Active Directory con Way We Do

En este tutorial, aprenderá a integrar Way We Do con Azure Active Directory (Azure AD).

La integración de Way We Do con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Way We Do.
- Puede permitir que los usuarios inicien sesión automáticamente en Way We Do (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Way We Do, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único de Way We Do

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Way We Do desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-way-we-do-from-the-gallery"></a>Incorporación de Way We Do desde la galería
Para configurar la integración de Way We Do en Azure AD, deberá agregar Way We Do desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Way We Do desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Way We Do**, seleccione **Way We Do** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Way We Do en la lista de resultados](./media/waywedo-tutorial/tutorial_waywedo_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Way We Do con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Way We Do para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Way We Do.

Para configurar y probar el inicio de sesión único de Azure AD con Way We Do, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Way We Do](#create-a-way-we-do-test-user)**: para tener un homólogo de Britta Simon en Way We Do que esté vinculado a su representación en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Way We Do.

**Para configurar el inicio de sesión único de Azure AD con Way We Do, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Way We Do**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/waywedo-tutorial/tutorial_waywedo_samlbase.png)

3. En la sección **Domain and URLs** (Dominio y direcciones URL de Way We Do), lleve a cabo los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de Way We Do](./media/waywedo-tutorial/tutorial_waywedo_url.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.waywedo.com/Authentication/ExternalSignIn`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.waywedo.com`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Way We Do](mailto:support@waywedo.com) para obtener estos valores. 
 
4. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (sin procesar)** y, a continuación, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/waywedo-tutorial/tutorial_waywedo_certificate.png) 

5. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/waywedo-tutorial/tutorial_general_400.png)

6. En la sección **Configuración de Way We Do**, haga clic en **Configurar Way We Do** para abrir la ventana **Configurar inicio de sesión**. Copie **SAML Entity ID and SAML Single Sign-On Service URL** (URL del servicio de inicio de sesión único de SAML e Identificador de entidad de SAML) de la sección **Referencia rápida**.

    ![Configuración de Way We Do](./media/waywedo-tutorial/tutorial_waywedo_configure.png) 

7. En otra ventana del explorador web, inicie sesión en Way We Do como Security Administrator (administrador de seguridad).

8. Haga clic en el **icono de persona** en la esquina superior derecha de cualquier página de Way We Do y, a continuación, haga clic en **Account** (Cuenta) en el menú desplegable.

    ![Cuenta de Way We Do](./media/waywedo-tutorial/tutorial_waywedo_account.png) 

9. Haga clic en el **icono de menú** para abrir el menú de navegación de inserción y haga clic en **Single Sign On** (Inicio de sesión único).

    ![Way We Do único](./media/waywedo-tutorial/tutorial_waywedo_single.png)

10. En la página **Single sign-on setup** (Configuración de inicio de sesión único), siga estos pasos:

    ![Way We Do guardar](./media/waywedo-tutorial/tutorial_waywedo_save.png)

    a. Haga clic en el botón de alternar de **Turn on single sign-on** (Activar inicio de sesión único) para cambiar a **Sí** y habilitar el inicio de sesión único.

    b. En el cuadro de texto **Single sign-on name**  (Nombre de inicio de sesión único), escriba su nombre.

    c. En el cuadro de texto **Id. de entidad**, pegue el valor de **SAML Entity ID** (Identificador de entidad de SAML) que copió de Azure Portal.

    d. En el cuadro de texto **SAML SSO URL** (Dirección URL de inicio de sesión único de SAML), pegue el valor de la **dirección URL del servicio de inicio de sesión único de SAML** que ha copiado de Azure Portal.

    e. Para cargar el certificado, haga clic en el botón **select** (seleccionar) situado junto a **Certificate** (Certificado).

    f. **Configuración opcional** -
    
    * Enable Passwords (Habilitar las contraseñas): cuando esta opción está deshabilitada, la contraseña normal funciona para Way We Do de modo que los usuarios solo pueden usar el inicio de sesión único.

    * Enable Auto-provisioning (Habilitar aprovisionamiento automático): cuando esta opción está habilitada, la dirección de correo electrónico usada para el inicio de sesión se compara automáticamente con la lista de los usuarios de Way We Do. Si la dirección de correo electrónico no coincide con un usuario activo de Way We Do, agrega automáticamente una nueva cuenta de usuario para la persona que inicia sesión y se solicita la información que falta.

      > [!NOTE]
      > Los usuarios agregados mediante el inicio de sesión único se agregan como usuarios generales y no se les asigna ningún rol en el sistema. Un administrador puede modificar su rol de seguridad como editor o administrador, y también puede asignar uno o varios roles de organigrama. 

    g. Haga clic en **Guardar** para guardar la configuración.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/waywedo-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/waywedo-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/waywedo-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/waywedo-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-way-we-do-test-user"></a>Creación de un usuario de prueba de Way We Do

El objetivo de esta sección es crear una usuaria de prueba llamada Britta Simon en Way We Do. Way We Do admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Al intentar acceder a Way We Do, se crea un nuevo usuario, en caso de que no exista.

> [!Note]
> Si necesita crear manualmente un usuario, es preciso que se ponga contacto con el [equipo de soporte técnico de Way We Do](mailto:support@waywedo.com).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Way We Do.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a Way We Do, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **Way We Do**.

    ![Vínculo a Way We Do en la lista de aplicaciones](./media/waywedo-tutorial/tutorial_waywedo_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Way We Do en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación Way We Do.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/waywedo-tutorial/tutorial_general_01.png
[2]: ./media/waywedo-tutorial/tutorial_general_02.png
[3]: ./media/waywedo-tutorial/tutorial_general_03.png
[4]: ./media/waywedo-tutorial/tutorial_general_04.png

[100]: ./media/waywedo-tutorial/tutorial_general_100.png

[200]: ./media/waywedo-tutorial/tutorial_general_200.png
[201]: ./media/waywedo-tutorial/tutorial_general_201.png
[202]: ./media/waywedo-tutorial/tutorial_general_202.png
[203]: ./media/waywedo-tutorial/tutorial_general_203.png

