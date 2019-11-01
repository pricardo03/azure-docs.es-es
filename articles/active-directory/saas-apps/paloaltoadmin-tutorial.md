---
title: 'Tutorial: Integración de Azure Active Directory con Palo Alto Networks - Admin UI | Microsoft Docs'
description: 'Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Palo Alto Networks: interfaz de usuario de administración.'
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: a826eaec-15af-4c85-8855-8a3374d1efb9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c77678c20cb9eed01e2cfa86014963f453a4839
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73160188"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---admin-ui"></a>Tutorial: Integración de Azure Active Directory con Palo Alto Networks - Admin UI

En este tutorial, obtendrá información sobre cómo integrar Palo Alto Networks: interfaz de usuario de administración con Azure Active Directory (Azure AD).
La integración de Palo Alto Networks: interfaz de usuario de administración con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Palo Alto Networks: interfaz de usuario de administración.
* Puede permitir que los usuarios inicien sesión automáticamente en Palo Alto Networks - Admin UI con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Palo Alto Networks: interfaz de usuario de administración, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción con inicio de sesión único de Palo Alto Networks - Admin UI

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Palo Alto Networks - Admin UI admite el inicio de sesión único iniciado por **SP**.
* Palo Alto Networks - Admin UI admite el aprovisionamiento de usuarios **Just-In-Time**.

## <a name="adding-palo-alto-networks---admin-ui-from-the-gallery"></a>Agregación de Palo Alto Networks: interfaz de usuario de administración

Para configurar la integración de Palo Alto Networks: interfaz de usuario de administración en Azure AD, es preciso agregar Palo Alto Networks: interfaz de usuario de administración desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Palo Alto Networks: interfaz de usuario de administración desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Palo Alto Networks: interfaz de usuario de administración**, seleccione **Palo Alto Networks: interfaz de usuario de administración** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Palo Alto Networks: interfaz de usuario de administración en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Palo Alto Networks - Admin UI con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Palo Alto Networks - Admin UI.

