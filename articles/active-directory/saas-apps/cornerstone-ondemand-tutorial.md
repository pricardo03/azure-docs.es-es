---
title: 'Tutorial: Integración de Azure Active Directory con Cornerstone OnDemand | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Cornerstone OnDemand.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f57c5fef-49b0-4591-91ef-fc0de6d654ab
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.openlocfilehash: 40d5905764dbf139db28cd1c113d06981be8205d
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2019
ms.locfileid: "53972335"
---
# <a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Tutorial: Integración de Azure Active Directory con Cornerstone OnDemand

En este tutorial, aprenderá a integrar Cornerstone OnDemand con Azure Active Directory (Azure AD).
Integrar Cornerstone OnDemand con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Cornerstone OnDemand.
* Puede permitir que los usuarios inicien sesión automáticamente en Cornerstone OnDemand (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Cornerstone OnDemand, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción habilitada para el inicio de sesión único en Cornerstone OnDemand

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Cornerstone OnDemand admite SSO iniciado por **SP**
* Cornerstone OnDemand admite el [aprovisionamiento automático de usuarios](cornerstone-ondemand-provisioning-tutorial.md)

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Adición de Cornerstone OnDemand desde la galería

Para configurar la integración de Cornerstone OnDemand en Azure AD, deberá agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Cornerstone OnDemand desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Cornerstone OnDemand**, seleccione **Cornerstone OnDemand** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Cornerstone OnDemand en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, va a configurar y probar el inicio de sesión único de Azure AD con Cornerstone OnDemand con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Cornerstone OnDemand.

Para configurar y probar el inicio de sesión único de Azure AD con Cornerstone OnDemand, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Cornerstone OnDemand](#configure-cornerstone-ondemand-single-sign-on)**: para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Cornerstone OnDemand](#create-cornerstone-ondemand-test-user)**: para tener un homólogo de Britta Simon en Cornerstone OnDemand que esté vinculado a la representación de Azure AD del usuario.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Cornerstone OnDemand, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Cornerstone OnDemand**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Cornerstone OnDemand](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<company>.csod.com`

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://<company>.csod.com`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Cornerstone OnDemand](mailto:moreinfo@csod.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Configurar Cornerstone OnDemand**, copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-cornerstone-ondemand-single-sign-on"></a>Configuración del inicio de sesión único en Cornerstone OnDemand

Para configurar el inicio de sesión único en **Cornerstone OnDemand**, es preciso enviar el **certificado (Base64)** descargado y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de Cornerstone OnDemand](mailto:moreinfo@csod.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Cornerstone OnDemand.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Cornerstone OnDemand**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **Cornerstone OnDemand**.

    ![El vínculo de Cornerstone OnDemand en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-cornerstone-ondemand-test-user"></a>Creación de un usuario de prueba de Cornerstone OnDemand

El objetivo de esta sección es crear un usuario llamado Britta Simon en Cornerstone OnDemand. Cornerstone OnDemand admite el aprovisionamiento automático de usuarios, que está habilitado de forma predeterminada. [Aquí](https://docs.microsoft.com/azure/active-directory/saas-apps/cornerstone-ondemand-provisioning-tutorial) puede encontrar más información sobre cómo configurar el aprovisionamiento automático de usuarios.

**Para crear un usuario manualmente, siga los pasos siguientes:**

Para configurar el aprovisionamiento del usuario, envíe la información (por ejemplo, nombre, correo electrónico) sobre el usuario de Azure AD que desee aprovisionar al [equipo de soporte técnico de Cornerstone OnDemand](mailto:moreinfo@csod.com).

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Cornerstone OnDemand ofrecida por Cornerstone OnDemand para aprovisionar cuentas de usuario de AAD.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Cornerstone OnDemand en el Panel de acceso, debería iniciar sesión automáticamente en la instancia de Cornerstone OnDemand para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Configuración del aprovisionamiento de usuarios](https://docs.microsoft.com/azure/active-directory/saas-apps/cornerstone-ondemand-provisioning-tutorial) 

