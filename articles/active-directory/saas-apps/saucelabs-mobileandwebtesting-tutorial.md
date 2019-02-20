---
title: 'Tutorial: integración de Azure Active Directory con Sauce Labs - Mobile and Web Testing | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Sauce Labs - Mobile and Web Testing.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3142d947-70e5-4345-8a30-b92d8715fac9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c4e3d074ad241ec6add6fe43b77413ce283e389
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56207014"
---
# <a name="tutorial-azure-active-directory-integration-with-sauce-labs---mobile-and-web-testing"></a>Tutorial: integración de Azure Active Directory con Sauce Labs - Mobile and Web Testing

En este tutorial, obtendrá información sobre cómo integrar Sauce Labs - Mobile and Web Testing con Azure Active Directory (Azure AD).

La integración de Sauce Labs - Mobile and Web Testing con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Sauce Labs - Mobile and Web Testing.
- Puede permitir que los usuarios inicien sesión automáticamente en Sauce Labs - Mobile and Web Testing (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Sauce Labs - Mobile and Web Testing, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único de Sauce Labs - Mobile and Web Testing

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Sauce Labs - Mobile and Web Testing desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-sauce-labs---mobile-and-web-testing-from-the-gallery"></a>Adición de Sauce Labs - Mobile and Web Testing desde la galería
Para configurar la integración de Sauce Labs - Mobile and Web Testing en Azure AD, deberá agregar Sauce Labs - Mobile and Web Testing desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Sauce Labs - Mobile and Web Testing desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Sauce Labs - Mobile and Web Testing**, seleccione **Sauce Labs - Mobile and Web Testing** en el panel de resultados y después haga clic en **Agregar** para agregar la aplicación.

    ![Sauce Labs - Mobile and Web Testing en la lista de resultados](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Sauce Labs - Mobile and Web Testing con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Sauce Labs - Mobile and Web Testing para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Sauce Labs - Mobile and Web Testing.

Para configurar y probar el inicio de sesión único de Azure AD con Sauce Labs - Mobile and Web Testing, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Sauce Labs - Mobile and Web Testing](#create-a-sauce-labs---mobile-and-web-testing-test-user)**: para tener un homólogo de Britta Simon en Sauce Labs - Mobile and Web Testing que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Sauce Labs - Mobile and Web Testing.

**Para configurar el inicio de sesión único de Azure AD con Sauce Labs - Mobile and Web Testing, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Sauce Labs - Mobile and Web Testing**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_samlbase.png)

3. En la sección **Dominio y direcciones URL de Sauce Labs - Mobile and Web Testing**, el usuario no tiene que realizar ningún paso ya que la aplicación se ha integrado previamente con Azure.

    ![Información de inicio de sesión único en Dominio y direcciones URL de Sauce Labs - Mobile and Web Testing](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_url.png)

4. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_certificate.png)

5. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_400.png)

6. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de Sauce Labs - Mobile and Web Testing como administrador.

7. Haga clic en el **icono de usuario** y seleccione la pestaña **Administración del equipo**.

    ![Configurar inicio de sesión único](./media/saucelabs-mobileandwebtesting-tutorial/configure1.png)

8. Escriba su **Nombre de dominio** en el cuadro de texto.

    ![Configurar inicio de sesión único](./media/saucelabs-mobileandwebtesting-tutorial/configure2.png)

9. Haga clic en la pestaña **Configurar**.

    ![Configurar inicio de sesión único](./media/saucelabs-mobileandwebtesting-tutorial/configure3.png)

10. En la sección **Configurar inicio de sesión único**, siga estos pasos.

    ![Configurar inicio de sesión único](./media/saucelabs-mobileandwebtesting-tutorial/configure4.png)

     a. Haga clic en **Examinar** y cargue el archivo de metadatos descargado de Azure AD.

    b. Seleccione la casilla **PERMITIR APROVISIONAMIENTO JUST-IN-TIME**.

    c. Haga clic en **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/saucelabs-mobileandwebtesting-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/saucelabs-mobileandwebtesting-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/saucelabs-mobileandwebtesting-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/saucelabs-mobileandwebtesting-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
  
### <a name="create-a-sauce-labs---mobile-and-web-testing-test-user"></a>Creación de un usuario de prueba de Sauce Labs - Mobile and Web Testing

El objetivo de esta sección es crear una usuaria llamada Britta Simon en Sauce Labs - Mobile and Web Testing. Sauce Labs - Mobile and Web Testing admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Al intentar acceder a Sauce Labs - Mobile and Web Testing, si aún no existe, se crea un usuario.
>[!Note]
>Si necesita crear manualmente un usuario, póngase en contacto con el  [equipo de soporte técnico de Sauce Labs - Mobile and Web Testing](mailto:support@saucelabs.com).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Sauce Labs - Mobile and Web Testing.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a Sauce Labs - Mobile and Web Testing, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Sauce Labs - Mobile and Web Testing**.

    ![Vínculo de Sauce Labs - Mobile and Web Testing en la lista de aplicaciones](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el mosaico de Sauce Labs - Mobile and Web Testing en el panel de acceso, debe iniciar sesión automáticamente en la aplicación Sauce Labs - Mobile and Web Testing.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_01.png
[2]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_02.png
[3]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_03.png
[4]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_04.png

[100]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_100.png

[200]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_200.png
[201]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_201.png
[202]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_202.png
[203]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_203.png
