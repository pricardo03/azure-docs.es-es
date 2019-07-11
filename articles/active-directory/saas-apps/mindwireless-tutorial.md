---
title: 'Tutorial: Integración de Azure Active Directory con mindWireless | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y mindWireless.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bd00a339-27c9-4904-b66f-a95bf597ac3c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/25/2019
ms.author: jeedes
ms.openlocfilehash: 2c320944e952dbea74c41ffd3471143f6713585b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67097368"
---
# <a name="tutorial-azure-active-directory-integration-with-mindwireless"></a>Tutorial: Integración de Azure Active Directory con mindWireless

En este tutorial, aprenderá a integrar mindWireless con Azure Active Directory (Azure AD).
La integración de mindWireless con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a mindWireless.
* Puede permitir que los usuarios inicien sesión automáticamente en mindWireless (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con mindWireless se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en mindWireless

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* mindWireless admite el inicio de sesión único iniciado por **IDP**

## <a name="adding-mindwireless-from-the-gallery"></a>Adición de mindWireless desde la galería

Para configurar la integración de mindWireless en Azure AD, deberá agregar mindWireless desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar mindWireless desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **mindWireless**, seleccione **mindWireless** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![mindWireless en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con mindWireless con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de mindWireless.

Para configurar y probar el inicio de sesión único de Azure AD con mindWireless, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de mindWireless](#configure-mindwireless-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de mindWireless](#create-mindwireless-test-user)** : para tener un homólogo de Britta Simon en mindWireless que esté vinculado a su representación en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con mindWireless, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **mindWireless**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la página **Configurar inicio de sesión único con SAML** realice los siguientes pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de mindWireless](common/idp-intiated.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<subdomain>.mwsmart.com/`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.mwsmart.com/SAML/AssertionConsumerService.aspx`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con el identificador y la URL de respuesta reales. Póngase en contacto con el [equipo de soporte técnico al cliente de mindWireless](mailto:sdulloor@mindwireless.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. La aplicación mindWireless espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos de token de SAML. La siguiente captura de muestra la lista de atributos predeterminados. El nombre de la notificación siempre será **Employee ID**, cuyo valor hemos asignado a **user.employeeid**, que contiene el valor de EmployeeID del usuario. Aquí se realiza la asignación de usuario de Azure AD a mindWireless en el valor EmployeeID, pero puede asignarle un valor diferente que también se base en la configuración de la aplicación. Puede trabajar primero con el [ de soporte técnico de mindWireless](mailto:sdulloor@mindwireless.com) para usar el identificador correcto de un usuario y asignar ese valor con la notificación **Employee ID**.

    ![imagen](common/edit-attribute.png)

6. Además de lo anterior, la aplicación mindWireless espera que se usen algunos atributos más en la respuesta de SAML. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, realice los siguientes pasos para agregar el atributo Token SAML como se muestra en la tabla siguientes:

    | NOMBRE | Espacio de nombres  |  Atributo de origen|
    | -------------- | --------------- | ----------------|
    | Id. de empleado | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`| user.employeeid |

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. En el cuadro de texto **Espacio de nombres**, escriba el espacio de nombres de atributo que se muestra para esa fila.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

8. En la sección **Set up mindWireless** (Configurar mindWireless), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-mindwireless-single-sign-on"></a>Configuración del inicio de sesión único de mindWireless

Para configurar el inicio de sesión único en **mindWireless**, es preciso enviar el **certificado (Base64)** descargado y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de mindWireless](mailto:sdulloor@mindwireless.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a mindWireless.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **mindWireless**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **mindWireless**.

    ![El vínculo de mindWireless en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-mindwireless-test-user"></a>Creación de un usuario de prueba de mindWireless

En esta sección, creará un usuario llamado Britta Simon en mindWireless. Trabaje con el  [equipo de soporte técnico de mindWireless](mailto:sdulloor@mindwireless.com) para agregar los usuarios en la plataforma de mindWireless. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de mindWireless en el panel de acceso, debería iniciar sesión automáticamente en la versión de mindWireless para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