Para configurar y probar el inicio de sesión único de Azure AD con Palo Alto Networks: interfaz de usuario de administración, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Palo Alto Networks - Admin UI](#configure-palo-alto-networks---admin-ui-single-sign-on)** : permite configurar las opciones de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Palo Alto Networks - Admin UI](#create-palo-alto-networks---admin-ui-test-user)** : para tener un homólogo de Britta Simon en Palo Alto Networks - Admin UI que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Palo Alto Networks - Admin UI, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración aplicaciones de **Palo Alto Networks - Admin UI**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de inicio de sesión único con Dominio y direcciones URL de Palo Alto Networks: interfaz de usuario de administración](common/sp-identifier-reply.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<Customer Firewall FQDN>/php/login.php`

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<Customer Firewall FQDN>:443/SAML20/SP`

    c. En el cuadro de texto **Dirección URL de respuesta**, escriba la dirección URL del Servicio de consumidor de aserciones (ACS) con el siguiente formato: `https://<Customer Firewall FQDN>:443/SAML20/SP/ACS`.

    > [!NOTE]
    > Estos valores no son reales. Actualícelos con la dirección URL de inicio de sesión, el identificador y la dirección URL de respuesta reales. Póngase en contacto con [el equipo de soporte técnico del cliente de Palo Alto Networks: interfaz de usuario de administración](https://support.paloaltonetworks.com/support) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. La aplicación Palo Alto Networks: interfaz de usuario de administración espera las aserciones de SAML en un formato concreto. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

   > [!NOTE]
   > Como los valores de atributo son solo ejemplos, asigne los valores adecuados para *username* y *adminrole*. Hay otro atributo opcional, *accessdomain*, que se usa para restringir el acceso de administrador a sistemas virtuales específicos en el firewall.
   >

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:

    | NOMBRE |  Atributo de origen|
    | --- | --- |
    | username | user.userprincipalname |
    | adminrole | customadmin |
    | | |

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

    > [!NOTE]
    > Para más información sobre los atributos, consulte los siguientes artículos:
    > * [Perfil de rol administrativo de Admin UI (adminrole)](https://www.paloaltonetworks.com/documentation/80/pan-os/pan-os/firewall-administration/manage-firewall-administrators/configure-an-admin-role-profile)
    > * [Dominio de acceso de dispositivo de Admin UI (accessdomain)](https://www.paloaltonetworks.com/documentation/80/pan-os/web-interface-help/device/device-access-domain)

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

8. En la sección **Configurar Palo Alto Networks - Admin UI**, copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-palo-alto-networks---admin-ui-single-sign-on"></a>Configuración del inicio de sesión único de Palo Alto Networks - Admin UI

1. Abra la interfaz de usuario de administración del firewall de Palo Alto Networks como administrador en una nueva ventana.

2. Seleccione la pestaña **Device** (Dispositivo).

    ![Pestaña Device (Dispositivo)](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin1.png)

3. En el panel izquierdo, seleccione **SAML Identity Provider** (Proveedor de identidades de SAML) y, luego, seleccione **Import** (Importar) para importar el archivo de metadatos.

    ![Botón para importar archivo de metadatos](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin2.png)

4. En la ventana **SAML Identify Provider Server Profile Import** (Importación del perfil de servidor del proveedor de identidades de SAML), haga lo siguiente:

    ![Ventana de importación del perfil de servidor del proveedor de identidades de SAML](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp.png)

    a. En el cuadro **Profile Name** (Nombre del perfil), proporcione un nombre (por ejemplo, **AzureAD Admin UI**).
    
    b. En **Identity Provider Metadata** (Metadatos del proveedor de identidades), seleccione **Browse** (Examinar) y elija el archivo metadata.xml que descargó anteriormente de Azure Portal.
    
    c. Desactive la casilla **Validate Identity Provider Certificate** (Validar certificado de proveedor de identidades).
    
    d. Seleccione **Aceptar**.
    
    e. Para confirmar las configuraciones en el firewall, seleccione **Commit** (Confirmar).

5. En el panel izquierdo, seleccione **SAML Identity Provider** (Proveedor de identidades de SAML) y elija el perfil del proveedor de identidades de SAML (por ejemplo, **AzureAD Admin UI**) que creó en el paso anterior.

    ![Perfil del proveedor de identidades de SAML](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp_select.png)

6. En la ventana **SAML Identity Provider Server Profile** (Perfil de servidor del proveedor de identidades de SAML), realice lo siguiente:

    ![Ventana de perfil de servidor del proveedor de identidades de SAML](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_slo.png)
  
    a. En el cuadro **Identity Provider SLO URL** (Dirección URL de SLO del proveedor de identidades), sustituya la dirección URL de SLO importada anteriormente y agregue la siguiente dirección URL: `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0`.
  
    b. Seleccione **Aceptar**.

7. En la interfaz de usuario del firewall de Palo Alto Networks, seleccione **Device** (Dispositivo) y elija **Admin Roles** (Roles de administrador).

8. Seleccione el botón **Agregar**.

9. En la ventana **Admin Role Profile** (Perfil de rol de administrador), en el cuadro **Name** (Name), proporcione un nombre para el rol de administrador (por ejemplo, **fwadmin**). El nombre del rol de administrador debe coincidir con el nombre de atributo del rol de administrador de SAML enviado por el proveedor de identidades. El nombre de rol de administrador y el valor que se crearon en la sección **Atributos de usuario** de Azure Portal.

    ![Configuración del rol de administrador de Palo Alto Networks](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_adminrole.png)
  
10. En la interfaz de usuario de administrador del firewall, seleccione **Device** (Dispositivo) y elija **Authentication Profile** (Perfil de autenticación).

11. Seleccione el botón **Agregar**.

12. En la ventana **Authentication Profile** (Perfil de autenticación), haga lo siguiente: 

    ![Ventana de perfil de autenticación](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authentication_profile.png)

    a. En el cuadro **Name** (Nombre), proporcione un nombre (por ejemplo, **AzureSAML_Admin_AuthProfile**).

    b. En la lista desplegable **Type** (Tipo), seleccione **SAML**. 

    c. En la lista desplegable **IdP Server Profile** (Perfil del servidor de IdP), seleccione el perfil adecuado de servidor del proveedor de identidades de SAML (por ejemplo, **AzureAD Admin UI**).

    c. Active la casilla **Enable Single Logout** (Habilitar el cierre de sesión único).

    d. En el cuadro **Admin Role Attribute** (Atributo de rol de administrador), escriba el nombre del atributo (por ejemplo, **adminrole**).

    e. Seleccione la pestaña **Advanced** (Opciones avanzadas) y, en **Allow List** (Lista de permitidos), seleccione **Add** (Agregar).

    ![Botón para agregar de la pestaña de opciones avanzadas](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_allowlist.png)

    f. Active la casilla **All** (Todos) o seleccione los usuarios y grupos que se pueden autenticar con este perfil.  
    Cuando un usuario se autentica, el firewall compara el nombre de usuario o grupo asociado con las entradas de esta lista. Si no agrega entradas, ningún usuario puede autenticarse.

    g. Seleccione **Aceptar**.

13. Para permitir que los administradores usen SSO de SAML mediante Azure, seleccione **Device** (Dispositivo)  > **Setup** (Configuración). En el panel **Setup** (Configuración), seleccione la pestaña **Management** (Administración) y, en **Authentication Settings** (Configuración de autenticación), seleccione el botón de engranaje **Settings** (Configuración).

    ![Botón de configuración](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsetup.png)

14. Seleccione el perfil de autenticación de SAML que creó en la ventana Perfil de autenticación (por ejemplo, **AzureSAML_Admin_AuthProfile**).

    ![Campo de perfil de autenticación](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsettings.png)

15. Seleccione **Aceptar**.

16. Para confirmar la configuración, seleccione **Commit** (Confirmar).

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

En esta sección, habilitará a Britta Simon para usar el inicio de sesión único de Azure al concederle acceso a Palo Alto Networks: interfaz de usuario de administrador.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Palo Alto Networks - Admin UI**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Palo Alto: interfaz de usuario de administrador**.

    ![El vínculo a Palo Alto Networks: interfaz de usuario de administrador en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-palo-alto-networks---admin-ui-test-user"></a>Creación de un usuario de prueba de Palo Alto Networks - Admin UI

Palo Alto Networks - Admin UI admite el aprovisionamiento de usuarios Just-In-Time. Si un usuario aún no existe, se crea automáticamente en el sistema después de una autenticación correcta. No es necesaria ninguna acción por su parte para crear el usuario.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Palo Alto Networks - Admin UI en el panel de acceso, debería iniciar sesión automáticamente en la versión de Palo Alto Networks - Admin UI para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
