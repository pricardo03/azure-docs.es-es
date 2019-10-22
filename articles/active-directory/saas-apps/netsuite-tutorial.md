---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con NetSuite | Microsoft Docs'
description: Más información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y NetSuite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a9b834feacd6241e66e18a4f4e6aadc43e909c7
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72438501"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-netsuite"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con NetSuite

En este tutorial, aprenderá a integrar NetSuite con Azure Active Directory (Azure AD). Al integrar NetSuite con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a NetSuite.
* Permitir que los usuarios puedan iniciar sesión automáticamente en NetSuite con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en NetSuite.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* NetSuite admite SSO iniciado por **IDP**

* NetSuite admite el aprovisionamiento de usuarios **Just-In-Time**

* NetSuite admite el [aprovisionamiento automático de usuarios](NetSuite-provisioning-tutorial.md)

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="adding-netsuite-from-the-gallery"></a>Adición de NetSuite desde la galería

Para configurar la integración de NetSuite en Azure AD, deberá agregar NetSuite desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **NetSuite** en el cuadro de búsqueda.
1. Seleccione **NetSuite** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-netsuite"></a>Configuración y prueba del inicio de sesión único de Azure AD para NetSuite

Configure y pruebe el inicio de sesión único de Azure AD con NetSuite mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de NetSuite.

Para configurar y probar el inicio de sesión único de Azure AD con NetSuite, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en NetSuite](#configure-netsuite-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de NetSuite](#create-netsuite-test-user)** , para tener un homólogo de B.Simon en NetSuite vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **NetSuite**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, especifique los valores de los siguientes campos:

    En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón:

    | |
    |--|
    | `https://<tenant-name>.NetSuite.com/saml2/acs` |
    | `https://<tenant-name>.na1.NetSuite.com/saml2/acs` |
    | `https://<tenant-name>.na2.NetSuite.com/saml2/acs` |
    | `https://<tenant-name>.sandbox.NetSuite.com/saml2/acs` |
    | `https://<tenant-name>.na1.sandbox.NetSuite.com/saml2/acs` |
    | `https://<tenant-name>.na2.sandbox.NetSuite.com/saml2/acs` |

    > [!NOTE]
    > Este valor no es real. Actualícelo con la dirección URL de respuesta real. Póngase en contacto con el [equipo de atención al cliente de NetSuite](http://www.netsuite.com/portal/services/support-services/suitesupport.shtml) para obtener este valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. La aplicación NetSuite espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados. Haga clic en el icono **Editar** para abrir el cuadro de diálogo Atributos de usuario.

    ![imagen](common/edit-attribute.png)

1. Además de lo anterior, la aplicación NetSuite espera que se usen algunos atributos más en la respuesta de SAML. En la sección Notificaciones del usuario del cuadro de diálogo Atributos de usuario, haga lo siguiente para agregar el atributo Token SAML como se muestra en la tabla siguiente:

    | NOMBRE | Atributo de origen | 
    | ---------------| --------------- |
    | account  | `account id` |

    1. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    1. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    1. Deje **Espacio de nombres** en blanco.

    1. Seleccione **Atributo** como origen.

    1. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    1. Haga clic en **Aceptar**.

    1. Haga clic en **Save**(Guardar).

    >[!NOTE]
    >El valor del atributo account no es real. Este valor se actualizará como se explica más adelante en el tutorial.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Configurar NetSuite**, copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a permitir que B.Simon acceda a NetSuite mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **NetSuite**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-netsuite-sso"></a>Configuración del inicio de sesión único de NetSuite

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

### <a name="create-netsuite-test-user"></a>Creación de un usuario de prueba de NetSuite

En esta sección se creará un usuario llamado Britta Simon en NetSuite. NetSuite admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario deja de existir en NetSuite, se crea uno nuevo después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de NetSuite en el panel de acceso y debería iniciar sesión automáticamente en la versión de NetSuite para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Pruebe NetSuite con Azure AD](https://aad.portal.azure.com/)

