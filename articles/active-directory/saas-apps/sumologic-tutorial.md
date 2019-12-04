---
title: 'Tutorial: Integración de Azure Active Directory con SumoLogic | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SumoLogic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 7548d7d7b808472b3f5446fadfe800584f61b1df
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74233350"
---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Tutorial: Integración de Azure Active Directory con SumoLogic

En este tutorial, aprenderá cómo integrar SumoLogic con Azure Active Directory (Azure AD).
La integración de SumoLogic con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a SumoLogic.
* Puede permitir que los usuarios inicien sesión automáticamente en SumoLogic (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con SumoLogic, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción habilitada para el inicio de sesión único en SumoLogic

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* SumoLogic admite el inicio de sesión único iniciado por **SP**.

## <a name="adding-sumologic-from-the-gallery"></a>Agregación de SumoLogic desde la galería

Para configurar la integración de SumoLogic en Azure AD, deberá agregar SumoLogic desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar SumoLogic desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **SumoLogic**, seleccione **SumoLogic** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![SumoLogic en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con SumoLogic con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de SumoLogic.

Para configurar y probar el inicio de sesión único de Azure AD con SumoLogic, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de SumoLogic](#configure-sumologic-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba para SumoLogic](#create-sumologic-test-user)** : para tener un homólogo de Britta Simon en SumoLogic que esté vinculado a la representación de usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con SumoLogic, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones **SumoLogic**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de SumoLogic](common/sp-identifier.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<tenantname>.SumoLogic.com`

   b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón:

    | |
    |--|
    | `https://<tenantname>.us2.sumologic.com` |
    | `https://<tenantname>.sumologic.com` |
    | `https://<tenantname>.us4.sumologic.com` |
    | `https://<tenantname>.eu.sumologic.com` |
    | `https://<tenantname>.au.sumologic.com` |

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte al cliente de SumoLogic](https://www.sumologic.com/contact-us/) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up SumoLogic** (Configurar SumoLogic), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-sumologic-single-sign-on"></a>Configuración de inicio de sesión único de SumoLogic

1. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de SumoLogic como administrador.

1. Vaya a **Administrar \> Seguridad**.

    ![Administración](./media/sumologic-tutorial/ic778556.png "administración")

1. Haga clic en **SAML**.

    ![Configuración de seguridad global](./media/sumologic-tutorial/ic778557.png "Configuración de seguridad global")

1. En la lista **Seleccionar una configuración o crear una nueva**, seleccione **Azure AD** y, después, haga clic en **Configurar**.

    ![Configurar SAML 2.0](./media/sumologic-tutorial/ic778558.png "Configurar SAML 2.0")

1. En el cuadro de diálogo **Configurar SAML 2.0** , realice los pasos siguientes:

    ![Configurar SAML 2.0](./media/sumologic-tutorial/ic778559.png "Configurar SAML 2.0")

    a. En el cuadro de texto **Nombre de configuración**, escriba **Azure AD**.

    b. Seleccione **Modo de depuración**.

    c. En el cuadro de texto **Issuer** (Emisor), pegue el valor de **Identificador Azure AD** que ha copiado de Azure Portal.

    d. En el cuadro de texto **Authentication request URL** (Dirección URL de solicitud de autenticación), pegue el valor del campo **Dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    e. Abra el certificado codificado en base 64 en el Bloc de notas, copie su contenido en el Portapapeles y luego pegue todo el certificado en el cuadro de texto **Certificado X.509** .

    f. Como **Atributo de correo electrónico**, seleccione **Usar SAML Subject**.  

    g. Seleccione **Configuración de inicio de sesión iniciada por el SP**.

    h. En el cuadro de texto de la **ruta de acceso de inicio de sesión**, escriba **Azure** y haga clic en **Guardar**.

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a SumoLogic.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **SumoLogic**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **SumoLogic**.

    ![Vínculo a SumoLogic en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-sumologic-test-user"></a>Creación de un usuario de prueba de SumoLogic

Para permitir que los usuarios de Azure AD inicien sesión en SumoLogic, deben aprovisionarse a SumoLogic. En el caso de SumoLogic, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en su inquilino de **SumoLogic**.

1. Vaya a **Administrar \> usuarios**.

    ![Usuarios](./media/sumologic-tutorial/ic778561.png "Usuarios")

1. Haga clic en **Agregar**.

    ![Usuarios](./media/sumologic-tutorial/ic778562.png "Usuarios")

1. En el cuadro de diálogo **Nuevo usuario** , realice los pasos siguientes:

    ![New User](./media/sumologic-tutorial/ic778563.png "Nuevo usuario") (Usuario nuevo) 

    a. Escriba la información relacionada de la cuenta de Azure AD que quiere aprovisionar en los cuadros de texto **First Name** (Nombre), **Last Name** (Apellidos) y **Email** (Correo electrónico).
  
    b. Seleccione un rol.
  
    c. Como **Estado**, seleccione **Activo**.
  
    d. Haga clic en **Save**(Guardar).

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de SumoLogic ofrecida por SumoLogic para aprovisionar cuentas de usuario de Azure AD.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de SumoLogic en el panel de acceso, debería iniciar sesión automáticamente en la versión de SumoLogic para la que configurara el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

