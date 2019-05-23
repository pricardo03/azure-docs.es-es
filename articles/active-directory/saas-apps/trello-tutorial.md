---
title: 'Tutorial: Integración de Azure Active Directory con Trello | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Trello.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: cd5ae365-9ed6-43a6-920b-f7814b993949
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 453827b42d12333fd4f27761e7f73484fb749532
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "65865786"
---
# <a name="tutorial-azure-active-directory-integration-with-trello"></a>Tutorial: Integración de Azure Active Directory con Trello

En este tutorial, obtendrá información sobre cómo integrar Trello con Azure Active Directory (Azure AD).
Integrar Trello con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Trello.
* Puede permitir que los usuarios inicien sesión automáticamente en Trello (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Trello, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una evaluación gratuita de un mes](https://azure.microsoft.com/pricing/free-trial/).
* Una suscripción habilitada para inicio de sesión único en Trello.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Trello admite SSO iniciado por SP e IDP

* Trello admite el aprovisionamiento de usuarios Just-In-Time

## <a name="add-trello-from-the-gallery"></a>Incorporación de Trello desde la galería

Para configurar la integración de Trello en Azure AD, antes agregue Trello desde la galería a la lista de aplicaciones SaaS administradas.

Para agregar Trello desde la galería, siga estos pasos:

1. En el panel izquierdo de [Azure Portal](https://portal.azure.com), seleccione el icono **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Seleccione **Aplicaciones empresariales** y, después, **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, seleccione el botón **Nueva aplicación** en la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Trello**y, después, seleccione **Trello** en el panel de resultados.

5. Seleccione el botón **Agregar** para agregar la aplicación.

     ![Trello en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Trello con un usuario de prueba llamado **Britta Simon**.

Para que el inicio de sesión único funcione, es preciso establecer un vínculo entre un usuario de Azure AD y el usuario relacionado de Trello.

Para configurar y probar el inicio de sesión único de Azure AD con Trello, es preciso completar los siguientes bloques de creación:

1. [Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on), para permitir que los usuarios utilicen esta característica.
2. [Configuración del inicio de sesión único de Trello](#configure-trello-single-sign-on): para configurar los valores de inicio de sesión único en la aplicación.
3. [Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user), para probar el inicio de sesión único de Azure AD con Britta Simon.
4. [Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user), para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. [Creación de usuario de prueba de Trello](#create-a-trello-test-user): para tener un homólogo de Britta Simon en Trello que esté vinculado a la representación de ella en Azure AD.
6. [Prueba del inicio de sesión único](#test-single-sign-on) para comprobar que la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

> [!NOTE]
> Debe obtener el campo de datos dinámico **\<enterprise\>** desde Trello. Si no tiene el valor del campo de datos dinámico, póngase en contacto con el [equipo de soporte técnico de Trello](mailto:support@trello.com) para obtenerlo.

Para configurar el inicio de sesión único de Azure AD con Trello, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Trello**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione **SAML** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, seleccione el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por IDP, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Trello](common/idp-intiated.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://trello.com/auth/saml/metadata`

    b. En el cuadro de texto **URL de respuesta** , escriba una dirección URL con el siguiente patrón: `https://trello.com/auth/saml/consume/<enterprise>`

5. Seleccione **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por SP:

    ![Información de dominio y direcciones URL de inicio de sesión único de Trello](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **Dirección URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://trello.com/auth/saml/login/<enterprise>`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con el identificador, la URL de respuesta y la dirección URL de inicio de sesión reales. Póngase en contacto con el [equipo de soporte técnico para clientes de Trello](mailto:support@trello.com) para obtener estos valores. También puede hacer referencia a los patrones de la sección **Configuración básica de SAML** de Azure Portal.

6. La aplicación Trello espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, seleccione el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![Cuadro de diálogo Atributos de usuario](common/edit-attribute.png)

7. En la sección **Notificaciones de usuario** del cuadro de diálogo **Atributos de usuario**, configure el atributo token SAML como muestra la imagen anterior. A continuación, siga estos pasos:

    | NOMBRE |  Atributo de origen|
    | --- | --- |
    | User.Email | user.mail |
    | User.FirstName | user.givenname |
    | User.LastName | user.surname |

     a. Seleccione **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![Cuadro de diálogo Notificaciones del usuario](common/new-save-attribute.png)

    ![Administrar las notificaciones del usuario](common/new-attribute-details.png)

    b. En el cuadro **Nombre**, escriba el nombre del atributo que se muestra para esa fila.

    c. Deje **Espacio de nombres** en blanco.

    d. En **Origen**, seleccione **Atributo**.

    e. En la lista **Atributo de origen**, escriba el valor del atributo que se muestra para esa fila.

    f. Seleccione **Aceptar**.

    g. Seleccione **Guardar**.

8. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, seleccione **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos. Luego, guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

9. En la sección **Set up Trello** (Configurar Trello), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-trello-single-sign-on"></a>Configuración del inicio de sesión único en Trello

Para configurar el inicio de sesión único en Trello, antes envíe el **certificado (Base64)** descargado y las direcciones URL copiadas de Azure Portal al [equipo de soporte técnico de Trello](mailto:support@trello.com). Estas garantizan que la conexión de inicio de sesión único de SAML está establecida correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En el cuadro de diálogo **Usuario**, siga los pasos que se indican a continuación.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba "brittasimon@yourcompanydomain.extension". Por ejemplo, en este caso, podría escribir "BrittaSimon@contoso.com".

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Seleccione **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Trello.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales**, **Todas las aplicaciones**, **Trello**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Trello**.

    ![Vínculo a Trello en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Seleccione el botón **Agregar usuario**. Después, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon**. A continuación, haga clic en el botón **Seleccionar** situado en la parte inferior de la pantalla.

6. Si espera algún valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione el rol adecuado para el usuario en la lista. A continuación, haga clic en el botón **Seleccionar** situado en la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="create-a-trello-test-user"></a>Creación de usuario de prueba de Trello

En esta sección, creará un usuario llamado Britta Simon en Trello. Trello admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario deja de existir en Trello, se crea uno después de la autenticación.

> [!NOTE]
> Si necesita crear un usuario manualmente, póngase en contacto con el  [equipo de soporte técnico de Trello](mailto:support@trello.com).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el portal MyApps.

Al seleccionar el icono de Trello en el portal MyApps, debería iniciar automáticamente sesión en Trello. Para más información acerca del portal MyApps, consulte [¿Qué es el portal MyApps?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

