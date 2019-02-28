---
title: 'Tutorial: Integración de Azure Active Directory con SAP Business Object Cloud | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SAP Business Object Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/31/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c397cd3b535c7bf918eac2ac1e9ae1f967d7b95a
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56877510"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a>Tutorial: Integración de Azure Active Directory con SAP Business Object Cloud

En este tutorial, aprenderá a integrar SAP Business Object Cloud con Azure Active Directory (Azure AD).
La integración de SAP Business Object Cloud con Azure AD proporciona las siguientes ventajas:

* En Azure AD se puede controlar quién tiene acceso a SAP Business Object Cloud.
* Puede permitir que los usuarios inicien sesión automáticamente en SAP Business Object Cloud (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con SAP Business Object Cloud, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en SAP Business Object Cloud

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* SAP Business Object Cloud admite el inicio de sesión único iniciado por **SP**

## <a name="adding-sap-business-object-cloud-from-the-gallery"></a>Adición de SAP Business Object Cloud desde la galería

Para configurar la integración de SAP Business Object Cloud en Azure AD, es preciso agregar SAP Business Object Cloud desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar SAP Business Object Cloud desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **SAP Business Object Cloud**, seleccione **SAP Business Object Cloud** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![SAP Business Object Cloud en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con SAP Business Object Cloud con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de SAP Business Object Cloud.

Para configurar y probar el inicio de sesión único de Azure AD con SAP Business Object Cloud, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de SAP Business Object Cloud](#configure-sap-business-object-cloud-single-sign-on)**: para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación del usuario de prueba de SAP Business Object Cloud](#create-sap-business-object-cloud-test-user)**: el objetivo es tener un homólogo de Britta Simon en SAP Business Object Cloud vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con SAP Business Object Cloud, siga estos pasos:

1. En la página de integración de la aplicación [SAP Business Object Cloud](https://portal.azure.com/) de **Azure Portal**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de SAP Business Object Cloud](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón:
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón:
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    > [!NOTE] 
    > Los valores de estas direcciones URL se muestran solo con fines demostrativos. Debe actualizarlos con las direcciones URL reales de inicio de sesión único y de identificador. Para obtener la dirección URL de inicio de sesión, póngase en contacto con el [equipo de soporte técnico de SAP Business Object Cloud](https://help.sap.com/viewer/product/SAP_BusinessObjects_Cloud/release/). Para obtener la dirección URL de identificador, descargue los metadatos de SAP Business Object Cloud desde la consola de administración. Esto se explica posteriormente en este tutorial.

4. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

### <a name="configure-sap-business-object-cloud-single-sign-on"></a>Configuración del inicio de sesión único de SAP Business Object Cloud

1. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de SAP Business Object Cloud como administrador.

2. Seleccione **Menu** > **System** > **Administration** (Menú > Sistema > Administración).
    
    ![Seleccionar Menu (Menú), System (Sistema) y, luego, Administration (Administración)](./media/sapboc-tutorial/config1.png)

3. En la pestaña **Security** (Seguridad), seleccione el icono **Edit** (Editar) (lápiz).
    
    ![En la pestaña Security (Seguridad), seleccionar el icono Edit (Editar)](./media/sapboc-tutorial/config2.png)  

4. Como **Authentication Method** (Método de autenticación), seleccione **SAML Single Sign-On (SSO)** (Inicio de sesión único [SSO] de SAML).

    ![Seleccionar SAML Single Sign-On (SSO) (Inicio de sesión único [SSO] de SAML) como método de autenticación](./media/sapboc-tutorial/config3.png)  

5. Para descargar los metadatos del proveedor de servicios (Paso 1), haga clic en **Download** (Descargar). En el archivo de metadatos, busque y copie el valor **entityID**. En Azure Portal, en el cuadro de diálogo **Configuración básica de SAML**, pegue el valor en el cuadro **Identificador**.

    ![Copiar y pegar el valor entityID](./media/sapboc-tutorial/config4.png)  

6. Para cargar los metadatos del proveedor de servicios (Paso 2) en el archivo que descargó desde Azure Portal en **Upload Identity Provider metadata** (Cargar metadatos del proveedor de identidades), seleccione **Upload** (Cargar).  

    ![En Upload Identity Provider metadata (Cargar metadatos del proveedor de identidades), seleccionar Upload (Cargar)](./media/sapboc-tutorial/config5.png)

7. En la lista **User Attribute** (Atributo de usuario), seleccione el atributo de usuario (Paso 3) que quiere usar en su implementación. Este atributo de usuario se asigna al proveedor de identidades. Para escribir un atributo personalizado en la página del usuario, use la opción **Custom SAML Mapping** (Personalizar asignación de SAML). O bien, puede seleccionar **Email** (Correo electrónico) o **USER ID** (Id. de usuario) como atributo de usuario. En nuestro ejemplo, hemos seleccionado **Email** (Correo electrónico) porque hemos asignado la notificación de identificador de usuario con el atributo **userprincipalname** en la sección **Atributos y notificaciones de usuario** de Azure Portal. Esto proporciona un correo electrónico de usuario único, que se envía a la aplicación SAP Business Object Cloud en cada respuesta SAML correcta.

    ![Seleccionar Atributos de usuario](./media/sapboc-tutorial/config6.png)

8. Para comprobar la cuenta con el proveedor de identidades (Paso 4), en el cuadro **Login Credential (Email)** [Credenciales de inicio de sesión (correo electrónico)], escriba la dirección de correo electrónico del usuario. A continuación, seleccione **Verify Account** (Comprobar cuenta). El sistema agrega credenciales de inicio de sesión a la cuenta de usuario.

    ![Escribir Email (Correo electrónico) y seleccionar Verify Account (Comprobar cuenta)](./media/sapboc-tutorial/config7.png)

9. Seleccione el icono **Save** (Guardar).

    ![Icono Save (Guardar)](./media/sapboc-tutorial/save.png)

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a SAP Business Object Cloud.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **SAP Business Object Cloud**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **SAP Business Object Cloud**.

    ![Vínculo de SAP Business Object Cloud en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-sap-business-object-cloud-test-user"></a>Creación del usuario de prueba de SAP Business Object Cloud

Se debe aprovisionar a los usuarios de Azure AD en SAP Business Object Cloud antes de que puedan iniciar sesión en SAP Business Object Cloud. En SAP Business Object Cloud, el aprovisionamiento es una tarea manual.

Para aprovisionar una cuenta de usuario:

1. Inicie sesión como administrador en su sitio de la compañía de SAP Business Object Cloud.

2. Seleccione **Menu** > **Security** > **Users** (Menú > Seguridad > Usuarios).

    ![Agregar empleado](./media/sapboc-tutorial/user1.png)

3. En la página **Users** (Usuarios), para agregar nuevos detalles de usuario, seleccione **+**. 

    ![Página Add Users (Agregar usuarios)](./media/sapboc-tutorial/user4.png)

    Después, complete los siguientes pasos:

     a. En el cuadro **USER ID** (Id. de usuario), escriba el identificador del usuario, en este caso **Britta**.

    b. En el cuadro **FIRST NAME** (Nombre), escriba el nombre del usuario, en este caso **Britta**.

    c. En el cuadro **LAST NAME** (Apellido), escriba el apellido del usuario, en este caso **Simon**.

    d. En el cuadro **DISPLAY NAME** (Nombre para mostrar), escriba el nombre completo del usuario, en este caso **Britta Simon**.

    e. En el cuadro **E-MAIL** (Correo electrónico), escriba la dirección de correo electrónico del usuario, en este caso **brittasimon@contoso.com**.

    f. En la página **Select Roles** (Seleccionar roles), seleccione el rol adecuado para el usuario y, luego, **OK** (Aceptar).

      ![Seleccionar rol](./media/sapboc-tutorial/user3.png)

    g. Seleccione el icono **Save** (Guardar).    

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de SAP Business Object Cloud en el panel de acceso, debería iniciar sesión automáticamente en la versión de SAP Business Object Cloud para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

