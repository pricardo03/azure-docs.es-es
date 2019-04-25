---
title: 'Tutorial: integración de Azure Active Directory con RingCentral | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y RingCentral.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5848c875-5185-4f91-8279-1a030e67c510
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2019
ms.author: jeedes
ms.openlocfilehash: 7cd20397c122834a23586bf51af6467f58531094
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59997247"
---
# <a name="tutorial-azure-active-directory-integration-with-ringcentral"></a>Tutorial: integración de Azure Active Directory con RingCentral

En este tutorial, aprenderá a integrar RingCentral con Azure Active Directory (Azure AD).
La integración de RingCentral con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a RingCentral.
* Puede permitir que los usuarios inicien sesión automáticamente en RingCentral (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con RingCentral, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Suscripción de RingCentral habilitada para el inicio de sesión único

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* RingCentral admite el inicio de sesión único con **SP** iniciado.

## <a name="adding-ringcentral-from-the-gallery"></a>Agregar RingCentral desde la Galería

Para configurar la integración de RingCentral en Azure AD, será preciso que agregue RingCentral desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar RingCentral desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **RingCentral**, seleccione **RingCentral** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![RingCentral en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con RingCentral con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de RingCentral.

Para configurar y probar el inicio de sesión único de Azure AD con RingCentral, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de RingCentral](#configure-ringcentral-single-sign-on)**: para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de RingCentral](#create-ringcentral-test-user)**: para tener un homólogo de Britta Simon en RingCentral que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con RingCentral, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **RingCentral**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si tiene el **archivo de metadatos del proveedor de servicios**, lleve a cabo los siguientes pasos:

     a. Haga clic en **Cargar el archivo de metadatos**.

    ![Carga del archivo de metadatos](common/upload-metadata.png)

    b. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.

    ![Elección del archivo de metadatos](common/browse-upload-metadata.png)

    c. Una vez que se haya cargado correctamente el archivo de metadatos, los valores **Identificador** y **URL de respuesta** se rellenan automáticamente en la sección **Configuración básica de SAML**.

    ![Información sobre dominio y direcciones URL de inicio de sesión único de RingCentral](common/sp-identifier-reply.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL:
    
    | |
    |--|
    | `https://service.ringcentral.com`|
    | `https://service.ringcentral.com.au`|
    | `https://service.ringcentral.co.uk`|
    | `https://service.ringcentral.eu`|

    > [!Note]
    > Obtendrá el **archivo de metadatos del proveedor de servicios** en la página Configuración de inicio de sesión único de RingCentral que se explica más adelante en el tutorial.

5. Si no dispone del **archivo de metadatos del proveedor de servicios**, lleve a cabo los pasos siguientes:

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL:

    | |
    |--|
    | `https://service.ringcentral.com` |
    | `https://service.ringcentral.com.au` |
    | `https://service.ringcentral.co.uk` |
    | `https://service.ringcentral.eu` |

    b. En el cuadro de texto **Identificador**, escriba una dirección URL:

    | |
    |--|
    |  `https://sso.ringcentral.com` |
    | `https://ssoeuro.ringcentral.com` |

    c. En el cuadro de texto **URL de respuesta**, escriba una dirección URL:

    | |
    |--|
    | `https://sso.ringcentral.com/sp/ACS.saml2` |
    | `https://ssoeuro.ringcentral.com/sp/ACS.saml2` |

    ![Información sobre dominio y direcciones URL de inicio de sesión único de RingCentral](common/sp-identifier-reply.png)

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el botón de copia para copiar **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

### <a name="configure-ringcentral-single-sign-on"></a>Configuración del inicio de sesión único de RingCentral

1. En otra ventana del explorador web, inicie sesión en RingCentral como administrador de seguridad.

1. En la parte superior, haga clic en **Herramientas**.

    ![imagen](./media/ringcentral-tutorial/ringcentral1.png)

1. Vaya a **Inicio de sesión único**.

    ![imagen](./media/ringcentral-tutorial/ringcentral2.png)

1. En la página **Inicio de sesión único**, en la sección **Configuración de inicio de sesión único**, desde **Paso 1** haga clic en **Editar** y realice los siguientes pasos:

    ![imagen](./media/ringcentral-tutorial/ringcentral3.png)

1. En la página **Configurar inicio de sesión único** realice los siguientes pasos:

    ![imagen](./media/ringcentral-tutorial/ringcentral4.png)

     a. Haga clic en **Examinar** para cargar el archivo de metadatos que descargó de Azure Portal.

    b. Después de cargar los metadatos los valores se rellenan automáticamente en la sección **Información general de inicio de sesión único**.

    c. En la sección **Asignación de atributos**, seleccione **Asignar atributo de correo electrónico a** como `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. Haga clic en **Save**(Guardar).

    e. En **Paso 2** haga clic en **Descargar** para descargar el **archivo de metadatos del proveedor de servicios** y cargarlo en la sección **Configuración básica de SAML** para rellenar de forma automática los valores **Identificador** y **URL de respuesta** en Azure Portal.

    ![imagen](./media/ringcentral-tutorial/ringcentral6.png) 

    f. En la misma página, vaya a la sección **Habilitar Inicio de sesión único** y realice los siguientes pasos:

    ![imagen](./media/ringcentral-tutorial/ringcentral5.png)

    * Seleccione **Habilitar inicio de sesión único**.

    * Seleccione **Permitir a los usuarios iniciar sesión con credenciales de inicio de sesión único o de RingCentral**.

    * Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba `brittasimon@yourcompanydomain.extension`.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a RingCentral.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **RingCentral**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **RingCentral**.

    ![Vínculo de RingCentral en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-ringcentral-test-user"></a>Creación de un usuario de prueba de RingCentral

En esta sección, creará un usuario llamado Britta Simon en RingCentral. Colabore con el  [equipo de soporte técnico para el cliente de RingCentral](https://success.ringcentral.com/RCContactSupp) para agregar usuarios en la plataforma de RingCentral. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de RingCentral en el Panel de acceso, debería iniciar sesión automáticamente en la instancia de RingCentral para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
