---
title: 'Tutorial: Integración de Azure Active Directory con Workday | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Workday.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/04/2019
ms.author: jeedes
ms.openlocfilehash: 9bcc74e395746ae82867d915ea37962bf8880a3e
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57857188"
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Tutorial: Integración de Azure Active Directory con Workday

En este tutorial, obtendrá información sobre cómo integrar Workday con Azure Active Directory (Azure AD).
La integración de Workday con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Workday.
* Puede permitir que los usuarios inicien sesión automáticamente en Workday (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Workday, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción que permita el inicio de sesión único en Workday

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Workday admite el inicio de sesión único iniciado por **SP** e **IDP**

## <a name="adding-workday-from-the-gallery"></a>Incorporación de Workday desde la galería

Para configurar la integración de Workday en Azure AD, hay que agregar Workday desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Workday desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Workday**, seleccione **Workday** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Workday en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, puede configurar y probar el inicio de sesión único de Azure AD con Workday con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Workday.

Para configurar y probar el inicio de sesión único de Azure AD con Workday, hay que completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Workday](#configure-workday-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Workday](#create-workday-test-user)**: para tener un homólogo de Britta Simon en Workday que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Workday, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Workday**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Workday](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https:\//impl.workday.com/<tenant>/login-saml2.flex`

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://www.workday.com`

5. Haga clic en **Establecer direcciones URL adicionales** y realice el paso siguiente:

    ![Información de dominio y direcciones URL de inicio de sesión único de Workday](./media/workday-tutorial/reply.png)

    En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https:\//impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de URL de respuesta y URL de inicio de sesión. La URL de respuesta debe tener un subdominio (por ejemplo, www, wd2, wd3, wd3-impl, wd5 y wd5-impl).
    > Algo como `http://www.myworkday.com` funciona, pero `http://myworkday.com` no. Póngase en contacto con el [equipo de soporte técnico de Workday](https://www.workday.com/en-us/partners-services/services/support.html) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

6. La aplicación Workday espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de pantalla muestra la lista de atributos predeterminados, donde **nameidentifier** se asigna con **user.userprincipalname**. La aplicación Workday espera que **nameidentifier** se asigne con **user.mail**, **UPN** etc., por lo que debe editar la asignación de atributos haciendo clic en el icono **Editar** y cambiar dicha asignación.

    ![imagen](common/edit-attribute.png)

    > [!NOTE]
    > Aquí hemos asignado el Identificador de nombre con el UPN (user.userprincipalname) como valor predeterminado. Tiene que asignar el Identificador de nombre con el Identificador de usuario real en su cuenta de Workday (su correo electrónico, UPN etc.) para el correcto funcionamiento del inicio de sesión único.

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

8. En la sección **Set up Workday** (Configurar Workday), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-workday-single-sign-on"></a>Configuración del inicio de sesión único en Workday

1. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de Workday como administrador.

2. En el **cuadro de búsqueda**, escriba el nombre **Edit Tenant Setup – Security** (Editar configuración de inquilino – Seguridad) en la parte superior izquierda de la página principal.

    ![Edición de seguridad del inquilino](./media/workday-tutorial/IC782925.png "Edición de seguridad del inquilino")

3. En la sección **URL de redireccionamiento** , siga estos pasos:

    ![Direcciones URL de redirección](./media/workday-tutorial/IC7829581.png "Direcciones URL de redirección")

     a. Haga clic en **Add Row**(Agregar fila).

    b. En los cuadros de texto **Dirección URL de redireccionamiento de inicio de sesión** y **URL de redireccionamiento móvil**, escriba la **URL de inicio de sesión** que ha especificado en la sección **Configuración básica de SAML** de Azure Portal.

    c. En Azure Portal, en la sección **Set up Workday** (Configurar Workday), copie la **URL de cierre de sesión** y péguela en el cuadro de texto **Logout Redirect URL** (URL de redireccionamiento de cierre de sesión).

    d. En el cuadro de texto **Used for Environments** (Se usa en entornos), seleccione el nombre del entorno.  

   > [!NOTE]
   > El valor del atributo Entorno está vinculado con el valor de la URL del inquilino:  
   > -Si el nombre de dominio de la URL de inquilino de Workday empieza por impl (por ejemplo, *https:\//impl.workday.com/\<tenant\>/login-saml2.flex*), el atributo **Entorno** tiene que establecerse en Implementación.  
   > Si el nombre de dominio empieza por otra cosa, deberá ponerse en contacto con el [equipo de soporte técnico de Workday](https://www.workday.com/en-us/partners-services/services/support.html) para obtener el valor de **Entorno** coincidente.

4. En la sección **Configuración de SAML** , realice los pasos siguientes:

    ![Configuración de SAML](./media/workday-tutorial/IC782926.png "Configuración de SAML")

     a.  Seleccione **Enable SAML Authentication**(Habilitar autenticación SAML).

    b.  Haga clic en **Add Row**(Agregar fila).

5. En la sección **Proveedores de identidades SAML**, realice los pasos siguientes:

    ![Proveedores de identidades SAML](./media/workday-tutorial/IC7829271.png "Proveedores de identidades SAML")

     a. En el cuadro de texto **Identity Provider Name** (Nombre del proveedor de identidades), escriba un nombre de proveedor (por ejemplo: *SPInitiatedSSO*).

    b. En Azure Portal, en la sección **Set up Workday** (Configurar Workday), copie el valor de **Identificador de Azure AD** y péguelo en el cuadro de texto **Emisor**.

    ![Proveedores de identidades SAML](./media/workday-tutorial/IC7829272.png "Proveedores de identidades SAML")

    c. En Azure Portal, en la sección **Set up Workday** (Configurar Workday), copie el valor de **URL de cierre de sesión** y péguelo en el cuadro de texto **Logout Request URL** (URL de solicitud de cierre de sesión).

    d. En Azure Portal, en la sección **Set up Workday** (Configurar Workday), copie el valor de **URL de inicio de sesión** y péguelo en el cuadro de texto **IdP SSO Service URL** (URL del servicio SSO de IdP).

    e. En el cuadro de texto **Used for Environments** (Se usa en entornos), seleccione el nombre del entorno.

    f. Haga clic en **Identity Provider Public Key Certificate** (Certificado de clave pública de proveedor de identidades) y, después, en **Crear**.

    ![Crear](./media/workday-tutorial/IC782928.png "Crear")

    g. Haga clic en **Create x509 Public Key**(Crear clave pública x509).

    ![Crear](./media/workday-tutorial/IC782929.png "Crear")

6. En la sección **View x509 Public Key** (Ver clave pública x509), siga estos pasos:

    ![Visualización de clave pública x509](./media/workday-tutorial/IC782930.png "Visualización de clave pública x509")

     a. En el cuadro de texto **Name** (Nombre), escriba un nombre para el certificado (por ejemplo: *PPE\_SP*).

    b. En el cuadro de texto **Válido desde** , escriba el valor del atributo Válido desde del certificado.

    c.  En el cuadro de texto **Válido hasta** , escriba el valor del atributo Válido hasta del certificado.

    > [!NOTE]
    > Puede obtener la fecha válido desde y la fecha válido hasta del certificado descargado haciendo doble clic en él.  Las fechas se muestran bajo la pestaña **Detalles** .
    >
    >

    d.  Abra el certificado codificado en base 64 en el Bloc de notas y luego copie el contenido del mismo.

    e.  En el cuadro de texto **Certificado** , pegue el contenido del portapapeles.

    f.  Haga clic en **OK**.

7. Lleve a cabo los siguiente pasos:

    ![Configuración de SSO](./media/workday-tutorial/WorkdaySSOConfiguratio.png "Configuración de SSO")

     a.  En el cuadro de texto **Service Provider ID** (Id. de proveedor de servicios), escriba **https://www.workday.com**.

    b. Seleccione **No desinflar la solicitud de autenticación iniciada por el SP**.

    c. Como **Método de firma de solicitud de autenticación**, seleccione **SHA256**.

    ![Método de firma de solicitud de autenticación](./media/workday-tutorial/WorkdaySSOConfiguration.png "Método de firma de solicitud de autenticación") 

    d. Haga clic en **OK**.

    ![Aceptar](./media/workday-tutorial/IC782933.png "Aceptar")

    > [!NOTE]
    > Asegúrese de configurar el inicio de sesión único correctamente. Si habilita el inicio de sesión único con una configuración incorrecta, es posible que no pueda entrar en la aplicación con sus credenciales y quede bloqueado fuera de esta. En esta situación, Workday proporciona una URL de inicio de sesión de respaldo donde los usuarios pueden iniciar sesión con su nombre de usuario y contraseña normal en el siguiente formato: [Su URL de Workday]/login.flex?redirect=n

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon\@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Workday.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, a continuación, **Workday**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Workday**.

    ![Vínculo a Workday en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-workday-test-user"></a>Creación de un usuario de prueba de Workday

En esta sección, creará un usuario llamado Britta Simon en Workday. Trabaje con el [equipo de soporte técnico al cliente de Workday](https://www.workday.com/en-us/partners-services/services/support.html) para agregar los usuarios a la plataforma de Workday. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Workday en el panel de acceso, debería iniciar sesión automáticamente en la versión de Workday para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

