---
title: 'Tutorial: Integración de Azure Active Directory con Cisco Webex | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Cisco Webex.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ef0638afd162dc179a2a7dec54b60bfb1f89144
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56869775"
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

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de inicio de sesión único de dominio y direcciones URL de Cisco Webex](common/sp-identifier.png)

     a. En el cuadro de texto **Dirección URL de inicio de sesión**, escriba una como: `https://web.ciscospark.com/#/signin`

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://idbroker.webex.com/<Org Id>`

    > [!NOTE]
    > Este valor del identificador no es real. Actualícelo con el identificador real. Si tiene los metadatos del proveedor de servicios, puede cargarlos en la sección **Configuración básica de SAML**; el valor de **Identificador (id. de entidad)** se rellenará automáticamente.

5. La aplicación Cisco Webex espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token SAML. La siguiente captura de muestra la lista de atributos predeterminados. Haga clic en el icono  **Editar**  para agregar los atributos.

    ![imagen](common/edit-attribute.png)

6. Además de lo anterior, la aplicación Cisco Webex espera que se usen algunos atributos más en la respuesta de SAML. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, realice los siguientes pasos para agregar el atributo Token SAML como se muestra en la tabla siguientes:
    
    | NOMBRE |  Atributo de origen|
    | ---------------|--------- |
    | UID | user.userprincipalname |

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

8. En la sección **Set up Cisco Webex** (Configurar Cisco Webex), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-cisco-webex-single-sign-on"></a>Configuración del inicio de sesión único de Cisco Webex

1. Inicie sesión en [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) (Administración de colaboración en la nube de Cisco) con sus credenciales completas de administrador.

2. Seleccione **Settings** (Configuración) y en la sección **Authentication** (Autenticación), haga clic en **Modify** (Modificar).

    ![Configurar inicio de sesión único](./media/cisco-spark-tutorial/tutorial_cisco_spark_10.png)
  
3. Seleccione **Integrate a 3rd-party identity provider. (Advanced)** (Integrar un proveedor de identidades de terceros [avanzado]).y vaya a la pantalla siguiente.

4. En la página **Import Idp Metadata** (Importar metadatos de LDP) arrastre y coloque el archivo de metadatos de Azure AD en la página o utilice la opción de explorador de archivos para buscar y cargar el archivo de metadatos de Azure AD. Después, seleccione **Require certificate signed by a certificate authority in Metadata (more secure)** (Requerir certificado firmado por una entidad de certificación en metadatos [más seguro]) y haga clic en **Next** (Siguiente).

    ![Configurar inicio de sesión único](./media/cisco-spark-tutorial/tutorial_cisco_spark_11.png)

5. Seleccione **Test SSO Connection** (Probar SSO de conexión) y cuando se abra una nueva pestaña de explorador, autentíquese con Azure AD mediante el inicio de sesión.

6. Vuelva a la pestaña del explorador de **Cisco Cloud Collaboration Management**. Si la prueba se realizó correctamente, seleccione **This test was successful. Enable Single Sign-On option** (Esta prueba se realizó correctamente. Habilite la opción de inicio de sesión único) y haga clic en **Next** (Siguiente).

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

En esta sección, creará un usuario llamado Britta Simon en Cisco Webex. En esta sección, creará un usuario llamado Britta Simon en Cisco Webex.

1. Vaya a [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) con sus credenciales de administración completas.

2. Haga clic en **Usuarios** y después en **Administrar usuarios**.
   
    ![Configurar inicio de sesión único](./media/cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. En la ventana **Manage User** (Administrar usuario), seleccione **Manually add or modify users** (Agregar o modificar usuarios manualmente) y haga clic en **Next** (Siguiente).

4. Seleccione **Names and Email address** (Nombres y direcciones de correo electrónico). Después, rellene el cuadro de texto de la forma siguiente:

    ![Configurar inicio de sesión único](./media/cisco-spark-tutorial/tutorial_cisco_spark_13.png) 

    a. En el cuadro de texto **First Name** (Nombre), escriba el nombre del usuario, en este caso, **Britta**.

    b. En el cuadro de texto **Last Name** (Apellidos), escriba los apellidos del usuario, en este caso, **Simon**.

    c. En el cuadro de texto **Email address** (Dirección de correo electrónico), escriba la dirección de correo electrónico del usuario, por ejemplo, **britta.simon@contoso.com**.

5. Haga clic en el signo más para agregar a Britta Simon. A continuación, haga clic en **Siguiente**.

6. En la ventana **Add Services for Users** (Agregar servicios para los usuarios), haga clic en **Save** (Guardar) y después en **Finish** (Finalizar).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Cisco Webex en el panel de acceso, debería iniciar sesión automáticamente en la versión de Cisco Webex para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
