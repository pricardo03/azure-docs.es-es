---
title: 'Tutorial: Integración de Azure Active Directory con PureCloud by Genesys | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y PureCloud by Genesys.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e16a46db-5de2-4681-b7e0-94c670e3e54e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/25/2019
ms.author: jeedes
ms.openlocfilehash: d1cc7735b8caa952a5ab7695d3e1f35b03fec1bb
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57890685"
---
# <a name="tutorial-azure-active-directory-integration-with-purecloud-by-genesys"></a>Tutorial: Integración de Azure Active Directory con PureCloud by Genesys

En este tutorial, aprenderá a integrar PureCloud by Genesys con Azure Active Directory (Azure AD).
La integración de PureCloud by Genesys con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a PureCloud by Genesys.
* Puede permitir que los usuarios inicien sesión automáticamente en PureCloud by Genesys (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con PureCloud by Genesys, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción habilitada para el inicio de sesión único en PureCloud by Genesys

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* PureCloud by Genesys admite el inicio de sesión único iniciado por **SP e IDP**.

## <a name="adding-purecloud-by-genesys-from-the-gallery"></a>Incorporación de PureCloud by Genesys desde la galería

Para configurar la integración de PureCloud by Genesys en Azure AD, deberá agregar PureCloud by Genesys desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar PureCloud by Genesys desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **PureCloud by Genesys**, seleccione **PureCloud by Genesys** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![PureCloud by Genesys en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con PureCloud by Genesys con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de PureCloud by Genesys.

Para configurar y probar el inicio de sesión único de Azure AD con PureCloud by Genesys, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único en PureCloud by Genesys](#configure-purecloud-by-genesys-single-sign-on)**: para configurar el inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de PureCloud by Genesys](#create-purecloud-by-genesys-test-user)**: para tener un homólogo de Britta Simon en PureCloud by Genesys que esté vinculado a la representación de ella en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con PureCloud by Genesys, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **PureCloud by Genesys**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en el modo iniciado por **IDP** siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de PureCloud by Genesys](common/idp-intiated.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL según su región:

    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.au/saml` |

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL según su región:

    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.com.au/saml` |

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de PureCloud by Genesys](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **Dirección URL de inicio de sesión**, escriba una dirección URL según su región:
    
    | |
    |--|
    | `https://login.mypurecloud.com` |
    | `https://login.mypurecloud.de` |
    | `https://login.mypurecloud.jp` |
    | `https://login.mypurecloud.ie` |
    | `https://login.mypurecloud.com.au` |

6. La aplicación PureCloud by Genesys espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de atributos de token SAML. La siguiente captura de muestra la lista de atributos predeterminados. Haga clic en el icono  **Editar**  para abrir el cuadro de diálogo  **Atributos de usuario** .

    ![imagen](common/edit-attribute.png)

7. Además de lo anterior, la aplicación PureCloud by Genesys espera que se pasen algunos atributos más en la respuesta de SAML. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, realice los siguientes pasos para agregar el atributo Token SAML como se muestra en la tabla siguientes:

    | NOMBRE | Atributo de origen|
    | ---------------| --------------- |
    | Email | user.userprinicipalname |
    | OrganizationName | `Your organization name` |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

8. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

9. En la sección **Set up PureCloud by Genesys** (Configurar PureCloud by Genesys), copie las direcciones URL adecuadas según sus requisitos.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-purecloud-by-genesys-single-sign-on"></a>Configuración del inicio de sesión único en PureCloud by Genesys

1. En una ventana del explorador web diferente, inicie sesión en PureCloud by Genesys como administrador.

2. Haga clic en **Admin** (Administrador) en la parte superior y, en **Integrations** (Integraciones), vaya a **Single Sign-on** (Inicio de sesión único).

    ![Configurar inicio de sesión único](./media/purecloud-by-genesys-tutorial/configure01.png)

3. Cambie a la pestaña **ADFS/Azure AD(Premium)** y realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/purecloud-by-genesys-tutorial/configure02.png)

     a. Haga clic en **Browse** (Examinar) para cargar en **ADFS Certificate** (Certificado de ADFS) el certificado codificado en base-64 que ha descargado de Azure Portal.

    b. En el cuadro de texto **ADFS Issuer URI** (URI de emisor de ADFS), pegue el valor de **Identificador de Azure AD** que ha copiado de Azure Portal.

    c. En el cuadro de texto **Target URI** (URI de destino), pegue el valor de **URL de inicio de sesión** que ha copiado de Azure Portal.

    d. Para el valor de **Relying Party Identifier** (Identificador de usuario de confianza), debe ir a Azure Portal, a la página de integración de aplicaciones de **PureCloud by Genesys**, hacer clic en la pestaña **Propiedades** y copiar el valor de **Id. de aplicación**. Péguelo en el cuadro de texto **Relying Party Identifier** (Identificador de usuario de confianza). 

    ![Configurar inicio de sesión único](./media/purecloud-by-genesys-tutorial/configure06.png)

    e. Haga clic en **Guardar**   

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

En esta sección, concederá acceso a Britta Simon a PureCloud by Genesys para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, luego, **PureCloud by Genesys**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **PureCloud by Genesys**.

    ![El vínculo de PureCloud by Genesys en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-purecloud-by-genesys-test-user"></a>Creación de un usuario de prueba de PureCloud by Genesys

Para permitir que los usuarios de Azure AD inicien sesión en PureCloud by Genesys, deben aprovisionarse en PureCloud by Genesys. En PureCloud by Genesys, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en PureCloud by Genesys como administrador.

2. Haga clic en **Admin** (Administrador) en la parte superior y vaya a **People** (Personas) en **People & Permissions** (Personas y permisos).

    ![Configurar inicio de sesión único](./media/purecloud-by-genesys-tutorial/configure03.png)

3. En la página People (Personas), haga clic en **Add Person** (Agregar persona).

    ![Configurar inicio de sesión único](./media/purecloud-by-genesys-tutorial/configure04.png)

4. En el elemento emergente **Add People to the Organization** (Agregar personas a la organización), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/purecloud-by-genesys-tutorial/configure05.png)

     a. En el cuadro de texto **Full Name** (Nombre), escriba el nombre de usuario **Brittasimon**.

    b. En el cuadro de texto **Email** (Correo electrónico), escriba el correo electrónico del usuario; por ejemplo, **brittasimon\@contoso.com**.
    
    c. Haga clic en **Create**(Crear).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de PureCloud by Genesys en el Panel de acceso, debería iniciar sesión automáticamente en la versión de PureCloud by Genesys para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

