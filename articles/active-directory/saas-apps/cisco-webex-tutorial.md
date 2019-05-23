---
title: 'Tutorial: Integración de Azure Active Directory con Cisco Webex Meetings | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Cisco Webex Meetings.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/01/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 308f745489fba2e2b539a2f2615b65228565dcf9
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65900114"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex-meetings"></a>Tutorial: Integración de Azure Active Directory con Cisco Webex Meetings

En este tutorial, aprenderá a integrar Cisco Webex Meetings con Azure Active Directory (Azure AD).
Integrar Cisco Webex Meetings con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Cisco Webex Meetings.
* Puede permitir que los usuarios inicien sesión automáticamente en Cisco Webex Meetings (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Cisco Webex Meetings, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en Cisco Webex Meetings

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Cisco Webex Meetings admite el inicio de sesión único iniciado por **SP**

* Cisco Webex Meetings admite el aprovisionamiento de usuarios **Just-In-Time**

## <a name="adding-cisco-webex-meetings-from-the-gallery"></a>Adición de Cisco Webex Meetings desde la galería

Para configurar la integración de Cisco Webex Meetings en Azure AD, será preciso que agregue Cisco Webex Meetings desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Cisco Webex Meetings desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Cisco Webex Meetings**, seleccione **Cisco Webex Meetings** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![Cisco Webex Meetings en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, va a configurar y probar el inicio de sesión único de Azure AD con Cisco Webex Meetings con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Cisco Webex Meetings.

Para configurar y probar el inicio de sesión único de Azure AD con Cisco Webex Meetings, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Cisco Webex Meetings](#configure-cisco-webex-meetings-single-sign-on)**: para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación del usuario de prueba de Cisco Webex Meetings](#create-cisco-webex-meetings-test-user)**: para tener un homólogo de Britta Simon en Cisco Webex Meetings vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Cisco Webex Meetings, siga estos pasos:

1. En la página de integración de la aplicación [Cisco Webex Meetings](https://portal.azure.com/) de **Azure Portal**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En Azure Portal, en la página **Set-up Single Sign-On with SAML** (Configurar inicio de sesión único con SAML), haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, cargue el **archivo de metadatos del proveedor de servicios** que descargó y configure la aplicación realizando estos pasos:

    >[!Note]
    >Obtendrá el archivo de metadatos del proveedor de servicios, como se explica más adelante en la sección **Configuración del inicio de sesión único de Cisco Webex Meetings** del tutorial. 

     a. Haga clic en **Cargar el archivo de metadatos**.

    ![Carga del archivo de metadatos](common/upload-metadata.png)

    b. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.

    ![Elección del archivo de metadatos](common/browse-upload-metadata.png)

    c. Cuando el archivo de metadatos del proveedor de servicios se haya cargado correctamente, los valores **Identificador** y **URL de respuesta** se rellenan automáticamente en la sección **Configuración básica de SAML**:

    ![Información de inicio de sesión único de dominio y direcciones URL de Cisco Webex Meetings](common/sp-identifier-reply.png)

    En el cuadro de texto **URL de inicio de sesión**, pegue el valor de **URL de respuesta** que se rellena automáticamente mediante la carga de archivo de metadatos de SP.

5. La aplicación Cisco Webex Meetings espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados. Haga clic en el icono  **Editar**  para agregar los atributos.

    ![imagen](common/edit-attribute.png)

6. Elimine los atributos predeterminados de la sección **Notificaciones de usuario**. La aplicación Cisco Webex Meetings espera que se usen algunos atributos más en la respuesta de SAML. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, realice los siguientes pasos para agregar el atributo Token SAML como se muestra en la tabla siguientes:
    
    | NOMBRE | Atributo de origen|
    | ---------------|  --------- |
    |   firstname    | user.givenname |
    |   lastname    | user.surname |
    |   email       | user.mail |
    |   uid    | user.mail |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](./media/cisco-webex-tutorial/tutorial-cisco-webex-addnewclaim.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

8. En la sección **Set up Cisco Webex Meetings** (Configurar Cisco Webex Meetings), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-cisco-webex-meetings-single-sign-on"></a>Configuración del inicio de sesión único de Cisco Webex Meetings

1. Acceda a [Cisco Cloud Collaboration Management](https://www.webex.com/go/connectadmin) (Administración de colaboración en la nube de Cisco) con sus credenciales de administración.

2. Vaya a **Security Settings** (Configuración de seguridad) y desplácese hasta **Federated Web SSO Configuration** (Configuración de SSO web federado).
 
    ![Configurar inicio de sesión único](./media/cisco-webex-tutorial/tutorial-cisco-webex-10.png)

3. En **Federated Web SSO Configuration** (Configuración de SSO web federado), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/cisco-webex-tutorial/tutorial-cisco-webex-11.png)

     a. En el cuadro de texto Federation Protocol (Protocolo de federación), escriba el nombre del protocolo.

    b. Haga clic en el enlace **Import SAML Metadata** (Importar metadatos de SAML) para cargar el archivo de metadatos que descargó de Azure Portal.

    c. Haga clic en el botón **Export** (Exportar) para descargar el archivo de metadatos del proveedor de servicios y cárguelo en la sección **Configuración básica de SAML** de Azure Portal.

    d. En el cuadro de texto **AuthContextClassRef**, escriba `urn:oasis:names:tc:SAML:2.0:ac:classes:unspecified` y, si desea habilitar la autenticación multifactor con Azure AD, escriba los dos valores, como `urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport;urn:oasis:names:tc:SAML:2.0:ac:classes:X509`.

    e. Seleccione **Auto Account Creation** (Creación de cuenta automática).

    >[!NOTE]
    >Para habilitar el aprovisionamiento de usuarios **Just-In-Time**, debe seleccionar **Auto Account Creation** (Creación de cuenta automática). Además, es necesario pasar atributos de token de SAML en la respuesta SAML.

    f. Haga clic en **Save**(Guardar). 

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Cisco Webex Meetings.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Cisco Webex Meetings**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Cisco Webex Meetings**.

    ![Vínculo a Cisco Webex Meetings en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-cisco-webex-meetings-test-user"></a>Creación del usuario de prueba de Cisco Webex Meetings

El objetivo de esta sección es crear un usuario llamado Britta Simon en Cisco Webex Meetings. Cisco Webex Meetings admite el aprovisionamiento **Just-In-Time**, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si el usuario no existe en Cisco Webex Meetings, se crea uno al intentar acceder.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Cisco Webex Meetings en el panel de acceso, debería iniciar sesión automáticamente en la versión de Cisco Webex Meetings para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

