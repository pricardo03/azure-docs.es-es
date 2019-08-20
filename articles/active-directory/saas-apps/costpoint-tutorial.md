---
title: 'Tutorial: Integración de Azure Active Directory con Costpoint | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Costpoint.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 9ecc5f58-4462-4ade-ab73-0a4f61027504
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6c1a8b916feb2ad67623434f2b63468be72bf1aa
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68879585"
---
# <a name="tutorial-integrate-costpoint-with-azure-active-directory"></a>Tutorial: Integración de Costpoint con Azure Active Directory

En este tutorial, aprenderá a integrar Costpoint con Azure Active Directory (Azure AD). Al integrar Costpoint con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Costpoint.
* Permitir que los usuarios puedan iniciar sesión automáticamente en Costpoint con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Costpoint.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba. Costpoint admite el inicio de sesión único iniciado por **SP e IDP**.

## <a name="adding-costpoint-from-the-gallery"></a>Adición de Costpoint desde la galería

Para configurar la integración de Costpoint en Azure AD, debe agregar Costpoint desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Costpoint** en el cuadro de búsqueda.
1. Seleccione **Costpoint** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

Configure y pruebe el inicio de sesión único de Azure AD con Costpoint mediante un usuario de prueba llamado **B. Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Costpoint.

Para configurar y probar el inicio de sesión único de Azure AD con Costpoint, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
2. **[Configuración de Costpoint](#configure-costpoint)** , para configurar el inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que B. Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Costpoint](#create-costpoint-test-user)** , para tener un homólogo de B.Simon en Costpoint que esté vinculado a la representación de este usuario en Azure AD.
6. **[Comprobación del inicio de sesión único](#test-sso)** , para verificar que la configuración funciona correctamente.

### <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Costpoint**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, seleccione **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si tiene el **archivo de metadatos del proveedor de servicios**, lleve a cabo los siguientes pasos:

    > [!NOTE]
    > Obtendrá el archivo de metadatos del proveedor de servicios en la sección **Generación de metadatos de Costpoint**, que se explica más adelante en el tutorial.
 
    1. Haga clic en **Cargar el archivo de metadatos**.
    
    1. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.
    
    1. Cuando se haya cargado correctamente el archivo de metadatos, los valores **Identificador** y **URL de respuesta** se rellenarán automáticamente en los cuadros de texto de la sección de Costpoint.

        > [!Note]
        > Si los valores **Identificador** y **Dirección URL de respuesta** no se rellenan automáticamente, hágalo manualmente según sus necesidades. Compruebe que los valores de **Identificador (id. de entidad)** y **Dirección URL de respuesta (URL del Servicio de consumidor de aserciones)** estén establecidos correctamente y que la **dirección URL de ACS** sea una dirección URL válida de Costpoint que finalice con **/LoginServlet.cps**.

    1. Haga clic en **Establecer direcciones URL adicionales**.

    1. En el cuadro de texto **Estado de la retransmisión**, escriba un valor con el siguiente patrón:`system=[your system], (for example, **system=DELTEKCP**)`

1. Si quiere configurar la aplicación en modo iniciado por **SP**, realice el siguiente paso:
    
    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL: `https://costpointteea.deltek.com/cpweb/cploginform.htm`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con el identificador real, la dirección URL de respuesta y la dirección URL de estado de la retransmisión. Para obtener estos valores, póngase en contacto con el [equipo de soporte técnico de Costpoint](https://www.deltek.com/about/contact-us) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el icono de copia para copiar la **dirección URL de metadatos de federación de aplicación** y guárdela en el Bloc de notas.

   ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

### <a name="generate-costpoint-metadata"></a>Generación de metadatos de Costpoint

La configuración del inicio de sesión único de SAML de Costpoint se explica en la guía **DeltekCostpoint711Security.pdf**. En ella, consulte la sección **SAML Single Sign-on Setup -> Configure SAML Single Sign-on between Costpoint and Azure AD** (Configuración del inicio de sesión único de SAML -> Configuración del inicio de sesión único de SAML entre Costpoint y Azure AD). Siga las instrucciones y genere el archivo **XML de metadatos de federación del proveedor de servicios de Costpoint**. Úselo en **Configuración básica de SAML** en Azure Portal.

![Utilidad de configuración de Costpoint](./media/costpoint-tutorial/config02.png)

> [!NOTE]
> Solicite la guía **DeltekCostpoint711Security.pdf** al [equipo de atención al cliente de Costpoint Client](https://www.deltek.com/about/contact-us). Si no tiene este archivo, póngase en contacto con ellos para obtenerlo.

### <a name="configure-costpoint"></a>Configuración de Costpoint

Vuelva a la **utilidad de configuración de Costpoint** y pegue la **dirección URL de metadatos de federación de aplicación** en el cuadro de texto **IdP Federation Metadata XML** (XML de metadatos de federación de IdP). A continuación, siga las instrucciones de la guía **DeltekCostpoint711Security.pdf** para finalizar la configuración de SAML de Costpoint. 

![Utilidad de configuración de Costpoint](./media/costpoint-tutorial/config01.png)

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

En esta sección, va a permitir que B.Simon acceda a Costpoint mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales** > **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Costpoint**.
1. En la sección **Administrar** de la página de información general de la aplicación, seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario** y, después, **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-costpoint-test-user"></a>Creación de un usuario de prueba de Costpoint

En esta sección, creará un usuario en Costpoint. Suponga que el valor de **User ID** (Id. de usuario) es **B.SIMON** y que el nombre es **B.Simon**. Trabaje con el [equipo de atención al cliente de Costpoint](https://www.deltek.com/about/contact-us) para agregar el usuario a la plataforma de Costpoint. El usuario se debe crear y activar antes de usar el inicio de sesión único.
 
Una vez creado, la selección de **Authentication Method** (Método de autenticación) del usuario debe ser **Active Directory**, la casilla **SAML Single Sign-on** (Inicio de sesión único de SAML) debe estar activada y el nombre de usuario de Azure Active Directory debe ser **Active Directory or Certificate ID** (Active Directory o id. de certificado) (como se muestra a continuación).

![Usuario de Costpoint](./media/costpoint-tutorial/user01.png)

### <a name="test-sso"></a>Prueba de SSO

Al seleccionar el icono de Costpoint en el Panel de acceso, debería iniciar sesión automáticamente en la versión de Costpoint para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)