---
title: 'Tutorial: Integración de Azure Active Directory con Pagerduty | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y PagerDuty.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 0e571880d9893c0027c200c6f49dc704fea09ead
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39443705"
---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Tutorial: Integración de Azure Active Directory con Pagerduty

En este tutorial, aprenderá a integrar PagerDuty con Azure Active Directory (Azure AD).

La integración de PagerDuty con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a PagerDuty
- Puede permitir que los usuarios inicien sesión automáticamente en PagerDuty (inicio de sesión único) con sus cuentas de Azure AD
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con PagerDuty, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en PagerDuty

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregación de PagerDuty desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-pagerduty-from-the-gallery"></a>Agregación de PagerDuty desde la galería
Para configurar la integración de PagerDuty en Azure AD, deberá agregar PagerDuty desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar PagerDuty desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **PagerDuty**, seleccione **PagerDuty** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/pagerduty-tutorial/tutorial_pagerduty_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con PagerDuty con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de PagerDuty para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario correspondiente de PagerDuty.

Para establecer la relación de vínculo, en PagerDuty, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con PagerDuty, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de PagerDuty](#create-a-pagerduty-test-user)**: para tener un homólogo de Britta Simon en PagerDuty que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación PagerDuty.

**Para configurar el inicio de sesión único de Azure AD con PagerDuty, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **PagerDuty**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/pagerduty-tutorial/tutorial_pagerduty_samlbase.png)

1. En la sección **Dominio y direcciones URL de PagerDuty**, lleve a cabo los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de PagerDuty](./media/pagerduty-tutorial/tutorial_pagerduty_url.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<tenant-name>.pagerduty.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<tenant-name>.pagerduty.com`

    > [!NOTE]
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte de cliente de PagerDuty](https://www.pagerduty.com/support/) para obtener estos valores.

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/pagerduty-tutorial/tutorial_pagerduty_certificate.png)

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/pagerduty-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de PagerDuty**, haga clic en **Configurar PagerDuty** para abrir la ventana **Configurar inicio de sesión**. Copie las **direcciones URL del servicio de inicio de sesión único de SAML y de cierre de sesión** de la sección **Referencia rápida**.

    ![Configuración de PagerDuty](./media/pagerduty-tutorial/tutorial_pagerduty_configure.png)

1. En otra ventana del explorador web, inicie sesión en el sitio de la compañía Pagerduty como administrador.

1. En el menú de la parte superior, haga clic en **Configuración de cuenta**.

    ![Configuración de la cuenta](./media/pagerduty-tutorial/ic778535.png "configuración de la cuenta")

1. Haga clic en **Inicio de sesión único**.

    ![Inicio de sesión único](./media/pagerduty-tutorial/ic778536.png "Inicio de sesión único")

1. En la página **Habilitar inicio de sesión único (SSO)**, siga estos pasos:

    ![Habilitar inicio de sesión único](./media/pagerduty-tutorial/ic778537.png "Habilitar inicio de sesión único")

    a. Abra el certificado codificado en Base 64 que descargó de Azure Portal en el Bloc de notas, copie el contenido en el Portapapeles y, luego, péguelo en el cuadro de texto **X.509 Certificate** (Certificado X.509)
  
    b. En el cuadro de texto **Login URL** (Dirección URL de inicio de sesión), pegue la **dirección URL del servicio de inicio de sesión único de SAML** que copió desde Azure Portal.
  
    c. En el cuadro de texto **Logout URL** (Dirección URL de cierre de sesión), pegue la **Dirección URL de cierre de sesión** que copió de Azure Portal.

    d. Seleccione **Allow username/password login** (Permitir inicio de sesión con nombre de usuario/contraseña).

    e. Seleccione la casilla de verificación **Require EXACT authentication context comparison** (Requerir comparación de contexto de autenticación EXACT).

    f. Haga clic en **Guardar cambios**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/pagerduty-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/pagerduty-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Botón Agregar](./media/pagerduty-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Cuadro de diálogo Usuario](./media/pagerduty-tutorial/create_aaduser_04.png) 

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-pagerduty-test-user"></a>Creación de un usuario de prueba de PagerDuty

Para permitir que los usuarios de Azure AD inicien sesión en PagerDuty, deben aprovisionarse en PagerDuty.  
En el caso de PagerDuty, el aprovisionamiento es una tarea manual.

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de PagerDuty ofrecida por PagerDuty para aprovisionar cuentas de usuario de Azure Active Directory.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en su inquilino de **Pagerduty** .

1. En el menú de la parte superior, haga clic en **Usuarios**.

1. Haga clic en **Agregar usuarios**.
   
    ![Agregar usuarios](./media/pagerduty-tutorial/ic778539.png "Agregar usuarios")

1.  En el cuadro de diálogo **Invite your team** (Invitar a su equipo), realice los pasos siguientes:
   
    ![Invite your team (Invitar a su equipo)](./media/pagerduty-tutorial/ic778540.png "Invite your team (Invitar a su equipo)")

    a. En el cuadro de texto **First and Last Name** (Nombre y apellidos), escriba el nombre de un usuario, por ejemplo, **Britta Simon**. 
   
    b. Escriba el **Email** (correo electrónico) del usuario, por ejemplo **brittasimon@contoso.com**.
   
    c. Haga clic en **Add** (Agregar) y después en **Send Invites** (Enviar invitaciones).
   
    >[!NOTE]
    >Todos los usuarios agregados recibirán una invitación para crear una cuenta de PagerDuty.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a PagerDuty.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a PagerDuty, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **PagerDuty**.

    ![Vínculo a PagerDuty en la lista de aplicaciones](./media/pagerduty-tutorial/tutorial_pagerduty_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de PagerDuty en el panel de acceso, debería iniciar sesión automáticamente en la aplicación PagerDuty.

Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/pagerduty-tutorial/tutorial_general_01.png
[2]: ./media/pagerduty-tutorial/tutorial_general_02.png
[3]: ./media/pagerduty-tutorial/tutorial_general_03.png
[4]: ./media/pagerduty-tutorial/tutorial_general_04.png

[100]: ./media/pagerduty-tutorial/tutorial_general_100.png

[200]: ./media/pagerduty-tutorial/tutorial_general_200.png
[201]: ./media/pagerduty-tutorial/tutorial_general_201.png
[202]: ./media/pagerduty-tutorial/tutorial_general_202.png
[203]: ./media/pagerduty-tutorial/tutorial_general_203.png
