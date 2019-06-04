---
title: 'Tutorial: Integración de Azure Active Directory con NEOGOV | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y NEOGOV.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6e4481af-54f1-4689-80d0-bd02a749ef53
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 060492d2ed551ed0e90aaf3c1a373572c0c0ab73
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66307500"
---
# <a name="tutorial-integrate-neogov-with-azure-active-directory"></a>Tutorial: Integración de Azure Active Directory con NEOGOV

En este tutorial, obtendrá información sobre cómo integrar NEOGOV con Azure Active Directory (Azure AD). Al integrar NEOGOV con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a NEOGOV.
* Permitir que los usuarios puedan iniciar sesión automáticamente en NEOGOV con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede obtener la evaluación gratuita de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).
* Una suscripción de NEOGOV con el inicio de sesión único (SSO) habilitado.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba. NEOGOV admite el inicio de sesión único iniciado por **IDP**.

## <a name="adding-neogov-from-the-gallery"></a>Adición de NEOGOV desde la galería

Para configurar la integración de NEOGOV en Azure AD, es preciso agregar NEOGOV desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **NEOGOV** en el cuadro de búsqueda.
1. Seleccione **NEOGOV** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

Configure y pruebe el inicio de sesión único de Azure AD con NEOGOV utilizando un usuario de prueba llamado **B. Simon**. Para que el SSO funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de NEOGOV.

Para configurar y probar el inicio de sesión único de Azure AD con NEOGOV, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
2. **[Configuración del inicio de sesión único en NEOGOV](#configure-neogov-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B. Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que B. Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de NEOGOV](#create-neogov-test-user)** , para tener un homólogo de B. Simon en NEOGOV que esté vinculado a la representación del usuario de Azure AD.
6. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **NEOGOV**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, seleccione **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la página **Configuración básica de SAML**, especifique los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente:
    
    | Entorno | Patrón de URL |
    | -- | -- |
    | Producción | `https://www.neogov.com/` |
    | Espacio aislado | `https://www.uat.neogov.net/` |
    | | |

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón:

    | Entorno | Patrón de URL |
    | -- | -- |
    | Producción | `https://login.neogov.com/authentication/saml/consumer` |
    | Espacio aislado | `https://login.uat.neogov.net/authentication/saml/consumer` |
    | | |

1. La aplicación NEOGOV espera las aserciones de SAML en un formato específico, lo que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token SAML. La siguiente captura de pantalla muestra la lista de atributos predeterminados, donde **nameidentifier** se asigna con **user.userprincipalname**. La aplicación NEOGOV espera que **nameidentifier** se asigne con **user.objectid**, por lo que debe editar la asignación de atributos haciendo clic en el icono **Editar** y cambiar esa asignación.

    ![imagen](common/edit-attribute.png)

1. Además de lo anterior, la aplicación NEOGOV espera que se usen algunos atributos más en la respuesta de SAML. En la sección Notificaciones del usuario del cuadro de diálogo Atributos de usuario, haga lo siguiente para agregar el atributo Token SAML como se muestra en la tabla siguiente:

    | NOMBRE |  Atributo de origen|
    | -------|--------- |
    | mail | user.mail |

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el botón de copia para copiar **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

### <a name="configure-neogov-sso"></a>Configuración del inicio de sesión único de NEOGOV

Para configurar el inicio de sesión único en **NEOGOV**, debe enviar la **dirección URL de metadatos de federación de la aplicación** al [equipo de soporte técnico de NEOGOV](mailto:itops@neogov.net). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado B. Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B. Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a permitir que B. Simon acceda a NEOGOV utilizando el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **NEOGOV**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B. Simon** en la lista de usuarios y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-neogov-test-user"></a>Creación de un usuario de prueba de NEOGOV

En esta sección, creará un usuario llamado B. Simon en NEOGOV. Colabore con el  [equipo de soporte técnico de NEOGOV](mailto:itops@neogov.net) para agregar los usuarios en la plataforma de NEOGOV. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="test-sso"></a>Prueba de SSO

Al seleccionar el icono de NEOGOV en el panel de acceso, debería iniciar sesión automáticamente en la versión de NEOGOV para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)