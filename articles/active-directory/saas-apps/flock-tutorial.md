---
title: 'Tutorial: Integración de Azure Active Directory con Flock | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Flock.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7b2c3ac5-17f1-49a0-8961-c541b258d4b1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: 6589bbe581ec5e4ca3a18363ede427bf08f8b7cc
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39428131"
---
# <a name="tutorial-azure-active-directory-integration-with-flock"></a>Tutorial: Integración de Azure Active Directory con Flock

En este tutorial obtendrá información sobre cómo integrar Flock con Azure Active Directory (Azure AD).

La integración de Flock con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Flock.
- Puede permitir que los usuarios inicien sesión automáticamente en Flock (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Flock, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Flock

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Flock desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-flock-from-the-gallery"></a>Incorporación de Flock desde la galería
Para configurar la integración de Flock en Azure AD, será preciso agregar Flock desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Flock desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **Flock**, seleccione **Flock** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

    ![Flock en la lista de resultados](./media/flock-tutorial/tutorial_flock_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección podrá configurar y probar el inicio de sesión único de Azure AD con Flock con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Flock para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Flock.

Para configurar y probar el inicio de sesión único de Azure AD con Flock, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Flock](#create-a-flock-test-user)**: para tener un homólogo de Britta Simon en Flock que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Flock.

**Para configurar el inicio de sesión único de Azure AD con Flock, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Flock**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/flock-tutorial/tutorial_flock_samlbase.png)

1. En la sección **Dominio y direcciones URL de Flock**, lleve a cabo los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de Flock](./media/flock-tutorial/tutorial_flock_url.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.flock.com/`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.flock.com/`

    > [!NOTE]
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Flock](mailto:support@flock.com) para obtener estos valores.

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/flock-tutorial/tutorial_flock_certificate.png)

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/flock-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de Flock**, haga clic en **Configurar Flock** para abrir la ventana **Configurar inicio de sesión**. Copie **SAML Entity ID and SAML Single Sign-On Service URL** (URL del servicio de inicio de sesión único de SAML e Identificador de entidad de SAML) de la sección **Referencia rápida**.

    ![Configuración de Flock](./media/flock-tutorial/tutorial_flock_configure.png) 

1. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de Flock como administrador.

1. Seleccione la pestaña **Autenticación** en el panel de navegación izquierdo y, después, seleccione **Autenticación SAML**.

    ![Configuración de Flock](./media/flock-tutorial/configure1.png)

1. En la sección **Autenticación SAML** , realice los pasos siguientes:

    ![Configuración de Flock](./media/flock-tutorial/configure2.png)

    a. En el cuadro de texto **SAML 2.0 Endpoint (HTTP)** (Punto de conexión SAML 2.0 [HTTP]), pegue el valor de la **dirección URL del servicio de inicio de sesión único de SAML** que ha copiado de Azure Portal.

    b. En el cuadro de texto **Emisor de proveedor de identidades**, pegue el valor de **ID de entidad de SAML** que ha copiado de Azure Portal.

    c. Abra el **Certificado (Base64)** descargado de Azure Portal en el Bloc de notas y pegue el contenido en el cuadro de texto **Certificado público**.

    d. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/flock-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/flock-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/flock-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/flock-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="create-a-flock-test-user"></a>Creación de un usuario de prueba de Flock

Para permitir que los usuarios de Azure AD inicien sesión en Flock, tienen que aprovisionarse en Flock. En el caso de Flock, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en su sitio de la compañía de Flock como administrador.

1. Haga clic en **Administrar equipo** en el panel de navegación de la izquierda.

    ![Agregar empleado](./media/flock-tutorial/user1.png)

1. Haga clic en las pestaña **Agregar miembro** y, después, seleccione **Miembros del equipo**.

    ![Agregar empleado](./media/flock-tutorial/user2.png)

1. Escriba la dirección de correo electrónico del usuario en formato **Brittasimon@contoso.com** y después seleccione **Agregar usuarios**.

    ![Agregar empleado](./media/flock-tutorial/user3.png)

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Flock.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a Flock, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

1. En la lista de aplicaciones, seleccione **Flock**.

    ![Vínculo a Flock en la lista de aplicaciones](./media/flock-tutorial/tutorial_flock_app.png)

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Flock en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación Flock.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/flock-tutorial/tutorial_general_01.png
[2]: ./media/flock-tutorial/tutorial_general_02.png
[3]: ./media/flock-tutorial/tutorial_general_03.png
[4]: ./media/flock-tutorial/tutorial_general_04.png

[100]: ./media/flock-tutorial/tutorial_general_100.png

[200]: ./media/flock-tutorial/tutorial_general_200.png
[201]: ./media/flock-tutorial/tutorial_general_201.png
[202]: ./media/flock-tutorial/tutorial_general_202.png
[203]: ./media/flock-tutorial/tutorial_general_203.png
