---
title: 'Tutorial: Integración de Azure Active Directory con Dropbox for Business | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Dropbox for Business.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/20/2018
ms.author: jeedes
ms.openlocfilehash: eadf6724891d348c2ea3654bcf19ef0d74078049
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "42143089"
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Tutorial: Integración de Azure Active Directory con Dropbox para Empresas

En este tutorial, aprenderá a integrar Dropbox for Business con Azure Active Directory (Azure AD).

La integración de Dropbox for Business con Azure AD proporciona las siguientes ventajas:

- En Azure AD se puede controlar quién tiene acceso a Dropbox for Business.
- Es posible habilitar que los usuarios inicien sesión automáticamente en Dropbox for Business (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Dropbox for Business, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción a Dropbox for Business con inicio de sesión único habilitado.

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.
El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Dropbox for Business desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-dropbox-for-business-from-the-gallery"></a>Adición de Dropbox for Business desde la galería

Para configurar la integración de Dropbox for Business en Azure AD, es preciso agregar Dropbox for Business desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Dropbox for Business desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Dropbox for Business**, seleccione **Dropbox for Business** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Dropbox for Business en la lista de resultados](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, se configura y prueba el inicio de sesión único de Azure AD con Dropbox for Business con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, es preciso que Azure AD sepa cuál es el usuario homólogo de Dropbox for Business para un usuario de Azure AD. Es decir, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Dropbox for Business.

Para establecer la relación de vínculo, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario** en Dropbox for Business.

Para configurar y probar el inicio de sesión único de Azure AD con Dropbox for Business, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Dropbox for Business](#create-a-dropbox-for-business-test-user)**: para tener un homólogo de Britta Simon en Dropbox for Business que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Dropbox for Business.

**Para configurar el inicio de sesión único de Azure AD con Dropbox for Business, siga estos pasos:**

1. En la página de integración de la aplicación **Dropbox for Business** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. En la sección de **dominio y direcciones URL de Dropbox for Business**, lleve a cabo los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de Dropbox for Business](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url1.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://www.dropbox.com/sso/<id>`.

    b. En el cuadro de texto **Identificador**, escriba un valor: `Dropbox`

    > [!NOTE]
    > El valor de URL de inicio de sesión anterior no es real. El valor se actualizará con la dirección URL de inicio de sesión real, que se explica más adelante en el tutorial.

4. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/dropboxforbusiness-tutorial/tutorial_general_400.png)

6. En la sección **Configuración de Dropbox for Business**, haga clic en **Configure Dropbox for Business** (Configurar Dropbox for Business) para abrir la ventana **Configurar inicio de sesión**. Copie la **dirección URL de servicio de inicio de sesión único de SAML** de la sección **Referencia rápida**.

    ![Configuración de Dropbox for Business](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. Para configurar el inicio de sesión único en **Dropbox for Business**, vaya al inquilino de Dropbox for Business e inicie sesión.

    ![Configurar inicio de sesión único](./media/dropboxforbusiness-tutorial/ic769509.png "Configurar inicio de sesión único")

8. Haga clic en el **icono de usuario** y seleccione la pestaña **Settings** (Configuración).

    ![Configurar inicio de sesión único](./media/dropboxforbusiness-tutorial/configure1.png "Configurar inicio de sesión único")

9. En el panel de navegación de la izquierda, haga clic en **Admin Console**(Consola de administración).

    ![Configurar inicio de sesión único](./media/dropboxforbusiness-tutorial/configure2.png "Configurar inicio de sesión único")

10. En **Admin Console** (Consola de administración), haga clic en la opción **Settings** (Configuración) del panel de navegación izquierdo.

    ![Configurar inicio de sesión único](./media/dropboxforbusiness-tutorial/configure3.png "Configurar inicio de sesión único")

11. Seleccione la opción **Single sign-on** (Inicio de sesión único) en la sección **Authentication** (Autenticación).

    ![Configurar inicio de sesión único](./media/dropboxforbusiness-tutorial/configure4.png "Configurar inicio de sesión único")

12. En la sección **Single sign-on** (Inicio de sesión único), siga estos pasos:  

    ![Configurar inicio de sesión único](./media/dropboxforbusiness-tutorial/configure5.png "Configurar inicio de sesión único")

    a. Seleccione **Required** (Obligatorio) como opción en el menú desplegable para **Single sign-on** (Inicio de sesión único).

    b. Haga clic en **Add sign-in URL** (Agregar inicio de sesión único) y en el cuadro de texto **Identity provider sign-in URL** (Dirección URL del inicio de sesión único del proveedor de identidades), pegue el valor de **Dirección URL del servicio de inicio de sesión único de SAML**, que ha copiado desde Azure Portal y luego seleccione **Done** (Listo).

    ![Configurar inicio de sesión único](./media/dropboxforbusiness-tutorial/configure6.png "Configurar inicio de sesión único")

    c. Haga clic en **Upload certificate** (Cargar certificado) y, a continuación, vaya al **archivo de certificado codificado en Base64** que ha descargado de Azure Portal.

    d. Haga clic en **Copy link** (Copiar vínculo) y pegue el valor copiado en el cuadro de texto **Dirección URL de inicio de sesión** en la sección de **Dominio y direcciones URL de Dropbox for Business** en Azure Portal.

    e. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/dropboxforbusiness-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/dropboxforbusiness-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/dropboxforbusiness-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/dropboxforbusiness-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-dropbox-for-business-test-user"></a>Creación de un usuario de prueba de Dropbox for Business

En esta sección, creará un usuario llamado a Britta Simon en Dropbox for Business. Dropbox for Business admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada.

No hay ningún elemento de acción para usted en esta sección. Si un usuario ya no existe en Dropbox for Business, se crea uno nuevo cuando se intenta acceder a esta aplicación.

>[!Note]
>Si necesita crear un usuario de forma manual, póngase en contacto con el [equipo de soporte de cliente de Dropbox for Business](https://www.dropbox.com/business/contact) 

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Dropbox for Business para que use el inicio de sesión único de Azure.

![Asignación de rol de usuario][200] 

**Siga estos pasos para asignar Britta Simon a Dropbox for Business:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Dropbox for Business**.

    ![El vínculo de Dropbox for Business en la lista Aplicaciones](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono Dropbox for Business en el Panel de acceso, debe ir a la página de inicio de sesión de la aplicación Dropbox for Business.
 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/dropboxforbusiness-tutorial/tutorial_general_203.png

