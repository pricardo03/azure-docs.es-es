---
title: 'Tutorial: Integración de Azure Active Directory con NetSuite | Microsoft Docs'
description: Más información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y NetSuite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/17/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ab329574ef425e8133ac746c185050efcc8bc15a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "65870939"
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Tutorial: Integración de Azure Active Directory con NetSuite

En este tutorial, aprenderá a integrar NetSuite con Azure Active Directory (Azure AD).
La integración de NetSuite con Azure AD proporciona las siguientes ventajas:

* En Azure AD puede controlar quién tiene acceso a NetSuite.
* Puede permitir que los usuarios inicien sesión automáticamente en NetSuite (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con NetSuite, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en NetSuite

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* NetSuite admite SSO iniciado por **IDP**
* NetSuite admite el aprovisionamiento de usuarios **Just-In-Time**
* NetSuite admite el [aprovisionamiento automático de usuarios](NetSuite-provisioning-tutorial.md)

## <a name="adding-netsuite-from-the-gallery"></a>Adición de NetSuite desde la galería

Para configurar la integración de NetSuite en Azure AD, deberá agregar NetSuite desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar NetSuite desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **NetSuite**, seleccione **NetSuite** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![NetSuite en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con NetSuite con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de NetSuite.

Para configurar y probar el inicio de sesión único de Azure AD con NetSuite, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de NetSuite](#configure-netsuite-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de NetSuite](#create-netsuite-test-user)**: para tener un homólogo de Britta Simon en NetSuite vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con NetSuite, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **NetSuite**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de NetSuite](common/idp-reply.png)

    En el cuadro de texto **URL de respuesta** , escriba una dirección URL con el siguiente patrón:

    `https://<tenant-name>.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.NetSuite.com/saml2/acs`

    `https://<tenant-name>.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.sandbox.NetSuite.com/saml2/acs`

    > [!NOTE]
    > Este valor no es real. Actualícelo con la dirección URL de respuesta real. Póngase en contacto con el [equipo de atención al cliente de NetSuite](http://www.netsuite.com/portal/services/support-services/suitesupport.shtml) para obtener este valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. La aplicación NetSuite espera las instrucciones de aserción de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, edite las notificaciones mediante el **icono Editar** o agregue notificaciones mediante **Agregar nueva notificación** para configurar el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:
    
    | NOMBRE | Atributo de origen | 
    | ---------------| --------------- |
    | account  | `account id` |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

    >[!NOTE]
    >El valor del atributo account no es real. Este valor se actualizará como se explica más adelante en el tutorial.

4. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar **XML de metadatos** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Set up Folloze** (Configurar NetSuite), copie las direcciones URL adecuada según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-netsuite-single-sign-on"></a>Configuración de inicio de sesión único de NetSuite

1. Abra una nueva pestaña en el explorador e inicie sesión en el sitio de la empresa NetSuite como administrador.

2. En la barra de herramientas de la parte superior de la página, haga clic en **Setup** (Configuración), vaya a **Company** (Empresa) y haga clic en **Enable Features** (Habilitar características).

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-setupsaml.png)

3. En la barra de herramientas de la parte central de la página, haga clic en **SuiteCloud**.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-suitecloud.png)

4. En la sección **Manage Authentication** (Administrar autenticación=, seleccione **SAML SINGLE SIGN-ON** (INICIO DE SESIÓN ÚNICO DE SAMLS) para habilitar la opción de inicio de sesión único de SAML en NetSuite.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-ticksaml.png)

5. En la barra de herramientas de la parte superior de la página, haga clic en **Setup** (Configuración).

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-setup.png)

6. En la lista **SETUP TASKS** (TAREAS DE CONFIGURACIÓN), haga clic en **Integration** (Integración).

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-integration.png)

7. En la sección **MANAGE AUTHENTICATION** (ADMINISTRAR AUTENTICACIÓN), haga clic en **SAML Single Sign-on** (Inicio de sesión único de SAML).

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-saml.png)

8. En la página **SAML Setup** (Configuración de SAML), en la sección **NetSuite Configuration** (Configuración de NetSuite), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-saml-setup.png)
  
     a. Seleccione **PRIMARY AUTHENTICATION METHOD** (MÉTODO DE AUTENTICACIÓN PRINCIPAL).

    b. En el campo con la etiqueta **SAMLV2 IDENTITY PROVIDER METADATA** (METADATOS DEL PROVEEDOR DE IDENTIDADES DE SAMLV2), seleccione **UPLOAD IDP METADATA FILE** (CARGAR ARCHIVO DE METADATOS DE IDP). A continuación, haga clic en **Examinar** para cargar el archivo de metadatos que descargó de Azure Portal.

    c. Haga clic en **Enviar**.

9. En NetSuite, haga clic en **Setup** (Configuración), vaya a **Company** (Empresa) y haga clic en **Company Information** (Información de empresa) en el menú de navegación superior.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-com.png)

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-account-id.png)

    b. En la página **Company Information** (Información de empresa), en la columna de la derecha, copie el **identificador de la cuenta**.

    c. Pegue el **identificador de la cuenta** que ha copiado de la cuenta de NetSuite en el campo **Valor de atributo** de Azure AD. 

10. Antes de que los usuarios puedan realizar el inicio de sesión único en NetSuite, se les deben asignar primero los permisos adecuados en NetSuite. Siga las instrucciones siguientes para asignar estos permisos.

     a. En el menú de navegación superior, haga clic en **Setup** (Configuración).

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-setup.png)

    b. En el menú de navegación izquierdo, seleccione **Usuarios/Roles** y haga clic en **Administrar roles**.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-manage-roles.png)

    c. Haga clic en **Nuevo rol**.

    d. Escriba un **Nombre** para el nuevo rol.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-new-role.png)

    e. Haga clic en **Save**(Guardar).

    f. En el menú de la parte superior, haga clic en **Permisos**. A continuación, haga clic en **Configuración**.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-sso.png)

    g. Seleccione **SAML Single Sign-on** (Inicio de sesión único de SAML) y haga clic en **Add** (Agregar).

    h. Haga clic en **Save**(Guardar).

    i. En el menú de navegación superior, haga clic en **Configuración** y en **Administrador de instalación**.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-setup.png)

    j. En el menú de navegación izquierdo, seleccione **Usuarios/Roles** y haga clic en **Administrar usuarios**.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-manage-users.png)

    k. Seleccione un usuario de prueba. A continuación, haga clic en **Edit** (Editar) y, a continuación, vaya a la pestaña **Access** (Acceso).

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-edit-user.png)

    l. En el cuadro de diálogo de roles, asigne el rol adecuado que haya creado.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-add-role.png)

    m. Haga clic en **Save**(Guardar).

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a NetSuite.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **NetSuite**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **NetSuite**.

    ![Vínculo a NetSuite en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-netsuite-test-user"></a>Creación de un usuario de prueba de NetSuite

En esta sección se creará un usuario llamado Britta Simon en NetSuite. NetSuite admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario deja de existir en NetSuite, se crea uno nuevo después de la autenticación.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de NetSuite en el panel de acceso y debería iniciar sesión automáticamente en la versión de NetSuite para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Configuración del aprovisionamiento de usuarios](NetSuite-provisioning-tutorial.md)

