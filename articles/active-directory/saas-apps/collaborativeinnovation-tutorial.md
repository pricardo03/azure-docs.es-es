---
title: 'Tutorial: Integración de Azure Active Directory con Collaborative Innovation | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Collaborative Innovation.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bba95df3-75a4-4a93-8805-b3a8aa3d4861
ms.service: Azure-Active-Directory
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/22/2019
ms.author: jeedes
ms.openlocfilehash: a625e6677a706ddfffe1305c862b5a1a8e47c9ac
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55769243"
---
# <a name="tutorial-azure-active-directory-integration-with-collaborative-innovation"></a>Tutorial: Integración de Azure Active Directory con Collaborative Innovation

En este tutorial, aprenderá a integrar Collaborative Innovation con Azure Active Directory (Azure AD).
Integrar Collaborative Innovation con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Collaborative Innovation.
* Puede permitir que los usuarios inicien sesión automáticamente en Collaborative Innovation (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Collaborative Innovation, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único de Collaborative Innovation

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Collaborative Innovation admite el inicio de sesión único iniciado por **SP**

* Collaborative Innovation admite el aprovisionamiento de usuarios **Just-In-Time**

## <a name="adding-collaborative-innovation-from-the-gallery"></a>Adición de Collaborative Innovation desde la galería

Para configurar la integración de Collaborative Innovation en Azure AD, deberá agregar Collaborative Innovation desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Collaborative Innovation desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Collaborative Innovation**, seleccione **Collaborative Innovation** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![Collaborative Innovation en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, va a configurar y probar el inicio de sesión único de Azure AD con Collaborative Innovation con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Collaborative Innovation.

Para configurar y probar el inicio de sesión único de Azure AD con Collaborative Innovation, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Collaborative Innovation](#configure-collaborative-innovation-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Collaborative Innovation](#create-collaborative-innovation-test-user)**: para tener un homólogo de Britta Simon en Collaborative Innovation que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Collaborative Innovation, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Collaborative Innovation**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Collaborative Innovation](common/sp-identifier.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<instancename>.foundry.<companyname>.com/`

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://<instancename>.foundry.<companyname>.com`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de cliente de Collaborative Innovation](https://www.unilever.com/contact/) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. La aplicación Collaborative Innovation espera que las aserciones SAML estén en un formato concreto. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, edite las notificaciones mediante el **icono Editar** o agregue notificaciones mediante **Agregar nueva notificación** para configurar el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos: 

    | NOMBRE | Atributo de origen|
    | ---------------| --------------- |
    | givenname | user.givenname |
    | surname | user.surname |
    | emailaddress | user.userprincipalname |
    | Nombre | user.userprincipalname |

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

4. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Configurar Collaborative Innovation**, copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-collaborative-innovation-single-sign-on"></a>Configuración del inicio de sesión único de Collaborative Innovation

Para configurar el inicio de sesión único en **Collaborative Innovation**, es preciso enviar el **XML de metadatos de federación** descargado y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de Collaborative Innovation](https://www.unilever.com/contact/). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Collaborative Innovation.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **Collaborative Innovation**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Collaborative Innovation**.

    ![Vínculo de Collaborative Innovation en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-collaborative-innovation-test-user"></a>Creación de un usuario de prueba de Collaborative Innovation

Para permitir que los usuarios de Azure AD inicien sesión en Collaborative Innovation, se les deben aprovisionar en Collaborative Innovation.  

En caso de esta aplicación, el aprovisionamiento es automático ya que la aplicación admite el aprovisionamiento de usuarios Just-In-Time. Por tanto no hay ninguna necesidad de realizar ningún paso aquí.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Collaborative Innovation en el panel de acceso y debería iniciar sesión automáticamente en la versión de Collaborative Innovation para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

