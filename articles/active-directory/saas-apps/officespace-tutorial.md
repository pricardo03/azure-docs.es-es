---
title: 'Tutorial: Integración de Azure Active Directory con OfficeSpace Software | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y OfficeSpace Software.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 95d8413f-db98-4e2c-8097-9142ef1af823
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/13/2019
ms.author: jeedes
ms.openlocfilehash: 6547a640f6e56865d44b6848cf37078161faa538
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65904781"
---
# <a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Tutorial: Integración de Azure Active Directory con OfficeSpace Software

En este tutorial, obtendrá información sobre cómo integrar OfficeSpace Software con Azure Active Directory (Azure AD).
La integración de OfficeSpace Software con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a OfficeSpace Software.
* Puede permitir que los usuarios inicien sesión automáticamente en OfficeSpace Software (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con OfficeSpace Software, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Un suscripción habilitada para el inicio de sesión único en OfficeSpace Software

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* OfficeSpace Software admite el inicio de sesión único iniciado por **SP**.

* OfficeSpace Software admite el aprovisionamiento de usuarios **Just-In-Time**.

## <a name="adding-officespace-software-from-the-gallery"></a>Adición de OfficeSpace Software desde la galería

Para configurar la integración de OfficeSpace Software en Azure AD, deberá agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar OfficeSpace Software desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **OfficeSpace Software**, seleccione **OfficeSpace Software** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![OfficeSpace Software en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con OfficeSpace Software con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de OfficeSpace Software.

Para configurar y probar el inicio de sesión único de Azure AD con OfficeSpace Software, debe completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de OfficeSpace Software](#configure-officespace-software-single-sign-on)**: para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de OfficeSpace Software](#create-officespace-software-test-user)**: para tener un homólogo de Britta Simon en OfficeSpace Software que esté vinculado a la representación de ella en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con OfficeSpace Software, realice los siguientes pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **OfficeSpace Software**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de OfficeSpace Software](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<company name>.officespacesoftware.com/users/sign_in/saml`

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `<company name>.officespacesoftware.com`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de OfficeSpace Software](mailto:support@officespacesoftware.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. La aplicación en OfficeSpace Software espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de pantalla muestra la lista de atributos predeterminados, donde **nameidentifier** se asigna con **user.userprincipalname**. La aplicación en OfficeSpace Software espera que **nameidentifier** se asigne con **user.mail**, por lo que debe editar la asignación de atributos haciendo clic en el icono **Editar** y cambiar dicha asignación.

    ![imagen](common/edit-attribute.png)

6. Además de lo anterior, la aplicación en OfficeSpace Software espera que se usen algunos atributos más en la respuesta de SAML. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, realice los siguientes pasos para agregar el atributo Token SAML como se muestra en la tabla siguientes:

    | NOMBRE | Atributo de origen|
    | ---------------| --------------- |
    | email | user.mail |
    | Nombre | user.displayname |
    | first_name | user.givenname |
    | last_name | user.surname |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

7. En la sección **Certificado de firma de SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Certificado de firma de SAML**.

    ![Edición del certificado de firma de SAML](common/edit-certificate.png)

8. En la sección **Certificado de firma de SAML**, copie el valor de **Huella digital** en el equipo.

    ![Copia del valor de la huella digital](common/copy-thumbprint.png)

9. En la sección **Set up en OfficeSpace Software**  (Configurar en OfficeSpace Software ), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-officespace-software-single-sign-on"></a>Configuración del inicio de sesión único de OfficeSpace Software

1. En otra ventana del explorador web, inicie sesión en como administrador en el inquilino de OfficeSpace Software.

2. Vaya a **Settings** (Configuración) y haga clic en **Connectors** (Conectores).

    ![Configuración del inicio de sesión único en la aplicación](./media/officespace-tutorial/tutorial_officespace_002.png)

3. Haga clic en **Autenticación SAML**.

    ![Configuración del inicio de sesión único en la aplicación](./media/officespace-tutorial/tutorial_officespace_003.png)

4. En la sección **Autenticación SAML** , realice los pasos siguientes:

    ![Configuración del inicio de sesión único en la aplicación](./media/officespace-tutorial/tutorial_officespace_004.png)

     a. En el cuadro de texto **Logout provider URL** (Dirección URL del proveedor de cierre de sesión), pegue el valor de **dirección URL de cierre de sesión** que copió de Azure Portal.

    b. En el cuadro de texto **Client idp target url** (Dirección URL de destino IDP de cliente), pegue el valor del campo **Login URL** (Dirección URL de inicio de sesión) que copió de Azure Portal.

    c. En el cuadro de texto **Client IDP certificate fingerprint** (Huella digital del certificado de IDP del cliente), pegue el valor de **Huella digital** que ha copiado de Azure Portal. 

    d. Haga clic en **Guardar configuración**.

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a OfficeSpace Software.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, luego, **OfficeSpace Software**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **OfficeSpace Software**.

    ![Vínculo a OfficeSpace Software en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-officespace-software-test-user"></a>Creación de un usuario de prueba de OfficeSpace Software

En esta sección, se crea un usuario llamado a Britta Simon en OfficeSpace Software. OfficeSpace Software admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario ya no existe en OfficeSpace Software, se crea uno después de la autenticación.

> [!NOTE]
> Si necesita crear manualmente un usuario, es preciso que se ponga contacto con el [equipo de soporte técnico de OfficeSpace Software](mailto:support@officespacesoftware.com).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de OfficeSpace Software en el Panel de acceso, debería iniciar sesión automáticamente en la aplicación OfficeSpace Software para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

