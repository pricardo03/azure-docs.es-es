---
title: 'Tutorial: integración de Azure Active Directory con Shuccho Navi| Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Shuccho Navi.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 32b6676c-d1ec-48c2-91b1-41990ed0560c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2018
ms.author: jeedes
ms.openlocfilehash: f90af5b57fcb9ed7f02bba0a184dacb17570136b
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2018
ms.locfileid: "49312188"
---
# <a name="tutorial-azure-active-directory-integration-with-shuccho-navi"></a>Tutorial: Integración de Azure Active Directory con Shuccho Navi

En este tutorial, aprenderá a integrar Shuccho Navi con Azure Active Directory (Azure AD).

La integración de Shuccho Navi con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Shuccho Navi.
- Puede habilitar que los usuarios inicien sesión automáticamente en Shuccho Navi (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Shuccho Navi, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción de Shuccho Navi habilitada para el inicio de sesión único

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Shuccho Navi desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-shuccho-navi-from-the-gallery"></a>Adición de Shuccho Navi desde la galería
Para configurar la integración de Shuccho Navi en Azure AD, será preciso que agregue Shuccho Navi desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Shuccho Navi desde la galería, siga los pasos a continuación:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/shucchonavi-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/shucchonavi-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/shucchonavi-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **Shuccho Navi**, seleccione **Shuccho Navi** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/shucchonavi-tutorial/tutorial_shucchonavi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Shuccho Navi utilizando una usuaria de prueba llamada "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Shuccho Navi para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Shuccho Navi.

Para configurar y probar el inicio de sesión único de Azure AD con Shuccho Navi, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Shuccho Navi](#create-a-shuccho-navi-test-user)**: para tener un homólogo de Britta Simon en Shuccho Navi que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Shuccho Navi.

**Para configurar el inicio de sesión único de Azure AD con Shuccho Navi, siga estos pasos:**

1. En la página de integración de la aplicación [Shuccho Navi](https://portal.azure.com/) de **Azure Portal**, haga clic en **Inicio de sesión único**.

    ![imagen](./media/shucchonavi-tutorial/b1_b2_select_sso.png)

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![imagen](./media/shucchonavi-tutorial/b1_b2_saml_sso.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/shucchonavi-tutorial/b1-domains_and_urlsedit.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://naviauth.nta.co.jp/saml/login?ENTP_CD=<Your company code>`

    ![imagen](./media/shucchonavi-tutorial/tutorial_shucchonavi_url.png)

    > [!NOTE]
    > El valor de la dirección URL de inicio de sesión no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de Shuccho Navi](mailto:sys_ntabtm@nta.co.jp) para obtener el valor.
 
5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el archivo **XML de metadatos de federación**  y guárdelo en su equipo.

    ![imagen](./media/shucchonavi-tutorial/tutorial_shucchonavi_certificate.png) 

6. Para configurar el inicio de sesión único en el lado de **Shuccho Navi**, tiene que enviar el archivo **XML de metadatos de federación** descargado al [equipo de soporte técnico de Shuccho Navi](mailto:sys_ntabtm@nta.co.jp). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, seleccione **Usuarios** y, luego, seleccione **Todos los usuarios**.

    ![imagen](./media/shucchonavi-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/shucchonavi-tutorial/d_adduser.png)

3. En las propiedades Usuario, siga los pasos a continuación.

    ![imagen](./media/shucchonavi-tutorial/d_userproperties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, seleccione **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-a-shuccho-navi-test-user"></a>Creación de un usuario de prueba de Shuccho Navi

En esta sección, creará un usuario llamado Britta Simon en Shuccho Navi. Trabaje con el [equipo de soporte técnico de Shuccho Navi](mailto:sys_ntabtm@nta.co.jp) para agregar los usuarios en la plataforma de Shuccho Navi. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Shuccho Navi.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, seleccione **Todas las aplicaciones**.

    ![imagen](./media/shucchonavi-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **Shuccho Navi**.

    ![imagen](./media/shucchonavi-tutorial/tutorial_shucchonavi_app.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/shucchonavi-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos**, en el panel **Agregar asignación**.

    ![imagen](./media/shucchonavi-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Shuccho Navi del panel de acceso, debería iniciar sesión automáticamente en su aplicación Shuccho Navi.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


