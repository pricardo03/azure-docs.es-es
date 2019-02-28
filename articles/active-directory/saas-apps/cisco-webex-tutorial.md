---
title: 'Tutorial: Integración de Azure Active Directory con Cisco Webex | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Cisco Webex.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/4/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2bdd51ee6f3ef69289bad83e7cb344df41020ac8
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56871492"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Tutorial: Integración de Azure Active Directory con Cisco Webex

En este tutorial, aprenderá a integrar Cisco Webex con Azure Active Directory (Azure AD).
Integrar Cisco Webex con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Cisco Webex.
* Puede permitir que los usuarios inicien sesión automáticamente en Cisco Webex (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Cisco Webex, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en Cisco Webex

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Cisco Webex admite el inicio de sesión único iniciado por **SP**

* Cisco Webex admite el aprovisionamiento de usuarios **Just-In-Time**

* Cisco Webex admite el aprovisionamiento de usuarios **Automatizado**

## <a name="adding-cisco-webex-from-the-gallery"></a>Adición de Cisco Webex desde la galería

Para configurar la integración de Cisco Webex en Azure AD, será preciso que agregue Cisco Webex desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Cisco Webex desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Cisco Webex**, seleccione **Cisco Webex** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![Cisco Webex en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, va a configurar y probar el inicio de sesión único de Azure AD con Cisco Webex con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Cisco Webex.

Para configurar y probar el inicio de sesión único de Azure AD con Cisco Webex, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Cisco Webex](#configure-cisco-webex-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación del usuario de prueba de Cisco Webex](#create-cisco-webex-test-user)**: para tener un homólogo de Britta Simon en Cisco Webex vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Cisco Webex, siga estos pasos:

1. En la página de integración de la aplicación [Cisco Webex](https://portal.azure.com/) de **Azure Portal**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía Cisco Webex.

4. Haga clic en **Settings** (Configuración) a la izquierda del menú.

    ![Configurar inicio de sesión único](./media/cisco-webex-tutorial/tutorial_cisco_webex_10.png)

5. En la página de configuración, desplácese hacia abajo a la sección **Authentication** (Autenticación) y haga clic en **Modify** (Modificar).

    ![Configurar inicio de sesión único](./media/cisco-webex-tutorial/tutorial_cisco_webex_14.png)

6. Seleccione **Integrate a 3rd-party identity provider. (Advanced)** (Integrar un proveedor de identidades de terceros [avanzado]).y vaya a la pantalla siguiente.

    ![Configurar inicio de sesión único](./media/cisco-webex-tutorial/tutorial_cisco_webex_15.png)

7. En la página **Export Directory Metadata** (Exportar metadatos de directorio), haga clic en **Download Metadata File** (Descargar archivo de metadatos) para descargar el archivo de metadatos.

    ![Configurar inicio de sesión único](./media/cisco-webex-tutorial/tutorial_cisco_webex_16.png)

8. En Azure Portal, en la página **Set-up Single Sign-On with SAML** (Configurar inicio de sesión único con SAML), haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

9. En la sección **Configuración básica de SAML**, cargue el **archivo de metadatos del proveedor de servicios** que descargó y configure la aplicación realizando estos pasos:

     a. Haga clic en **Cargar el archivo de metadatos**.

    ![Carga del archivo de metadatos](common/upload-metadata.png)

    b. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.

    ![Elección del archivo de metadatos](common/browse-upload-metadata.png)

    c. Cuando el archivo de metadatos del proveedor de servicios se haya cargado correctamente, los valores **Identificador** y **URL de respuesta** se rellenan automáticamente en la sección **Configuración básica de SAML**:

    ![Información de inicio de sesión único de dominio y direcciones URL de Cisco Webex](common/sp-identifier-reply.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.webex.com/`

    > [!Note]
    > El valor de la dirección URL de inicio de sesión no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de clientes de Cisco Webex](https://www.webex.co.in/support/support-overview.html) para obtener este valor.

10. La aplicación Cisco Webex espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

11. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:
    
    | NOMBRE | Atributo de origen|
    | ---------------| --------------- | --------- |
    |   firstname    | user.givenname |
    |   lastname    | user.surname |
    |   uid    | user.mail |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

12. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

13. En la sección **Set up Cisco Webex** (Configurar Cisco Webex), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-cisco-webex-single-sign-on"></a>Configuración del inicio de sesión único de Cisco Webex

1. En la página de administrador del sitio de la compañía Cisco Webex, use la opción del explorador de archivos para localizar y cargar el archivo de metadatos de Azure AD. Después, seleccione **Require certificate signed by a certificate authority in Metadata (more secure)** (Requerir certificado firmado por una entidad de certificación en metadatos [más seguro]) y vaya a la siguiente pantalla. 

    ![Configurar inicio de sesión único](./media/cisco-webex-tutorial/tutorial_cisco_webex_11.png)

2. Seleccione **Test SSO Connection** (Probar SSO de conexión) y cuando se abra una nueva pestaña de explorador, autentíquese con Azure AD mediante el inicio de sesión.

3. Vuelva a la pestaña del explorador de **Cisco Cloud Collaboration Management**. Si la prueba se realizó correctamente, seleccione **This test was successful. Enable Single Sign-On option** (Esta prueba se realizó correctamente. Habilite la opción de inicio de sesión único) y haga clic en **Save** (Guardar).

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Cisco Webex.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Cisco Webex**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Cisco Webex**.

    ![Vínculo a Cisco Webex en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-cisco-webex-test-user"></a>Creación del usuario de prueba de Cisco Webex

El objetivo de esta sección es crear un usuario llamado Britta Simon en Cisco Webex. Cisco Webex admite el aprovisionamiento Just-In-Time y el aprovisionamiento automático de usuarios, el cual está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si el usuario no existe en Cisco Webex, se crea uno al intentar acceder.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Cisco Webex en el panel de acceso, debería iniciar sesión automáticamente en la versión de Cisco Webex para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

