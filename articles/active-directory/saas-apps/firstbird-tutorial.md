---
title: 'Tutorial: Integración de Azure Active Directory con Firstbird | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Firstbird.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: acab1200-32d3-4f4b-953f-f2a7e812b6a3
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e97a80e3f8c646c4bdfc3356db87939794eddc48
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56877782"
---
# <a name="tutorial-azure-active-directory-integration-with-firstbird"></a>Tutorial: Integración de Azure Active Directory con Firstbird

En este tutorial, aprenderá a integrar Firstbird con Azure Active Directory (Azure AD).
La integración de Firstbird con Azure AD proporciona las siguientes ventajas:

* En Azure AD se puede controlar quién tiene acceso a Firstbird.
* Puede permitir que los usuarios inicien sesión automáticamente en Firstbird (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Firstbird se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en Firstbird

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Firstbird admite SSO iniciado por **SP e IDP**
* Firstbird admite el aprovisionamiento de usuarios **Just-In-Time**

## <a name="adding-firstbird-from-the-gallery"></a>Adición de Firstbird desde la galería

Para configurar la integración de Firstbird en Azure AD, será preciso que agregue Firstbird desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Firstbird desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select_azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise_applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add_new_app.png)

4. En el cuadro de búsqueda, escriba **Firstbird**, seleccione **Firstbird** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Firstbird en la lista de resultados](common/search_new_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con [nombre de aplicación] utilizando usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de [nombre de aplicación].

Para configurar y probar el inicio de sesión único de Azure AD con [nombre de aplicación], es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Firstbird](#configure-firstbird-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Firstbird](#create-firstbird-test-user)**: para tener un homólogo de Britta Simon en Firstbird que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con [nombre de aplicación], siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Firstbird**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select_sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select_saml_option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit_urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Firstbird](common/idp_intiated.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<company-domain>.auth.1brd.com/saml/sp`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<company-domain>.auth.1brd.com/saml/callback`

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Firstbird](common/metadata_upload_additional_signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<company-domain>.1brd.com/login`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico del cliente de Firstbird](mailto:support@firstbird.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

6. La aplicación Firstbird espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit_attribute.png)

7. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:

    | NOMBRE | Atributo de origen|
    | ---------------| --------- |
    | first_name | `user.givenname` |
    | last_name | `user.surname` |
    | email | `user.mail` |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new_save_attribute.png)

    ![imagen](common/new_attribute_details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

8. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el archivo **XML de metadatos de federación**  y guárdelo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

### <a name="configure-firstbird-single-sign-on"></a>Configuración del inicio de sesión único en Firstbird

Una vez completados estos pasos, envíe el XML de metadatos de federación de Firstbird en una solicitud de soporte técnico a través de correo electrónico a [support@firstbird.com](mailto:support@firstbird.com) con el asunto: "Configuración de inicio de sesión único".

Luego, Firstbird almacenará la configuración en el sistema en consecuencia y activará SSO para su cuenta. Después, un miembro del personal de soporte técnico se pondrá en contacto con usted para comprobar la configuración.

> [!NOTE]
> Debe tener la opción de inicio de sesión único incluida en el contrato.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new_user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user_properties.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.

    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Firstbird.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **Firstbird**.

    ![Hoja Aplicaciones empresariales](common/enterprise_applications.png)

2. En la lista de aplicaciones, escriba y seleccione **Firstbird**.

    ![Enlace a Firstbird en la lista de aplicaciones](common/all_applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users_groups_blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add_assign_user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-firstbird-test-user"></a>Creación de un usuario de prueba en Firstbird

En esta sección se creará un usuario llamado Britta Simon en Firstbird. Firstbird admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si el usuario ya no existe en Firstbird, se crea uno nuevo cuando se intenta acceder a Firstbird.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Firstbird en el panel de acceso y debería iniciar sesión automáticamente en la versión de Firstbird para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
