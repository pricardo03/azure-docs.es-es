---
title: 'Tutorial: Integración de Azure Active Directory con Peakon | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Peakon.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a944c397-ed3f-4d45-b9b2-6d4bcb6b0a09
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b841cbe838156cd00876a9a13c7a9cbe1790ea80
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57842219"
---
# <a name="tutorial-azure-active-directory-integration-with-peakon"></a>Tutorial: Integración de Azure Active Directory con Peakon

En este tutorial, obtendrá información sobre cómo integrar Peakon con Azure Active Directory (Azure AD).

La integración de Peakon con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Peakon.
- Puede permitir que los usuarios inicien sesión automáticamente en Peakon (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Peakon, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción de Peakon habilitada para el inicio de sesión único

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Peakon desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-peakon-from-the-gallery"></a>Adición de Peakon desde la galería

Para configurar la integración de Peakon en Azure AD, deberá agregar Peakon desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Peakon desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Peakon**, seleccione **Peakon** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Peakon en la lista de resultados](./media/peakon-tutorial/tutorial_peakon_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Peakon con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Peakon para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Peakon.

Para configurar y probar el inicio de sesión único de Azure AD con Peakon, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Peakon](#creating-a-peakon-test-user)**: para tener un homólogo de Britta Simon en Peakon que esté vinculado a la representación de Azure AD de usuario.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Peakon.

**Para configurar el inicio de sesión único de Azure AD con Peakon, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Peakon**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial_general_301.png)

3. En la página **Set-up Single Sign-On with SAML** (Configurar inicio de sesión único con SAML), haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

4. En la sección **Configuración de SAML básica**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por **IDP**:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Peakon](./media/peakon-tutorial/tutorial_peakon_url.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://app.peakon.com/saml/<companyid>/metadata`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://app.peakon.com/saml/<companyid>/assert`.

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Peakon](./media/peakon-tutorial/tutorial_peakon_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL: `https://app.peakon.com/login`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL de respuesta y el identificador reales, que se explican más adelante en el tutorial.

6. En la sección **Certificado de firma SAML**, haga clic en **Descargar** para descargar el **Certificado (sin procesar)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/peakon-tutorial/tutorial_peakon_certificate.png) 

7. En la sección **Set up Peakon** (Configurar Peakon), copie la dirección URL adecuada según sus necesidades.

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![Configuración de Peakon](common/configuresection.png)

8. En otra ventana del explorador web, inicie sesión en Peakon como administrador.

9. En la barra de menús en el lado izquierdo de la página, haga clic en **Configuración** y, a continuación, vaya a **Integraciones**.

    ![Página Configuración](./media/peakon-tutorial/tutorial_peakon_config.png)

10. En la página **Integraciones**, haga clic en **Inicio de sesión único**.

    ![Sección Inicio de sesión único](./media/peakon-tutorial/tutorial_peakon_single.png)

11. En la sección **Inicio de sesión único**, haga clic en **Habilitar**.

    ![Opción habilitar](./media/peakon-tutorial/tutorial_peakon_enable.png)

12. En la sección **Single sign-on for employees using SAML** (Inicio de sesión único para empleados mediante SAML), siga estos pasos:

    ![SAML](./media/peakon-tutorial/tutorial_peakon_saml.png)

     a. En el cuadro de texto **SSO Login URL** (URL de inicio de sesión SSO), pegue el valor de la **URL de inicio de sesión** que ha copiado de Azure Portal.

    b. En el cuadro de texto **SSO Logout URL** (URL de cierre de inicio sesión SSO), pegue el valor de la **URL de cierre de sesión** que ha copiado de Azure Portal.

    c. Haga clic en **Choose a File** (Elegir un archivo) para cargar el certificado que descargó de Azure Portal en el cuadro Certificado.

    d. Haga clic en el **icono** para copiar el **Id. de entidad** y péguelo en el cuadro de texto **Identificador** de la sección **Configuración básica de SAML** de Azure Portal.

    e. Haga clic en el **icono** para copiar la **Reply URL (ACS)** [URL de respuesta (ACS)] y péguela en el cuadro de texto  **URL de respuesta** de la sección  **Configuración básica de SAML** de Azure Portal.

    f. Haga clic en **Guardar**

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_02.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el **nombre de usuario** , escriba **brittasimon\@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.

### <a name="creating-a-peakon-test-user"></a>Creación de un usuario de prueba de Peakon

Para permitir que los usuarios de Azure AD inicien sesión en Peakon, deben aprovisionarse en Peakon.  
En el caso de Peakon, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en el sitio de la compañía de Peakon como administrador.

2. En la barra de menús en el lado izquierdo de la página, haga clic en **Configuración** y, a continuación, vaya a **Empleados**.

    ![Empleados](./media/peakon-tutorial/tutorial_peakon_employee.png)

3. En la parte superior derecha de la página, haga clic en  **Add employee** (Agregar empleado).

      ![Agregar empleado](./media/peakon-tutorial/tutorial_peakon_addemployee.png)

3. En la página del cuadro de diálogo **New employee** (nuevo empleado), realice los siguientes pasos:

     ![Nuevo empleado](./media/peakon-tutorial/tutorial_peakon_create.png)

     a. En el cuadro de texto **Nombre**, escriba un nombre como **Britta** y un apellido como **Simon**.

    b. En el **correo electrónico** cuadro de texto, escriba la dirección de correo electrónico como **Brittasimon\@contoso.com**.

    c. Haga clic en **Crear empleado**.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure al concederle acceso a Peakon.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Peakon**.

    ![Configurar inicio de sesión único](./media/peakon-tutorial/tutorial_peakon_app.png) 

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Peakon en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Peakon.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
